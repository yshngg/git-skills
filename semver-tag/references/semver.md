# Semantic Versioning 2.0.0 — Reference

Source: https://semver.org/

## Version format

```
MAJOR.MINOR.PATCH[-PRERELEASE][+BUILD]
```

## Core increment rules

| Component | Increment when…                                                    | Reset rule                       |
|-----------|--------------------------------------------------------------------|----------------------------------|
| MAJOR     | Backward-incompatible API changes are introduced                   | Reset MINOR and PATCH to 0       |
| MINOR     | New backward-compatible functionality or deprecations are added    | Reset PATCH to 0                 |
| PATCH     | Backward-compatible bug fixes only                                 | —                                |

Key rules:
- Versions MUST NOT have leading zeroes: `1.9.0 → 1.10.0 → 1.11.0` (not `1.09.0`).
- Once a version is released, its contents MUST NOT be modified. Any change requires a new version.
- Major version **0** (0.y.z) is for initial development — anything may change at any time; the API is not considered stable.
- **1.0.0** marks the first stable public API.

## Pre-release versions

Append `-` and dot-separated identifiers after PATCH:

```
1.0.0-alpha
1.0.0-alpha.1
1.0.0-0.3.7
1.0.0-x.7.z.92
```

Rules:
- Identifiers: ASCII alphanumerics and hyphens `[0-9A-Za-z-]`; MUST NOT be empty
- Numeric identifiers MUST NOT have leading zeroes (`1.0.0-01` is invalid)
- Pre-release versions have **lower precedence** than the associated normal version (`1.0.0-alpha < 1.0.0`)

### Pre-release precedence ordering example
```
1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0
```

Comparison algorithm (left to right per dot-separated identifier):
1. Numeric-only identifiers: compared numerically
2. Identifiers with letters/hyphens: compared lexically in ASCII order
3. Numeric < alphanumeric (e.g., `beta.2` < `beta.11` but `2` < `11`)
4. Larger set of pre-release fields beats smaller set when all preceding are equal

## Build metadata

Append `+` and dot-separated identifiers after PATCH or pre-release:

```
1.0.0+20130313144700
1.0.0-beta+exp.sha.5114f85
1.0.0+21AF26D3----117B344092BD
```

Rules:
- Identifiers: ASCII alphanumerics and hyphens
- Build metadata MUST be **ignored** when determining version precedence — two versions differing only in build metadata have equal precedence

## Version precedence

Precedence is determined left to right: MAJOR → MINOR → PATCH → PRERELEASE (build metadata excluded).

```
1.0.0 < 2.0.0 < 2.1.0 < 2.1.1
1.0.0-alpha < 1.0.0
```

## Conventional Commits → SemVer mapping

When using Conventional Commits, map commit types to version bumps:

| Commit signal             | SemVer bump |
|---------------------------|-------------|
| `fix:` type               | PATCH        |
| `feat:` type              | MINOR        |
| `BREAKING CHANGE:` or `!` | MAJOR        |

## Common decisions

- **Unsure between MINOR and MAJOR?** Choose MAJOR to protect users.
- **Deprecating a feature?** Increment MINOR (and document in release notes); deprecation alone is not MAJOR.
- **Dependency update with no public API change?** PATCH if bug fix, MINOR if it unlocks new functionality.
- **Internal refactor, no public API change?** PATCH or MINOR at your discretion.
- **Never re-tag or mutate a published version.** Create a new version instead.

## Initial development guidance

- Start at `0.1.0`; increment MINOR for each subsequent release during initial development.
- When the API is stable and in production use, release `1.0.0`.
