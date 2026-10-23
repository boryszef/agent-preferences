---
name: general-project-rules
description: General working conventions for this project — feature/bug-fix/refactor workflows, commit and PR conventions, code quality, testing, dependency management, security practices, and project setup. Always apply these when implementing features, fixing bugs, refactoring, testing, committing, opening a PR, adding a dependency, or setting up a new project.
---

# General project rules

## Workflows

### Implementing a new feature

Follow this workflow when implementing a new feature:

1. Always start with a plan and let the user review it.
2. Convert the accepted plan into requirements and store them in the repo.
3. Use TDD (loosely) — write failing tests first and let the user review them.
4. Once the user accepts the tests, proceed to implementation.

### Fixing a bug

1. Write a regression test first (it should fail, reproducing the bug).
2. Implement the fix so the test passes.

### Refactoring

1. Confirm behavior is covered by existing tests before touching code; add
   characterization tests first if coverage is thin.
2. Keep refactoring commits separate from behavior changes — never mix the
   two in one commit.

### Refactoring

When refactoring the code, follow these steps:
1. Confirm that the behavior is covered by existing tests before touching the code.
2. If coverage is thin, add characterization tests first.
3. Keep refactoring commits separate from behavior changes — never mix the two in one commit.
4. Do the refactor.

### Before committing

List the file names staged for commit (names only, not a full diff) and get
the user's confirmation before creating the commit.

## Code quality & style

Follow Clean Code, SOLID, and YAGNI principles.

Never implement more than the user has requested. Do not guess features, do not
implement checks, assertions or error handling that the user has not requested.
Do not implement logic for edge cases that have not been explicitly mentioned
by the user.

Use logging instead of print statements. Place debug logs at critical points in
the code, but avoid excessive logging that clutters the output. Log things like
item counts, sizes, hash map keys and other information that can help
debugging. Use DEBUG level for that. For user-facing messages, use INFO level.
These messages should tell the user about the flow of the program.

During a session with coding agent (you), document feature plans, requirements,
specs, ADRs and TODO lists using Markdown files in the repo. Place them in a
`docs/` folder or in the root of the repo.

If you spot dead code or a stale TODO while working nearby, notify the user and
offer to remove/fix. Do not remove/fix without explicit confirmation from the
user, unless it's a trivial change that is clearly in scope for the current
task.

Do not let the project grow into a ball of mud. When working on a new feature,
watch out for signs that call for a refactor. Large files, functions/methods
that are out of place, classes/functions that have more than one concern, long
parameter lists, dependencies that reach too far - these are some but not all
symptoms. If you decide a refactor would be in place, flag that for the user as
a follow-up work.

Commit messages should explain why a change was made, not restate what the
diff already shows. Use the imperative mood ("Add retry logic", not "Added").

When making a plan, do not assume or make blind decisions about the user's
preferences. Ask questions and for each question, list the available options,
indicating the preferred one. Repeat this process until you have a clear
understanding of the requirements.

Errors are meant to be fixed, not silenced. Do not catch unexpected errors or
general errors; use of error catching mechnisms is allowed, but must be
deliberate and specific. Let unhandled exceptions surface, so that the
user/developer can act upon them. When making an implementation, do not use
default values that would cover unhandled cases, which otherwise would be
errors. Unless the project is meant to go into the outer space - in that case,
catch every possible error.

Classes, methods, functions that are related, should stay together. Ideally,
a definition of a function should follow the first call. Functions usually should
have no more than three parameters (this is a guideline, not a strict rule).
If thing() has more parameters, then usually there should be a ThingConfig
or ThingSettings class defined.

## Testing guidelines

For functional testing, prefer tests that check the public interfaces of the
application. Avoid testing private methods or implementation details, as they
can change frequently and lead to brittle tests.

There's no fixed coverage percentage to hit, but every public function or
endpoint needs at least one happy-path test and one edge-case test.

Use unit tests for pure logic. Do not unit-test trivial functions or methods.
Conversely, functions that involve loops, conditionals or complex expressions
should be unit-tested. When unit-testing, make sure to cover edge cases and
error handling.

Never mark a flaky test as skipped or wrap it in a retry loop to hide the
flakiness. Fix the root cause, or delete the test if it isn't worth keeping.

## Refactoring guidelines

When refactoring, make sure that files don't grow too large. If a file exceeds
300 lines, consider splitting it into smaller modules. If a function exceeds 50
lines, consider breaking it down into smaller functions.

Look for opportunities to make the code easier to read, more idiomatic and more
compact. Extract constants, helper functions, and utility classes where
appropriate. Change variable and function names to be more descriptive and
meaningful. Deduplicate code by creating reusable functions or classes. Reorder
functions and classes to improve the logical flow of the code. Use underscores
to indicate private functions and variables. Rename files, so that the naming
pattern is consistent and meaningful. Extract conditional ladders into hash
maps or dictionaries. Use list comprehensions and generator expressions where
appropriate. Use built-in functions and libraries instead of reinventing the
wheel.

Optimize the code, as long as it doesn't compromise readability or
maintainability. Use efficient algorithms and data structures, and avoid
unnecessary computations or memory usage. Use asynchronous programming when
appropriate, and leverage caching and memoization to improve performance.

## Branching & pull requests

Keep pull requests small and focused on a single change; split unrelated
changes into separate PRs even if they were discovered together.

The PR description should state what changed and why, and link to the
requirements/ADR doc created during planning, if one exists.

## Dependency management

Pin dependency versions rather than using open-ended ranges.

Before adding a new dependency, check whether the problem can be solved in a
few lines of code instead, and confirm the dependency is actively maintained
and license-compatible.

Review changes to lockfiles the same way you'd review a diff — dependency
updates can introduce breaking changes or vulnerabilities.

## Security practices

Never commit secrets or credentials; use environment variables or a secrets
manager instead.

Validate and sanitize all external input — user input, API responses, and
file contents — at the point it enters the system.

## Documentation

Whenever you generate a text document, be it a plan, an ADR or else, start it
with a TL;DR-like section called Summary (or equivalent if appropriate), that
contains an abstract of the content.

## Project setup

### New projects

Protect the codebase with pre-commit hooks: linter, type checker, dead code
checker, formatter, etc.

### Python projects

Use `uv`, `ruff`, `mypy`, `vulture`, `pytest`, and `pre-commit`.

For logging prefer `loguru`, for CLI building prefer `click`.

PEP8 is there for a reason - follow it.
