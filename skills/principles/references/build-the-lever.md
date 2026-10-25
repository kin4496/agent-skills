# Build the lever

When the same change repeats, build the tool that makes it, not the change by hand.

## Pattern
1. Do one instance by hand, carefully. This is the reference.
2. Write the codemod, script, generator, or lint rule.
3. Run it on the reference's original and diff against your hand version. They must match.
4. Run it everywhere. Review the diff by sampling, plus every place the tool reported uncertainty.

## Thresholds
- More than ~5 identical edits: script it.
- More than ~500 lines of mechanical change: codemod it (AST-based, not regex, when syntax matters).
- A mistake you have now made twice: make a lint rule or type that names the fix in its error message.

## Applied means in the diff
If you claim this principle and there is no script, codemod, or rule in the change, you didn't apply it.
