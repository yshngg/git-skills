---
name: add-commit
description: Stage related changes and create a Conventional Commit message (type/scope/description, optional body/footer) with safe checks before committing.
---

# add-commit

Use this skill when you want to turn local changes into a clean, meaningful Git commit.

> **Reference:** Full Conventional Commits 1.0.0 spec, examples, and type→SemVer table:
> `references/conventional-commits.md` (source: https://www.conventionalcommits.org/en/v1.0.0/)

## What this skill does

- Reviews repository state before staging.
- Stages only relevant files/hunks for one logical change.
- Builds a Conventional Commit message.
- Highlights when `!` or `BREAKING CHANGE:` is required.
- Commits only after a quick sanity check.

## Inputs to gather

Before committing, collect:

- Change intent: what was changed and why.
- Scope (optional): subsystem affected (e.g. `api`, `cli`, `docs`).
- Breaking change status: yes/no.
- Related issue/reference IDs (optional).

## Conventional Commit format

```text
<type>[optional scope][!]: <description>

[optional body]

[optional footer(s)]
```

Rules:

- Use lowercase type (`feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `build`, `perf`, `revert`).
- Keep description imperative and concise (immediately after `type[scope]: `).
- Keep one commit per logical change.
- Use `!` and/or `BREAKING CHANGE:` footer for incompatible behavior changes — either works; both is most explicit.
- Body starts one blank line after the description; footers start one blank line after the body.
- Footer tokens use `-` for spaces (e.g., `Reviewed-by`, `Refs`); exception: `BREAKING CHANGE` is allowed as-is.
- Footer values use `Token: value` or `Token #value` (for issue refs) format.

### Type → SemVer impact

| Type                      | SemVer bump |
|---------------------------|-------------|
| `fix`                     | PATCH        |
| `feat`                    | MINOR        |
| `BREAKING CHANGE` / `!`   | MAJOR        |
| all others                | none (unless also breaking) |

This matters when using tools like `semantic-release` or `release-please` to automate versioning.

## Suggested workflow

1. Inspect changes.
2. Stage only related hunks/files.
3. Validate staged diff matches one intent.
4. Compose commit message from the template.
5. Commit.
6. Re-check `git status`.

## Command checklist

```bash
# 1) inspect
 git status --short
 git diff

# 2) stage (choose one or combine)
 git add <path>
 git add -p

# 3) verify staged content
 git diff --staged

# 4) commit
 git commit -m "feat(cli): add interactive tag prompt"

# 5) confirm clean/expected state
 git status --short
```

## Message templates

### Simple

```text
fix(parser): handle empty input
```

### With body

```text
feat(auth): add token refresh endpoint

Add refresh token validation and rotation to reduce session drop-offs.
```

### Breaking change (via `!`)

```text
feat(api)!: remove v1 user payload fields
```

### Breaking change (via footer — both combined is most explicit)

```text
feat(api)!: remove v1 user payload fields

BREAKING CHANGE: `first_name` and `last_name` were replaced by `full_name`.
```

### With footers (issue reference + reviewer)

```text
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Reviewed-by: Z
Refs: #123
```

## Quality guardrails

- Do not stage unrelated refactors with a bug fix.
- Do not commit generated noise unless intended.
- If commit is too broad, split it.
- If message type is unclear, prefer splitting commit by intent.

## Output expectation

A commit that is:

- Atomic (single logical unit)
- Traceable (clear intent)
- Automation-friendly (Conventional Commit compliant)
