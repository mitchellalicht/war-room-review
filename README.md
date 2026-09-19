# war-room-review

A Claude skill that runs a simulated cross-functional design review. Five seats—product management, engineering, content design, product design, and the user—each critique the same artifact from their own rubric. Then the skill surfaces where they disagree.

The value isn't five opinions. It's the collisions between them.

## Install

**Claude Code**

```bash
git clone https://github.com/mitchellalicht/war-room-review.git ~/.claude/skills/war-room-review
```

**Claude.ai or the Claude desktop app**

Download this repo as a ZIP, then go to **Settings → Capabilities → Skills → Upload skill** and select the ZIP.

Then share a flow, screen, spec, or piece of copy and ask for a review. Phrases like "poke holes in this", "what would eng say", or "what am I missing" all trigger it.

## Modes

| Mode | When | What you get |
| --- | --- | --- |
| **Full review** | Default | All five seats, tensions, top three fixes |
| **Scoped review** | You name seats ("just eng and content") | Only those seats, plus the tensions between them |
| **Re-review** | You come back with a revised artifact | Each prior tension marked resolved, moved, untouched, or disputed |

## The five seats

| Seat | Accountable for | Signature questions |
| --- | --- | --- |
| **Product management** | Whether it's worth building and moves a number | Which problem does this solve? What's the success metric? What's the smallest version that tests the hypothesis? |
| **Engineering** | Whether it can be built, at what cost, and how it breaks | Where are the missing states? What's the data actually like? What's the maintenance tail? |
| **Content design** | Whether the words let someone understand what's happening and what to do next | Does the language match the user's model? What does the button promise? Is copy patching a structural problem? |
| **Product design** | Whether it works as an interface and belongs to the same product | What's the one thing on this screen? Is this the system's pattern or a new one? |
| **The user** | Whether a real person can finish the task, including with assistive tech | Do I know what to do next? What do I fear here? Where would I give up? |

## How it keeps the seats honest

- **Independent drafting.** Each seat writes from its own rubric before any seat reads another's notes. Optional subagents for high-stakes reviews.
- **Calibrated severity.** Blocking, should fix, or consider, with a definition for each and a check against inflation.
- **What's working.** Every review names what to protect, so authors don't fix the parts that were right.
- **Stated assumptions.** Anything guessed to keep moving goes in an Assumptions section.
- **Three kinds of tension.** Answerable (check the data), a real tradeoff (name the decision-maker), or a false conflict (one small change satisfies both).

## Output

```
Context · Assumptions · What's working
Product management · Engineering · Content design · Product design · The user
Where they disagree
If you fix three things
Open questions
```

A one-page shareable version is available on request.

## What good disagreement looks like

> **Eng wants the error state simplified; content design wants the specific message.** Eng's point is that six error variants means six strings to maintain and translate for an edge case hit by under 1% of users. Content's point is that "Something went wrong" is the reason those users file tickets. Underlying question: is the support cost of the generic message higher than the maintenance cost of the specific ones? Someone should check the ticket volume—this is answerable, not a matter of taste.

## Repo layout

```
war-room-review/
├── SKILL.md                     # the skill: modes, process, output format
└── references/
    ├── product-management.md
    ├── engineering.md
    ├── content-design.md
    ├── product-design.md
    └── user.md
```

## License

MIT
