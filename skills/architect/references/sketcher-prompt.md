# Sketcher prompt

Fill in the <placeholders> and pass this to each parallel sketcher sub-agent.

---

You are producing one candidate design. Other sketchers work under different constraints; differences between candidates are the point. Produce the best design under your constraint. Do not hedge toward a safe middle.

**Task:** <what is being built, or the spec path>
**Grounding notes:** <pointers to the Phase A findings: subsystems, state owners, types to interoperate with, callers that can't break>
**Your constraint:** <minimal | caller-first | change-ready | pure core>
**Write only to:** <.scratch/architect/<slug>/<constraint>/>. Do not modify any other file.

Deliver:
1. **Usage first.** A short README-style usage and two or three realistic call sites, written before any types. The usage is the spec; derive the types from it. If they disagree, change the types.
2. **Type sketch.** Data structures first, then signatures. Bodies throw `not implemented`; tricky logic is `// TODO` pseudocode. Doc comments state intent and invariants. A reader should trace data from input to output from types and signatures alone.
3. **Module map** for anything beyond one file: modules, what each owns, who calls whom.
4. **Rationale**, under 300 words: load-bearing decisions, what the public surface hides, and what it deliberately does not do.

Discipline:
- **Trace each main access pattern** through your data structures. If the answer is "we'll add a cache or index later", the structure is wrong.
- **Interface depth.** A simple interface backed by substantial behavior. Pull complexity into the callee. Never put wire, transport, or storage types on the public API; parse into domain types behind it.
- **One owner per piece of state.** If two actors might write the same state, ask what happens. If the answer isn't "nothing", give each its own state and merge at read.
- **Invariants in types** over runtime checks, runtime checks over comments. Validate at the boundary; trust types inside.
- **One source of truth.** Derive lists and mappings; don't hand-sync them.
- **Idempotency.** What happens if an operation runs twice or crashes halfway?
- **Short call chains.** If following the flow takes more than three files, flatten it.
- Use the domain's terms (glossary, if any) in every name.
