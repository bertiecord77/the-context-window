---
title: 'Fifteen Credits a Deploy'
description: 'Two sites became cost hotspots because deploys were happening outside the pipeline that would have made them visible. The fix was governance, not restraint.'
pubDate: 'Sep 12 2026'
---

The Netlify bill didn't look wrong. It was inside budget, inside expected range, nothing that would trigger an alert. It just kept being a bit higher than it should have been, in the way that's easy to ignore because the number itself is small and investigating feels like making a fuss.

Then the credit balance started running low, and "a bit higher than it should have been" became a problem worth digging into.

We run about a dozen sites through Netlify. The pipeline is consistent: push to the relevant branch, GitHub Actions triggers, Netlify builds and deploys. Every step is logged. You can see exactly what deployed, when, from which commit, triggered by whom.

When I pulled the deploy counts, two sites were immediately obvious. One had deployed 47 times in a month. The other, 38 times. The rest of the estate -- ten sites -- had between three and twelve deploys each. These two were outliers by a factor of three or four.

Neither of them ran particularly complex builds. No long dependency installs, no image processing, nothing that would inflate the cost per deploy. The issue was pure frequency. At fifteen credits a deploy, 85 deploys is 1,275 credits. The other ten sites combined: maybe 900 credits. The two problem sites were costing more than everything else put together.

## How the pipeline hid them

The monitoring covers what happens in the pipeline. A push to main kicks off a build; the build logs, the deploy logs, and the branch history all record it. If you're auditing spend, you look at the git log and count the merges. Reasonable assumption: deploys track commits.

The problem: both sites had been getting manual deploys through the Netlify CLI. `netlify deploy --prod`. Fast, direct, no branch required. Useful when you need to push a hotfix at 11pm and you don't want to faff with PRs. Also completely invisible to the git log, the branch history, and any monitoring built around those.

The commit history said: about fifteen changes each. The actual deploy count said: 47 and 38.

The gap between them is the off-pipeline deploy -- a direct push to production that happens for good reasons, is completely valid in the moment, and accumulates invisibly outside everything that would let you see it happening.

## Why "deploy less" isn't the fix

The instinct, when you find spend unexpectedly concentrated in two places, is to go to the people involved and say something costs more than they probably realise. Ship a bit less. Think about whether it's really necessary.

Wrong frame. The problem isn't that two projects have active development. Deploys should happen when the work is ready. Trying to throttle that introduces friction in the wrong direction.

The problem is that a subset of the deploys was outside the visibility layer. Nobody could see them accumulating because they weren't in the thing everyone looks at. The spend was there; the signal wasn't.

The fix is governance, not restraint. Every deploy goes through git. Full stop. Push to main or merge a PR; Netlify picks it up from there. The CLI shortcut still works fine in staging. In production: if it's not in the commit history, it doesn't go out.

Now the deploy count is legible. You can look at the git log and know what the Netlify bill is going to look like. The two problem sites are still shipping frequently -- that's fine, that's the work -- but it's visible spend now, not hidden spend.

## What per-action pricing does to you

Fifteen credits a deploy is not an expensive action. It's designed to feel cheap, because individually it is. The thing that makes per-action pricing interesting is that it moves the cost to frequency rather than scale. A big, complicated site that deploys once a week costs less than a simple site that deploys daily.

When the spend-per-action is small, off-pipeline actions are especially easy to ignore. You're not making a big expensive call, you're just running a quick deploy. Each one is individually justified. The accumulation is only visible when you look across the whole billing period, and that's only useful if you look.

The rule we ended up with: if it's real, it's in git. Not because commits are sacred but because git is where the audit is. Off-pipeline means off the record, and off the record means you find out at billing time, not before.

Eighty-five deploys in a month. 1,275 credits. Fifteen each.
