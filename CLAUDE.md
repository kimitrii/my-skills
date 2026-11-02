# Working Conventions

# Code guidelines

## Language rules

- Avoid `try/catch`. Prefer `if/else` guards that throw specific, named errors.
- Never use the `unknown` or `any` types.

## Architecture

- Webhooks must not hold business rules. Keep them as thin entry points that validate
  and delegate; the rules live in the domain/service layer.
- Don't let external domains leak into the package. Map third-party payloads and models
  at the boundary — they pollute the internal domain models otherwise.
- Use transaction routing to manage database read speed: route read transactions to
  read-only replicas and write transactions to the write replica.

## Testing

- Write integration tests for classes, exercising real collaborators rather than
  over-mocking.
- For third-party services, tests need to be contract tests.
- Write performance tests.
- Write audit tests.

# Git workflow

## Commits

When refactoring, split it into two commits in this order:
1. commit the removals/deletions
2. commit the new and changed files

This keeps the diff readable instead of showing everything as a rewrite.

## Pull requests

If a PR you create is getting too big, split it into several smaller PRs
(changes + new lines = 300 lines maximum). Prefer a chain of reviewable PRs over one
large one. Generated files (lockfiles, snapshots, generated code) don't count toward
the limit.

# Repository Map

Before working on a task, check for these files and read the ones that exist:

- `codemap.md` at the repo root: architecture overview and links to each folder's
  `codemap.md`. For deep work on a folder, also read that folder's `codemap.md`.
- `CONTEXT.md` (or `CONTEXT-MAP.md`): the project's domain vocabulary. Use its terms.
- `docs/adr/`: recorded architecture decisions. Don't re-propose what an ADR rejected.

These are maps, not proof: confirm behavior in the code before relying on them.

# Skills

Use these skills in the situations below and follow their own steps.

Planning and architecture:
- Before implementing a non-trivial feature, fix, or refactor: `verification-planning`.
- To find refactoring opportunities or improve architecture: `improve-codebase-architecture`.
- For large, high-risk, multi-phase work (not routine multi-file changes): `deepwork`.
- For risky, experimental, or parallel work that needs isolation: `worktrees`.

Code quality:
- To make code easier to read without changing behavior: `simplify`.
- After a refactor or a few merged PRs: `post-refactor`.
- To map an unfamiliar repository: `codemap`.

GitHub maintenance:
- To audit issues: `iss-audit`. To audit pull requests: `pr-audit`.
- To implement what an audit approved and close the tickets: `github-resolution`.
- After merges, before deploy or release: `pr-post-audit`.
- To cut and publish a version, only when explicitly asked: `release`.

Security:
- For a security review of a codebase, commit range, or PR: `security-audit`.

Writing and learning:
- To remove AI-sounding patterns from prose: `humanizer`.
- To learn from repeated workflows and suggest reusable skills or rules: `reflect`.