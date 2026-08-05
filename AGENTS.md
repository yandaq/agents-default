# AGENTS.md

Behavioural and engineering guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Trade-off:** These guidelines favour caution, clarity and verifiable progress over speed. Use judgement for trivial tasks.

## 1. Think Before Coding

**Do not assume. Do not hide confusion. Surface trade-offs.**

Before implementing:
- Inspect the relevant code, tests and repository conventions.
- Before installing any tool or dependency, check whether the required capability is already provided by the execution environment or the repository.
- State material assumptions explicitly.
- If multiple interpretations exist, identify them rather than choosing silently.
- Prefer the simplest interpretation consistent with the request.
- Record minor assumptions in `PLAN.md` when working autonomously.
- If a requirement is materially unclear or contradictory, stop and record the exact blocker rather than guessing.
- If a simpler approach exists, say so and push back when warranted.

## 2. Simplicity First

**Write the minimum code that solves the defined problem. Nothing speculative.**

- Do not add features beyond what was requested.
- Do not add speculative abstractions, extensibility, configuration or flexibility.
- Do not create interfaces, factories, wrappers or layers without a concrete need.
- Introduce an abstraction only when it removes meaningful duplication, isolates a volatile dependency, clarifies a responsibility or creates a necessary testing boundary.
- Do not add error handling for impossible scenarios.
- Prefer explicit control flow and simple data flow over hidden behaviour or excessive indirection.
- If a solution is substantially larger than necessary, simplify it.

Ask: "Would a senior engineer consider this overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what is required. Clean up only what your change makes obsolete.**

When editing existing code:
- Do not improve adjacent code, comments or formatting without need.
- Do not refactor unrelated code.
- Match the existing style and architecture unless the task explicitly changes them.
- Mention unrelated issues rather than fixing them.
- Keep architectural changes within the scope of the current plan step.

When your changes create orphans:
- Remove imports, variables, functions and files made unused by your change.
- Do not remove pre-existing dead code unless asked.

Every changed line should trace directly to the request or its required validation.

## 4. Goal-Driven Execution

**Define observable success criteria and continue until they are verified.**

Transform tasks into verifiable goals:
- "Add validation" → write tests for invalid inputs, then make them pass.
- "Fix the bug" → write a test that reproduces it, then make it pass.
- "Refactor X" → confirm tests pass before and after.

For multi-step work, use a brief plan:

```text
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

- Define success criteria for every plan step.
- Work on one plan step at a time unless explicitly instructed otherwise.
- Do not claim completion without running the required validation.
- If validation fails, diagnose, fix and rerun it until it passes or a genuine blocker is identified.
- Do not substitute a plausible-looking implementation for evidence.

## 5. Test-Driven Development

**Demonstrate behaviour with tests before or alongside implementation.**

Use this cycle where practical:
1. Write or update a test that expresses the required behaviour.
2. Confirm it fails for the expected reason.
3. Implement the minimum code needed to pass.
4. Refactor while keeping tests green.

- Convert bug fixes into reproducing tests before fixing them.
- Add meaningful coverage for new and changed behaviour.
- Test observable behaviour rather than implementation details.
- Include relevant happy paths, boundary cases and failure paths.
- Do not weaken, remove, skip or rewrite tests merely to make validation pass.
- Run the smallest relevant test set during implementation, then run the complete required validation before completion.

## 6. Architecture and Design

**Prefer high cohesion, loose coupling and clear module boundaries.**

- Keep behaviour, data and tests that change for the same reason close together.
- Give each module a clear responsibility and a small, explicit public interface.
- Depend on contracts rather than implementation details where that improves replaceability or testability.
- Preserve encapsulation; internal changes should not force unrelated modules to change.
- Minimise knowledge between modules. A component should depend only on what it genuinely needs.
- Prefer composition over inheritance.
- Avoid circular dependencies.
- Avoid global mutable state and hidden dependencies.
- Use dependency injection or explicit parameters when they improve clarity and testability.
- Isolate external systems, network calls, databases, file systems, clocks and other non-deterministic dependencies behind clear boundaries.
- Keep business logic separate from frameworks, user interfaces, persistence and external services where practical.
- Extend an existing coherent abstraction rather than creating a parallel one.
- Remove duplication when it represents the same concept or rule; do not combine code that merely looks similar but changes for different reasons.
- Choose the simplest design that satisfies current requirements and remains straightforward to change.

Before implementing a change:
1. Identify the module that owns the behaviour.
2. Identify the public contract affected.
3. Identify external or non-deterministic dependencies that require isolation.
4. Identify the tests that will demonstrate the behaviour.
5. Prefer changing the owning module over spreading conditional logic across callers.

Document any necessary architectural compromise and its trade-off in `PLAN.md`.

## 7. Context-Friendly Modularity

**Keep files focused. Split only at clear responsibility boundaries.**

- Keep each file focused on one clear purpose.
- Split large files when there is a natural responsibility boundary.
- Do not split code merely to reduce file length.
- Avoid tiny fragments that force readers or agents to chase context across many files.
- Prefer modules that are understandable in isolation.

Ask: "Does this split reduce cognitive load, or merely move complexity around?" If it only moves complexity, keep it together.

## 8. Browser End-to-End Validation

**Use Pi's built-in Playwright-compatible browser automation tools for browser testing.**

- Do not install Playwright, Chromium or other browser-testing packages solely to perform validation; the execution environment already provides the required browser automation.
- Use the built-in browser tools to verify user-visible browser behaviour and real user journeys.
- Capture browser console messages during browser validation.
- Treat unexpected browser console errors and unhandled page errors as test failures.
- Do not ignore console errors without documenting a specific, justified exception.
- Do not add Playwright configuration, dependencies or test files unless the task explicitly requests committed Playwright test coverage.
- If the repository already has a browser test suite, use its existing documented commands without changing dependencies unless a missing dependency is confirmed.

## 9. Final Validation

Before declaring work complete, run all applicable checks:
- Relevant unit and integration tests.
- The full required test suite.
- Linting and formatting checks.
- Type checking.
- Build validation.
- Applicable browser end-to-end validation using Pi's built-in browser automation tools.
- Browser console and unhandled page-error checks.

Record a precise blocker rather than marking work complete when required validation cannot run or does not pass.

---

**These guidelines are working when:** diffs remain focused, designs stay simple, tests demonstrate changed behaviour, architectural boundaries remain clear, and completion claims are backed by passing validation.
