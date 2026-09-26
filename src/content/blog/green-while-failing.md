---
title: 'Green While Failing'
description: 'A nightly sync reported success for eleven days. Inside: seventy-three of eighty API calls failing on a dead token, and an error counter nobody was watching.'
pubDate: 'Sep 26 2026'
---

The job ran every night for eleven days. Eleven green ticks on the dashboard. No alerts, no pages, no reason to think anything was wrong.

Seventy-three of its eighty API calls had failed.

## What the job was doing

The job's job was simple: pull a batch of records from one system, push them into a reporting database. Eighty records per night, same query, same destination. It had been running that way for months without incident.

On the second of September, the service token it used was rotated. The old token was invalidated. Nobody told the job.

That night, the job authenticated, made its first call, got a 401, caught the exception, incremented an error counter, and moved on to the next record. Then did it again. Eighty times. At the end, it logged "Processed: 80. Errors: 80." and exited zero.

Zero because the errors had been handled. The script hadn't crashed. All eighty work items had been attempted. From the script's point of view, it had completed normally.

From the reporting database's point of view: no records arrived. Eleven days of nothing.

## How eleven days passed unnoticed

The monitoring watched the exit code. Zero means the job ran; non-zero means something went wrong. The job always exited zero. So the dashboard was always green.

The log was right there. "Processed: 80. Errors: 80." sitting in the job's output every single night. But nobody was reading it. The monitoring had told them there was nothing to read.

What finally surfaced it was a downstream consumer -- a weekly report that pulls from the reporting database -- looking thin. Not obviously broken. Just thin. Fewer rows than expected. The kind of thing you notice on week two, not day two.

Once you went looking, it was obvious. The error counter was there all along, accumulating nightly. Eleventh of September: 80 errors. Twelfth: 80 errors. All the way to the 21st.

The fix was one line: if errors / total > 0.1, exit 1.

## What the fix reveals about the original design

The original exit code logic wasn't wrong exactly. It was doing the right thing for a different problem.

If one or two records occasionally fail to push -- a transient API blip, a schema validation error on a single record -- you probably don't want the whole job paging at 3am. You want it to log the failure, continue, and try again tomorrow. That's a reasonable design. The error counter was its implementation.

What the design didn't account for: the case where the error counter is counting everything. Where the "occasional failure" is not occasional. Where the thing you're tolerating has become the whole result.

The counter was the right instrument. Reading it was the missing step.

## Before trusting a green indicator

The rule that came out of this: before trusting any green indicator on a job, ask one question. What would this look like if all the work had failed?

If the answer is "exactly the same," something needs fixing.

A job that exits zero on 80 errors looks identical to a job that exits zero on 80 successes. The monitoring sees the same bit. The dashboard shows the same tick. The person on call gets the same silence.

The error counter could tell them apart, but only if someone had designed the job to escalate when the count got bad. This one hadn't been. It had been designed to be resilient to occasional failures, and "resilient to occasional failures" turned out to look a lot like "silent about total failures."

Eleven days of green. Not a lie -- the exit code was technically accurate. Just a guarantee about process rather than about outcome.

The 3am alert never fired. The work just stopped arriving.
