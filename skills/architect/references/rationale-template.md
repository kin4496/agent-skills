# Design: <title>

**Status:** proposed | approved | superseded
**Spec:** <path, if any>

## Problem
<One paragraph: what we're doing and what about the existing system or constraints makes the shape non-obvious. Name the constraints from grounding: types to interoperate with, callers that can't break, invariants crossing our boundary.>

## Usage (caller's view)
<Written first. The README or quickstart a consumer reads, plus two or three realistic call sites. The shape below is derived from this; if they disagree, fix the shape.>

## Shape
<Data structures first, then how data flows through the signatures. The load-bearing decisions; which invariants live in types; where validation happens; what the system deliberately does not do. What complexity the public surface hides and why it is no larger than needed. Cite a principle where one drove a choice; don't restate it.>

## Synthesis
<Which candidate became the base and why; what was grafted from the others; what was rejected and why.>

## Trade-offs accepted
- We accept <X> in exchange for <Y>.
<Include anything a future reader might mistake for an oversight, and any red flags deliberately kept.>

## Alternatives considered
- <A structurally different shape> — lost because <reason, judged on interface depth and what it exposes to callers>.
<At least one. If constraints forced the answer, say "the only viable shape because ...". Not flavors of the same shape.>

## Open questions and risks
- <Phrased as questions, so the user's answer resolves them.>

## First implementation step
<One sentence: the first thing to build against the sketch.>
