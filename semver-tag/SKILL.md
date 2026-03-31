---
name: semver-tag
description: Create and validate Semantic Versioning tags from existing release history, then annotate and push tags safely.
---

# semver-tag

Use this skill when you need to create or validate Git tags that follow SemVer 2.0.0.

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

- MAJOR: backward-incompatible API changes.
- MINOR: backward-compatible features.
- PATCH: backward-compatible fixes.

Common Git tag naming:

- Use `vX.Y.Z` for tag names (e.g. `v1.4.2`).
- The SemVer core remains `X.Y.Z`.

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
- For unstable development, `0.y.z` is acceptable.
- Do not re-tag or mutate an already published version; create a new version instead.

## Output expectation

A tag strategy that is:

- SemVer-compliant
- Consistent with repo history
- Safe for downstream automation and release tooling
