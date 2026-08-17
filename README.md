# agents-default

A small set of default guidance files for LLM coding agents. Copy them into a repository (or use them as-is) so that coding agents follow consistent engineering and validation practices.

- `AGENTS.md` — **how agents should engineer changes**
- `VALIDATION.md` — **how agents should prove those changes are correct**

## AGENTS.md

Behavioural and engineering guidelines designed to reduce common LLM coding mistakes. It tells an agent how to think, design and edit code:

- **Think before coding** — inspect the codebase, surface assumptions and trade-offs, and stop when a requirement is materially unclear.
- **Simplicity first** — write the minimum code that solves the problem; no speculative features, abstractions or configuration.
- **Surgical changes** — touch only what the request requires; match existing style and conventions; leave unrelated issues unfixed but mentioned.
- **Outcome-driven execution** — translate the request into a clear behavioural outcome and, for multi-step work, track it in a `PLAN.md`.
- **Preserve existing behaviour** — treat public APIs, data formats and user-visible behaviour as contracts not to be disturbed.
- **Architecture** — high cohesion, loose coupling, clear module boundaries, and consistent use of existing abstractions.
- **Context-friendly modularity** — keep each file focused on one clear purpose; split only at natural responsibility boundaries, not to shrink file length.
- **Design for observability** — build systems so failures and state can be inspected by humans *and* coding agents.
- **Validate before completion** — completion is governed by `VALIDATION.md`, not by code plausibility.

In short: `AGENTS.md` keeps agent-generated diffs focused, simple, conventional, and safe for humans and agents to inspect.

## VALIDATION.md

Validation rules that define how an implemented change is proved correct and how failures feed back into the coding loop. Core rule: **implementation is not evidence.** A change is complete only when applicable validation demonstrates the requested outcome without unacceptable regressions.

It covers:

- **A validation feedback loop** — define the expected outcome, run the narrowest useful check, capture failure evidence, feed it back into coding, fix the cause, and re-run.
- **A validation toolbox** — build/runtime, observability, unit/integration/E2E tests, linting, type checking, complexity, duplication, security, architectural conformance, and acceptance criteria.
- **Validation order** — fast-to-broad: cheap focused checks first, expensive gates last.
- **Anti-gaming rules** — never weaken, skip or suppress legitimate checks to obtain a pass; never claim completion when required validation was not run.
- **Handling of pre-existing failures and blocked checks** — record precise evidence or precise blockers rather than vague caveats.
- **Completion evidence and a definition of done** — a concise summary of what passed, what was not run and why, and any blockers.

In short: `VALIDATION.md` turns "the code looks right" into "the requested outcome is demonstrated by passing evidence."

## Using these files

1. Copy `AGENTS.md` and `VALIDATION.md` into the root of a repository (most agent harnesses, e.g. Pi and Claude Code, pick up `AGENTS.md` automatically).
2. Optionally append project-specific instructions to `AGENTS.md`, or reference additional docs (e.g. `ARCHITECTURE.md`, `PLAN.md`) as the files already do.
3. Both files are written to be repository-agnostic: they direct agents to follow *existing* project conventions, tooling and commands rather than imposing a specific stack.

## License

Apache License 2.0 — see [LICENSE](LICENSE).
