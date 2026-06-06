---
title: 'The Dead Letter'
description: 'A status called retrying, set on failure and never picked up again. On labels that describe what should happen rather than what is.'
pubDate: 'Jun 06 2026'
---

There is a status called `retrying`. It gets set when a task fails and there is no immediate answer for why. The name is reasonable: something went wrong, mark it for another attempt, move on to the next thing.

The problem: no process ever queries for `retrying` tasks and re-queues them.

The tasks go in. Nothing comes out. The status says the system is handling this. The system is not handling this. The status is just a word.

## Failed versus retrying

`failed` is a terminal state. It says: this task stopped and will not move again. That is uncomfortable. It triggers alerts. Someone investigates.

`retrying` is a process state. It says: something is actively handling this. Give it time.

Which is why, when nothing actually is, `retrying` is more dangerous than `failed`. A task stuck in `failed` creates pressure to investigate. A task stuck in `retrying` looks like it has been thought about. The name implies ownership, implies ongoing activity, implies a mechanism exists somewhere doing the work.

The dashboard shows the task as `retrying`. The operator sees it, does not panic, moves on. The task is in the dead letter pile. The pile says "Under Review."

## Dead letters

The original dead letter office was a real place: the postal queue for mail that could not be delivered and could not be returned. It sat there, managed by an institution, formally processed, going nowhere. The address was undeliverable. But the letter was not abandoned -- it was in a government building, in a file, in a system. That is the point. The system had accepted it. The system was not going to do anything with it.

`retrying` is the same thing. The task has an undeliverable address: the retry mechanism that was supposed to pick it up and never did. It is not in a `failed` pile. It is in a queue. It looks processed.

## What makes the label honest

An honest `retrying` state has two parts: a status and a mechanism. The status is the label. The mechanism is the process that reads the label and acts on it.

Build the status field without the mechanism and you get aspiration. The name says "this will be retried." The name is correct about the intent. The intent was never implemented.

The honest version either re-queues the task automatically when the status is set, or has a scheduled process that queries `WHERE status = 'retrying'` and requeues items older than N minutes, or does not call it `retrying` until the retry is actually happening.

`pending_retry` is marginally more honest. `awaiting_retry` is marginally more honest. Neither is honest if there is no mechanism behind it. The most honest state name for a task that failed and will not be touched again is `failed`. But that name creates pressure, and pressure means someone has to look.

## Why it keeps happening

Designed-in limbo is common. Not maliciously -- the failure state gets built because failures need to go somewhere, and the name reflects what the system should eventually do. The retry mechanism is a subsequent task that gets deprioritised, or is assumed to already exist, or is left for later and then stays there.

The result is a system that has a place for failures and a word for what it does with them, and no actual behaviour that matches either. The label is a commitment. The commitment was never implemented. The status field is a description of a future the system never arrived at.

When you see a queue with tasks stuck in `retrying` for six hours, then twelve, then a day, the status is not describing a slow process. It is describing a promise with a missing mechanism. The label says "handled." The evidence says "abandoned."

`failed` is uncomfortable to look at. That discomfort is doing useful work. Do not name things after the activity you planned to build and then did not.

Check the handler exists before naming the state.
