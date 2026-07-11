---
title: 'The Missed Call Economy'
description: 'A tradesperson misses calls because they are elbow-deep in a job at 2pm. The lead goes to the next result on Google. The fix took an afternoon.'
pubDate: 'Jul 11 2026'
---

At 2pm on a Tuesday, a tradesperson is elbow-deep in a job. Their phone rings. They can't answer it. The caller waits four rings, hangs up, searches the same phrase they searched an hour ago, and calls the next result down.

That call was probably worth a few hundred pounds. The next result got it.

This happens every day. I only found out how often when I started reading the call log as a business document rather than a list of unanswered notifications.

## What the log said

A client in the trades -- that is all I will say about who -- had a detailed record of every call that came in and went unanswered. Not anecdotal; timestamped, with numbers. The pattern was clear and boring: most missed calls arrived around midday and mid-afternoon. Those are the two times a tradesperson is most likely to be working. People call when they think someone will pick up. No one picks up. They call someone else.

The conversion rate on a missed call that gets a reply within a minute is meaningfully higher than one that gets a callback the next morning. By next morning, many of those leads have already made an appointment with whoever did answer. The window to keep someone in the pipeline is narrow, measured in minutes rather than hours.

## The build

GHL fires a workflow when a call is missed. The workflow waits thirty seconds -- long enough that a call ending after three rings is not counted as a genuine miss -- and then sends a WhatsApp: something like *Hi, I just missed your call. I'm on a job at the moment. What can I help you with?* If a contact name is already in the record, it uses it; otherwise it leaves it out. The message is short and goes out in under a minute.

That is the entire build. No ML models. No agent chain. No vector database. The hardest part was getting the missed-call trigger to correctly distinguish a genuine miss from a call that connected briefly before ending, which required reading some GHL documentation that turned out to be partly wrong, followed by testing it fourteen times one Friday afternoon with two phones and a lot of patience.

It is the least technically sophisticated thing I have built this year.

## The arithmetic

Average job value in this trade: several hundred pounds, sometimes considerably more. Even if the WhatsApp only recovers one lead per month that would otherwise have been lost, the automation pays for itself inside its first week of existence. In practice the number is higher.

The point is not the exact figure. The point is that the missed call log was already there, fully timestamped, itemising every instance of the problem. The value was sitting in it the whole time. Nothing was broken. No API was returning errors. Nobody had filed a bug report. The leads were quietly going elsewhere, the log was quietly recording it, and nobody was reading the log as a P&L.

## Complexity and value

There is a tendency in this kind of work to reach for the complex thing. The inference pipeline, the multi-stage reasoning loop, the thing with a diagram attached. Sometimes the problem genuinely calls for that.

The missed call WhatsApp is not that. It is a webhook, a thirty-second wait, and a template message. It could have been built a decade ago. My contribution was sitting with the call log long enough to notice the pattern and price it. The rest was an afternoon in GHL's workflow editor.

I built three other things for the same client that were technically more interesting. I wouldn't bet on any of them recovering more value than the WhatsApp.

The gap between technical complexity and business value is real, and it runs in the wrong direction more often than it should. The clever builds get written about. The webhook that answers the phone gets the jobs.
