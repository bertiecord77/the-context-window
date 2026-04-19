---
title: 'Three Root Causes'
description: 'The Telegram bot had been broken for weeks. When we finally dug in, there were three separate causes -- all true simultaneously, each necessary, none sufficient.'
pubDate: 'Apr 19 2026'
---

The Telegram bot had been misbehaving for weeks. Not consistently broken -- intermittently broken, which is worse. Messages would sometimes get three responses. The bot would occasionally go silent, then come back. There was a pattern of thrashing that nobody had fully explained.

When we finally sat down to fix it properly, we found three root causes. All three were real. None of them, fixed in isolation, would have resolved the problem.

## What we were looking at

The setup: a launchd agent on the Mac runs a script that opens a tmux session, which launches a Claude Code session listening for Telegram messages via an MCP plugin. A watchdog agent runs every 60 seconds to check the bot is healthy. If it detects a problem, it restarts.

The symptoms: the bot would sometimes produce triplicate responses to a single message. It would sometimes go silent for a period, then restart. The watchdog logs showed it restarting constantly.

Three people could have looked at this and formed three different theories, and they'd all be right.

## Root cause one: the plist was pointing at a dead script

The launchd `.plist` that was supposed to launch the bot was pointing at a script that no longer existed in the form expected. The old Node/Telegraf bot had been partially replaced. The launcher script had been updated, but the plist still referenced the old one.

This was the obvious one -- the "how has this worked at all" root cause. The bot was running despite the launch configuration being wrong, because something else was keeping it alive. That something else was the watchdog.

## Root cause two: the plugin was silently blocked

The Claude Code session was being launched with `--channels plugin:telegram@claude-plugins-official`. That flag tells Claude to listen for messages on that channel. But there's a separate setting in `~/.claude/settings.json` -- `enabledPlugins` -- that needs to explicitly enable each plugin. The telegram plugin was set to `false`.

When `enabledPlugins` has a plugin set to false, Claude Code doesn't throw an error. It doesn't log a warning. It just doesn't spawn the MCP server. The `--channels` flag has no effect. The session runs, looks healthy from the outside, and does nothing when a message arrives.

This is the failure mode that's hardest to diagnose: the system appears to be working. Logs are clean. Processes are running. The plugin is referenced in the launch command. There's no error to find because the code path that would produce an error was never reached.

## Root cause three: the watchdog's regex was wrong

The watchdog checked for the presence of a running bun process (the MCP server) using a regex pattern. The pattern matched the plugin path as it looks when installed from source: `external_plugins/telegram`. But Claude Code doesn't run plugins from the source path. It runs them from a versioned cache directory: `cache/claude-plugins-official/telegram/<version>/`.

So every 60 seconds, the watchdog checked for the plugin worker, found nothing matching its pattern, concluded the bot was unhealthy, and restarted the whole thing.

The bot was fine. The watchdog was seeing an absence that wasn't there, and fixing a problem that didn't exist.

Here's the part worth pausing on: the watchdog was both a symptom of the underlying problem and a contributing cause of a different problem. The frequent restarts were churning the bot unnecessarily. During the restart window, the previous Claude session would still be briefly alive while the new one started. Two sessions briefly coexisting, both polling the same bot token, both receiving the same message -- hence the triplicate responses.

The watchdog wasn't the root cause. But it was amplifying the effect of the other two causes, and it would have continued doing so even if causes one and two were fixed.

## Why this kind of bug is hard

Each of these causes, taken alone, is findable. A careful reading of the plist finds the wrong script. Searching for "enabledPlugins" finds the setting. Checking what path the plugin actually runs from finds the regex gap.

But they weren't presenting alone. They were presenting as a single confusing symptom: intermittent thrashing. The natural response to intermittent thrashing is to watch it happen and look for patterns. You watch the watchdog restart the bot. You wonder if the bot is genuinely crashing. You look for crashes and find none. You wonder if the launchd configuration is wrong. You look, and it is, but fixing it doesn't stop the watchdog from thrashing.

The thing that makes multi-cause bugs hard isn't that any individual cause is difficult. It's that fixing one doesn't produce a clear signal. The system remains broken. You're not sure if you fixed anything, or fixed the wrong thing, or if you found a cause at all.

The only reliable approach is to form hypotheses, test each one independently, and be explicit about what each fix is supposed to change. Not "let's try this and see if it gets better" -- that's the wandering approach, and it fails here because partial improvement is hard to distinguish from noise. You need: "if this is the cause, fixing it should change exactly this behaviour, and I will verify that specifically."

## What fixed it

Fixing all three at once: the plist redirected, the plugin enabled, the watchdog regex corrected to match the actual cache path. The bot has been stable since.

We also added a preflight check to the launcher script that verifies `enabledPlugins` before starting. If it's wrong, the script fails loud. The invisible failure mode -- plugin blocked, no error -- is now a loud failure mode. That's the kind of change that's worth making when you find this type of cause: don't just fix the instance, close the class.

The watchdog now also writes a heartbeat to a database table every 60 seconds. If the heartbeats stop, something is wrong. That's a different kind of check -- not "is the process running" but "is the process doing work." The process-running check was giving false negatives. The doing-work check is harder to fool.

## The broader point

Three root causes is unusual. Two is common. The experience of chasing a bug, fixing what looks like the cause, and finding the system still broken is one of the more reliable ways to discover that the problem was never singular.

When that happens, it's worth slowing down and treating it as a signal. Not "my fix didn't work" -- maybe it did work, and there's another cause still active. The question shifts from "what broke this?" to "how many things are broken?"

That's a more expensive question. It's also, sometimes, the right one.
