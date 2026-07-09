---
name: review-requirements
description: Audit the requirements Spindle tracks for this project and fix the ones that violate the extraction guidelines. Use when asked to review, clean up, or fix tracked requirements.
---

Audit the requirements Spindle already tracks for this project and fix the ones
that violate the project's extraction guidelines.

## Steps

1. Call the `load_requirements_review` tool. It returns the project's selected
   extraction guidelines — the requirement kinds, the critical rules, and the
   extraction guidance — followed by every requirement currently tracked, each
   with its id.
2. Review each tracked requirement against those guidelines. Flag a requirement
   when it:
   - is filed under the wrong kind;
   - is not atomic — it bundles several distinct requirements;
   - duplicates an idea another requirement already covers;
   - is vague, carries dangling references, or otherwise fails standalone
     clarity;
   - should not have been extracted at all (see Critical Rule #2 on
     over-extraction).
3. Submit every fix in a single `apply_requirements` call, passing
   `review: true` so the quote check is skipped (at review time there is no
   originating user message to quote against):
   - `removed`: the ids of requirements you are deleting or replacing.
   - `added`: the corrected requirements, each an object with a `kind` and a
     `statement`. To fix a requirement in place, put its id in `removed` and the
     corrected version in `added`. Leave well-formed requirements untouched.
4. Summarise what you changed and why.
