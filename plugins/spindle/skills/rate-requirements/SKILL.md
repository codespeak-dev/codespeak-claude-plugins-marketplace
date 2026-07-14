---
name: rate-requirements
description: Collect good/bad/free-form feedback on the requirements Spindle recorded — the last turn's additions by default, or every unrated requirement of the session. Use when asked to rate requirements or give feedback on requirement extraction.
---

Collect the user's feedback on requirements Spindle recorded.

1. Target set: the requirement lines the invoking auto-ask directive listed (id,
   kind, statement). Without such a directive, Bash:
   `spindle-client feedback list --scope last-diff` (default) or
   `--scope session-unrated` (user asked to sweep the session). Output: JSON
   array of requirements with ids.
2. Empty set: say so; still ask the overall question.
3. Call AskUserQuestion (max 4 questions/call): per requirement one question —
   question = its statement, header = its kind abbreviated to 12 chars or less
   (functional_requirements → "Functional", non_functional_requirements →
   "Quality", ui_requirements → "UI", test_requirements → "Tests", user_stories
   → "Story", alternatives_attempted → "Alternative", debugging → "Debugging",
   non_goals → "Non-goal", anything else → its capitalized first word), options
   exactly ["Good" with description "", "Bad — say what's wrong below" with
   description "PLEASE type the problem into Type something"] — plus one final
   question "Overall feedback?" (header "Overall", same two options). Never add
   Skip/None options.
4. Map answers: Good → {"type":"good","requirementId":"<id>"}; Bad — say what's
   wrong below → {"type":"bad","requirementId":"<id>"}; typed text →
   {"type":"freeform","text":"<text>","requirementId":"<id>"}; overall answer →
   no requirementId; skipped → nothing.
5. If ≥1 answer: run one Bash command: spindle-client feedback submit --session
   <id> '<JSON array>' — <id> is the value of the directive's "Session:" line
   (omit --session when there is none); bare spindle-client, one quoted
   argument, no pipes.
6. Reply with the command's output verbatim (all lines, including the upload
   reference) and nothing else — no summaries, no explanations.
