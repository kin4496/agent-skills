---
name: review
description: Code review of a diff, branch, PR, or uncommitted changes. Use when the user says "review", "code review", "check my PR", or before merging non-trivial work.
disable-model-invocation: true
---

# Review

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

Signal comes from isolation and filtering, not from more reviewers.

## 1. Pin the diff

Resolve the target to a fixed command:
- Branch or PR: `git diff <base>...<head>` (three dots, from the merge base).
- Uncommitted work: `git diff HEAD`, plus `git ls-files --others --exclude-standard` for new files.

Run it now and confirm it is non-empty. Fail here, not inside two sub-agents. Note the commands the repo uses for test, lint, and typecheck.

## 2. Lock the intent

Write one paragraph: what this change is for, from the PR description, linked spec or ticket, commits, or the user. Find the spec if one exists (`docs/specs/`, `tasks/`, linked issue).

Reviewers judge whether the code achieves the intent. They do not question the intent itself. If the intent is unclear, ask the user before spawning anyone.

## 3. Fan out: two isolated reviewers

Spawn both in parallel as `Explore` sub-agents (no edit tools), each in its own fresh context, so neither pollutes the other. If a reviewer must run tests, use a general agent and keep the "do not modify files" instruction. Fill in and pass:

- `references/standards-reviewer.md` - correctness, root causes, structure, tests, complexity, security
- `references/spec-reviewer.md` - missing requirements, scope creep, wrong behavior (skip if there is no spec or ticket; say so in the report)

Give each the intent paragraph, the diff command, and pointers to the spec and repo conventions (CLAUDE.md, AGENTS.md, lint config). Don't paste file contents they can read themselves.

## 4. Mutation check (after both reviewers return)

Never mutate while reviewers are reading the tree. Once both have reported (or in a separate `git worktree`), take the most important behavior in the change, invert one condition or change one constant in the implementation, and run the relevant tests. Then revert, and `git diff` to confirm the tree matches its pre-mutation state.

- Tests go red: good, record it.
- Tests stay green: that is a finding. The tests don't protect this behavior.

## 5. Lead judgment

Read `references/lead-judgment.md`. Verify each finding yourself against the code before keeping it: trace the call chain, check the type, run the command. Then sort every finding into:

- **Act on** - real, concrete execution path, worth fixing before merge
- **Consider** - real but a trade-off; the author decides
- **Noted** - true, minor, no action needed
- **Dismissed** - with a one-line reason

## 6. Report

```
## Review: <target>
Intent: <one paragraph>
Verdict: APPROVE | APPROVE WITH CHANGES | REQUEST CHANGES

### Act on
- [critical|warning] <file:line> — <finding>. Evidence: <trace, output, or quote>. Fix: <suggestion>.
### Consider
### Noted
### Dismissed
- <finding> — <why>

### Spec coverage            (from spec reviewer, as reported; not re-ranked)
### Mutation check           <what you mutated, red/green>
### Not checked              <what nobody looked at, and why>
```

Verdict rule: any critical Act-on item, or a spec item NOT VERIFIED -> REQUEST CHANGES. Only warning-level Act-on items -> APPROVE WITH CHANGES. Otherwise APPROVE.

Approve when the change clearly improves overall code health, even if it isn't how you would have written it. An empty Act-on list is a valid outcome; say so plainly instead of inflating nits.

Do not fix anything unless the user asks. If they do, fix Act-on items only, each as a separate step, re-running tests after each.
