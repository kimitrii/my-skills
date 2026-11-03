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

# Default workflow
 
When asked to build a feature, fix a bug, refactor, or work on a ticket, follow these
steps without waiting to be told each one. Skip a step only when it clearly doesn't
apply (a one-line fix doesn't need a verification plan).
 
1. **Understand the task.** If it references a GitHub issue, read it with
   `gh issue view <N>`. If the requirements are ambiguous, summarize your
   understanding and ask before writing code.
2. **Plan verification** with `verification-planning` for any non-trivial change.
3. **Branch.** Create a branch with `gt create`; never work directly on `main`/`master`.
4. **Implement** following the Code guidelines and Testing sections above.
5. **Clean up** the changed code with `simplify`.
6. **Security check.** If the change touches authentication, passwords, personal data,
   payments, permissions, or CI/deploy, run `security-audit` on the branch diff.
7. **Commit** with conventional commits (`type(scope): subject`).
8. **Open PRs.** Respect the 300-line limit, splitting into stacked PRs with `gt`.
   Write PR descriptions with `humanizer`. Add `Closes #N` when there is a GitHub issue.
   Submit with `gt submit`.
This workflow is for your own development work. When a skill below is in use (for
example `github-resolution` or `release`), follow that skill's steps instead.
 


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