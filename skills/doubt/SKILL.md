---
name: doubt
description: Adversarial verification of a claim before acting on it. Use before claiming a change is safe, correct, or the root cause when no single command can show it, or when the user asks "are you sure", "double-check", or "verify".
---

# Doubt

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

A convincing writeup reads the same whether or not it's true. Check the claim with something that didn't write it.

## When

- **A run settles it** ("tests pass", "the bug no longer reproduces"): run the claim's own check and paste the output. That is enough; no cycle needed.
- **A run can't settle it** ("cannot double-charge", "no regressions elsewhere", "this is the cause", "matches the spec"): run the cycle below.
- **The user asks** to verify or double-check: run the cycle.

## Cycle

1. **CLAIM.** Write the claim in one sentence. "The retry change cannot double-charge a customer."
2. **EXTRACT.** Collect the artifact (diff, file, output) and the contract (spec, ticket, user's words). Nothing else.
3. **DOUBT.** Spawn a fresh `Explore` sub-agent (no edit tools). Give it the **artifact and contract only. Do not pass the claim or your reasoning.** Instruction:

   > Find where this artifact fails to meet this contract. Do not validate. Do not summarize. Report concrete failures with evidence (file:line, a command and its output, or a quoted contract line). If you find none, say what you checked.

4. **RECONCILE.** Classify each finding, in this order:
   1. **Contract misread** - you or the reviewer misunderstood the contract. Resolve with the user if it's yours.
   2. **Actionable** - a real failure. Fix it, then run another cycle.
   3. **Trade-off** - real but deliberate. State it in your reply.
   4. **Noise** - wrong or irrelevant, with a one-line reason.
5. **End the loop** when a cycle produces no actionable finding, or after **3 cycles**. At the cap, don't lift the bound; report what's still open to the user.

## Prove, don't argue

Where you can, turn the claim into something that runs: a test, a script calling the real code, a reproduction in the app. A run outranks any amount of reasoning. Use the proof ladder: said so < cited file:line < walked the failure path < ran it < reproduced in the real app. Report how far up the ladder the claim got.

## Verdict

End with one of:

- **VERIFIED** - checked by a run or by the doubt cycle, evidence attached.
- **NOT VERIFIED** - a check failed; say which.
- **INCONCLUSIVE** - the check couldn't be completed; say what's missing. Inconclusive is not a pass, and a gap in coverage is not a pass.

## Red flags

- **Doubt theater**: two or more cycles where no finding was ever classified actionable, yet you keep cycling. Either the reviewer is too weak (sharpen the contract) or the claim was already proven; stop.
- Passing the claim to the reviewer "for context". That turns doubt into agreement.
- Classifying every finding as noise.
- Saying "verified" with no command output in the message.
