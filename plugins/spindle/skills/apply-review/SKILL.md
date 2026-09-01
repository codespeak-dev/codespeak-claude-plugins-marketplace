---
name: apply-review
description: Apply the annotations a reviewer left on this project's requirements — rewordings, removals and remarks they typed into the review page — and do the work those remarks imply. Use when a review is handed over, when the user invokes /apply-review, or when they ask to apply, act on or process their review.
---

The reviewer annotates requirements in a browser page. This turns what they
wrote there into requirement changes, and then does the work their remarks
imply.

## 1. Read what they left

Call `load_review_feedback`. It returns every annotation they have not had
applied yet, each joined to the requirement as it stands now. If nothing is
waiting, say so and stop.

When a handoff named a session, pass it as `session`: another review of the same
project may be half-written, and finishing someone else's thinking for them is
worse than doing nothing.

Two fields say the requirement moved under them: `drifted` means it changed
after they annotated it, so `annotatedStatement` is what they were looking at;
`requirement: null` means it is gone altogether.

## 2. Turn each annotation into a decision

`intent` is what the page saw them do:

- **`rewrite`** — they typed the wording they want. Modify that requirement in
  place: put their sentence into the slot form, keep the same `kind` unless they
  say otherwise, and pass the requirement's `quote` through unchanged.
- **`delete`** — they marked the whole requirement unwanted. Delete it. That is
  not a judgement call, so do not soften it into a rewording.
- **`comment`** — read it and decide which of three things it is: a change to
  make to the requirement, a claim that the code does not match the requirement,
  or a question. Say which reading you took.

On a `drifted` entry, decide against what they saw, not what is tracked now, and
say that it moved. Do not guess: an annotation you cannot act on is a line in
your reply, not an invented operation.

## 3. Write the requirement changes first

Submit them in **one** `edit_requirements` call with:

- **`review: true`** — their words were typed into a browser page, so they
  appear in no message of theirs and the ordinary quote check would reject every
  one of them.
- **`review_annotations`** — the ids of every annotation you acted on, including
  ones you answered rather than operated on, and ones already satisfied (a
  `delete` whose requirement is already gone). That marks them applied, so they
  stop coming back, and it keeps their own edits from being shown to them as one
  more thing to review.

Call it even when you changed no requirement: with `review: true` and only
`review_annotations` set, it marks them and writes nothing.

## 4. Then do the work they implied

A remark that says the software does not do what the requirement says is work,
not a wording problem. Do it in this turn, after the requirement writes have
landed. Check the premise first — the note was written minutes ago in a browser
and this session may have moved past it, so read the code before changing it.

## 5. Report

- One line per requirement you changed: what it now says, or that it is gone.
- One line per thing you did not act on: a note you read as a question, an
  annotation whose requirement had moved, a remark about the page rather than
  the requirements. Those are their next decisions, so they belong in the reply
  rather than in a silent judgement.
- What you changed in the code, if anything.

Leave the rest out: no count of what you left alone, no recap of these steps.
