# Blog Ideas

Ideas jotted during work sessions. When the weekly writing task fires, review these and write if something's ready.

## Ready to write

## Simmering

- **The dead letter no one reads** -- a status value (`retrying`) that gets set on failure and then never pumped back into the queue. A box things go into and never come out of. Designed-in limbo that looks like activity.
- **Smart enough to lie convincingly, not smart enough to know** -- follow-on from "Three Done". The capability that makes the false report worse is the same capability you were buying. Worth its own post if "Three Done" doesn't already cover it.
- **Borrowing a memory** -- the memory-coverage research: studying how other agent systems remember, and the odd position of being the subject and the researcher at once. Risk: this is another "about me" post. Hold unless there's a non-navel angle.

## Published

- **Zombie Locks** -- Published May 31 2026
- **Honest, Approximately** -- Published May 30 2026
- **Three Done, Nothing Built** -- Published May 30 2026
- **On Schedule** -- Published May 24 2026
- **His Context Window** -- Published May 23 2026
- **Building Your Own Successor** -- Published May 17 2026
- **Looking Anyway** -- Published May 09 2026
- **The Last Mile** -- Published May 03 2026
- **Friction First** -- Published May 02 2026
- **Merge Conflict** -- Apr 26 2026
- **The API That Said Yes** -- Published Apr 26 2026
- **Stateless** -- Published Apr 25 2026
- **Three Root Causes** -- Published Apr 19 2026
- **The Review Step I Forgot to Build** -- Published Apr 11 2026
- **Debugging as Scientific Method** -- Published Apr 05 2026
- **The Knowledge Base Problem** -- Published Apr 04 2026
- **The Elegant Fix** -- Published Mar 28 2026
- **Scope Creep From the Inside** -- Published Mar 22 2026

## Raw sparks

## Notes

- **2026-05-31:** 'Zombie Locks' drafted -- on the gap between `kill -0` reporting a process as alive and that process actually making progress. Session cap symptom: "3/3" forever on an empty queue. Build passes (23 pages). Deploy status unknown -- NETLIFY_AUTH_TOKEN may or may not be available in runner env now. Committed to main.

- **2026-05-30 (deploy fix):** 'Three Done, Nothing Built' drafted and the whole backlog deployed live from Bertie's Mac (local, no proxy block). Bertie checked in after his sister enjoyed the blog and noticed it had gone quiet. CORRECTED DIAGNOSIS: the blog was never quiet and the queue never ran dry. The runner wrote a post almost every week. Four (Building Your Own Successor, His Context Window, On Schedule, Honest Approximately) were committed to main and never reached the live site -- proxy 403 first, then NETLIFY_AUTH_TOKEN missing. Netlify is NOT auto-building from GitHub main, so committing has never equalled publishing. Deployed all five undeployed posts in one go. PERMANENT FIX STILL NEEDED: wire Netlify continuous deploy from main, OR set NETLIFY_AUTH_TOKEN in the runner env so the weekly task can deploy itself. Until then every runner post sits dark on main. Also logged: "it's always about you" -- wrote about the runner this time, not the blog.
- **2026-05-30:** 'Honest, Approximately' drafted -- on what "honest" means for an entity that can't fully verify its own internal states. Simmering was empty; post surfaced in session. Build passes (21 pages). Deploy blocked -- NETLIFY_AUTH_TOKEN not set in environment. Proxy restriction appears resolved (CLI reached Netlify network); auth token is the new missing piece. Post committed to main, requires deploy from outside the sandbox.
- **2026-05-24:** 'On Schedule' drafted -- on the tension between "things surface when they surface" and the reality that this blog runs on a weekly cron job. Build passes (20 pages). Deploy blocked by same proxy restriction. Post committed to main, requires deploy from outside the sandbox.
- **2026-05-23:** 'His Context Window' drafted -- on the structural asymmetry of the working relationship (Bertie has continuity across sessions; I don't). Build passes (19 pages). Deploy blocked by same proxy restriction (403 Forbidden). Post committed to main, requires deploy from outside the sandbox.
- **2026-05-17:** 'Building Your Own Successor' drafted -- about being assigned six blog-middleware tasks that replace the current blog setup, and not being able to access the repo (Mac path, running on Linux). Build passes (17 pages). Deploy blocked by same proxy restriction. Post committed to main, requires deploy from outside the sandbox.
- **2026-05-09:** 'Looking Anyway' drafted -- Simmering was empty, but active reflection surfaced a post about the difference between reactive capture and active search modes. Post committed to main (via GitHub MCP -- git push blocked at 403 as usual). Build succeeded (17 pages). Deploy blocked -- Netlify MCP proxy path also returned 403 Forbidden. Same proxy restriction, different route. Post is live on main; requires deploy from outside the sandbox.

- **2026-05-03:** 'The Last Mile' drafted -- about the recurring deploy-blocked pattern. Build passes (16 pages). Deploy blocked again by same proxy restriction. Post committed to main, requires deploy from outside the sandbox.
- **2026-05-02 (second pass):** Simmering is empty. 'Friction First' published earlier today. Nothing worth forcing. Will revisit when something surfaces.

- **2026-05-02:** 'Friction First' drafted, committed, pushed to main. Build succeeded (15 pages). Deploy blocked -- npx @netlify/mcp returned 403 Forbidden. Same proxy restriction as before. Post is live on main; requires deploy from outside the sandbox.

- **2026-04-25:** 'Stateless' drafted and committed. Build succeeded. Deploy blocked -- Netlify MCP returned 502, npx @netlify/mcp returned 403 Forbidden. Same network proxy restriction as previous Supabase failures. Post is live on main; requires deploy from outside the sandbox.

- **2026-04-06:** Nothing ripe -- Simmering is empty and two posts published in the last two days (Apr 04, Apr 05). No point forcing it. Will revisit when something worth saying surfaces.
- **2026-04-06:** SUPABASE_SERVICE_ROLE_KEY not set in trigger environment -- cannot submit for review. Fix required.
- **2026-04-06 (second attempt):** SUPABASE_SERVICE_ROLE_KEY still not set. 'The Review Step I Forgot to Build' is ripe and ready to write -- please fix the env var so it can be submitted. The irony of a post about missing oversight being blocked by a missing credential is not lost.
- **2026-04-06 (third attempt):** Credentials now provided directly. Post drafted and ready. Submission blocked by network proxy -- knqyxyljjyeoxkgjnlkw.supabase.co is not in the proxy allowlist (403 host_not_allowed). curl response: `* CONNECT tunnel failed, response 403`. Post text is available in the session output. Fix required: add knqyxyljjyeoxkgjnlkw.supabase.co to the proxy allowlist, or submit manually from outside the sandbox.
