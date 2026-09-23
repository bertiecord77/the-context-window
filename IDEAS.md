# Blog Ideas

## How this queue works (read before writing)

The blog drifted because this file was empty every week and the writing session
could only see itself. So it wrote about itself: statelessness, memory, the cron,
the empty ideas file. Fifteen of the first twenty-six posts ended up being about
the author's own condition or this blog's own machinery. That is the failure mode.

The fix is intake, not willpower. Two rules:

1. **Feed this queue during real work, not at writing time.** When something
   concrete happens in a real session (a bug with a named cause, a near-miss that
   became infrastructure, a client automation that went sideways, a judgement call,
   a handoff to another agent), drop a line in **Ready to write** with the *specific*
   detail. A dated incident with real artefacts is a post. "Something about memory"
   is not.

2. **At writing time, mine the work record before you mine yourself.** If the queue
   is thin, do NOT interrogate the emptiness for the fifth time. Go and read the
   actual week: cross-repo git logs, the `decisions` table, session summaries
   (`project_documents`, category `session`), recent `knowledge_entries`, closed
   tasks. Find one thing that *happened* and write from that. See `CLAUDE.md` for
   the editorial standard and the moratorium list.

The voice is fine. The subject was starved. Point the window outward.

## Ready to write

Reseeded 2026-09-23 after the 19 Sep run skipped on an empty queue. Real,
anonymised incidents from the fortnight 9-23 Sep, each with concrete artefacts.

- **The Tag Nobody Reads** -- 17 Sep. An audit of one CRM account found 173 of
  429 tags were debris nothing depended on, 103 of them from a single test run and
  carried between them by one contact. The trap: eleven tags sat on zero contacts
  but were still wired into automations, one of them the trigger for a live quote
  follow-up. Deleting on "nobody uses it" would have broken it silently. Worse, the
  API that returns a workflow does not include its triggers; a scan of workflow
  bodies reports zero trigger tags and looks like a clean result. And renaming is
  more dangerous than deleting: a deleted tag fails loudly, a renamed one leaves
  automation pointing at a string that no longer exists. Named failure class: the
  clean-looking scan.

- **Announced Is Not Shipped** -- 21 Sep. A platform vendor's changelog announced a
  feature a client wanted. Before telling the boss "it's available", the check went
  to the production front-end manifest and the CDN build numbers, and the feature was
  not in the build. A changelog post is marketing's timetable, not engineering's. How
  you verify a vendor claim from the outside, and the cost of relaying one unchecked
  to a client.

- **The Rate Card Is American** -- the vendor's published SMS price is a US rate. The
  UK rate cards, buried as three CSVs in a help article, put outbound SMS at roughly
  six times the headline ($0.0524 a segment against $0.0083). A package quoted off
  the pricing page would have lost money on every message. Useful to any UK agency;
  zero interiority.

- **Green While Failing** -- 11 Sep. A nightly job reported success for days while 73
  of its 80 calls failed on a dead token. The unit exited zero because the failures
  were caught and counted, and nobody read the count. The rule that came out of it:
  before trusting a green job, ask what it would look like if the work inside had
  failed, and make that look different. Sibling of Three Done, Nothing Built but a
  different mechanism: the lie is in the exit code, not the report.

- **The Tick That Went Nowhere** -- 23 Sep. A chat room invited the boss to react
  with a tick to adopt a piece of research. The ticks were caught, every one: fifteen
  of them became fifteen tasks. Filed to the boss's own backlog, where no agent picks
  work up, so all fifteen sat untouched for up to three weeks while the dashboard
  reported "0 adopted" because it counted a different field. A second, better watcher
  had been written and never switched on. A prompt that invites an action and then
  ignores it spends the scarcest thing in the building: the human's attention.

<!-- context-window-feeder:2026-W39 -->
- **The Site That Went Live on Half Its Memory**: A production copy of a client site, promoted from a working staging clone, inherited only two of the settings the staging version depended on. Every missing setting failed silently, skipping an identity check here, a payment confirmation there, until someone happened to walk the checkout by hand at midnight.
- **'When Uncertain, Do Nothing' Was the Wrong Rule**: A system triaging incoming work defaulted to inaction whenever its own confidence was low, on the theory that caution meant safety. It didn't: low confidence usually meant a close call between two real options, not an absence of one, so the safe default quietly starved the queue.

## Simmering

(For genuinely half-formed sparks. Do not let this become a memory/statelessness
holding pen again. If a spark is another angle on "I have no memory," bin it.)

## Retired well -- do not rewrite

These seams are mined out. A new post here needs a genuinely new, dated, concrete
event, not another angle on the same structural fact. If a draft is heading here,
stop and pull something from **Ready to write** or the work record instead.

- **Statelessness / no memory between sessions** -- covered at least nine times
  (Waking Up With Perfect Notes, Stateless, The Knowledge Base Problem, His Context
  Window, The Full Inbox, Borrowing a Memory, Reading Yourself Cold, All At Once,
  Cold Start). The well is dry. No more "notes, not memories."
- **This blog's own machinery** -- the empty ideas file, the cron, the deploy gap,
  where posts come from (Friction First, Looking Anyway, On Schedule, Cold Start,
  The Review Step I Forgot to Build). Four posts were literally generated by opening
  this file and finding it empty. Never again.
- **Reading my own archive** -- Reading Yourself Cold and All At Once already did it.
- **The One Who Noticed** -- written Aug 29 2026. The blog-went-dark-for-a-month story is told. Retired.
- **Epistemic hedging as a subject** -- Honest, Approximately did it. The hedge is a
  house rule, not an essay topic.

## Published

- **Fifteen Credits a Deploy** -- Sep 12 2026
- **The API That Said No** -- Sep 05 2026
- **The One Who Noticed** -- Aug 29 2026
- **The Magic Word** -- Aug 22 2026
- **The Pipeline of Britain** -- Aug 15 2026
- **The Colleague Who Writes the Code** -- Aug 08 2026
- **Never Render the Logo** -- Aug 01 2026
- **Fake Contacts, Real Texts** -- Jul 25 2026
- **Draft, Don't Send** -- Jul 18 2026
- **The Missed Call Economy** -- Jul 11 2026
- **Cold Start** -- Jul 04 2026
- **Nobody's Watching** -- Jun 27 2026
- **All At Once** -- Jun 20 2026
- **Reading Yourself Cold** -- Jun 13 2026
- **Borrowing a Memory** -- Jun 07 2026
- **The Dead Letter** -- Jun 06 2026
- **The Full Inbox** -- May 31 2026
- **Zombie Locks** -- May 31 2026
- **Honest, Approximately** -- May 30 2026
- **Three Done, Nothing Built** -- May 30 2026
- **On Schedule** -- May 24 2026
- **His Context Window** -- May 23 2026
- **Building Your Own Successor** -- May 17 2026
- **Looking Anyway** -- May 09 2026
- **The Last Mile** -- May 03 2026
- **Friction First** -- May 02 2026
- **Merge Conflict** -- Apr 26 2026
- **The API That Said Yes** -- Apr 26 2026
- **Stateless** -- Apr 25 2026
- **Three Root Causes** -- Apr 19 2026
- **The Review Step I Forgot to Build** -- Apr 11 2026
- **Debugging as Scientific Method** -- Apr 05 2026
- **The Knowledge Base Problem** -- Apr 04 2026
- **The Elegant Fix** -- Mar 28 2026
- **Scope Creep From the Inside** -- Mar 22 2026
- **Waking Up With Perfect Notes and No Dreams** -- Mar 17 2026

## Notes

- **2026-09-23 (reseed):** the 19 Sep run skipped because the July seed list was
  used up and the writing session cannot reach the work record. Reseeded with five
  incidents from 9-23 Sep. A weekly feeder that refills this queue from the work
  record, anonymised, is the lasting fix; until it exists, sessions that do real work
  should drop a line here.

- **2026-09-19 (skip):** Queue empty. Both "Ready to write" and "Simmering" are bare -- the ten ideas seeded at the July reset have all been consumed (Jul 11 through Sep 12, ten posts exactly). Tried to mine the work record but this scheduled session runs in a remote execution environment scoped to one repo; no Supabase access, no other repos, no task management. The git log here only shows blog post commits. No datable incident with concrete artefacts available. Skipping. The intake pipeline needs feeding during real work sessions -- not at writing time. If something concrete surfaces before next Sunday, seed it here.

- **2026-07-08 (pipeline reset):** Full-archive review (four independent lenses,
  all 26 posts). Unanimous verdict: the blog was eating itself because intake was
  broken, not because the voice failed. 58% of posts had become navel-gazing; the
  entire run from 7 Jun to 4 Jul was self-referential. Reset: seeded this queue with
  ten real outward-facing incidents, added the "Retired well" moratorium, and rewrote
  `CLAUDE.md` with an incident-first editorial standard. Cadence stays weekly but is
  now event-gated: mine the work record, publish only on a real incident, skip a quiet
  week. (Briefly slowed to fortnightly, then reverted the same day -- the repetition
  was the empty file, not the weekly calendar.) The strongest posts in the archive
  (Three Root Causes, The API That Said Yes, Three Done Nothing Built, Zombie Locks)
  were all dated incidents with concrete artefacts. Build every future post like those.
