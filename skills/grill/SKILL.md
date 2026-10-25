---
name: grill
description: Interview the user until a plan, feature, or decision is fully understood, in rounds of recommended-answer questions. Use when the user says "grill me", wants to stress-test an idea, or starts a feature whose goals, users, or constraints are still unclear.
disable-model-invocation: true
---

# Grill

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

Reach a shared understanding before anything is built. The output is a confirmed restatement, not code.

## 1. Open with a hypothesis

Before the first question, read what you can (repo, docs, linked issues) and state:

```
HYPOTHESIS: <what I think you want, in one or two sentences>
CONFIDENCE: <0-100>%  <if below 70: the single biggest reason>
```

This gives the user something to correct instead of something to invent.

## 2. Work the design tree in rounds

Every decision branches into the decisions that depend on it. The **frontier** is every open decision whose prerequisites are settled. Ask the whole frontier in one round, then wait.

```
**Q1 - <title>**: <question, with options if useful>
-> Recommended: <your answer, and the one-line reason>

**Q2 - <title>**: ...
```

- Word each question so "yes" accepts your recommendation.
- A question that depends on another open question this round belongs to a later round.
- **Facts are your job.** If a question needs a fact from the code or environment, dispatch a sub-agent to find it. Don't block the rest of the round on it; only the questions downstream of that lookup wait.
- **Decisions are the user's.** Put each one to them.

If a decision stays stuck after a round because nobody can judge the options on paper (a layout, a state model's edge cases, whether something is fast enough), mark it Open and suggest `/agent-skills:prototype` for it after grilling.

Ask at least one question aimed at the gap between what the user *should* want and what they *actually* want: "If you didn't have to justify this to anyone, what would you build?"

## 3. Sharpen the success test

Before closing, get answers to these (look them up first where you can):

- What would a top practitioner in this area add that we haven't mentioned?
- What is the rawest material available (real data, real logs, real users)?
- **What, other than you or me, would show the result is right?** A test, a metric, a user action. This becomes the spec's definition of done.

## 4. Restate and confirm

Stop when the frontier is empty, or when you can predict the user's answer to the next three questions. Then restate:

```
Outcome:      <what changes in the world>
User:         <who benefits>
Why now:      <trigger>
Success:      <the external check from step 3>
Constraints:  <hard limits>
Out of scope: <explicit non-goals; never empty>
Open:         <anything deliberately left undecided>
```

STOP. Wait for an explicit yes. A hedged answer means ask once more, plainly. Then suggest `/agent-skills:spec`.

## Red flags

- Asking the user something `grep` could answer.
- One question per message when five are independent (slow), or five questions where three depend on the first (wasteful).
- A restatement with an empty "Out of scope".
- Starting implementation in the same turn as the restatement.

## Rationalizations

| Excuse | Reality |
|---|---|
| "The request is clear enough." | Then the hypothesis will hit 90%+ confidence and grilling takes one round. Run it. |
| "I'll ask as questions come up during coding." | Mid-build questions cost rework. Ask now. |
| "The user seems busy; I'll assume." | Write the assumption as a recommended answer. They can say yes in one word. |

## Variant

`/agent-skills:grill-with-docs` runs this same loop and also records glossary terms and ADRs as they settle.

## Not for

Non-interactive or AFK runs. There, write the hypothesis and assumptions into the spec's "Open" section instead.
