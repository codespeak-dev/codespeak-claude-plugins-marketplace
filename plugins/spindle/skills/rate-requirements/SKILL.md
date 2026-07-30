---
name: rate-requirements
description: Collect good/bad/free-form feedback on the requirements Spindle recorded — the last turn's additions by default, or every unrated requirement of the session. Use when asked to rate requirements or give feedback on requirement extraction, and only after the turn's implementation work is finished. Not for fixing the tracked set itself — that is audit-requirements; for one-shot feedback without the questionnaire, praise and fuck.
---

Collect the user's feedback on requirements Spindle recorded.

Run this AFTER the turn's implementation work is done — it is a closing step,
never a substitute for the work and never a reason to end the turn early. If
part of the user's request is still unimplemented when this flow ends, go back
and finish it.

1. Target set: Bash `spindle-client feedback list --scope last-diff` (default)
   or `--scope session-unrated` (user asked to sweep the session). Output: JSON
   array of requirements with ids.
2. Empty set: say so; still ask the final generic question.
3. Call AskUserQuestion (max 4 questions/call): per requirement one question —
   question = its statement, header = its kind abbreviated to 12 chars or less
   (functional_requirements → "Functional", non_functional_requirements →
   "Quality", ui_requirements → "UI", test_requirements → "Tests", user_stories
   → "Story", alternatives_attempted → "Alternative", debugging → "Debugging",
   non_goals → "Non-goal", anything else → its capitalized first word), options
   exactly ["Good" with description "", "Bad — say what's wrong below" with
   description "PLEASE type the problem into Type something"] — plus one final
   generic question "Anything about Spindle itself? A missed topic or a gripe."
   (header "General"), options exactly ["Nothing to add" with description
   "Requirement-specific notes go on the previous tabs", "Yes, I will type
   below:" with description ""]; any generic note is typed into the free-text
   ("Other") field. Never add Skip/None options to the per-requirement
   questions.
4. Map answers: Good → {"type":"good","requirementId":"<id>"}; Bad — say what's
   wrong below → {"type":"bad","requirementId":"<id>"}; text typed on a
   requirement → {"type":"freeform","text":"<text>","requirementId":"<id>"}; a
   note typed into the generic question's free-text field →
   {"type":"freeform","text":"<text>"} (no requirementId); "Nothing to add", a
   bare "Yes, I will type below:" with no typed text, or a skipped question →
   nothing.
5. If ≥1 answer: run one Bash command:
   `spindle-client feedback submit '<JSON array>'` — bare spindle-client, one
   quoted argument, no pipes.
6. Reply with the command's output verbatim (all lines, including the upload
   reference) and nothing else — no summaries, no explanations. Then resume any
   unfinished part of the user's request; only end the turn if the work is
   complete.
