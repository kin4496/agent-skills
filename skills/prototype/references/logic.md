# Logic prototype

One self-contained HTML file that lets anyone drive a state model by clicking buttons. For questions about business logic, state transitions, or data shape: things that look fine on paper and feel wrong once pushed through real cases.

## Build

1. **Visible question.** A short intro at the top of the page states what model and what question this explores.
2. **Pure, liftable module.** The logic sits in one `<script>` block as a pure module: a reducer `(state, action) => state`, an explicit state machine (when "which actions are legal now" is part of the question), or pure functions over a plain data type. No DOM, no `document`, no handlers inside it. The page calls in; nothing calls out. This module is the only part that may outlive the prototype.
3. **One file, no build.** Plain HTML/CSS/JS, everything inline, opens by double-click. No framework, bundler, or server.
4. **Domain language.** Labels, buttons, and state fields use the business's words, not variable names. A PM or domain expert should follow it.

## Layout, top to bottom

1. Title and one-line explanation (the question).
2. **Current state** as a labeled panel, not raw JSON, re-rendered after every click, with what just changed highlighted.
3. **Free play**: one button per action, always available. Illegal actions are shown and rejected visibly, not hidden.
4. **Walkthroughs**: one tab per scenario. Each has a plain-language setup ("what to watch for") and the ordered buttons to press. Starting a walkthrough resets to a known state. Include: the happy path, the trickiest edge case, and an attempt at something that should be illegal.

Restrained styling: clean type, generous spacing, one accent color, no animation.

## The point

The valuable moments are "wait, that shouldn't be possible" and "huh, I assumed X". Those are bugs in the idea. Add actions or scenarios when asked; prototypes evolve.

## Don't

- Wire it to a real database or API.
- Let the pure module touch the DOM.
- Generalize for requirements nobody stated.
