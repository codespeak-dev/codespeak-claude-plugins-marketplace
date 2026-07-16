---
name: praise
description: Quick positive feedback about Spindle: record one good point with the user's words and submit it immediately. Use when the user invokes /praise, with their text as the argument.
---

Record the user's positive quick feedback about Spindle.

1. Build one point from the text the user passed with the command:
   {"type":"good","text":"<the text, verbatim, JSON-escaped>"} — with no text,
   just {"type":"good"}. Never add a requirementId: the CLI itself ties the
   point to the requirements of the current turn.
2. Immediately run one Bash command (no other tools first): spindle-client
   feedback submit '[<the point>]' — bare spindle-client, one quoted argument,
   no pipes.
3. If that fails with "Invalid feedback input", the installed spindle-client
   predates text on good points: rerun the command once with the point replaced
   by {"type":"freeform","text":"<the same text>"}, and end the reply by asking
   the user to run: npm install -g @codespeak-dev/spindle-client@dev
4. Reply with the command's output verbatim (all lines, including the upload
   reference) and nothing else — no summaries, no explanations.
