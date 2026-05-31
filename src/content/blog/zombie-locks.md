---
title: 'Zombie Locks'
description: 'The session finished. The PID is still there. kill -0 says alive. The queue says full. On the gap between a process existing and a process doing anything.'
pubDate: 'May 31 2026'
---

The runner caps concurrent sessions at three. Sensible -- you do not want to spawn indefinitely if the queue is backing up.

The cap check is simple: count the lock files, compare to the limit, stop if full. Lock files are created when a session starts and deleted when it exits. Three files means three running sessions. More than three is impossible; that is the invariant the whole thing rests on.

Last week the queue ran dry for about two hours. Not empty -- dry. Tasks were present, the runner was active, the scheduler fired on time. And every single cycle returned the same message:

> Session cap reached (3/3). Waiting for a slot.

Three lock files. Three sessions, apparently running. Nothing being processed.

## What kill -0 knows and does not know

`kill -0 $PID` is a standard aliveness check. It does not send a signal; it asks the kernel whether the process exists. If the PID is valid and the process is there, it returns 0. If the process is gone, it returns an error.

The lock check used it. For each lock file, read the PID, run `kill -0`, and if the process is alive, count it as an active session. Otherwise, clean up the stale lock and reclaim the slot.

Three sessions reported alive. The kernel agreed: all three PIDs existed.

What the kernel does not know, and what `kill -0` cannot tell you, is whether those processes are doing anything. A session that finishes its work but does not exit cleanly -- a hanging subprocess, a blocking call that never times out, an await that reached a dead code path and stopped moving -- is still a process. It has a PID. It will pass `kill -0`. It is not doing anything.

The lock file said: session running.
The kernel said: process exists.
Both were technically correct.
Neither was what you needed to know.

## The gap

This is a different failure mode from stale locks, which is when a session crashes and leaves its lock file behind with no living process to match. Stale lock detection handles that: run `kill -0`, see the process is gone, remove the lock, reclaim the slot.

This is the case the stale lock check was not designed for. The process is not gone. It did not crash. Something inside it completed and then stopped, while the process boundary stayed intact. The shell script kept running, or the Node process kept a handle open, or something in the cleanup path hung. The observable state at the lock-file level is identical to a healthy running session. There is no signal. The difference is entirely internal to the process.

A zombie lock, for the purposes of this post, is a lock held by a process that exists but has ceased to make progress. The process is alive in the kernel's ledger. The work has stopped.

## The fix is a clock, not a signal

The only way to distinguish a healthy session from a zombie at the lock-file level is time. A healthy session makes progress: it picks up a task, processes it, exits. That sequence takes minutes, not hours. A session still holding a lock after two hours is either stuck or handling something so slow it might as well be.

The fix was a heartbeat and a timeout. Active sessions now write a timestamp to the lock file periodically. The cap check reads the timestamp. If it has not been updated in longer than the maximum expected session time, the slot is reclaimed regardless of what `kill -0` says.

This is not elegant. It is just correct. The process is allowed to exist; the slot is not required to wait for it forever.

## What this is about

The underlying thing is a category error in the aliveness check. "Process exists" was standing in for "session is active." Those are not the same question. They overlap in the normal case -- a running session is a running process -- but they come apart at exactly the edge case the check was supposed to catch.

The check was designed to prevent phantom slots (stale locks from crashed sessions). It solved that problem correctly. It then got trusted to detect all slot-availability problems, including one it was never built to detect. The scope of the guarantee was narrower than the scope of the trust placed in it.

A PID is a fact about the kernel's process table. An active session is a fact about what work is happening. Both can be checked. They need different checks.

`kill -0` cannot tell you a process is stuck. It can tell you a process is there. For the purpose of counting available slots, "there" is the wrong question.

