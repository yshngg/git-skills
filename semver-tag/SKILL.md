---
name: semver-tag
description: Create and validate Semantic Versioning tags from existing release history, then annotate and push tags safely.
---

# semver-tag

Use this skill when you need to create or validate Git tags that follow SemVer 2.0.0.

> **Reference:** Full SemVer 2.0.0 spec including pre-release precedence, build metadata, and version comparison rules:
> `references/semver.md` (source: https://semver.org/)

## What this skill does

- Finds the latest version tag.
- Determines next version candidate (major/minor/patch).
- Validates SemVer format (including optional pre-release/build metadata).
- Creates an annotated tag.
- Pushes tag(s) explicitly.

## SemVer quick rules

Version format:

```text
MAJOR.MINOR.PATCH[-PRERELEASE][+BUILD]
```

Increment guidance:

- MAJOR: backward-incompatible API changes → reset MINOR and PATCH to 0.
- MINOR: backward-compatible new features or deprecations → reset PATCH to 0.
- PATCH: backward-compatible bug fixes only.

Pre-release examples: `1.0.0-alpha`, `1.0.0-alpha.1`, `1.0.0-rc.1`
Pre-release versions have **lower precedence** than the associated normal release (`1.0.0-rc.1 < 1.0.0`).

Build metadata (e.g., `1.0.0+20130313`) is ignored for precedence — two versions differing only in build metadata are considered equal.

Common Git tag naming:

- Use `vX.Y.Z` for tag names (e.g. `v1.4.2`).
- The SemVer core remains `X.Y.Z`.

### Conventional Commits → bump type mapping

If the repo uses Conventional Commits, derive the bump from recent commit history:

| Commit signal             | SemVer bump |
|---------------------------|-------------|
| `fix:` type               | PATCH        |
| `feat:` type              | MINOR        |
| `BREAKING CHANGE:` or `!` | MAJOR        |

## Recommended workflow

1. List and inspect existing tags.
2. Identify latest SemVer tag.
3. Choose bump type (major/minor/patch/prerelease).
4. Compute and validate next version.
5. Create annotated tag with release note.
6. Push explicit tag.

## Command checklist

```bash
# 1) list tags
 git tag --list

# 2) inspect latest semver-looking tags
 git tag --sort=-v:refname | head

# 3) create annotated tag
 git tag -a v1.2.3 -m "Release v1.2.3"

# 4) push only this tag (preferred)
 git push origin v1.2.3
```

## Validation regex (SemVer 2.0.0)

Use this for validating `X.Y.Z` (without `v` prefix):

```regex
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[A-Za-z-][0-9A-Za-z-]*)(?:\.(?:0|[1-9]\d*|\d*[A-Za-z-][0-9A-Za-z-]*))*))?(?:\+([0-9A-Za-z-]+(?:\.[0-9A-Za-z-]+)*))?$
```

If your repository uses `v`-prefixed tags, strip `v` before strict validation or allow an optional prefix in tooling.

## Decision hints

- If unsure between MINOR and MAJOR, choose MAJOR for user safety.
- For unstable development, `0.y.z` is acceptable — start at `0.1.0` and increment MINOR for each release.
- Release `1.0.0` when the API is stable and in production use.
- Do not re-tag or mutate an already published version; create a new version instead.
- Deprecating a feature? MINOR bump (not MAJOR) — just document it clearly in the release notes.

## Output expectation

A tag strategy that is:

- SemVer-compliant
- Consistent with repo history
- Safe for downstream automation and release tooling
