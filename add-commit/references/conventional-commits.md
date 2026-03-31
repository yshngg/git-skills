# Conventional Commits 1.0.0 — Reference

Source: https://www.conventionalcommits.org/en/v1.0.0/

## Commit message structure

```
<type>[optional scope][!]: <description>

[optional body]

[optional footer(s)]
```

## Types and SemVer correlation

| Type              | SemVer bump | Meaning                                              |
|-------------------|-------------|------------------------------------------------------|
| `fix`             | PATCH        | Bug fix for your application or library              |
| `feat`            | MINOR        | New feature introduced to the codebase               |
| `BREAKING CHANGE` | MAJOR        | Incompatible API change (any type can carry this)    |
| others            | none         | `build`, `chore`, `ci`, `docs`, `refactor`, `style`, `perf`, `test`, `revert`, etc. |

A type other than `fix` or `feat` has no implicit SemVer effect **unless** it also carries `BREAKING CHANGE`.

## Specification rules (essential)

1. Commits MUST be prefixed with a `type` (noun), followed by optional scope, optional `!`, and a REQUIRED `: ` (colon + space).
2. `feat` MUST be used for commits that add a new feature.
3. `fix` MUST be used for commits that represent a bug fix.
4. Scope MAY be provided after the type in parentheses: `fix(parser): …`
5. A **description** MUST immediately follow the `type[scope]: ` prefix — a short summary in the imperative mood.
6. A **body** MAY follow after one blank line; it is free-form and may span multiple paragraphs.
7. One or more **footers** MAY follow after one blank line from the body. Each footer MUST use the format:
   - `Token: value` (colon + space), **or**
   - `Token #value` (space + hash) — used for issue references
8. Footer tokens MUST use `-` instead of spaces (e.g., `Reviewed-by`, `Refs`). Exception: `BREAKING CHANGE` is allowed as-is.
9. **Breaking changes** must appear as either:
   - `!` immediately before `:` in the type/scope prefix, **or**
   - `BREAKING CHANGE: <description>` footer entry (uppercase, the token `BREAKING-CHANGE` is also valid)
   - Both MAY be used together.
10. Types, scopes, and descriptions are case-insensitive EXCEPT `BREAKING CHANGE` which MUST be uppercase.

## Examples

### Simple fix
```
fix(parser): handle null pointer on empty input
```

### New feature with scope
```
feat(auth): add OAuth2 login flow
```

### Breaking change via `!`
```
feat(api)!: remove deprecated v1 endpoints
```

### Breaking change via footer
```
feat: allow config to extend other configs

BREAKING CHANGE: `extends` key in config file now used for extending other config files
```

### Both `!` and footer (most explicit)
```
feat!: drop support for Node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

### Multi-paragraph body with footers
```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

### Revert commit
```
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

## Why it matters

- Enables automatic CHANGELOG generation
- Drives automated semantic version bumps (CI/CD tooling like `semantic-release`, `release-please`)
- Makes intent clear to teammates, code reviewers, and downstream consumers
- Encourages atomic, well-scoped commits
