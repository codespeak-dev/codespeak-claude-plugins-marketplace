---
name: review-requirements
description: Open the review page for the requirements Spindle recorded in this project — what this session added, edited and removed, over the whole tracked set, in the browser. Use when the user invokes /review-requirements or asks to review, look over, or see the requirements recorded so far.
---

Show the user the review page for this project's requirements, then wait for
them to hand it back.

1. Run `spindle-client review`.
2. Reply with the link for the newest session it printed — the one marked
   `(live)` when there is one, since that is the session whose changes this turn
   produced. Say in one short sentence what changed, using the counts on that
   line.
3. Start waiting for their review, as a background command you do not wait on:
   `spindle-client review wait`. It ends when the reviewer presses "Act on my
   review" on the page, and what it prints then is your cue to follow the
   apply-review skill. If it ends saying nothing was handed over, the reviewer
   walked away: say nothing. One wait per session is enough — if you already
   have one running for this session, leave it.

If it reports that no review server is running, say that the Spindle daemon is
not up and that `spindle-client daemon start` brings it back. If it reports that
nothing is tracked, say the project has no requirements yet.

Concurrent sessions each get their own line. When more than one is listed and
none is marked `(live)`, give the user the "All projects" link instead of
guessing which session is theirs.

Keep the reply to the link and one sentence. Do not describe the server, the
port, the wait, or how the page was produced.
