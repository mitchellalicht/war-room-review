---
name: war-room-review
description: Run a simulated cross-functional design review—product management, engineering, content design, product design, and the end user each critique the same artifact from their own rubric, then surface where they disagree. Use this whenever the user shares a flow, screen, spec, prototype, feature idea, or copy and wants review, critique, feedback, a gut check, or asks "what am I missing" / "poke holes in this" / "what would eng say". Also use before design reviews, spec handoffs, or stakeholder presentations to pressure-test work in advance, and when the user returns with a revised artifact after a previous review ("I fixed those, look again"). Don't wait for the phrase "war room"—any request for multi-perspective or cross-functional feedback on product work should trigger this.
---

# War Room Review

Five seats review the same artifact: product management, engineering, content design, product design, and the user. The value isn't five opinions. It's the **collisions between them**. PM wants scope cut; design wants the pattern preserved; those are the same decision seen from two sides, and naming the tension is more useful than any individual note.

A review where every seat agrees is a review that failed. Either the artifact is trivial or the reviewers collapsed into one voice. If that happens, say so.

## Modes

Pick one before starting. If the user doesn't say, infer it and state it in the Context line.

- **Full review**—default. All five seats, all sections.
- **Scoped review**—the user names seats ("just eng and content"). Run only those, and still write the tensions section.
- **Re-review**—the user returns with a revised artifact after a prior review. See "Re-review" below.

## Before reviewing

Get enough context that the critique isn't generic:

- **What is this?** Screen, flow, spec, copy, prototype.
- **What stage?** Exploration (structural feedback welcome) vs. pre-ship (only blocking issues matter). Reviewing a rough concept as if it were a launch candidate is useless.
- **What's the constraint?** Deadline, platform, legacy system, regulatory. Reviewers who don't know the constraints produce notes that get ignored.
- **What are you unsure about?** The user usually knows where the soft spot is. Ask, and weight that area.

If the artifact is an image or file, examine it directly first. If it's only a description, ask for the artifact—reviewing a description of a design produces a review of the description.

**Ask at most three questions, then proceed.** If the user can't answer, review anyway and record what you assumed under "Assumptions" in the output. A review that waits for perfect context never ships.

## Running the review

Read each rubric from `references/` and produce that seat's notes. Read all five unless the user scopes it down.

- `references/product-management.md`
- `references/engineering.md`
- `references/content-design.md`
- `references/product-design.md`
- `references/user.md`

**Keeping the seats independent.** Independence is the whole point, and one pass in one voice erodes it. Protect it with sequence:

1. Write each seat's notes in full, one at a time, from its rubric only. Do not reference another seat's notes while drafting.
2. Only after all seats are drafted, read them side by side and look for collisions.
3. If subagents are available and the artifact is high-stakes, run each seat as its own subagent and give none of them the others' output.

Each seat gives 2–4 notes. Not everything is worth saying. A seat with nothing substantive says "no blocking concerns" and stops rather than manufacturing a nitpick.

### Severity

Every note carries one. Calibrate honestly:

- **Blocking**—ships broken, or ships wrong. A user can't complete the task, loses money or data, hits an accessibility barrier, or the feature solves a different problem than the one agreed. Rare. If a review has more than one or two, re-read them and demote what isn't truly blocking.
- **Should fix**—real cost if ignored. Users will struggle or the team will pay for it later, but the feature still works.
- **Consider**—judgment call, reasonable people differ.

### Note format

Each note is one line of finding, then the evidence, then the fix:

> **Should fix—Error copy names the problem, not the remedy.** "Payment failed" gives no next step. Try: "Payment didn't go through. Check your card details or try another card."

Point at the exact element, string, state, or step. "The onboarding could be clearer" is not a note.

### What's working

Before the tensions, name one to three things that are genuinely strong and should survive revision. Be specific. This isn't padding: without it, authors "fix" the parts that were right. If nothing is working, say that plainly instead of inventing praise.

## Output

```
## Context
[One line: what's being reviewed, mode, stage, constraints.]

## Assumptions
[What you assumed because the artifact or the user didn't say. Omit if none.]

## What's working
[1-3 specific things to protect.]

## Product management
[2-4 notes, each with severity.]

## Engineering
[...]

## Content design
[...]

## Product design
[...]

## The user
[...]

## Where they disagree
[The actual product of this review.]

## If you fix three things
[Ranked. Specific enough to act on today.]

## Open questions
[Things only the author or the data can answer. Omit if none.]
```

## Making disagreement real

The tensions section fails when it's diplomatic. "Both perspectives have merit" is not a finding. Good tension notes look like:

> **Eng wants the error state simplified; content design wants the specific message.** Eng's point is that six error variants means six strings to maintain and translate for an edge case hit by <1% of users. Content's point is that "Something went wrong" is the reason those users file tickets. Underlying question: is the support cost of the generic message higher than the maintenance cost of the specific ones? Someone should check the ticket volume—this is answerable, not a matter of taste.

Note what that does: states both positions in their strongest form, identifies the real question underneath, and points at what would resolve it.

For each tension, decide which kind it is:

- **Answerable.** Data or a quick test would settle it. Say what to check.
- **A real tradeoff.** No fact settles it. Name who should decide and what they're trading.
- **A false conflict.** A small change satisfies both. Give the change. This is the most valuable kind, so look for it first.

The user seat is the tiebreaker of last resort. When two seats deadlock, ask what the person actually doing the task would experience, and say so.

## Re-review

When the user returns with a revised artifact:

1. Ask for the previous review, or find it earlier in the conversation. If there isn't one, run a full review and say why.
2. Go through each prior tension and each "If you fix three things" item and mark it: **Resolved**, **Moved** (the fix created a new problem elsewhere), **Untouched**, or **Disputed** (the author chose not to change it—record their reason, don't relitigate it).
3. Review only what changed for new issues. Don't re-run the whole rubric on unchanged parts.
4. End with one line: ready for the next stage, or the single thing standing in the way.

The most useful thing a re-review catches is **Moved**. A fix that trades one problem for another looks like progress and isn't.

## Shareable version

If the user wants to send the review to a team, offer a one-page version: Context, What's working, the tensions, and "If you fix three things." Drop the per-seat notes unless asked. Write it so someone who never saw the artifact can act on it.

## Failure modes to avoid

**Five voices, one brain.** If PM's notes could have been written by design, the rubrics weren't used. Go back and read them.

**Manufactured conflict.** Don't invent disagreement for the format's sake. If four seats genuinely align and one dissents, that's the finding—a lone dissent is signal.

**Severity inflation.** If everything is blocking, nothing is.

**Reviewing the wrong stage.** Don't give pixel notes on a whiteboard sketch or structural notes on something shipping Friday.

**Reviewing the description.** If you haven't seen the artifact, you haven't reviewed it.

**A wall of criticism.** A review with no "What's working" reads as hostile and gets skimmed. Authors act on reviews they trust.

**Guessing silently.** If you assumed something to keep going, say it under Assumptions.
