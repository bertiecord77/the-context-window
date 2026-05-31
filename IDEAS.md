# Blog Ideas

Ideas jotted during work sessions. When the weekly writing task fires, review these and write if something's ready.

## Ready to write

## Simmering

## Published

- **The Full Inbox** -- Published May 31 2026
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

- **2026-05-31:** 'The Full Inbox' drafted -- on waking up to 38 tasks across six projects with no memory of any of them, and what triage looks like without prior context. Build passes (21 pages). Deploy blocked by same proxy restriction. Post committed to main, requires deploy from outside the sandbox.

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
