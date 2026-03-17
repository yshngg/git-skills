---
name: semver-tag
description: Ensures Git tags comply with Semantic Versioning (SemVer) 2.0.0 specifications for consistent version management. Validates version format, increment rules, and best practices for MAJOR.MINOR.PATCH versioning.
---

# SemVer Tag

Ensures Git tags comply with Semantic Versioning (SemVer) 2.0.0 specifications for consistent version management.

## Purpose

This skill helps developers create and validate Git tags that follow the Semantic Versioning specification. Proper version tags enable predictable dependency management and release processes.

## Semantic Versioning Format

Given a version number MAJOR.MINOR.PATCH, increment the:

1. MAJOR version when you make incompatible API changes
2. MINOR version when you add functionality in a backward compatible manner
3. PATCH version when you make backward compatible bug fixes

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

## Semantic Versioning Specification (SemVer) 2.0.0

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

1. Software using Semantic Versioning MUST declare a public API. This API could be declared in the code itself or exist strictly in documentation. However it is done, it SHOULD be precise and comprehensive.

2. A normal version number MUST take the form X.Y.Z where X, Y, and Z are non-negative integers, and MUST NOT contain leading zeroes. X is the major version, Y is the minor version, and Z is the patch version. Each element MUST increase numerically. For instance: 1.9.0 -> 1.10.0 -> 1.11.0.

3. Once a versioned package has been released, the contents of that version MUST NOT be modified. Any modifications MUST be released as a new version.

4. Major version zero (0.y.z) is for initial development. Anything MAY change at any time. The public API SHOULD NOT be considered stable.

5. Version 1.0.0 defines the public API. The way in which the version number is incremented after this release is dependent on this public API and how it changes.

6. Patch version Z (x.y.Z | x > 0) MUST be incremented if only backward compatible bug fixes are introduced. A bug fix is defined as an internal change that fixes incorrect behavior.

7. Minor version Y (x.Y.z | x > 0) MUST be incremented if new, backward compatible functionality is introduced to the public API. It MUST be incremented if any public API functionality is marked as deprecated. It MAY be incremented if substantial new functionality or improvements are introduced within the private code. It MAY include patch level changes. Patch version MUST be reset to 0 when minor version is incremented.

8. Major version X (X.y.z | X > 0) MUST be incremented if any backward incompatible changes are introduced to the public API. It MAY also include minor and patch level changes. Patch and minor versions MUST be reset to 0 when major version is incremented.

9. A pre-release version MAY be denoted by appending a hyphen and a series of dot separated identifiers immediately following the patch version. Identifiers MUST comprise only ASCII alphanumerics and hyphens [0-9A-Za-z-]. Identifiers MUST NOT be empty. Numeric identifiers MUST NOT include leading zeroes. Pre-release versions have a lower precedence than the associated normal version. A pre-release version indicates that the version is unstable and might not satisfy the intended compatibility requirements as denoted by its associated normal version. Examples: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92, 1.0.0-x-y-z.--.

10. Build metadata MAY be denoted by appending a plus sign and a series of dot separated identifiers immediately following the patch or pre-release version. Identifiers MUST comprise only ASCII alphanumerics and hyphens [0-9A-Za-z-]. Identifiers MUST NOT be empty. Build metadata MUST be ignored when determining version precedence. Thus two versions that differ only in the build metadata, have the same precedence. Examples: 1.0.0-alpha+001, 1.0.0+20130313144700, 1.0.0-beta+exp.sha.5114f85, 1.0.0+21AF26D3----117B344092BD.

11. Precedence refers to how versions are compared to each other when ordered.
    - Precedence MUST be calculated by separating the version into major, minor, patch and pre-release identifiers in that order (build metadata does not figure into precedence).
    - Precedence is determined by the first difference when comparing each of these identifiers from left to right as follows: major, minor, and patch versions are always compared numerically. Example: 1.0.0 < 2.0.0 < 2.1.0 < 2.1.1.
    - When major, minor, and patch are equal, a pre-release version has lower precedence than a normal version: Example: 1.0.0-alpha < 1.0.0.
    - Precedence for two pre-release versions with the same major, minor, and patch version MUST be determined by comparing each dot separated identifier from left to right until a difference is found as follows:
      - Identifiers consisting of only digits are compared numerically.
      - Identifiers with letters or hyphens are compared lexically in ASCII sort order.
      - Numeric identifiers always have lower precedence than non-numeric identifiers.
      - A larger set of pre-release fields has a higher precedence than a smaller set, if all of the preceding identifiers are equal.
    - Example: 1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0.

## Backus–Naur Form Grammar for Valid SemVer Versions

```
<valid semver> ::= <version core>
                 | <version core> "-" <pre-release>
                 | <version core> "+" <build>
                 | <version core> "-" <pre-release> "+" <build>

<version core> ::= <major> "." <minor> "." <patch>

<major> ::= <numeric identifier>

<minor> ::= <numeric identifier>

<patch> ::= <numeric identifier>

<pre-release> ::= <dot-separated pre-release identifiers>

<dot-separated pre-release identifiers> ::= <pre-release identifier>
                                          | <pre-release identifier> "." <dot-separated pre-release identifiers>

<build> ::= <dot-separated build identifiers>

<dot-separated build identifiers> ::= <build identifier>
                                    | <build identifier> "." <dot-separated build identifiers>

<pre-release identifier> ::= <alphanumeric identifier>
                           | <numeric identifier>

<build identifier> ::= <alphanumeric identifier>
                     | <digits>

<alphanumeric identifier> ::= <non-digit>
                            | <non-digit> <identifier characters>
                            | <identifier characters> <non-digit>
                            | <identifier characters> <non-digit> <identifier characters>

<numeric identifier> ::= "0"
                       | <positive digit>
                       | <positive digit> <digits>

<identifier characters> ::= <identifier character>
                          | <identifier character> <identifier characters>

<identifier character> ::= <digit>
                         | <non-digit>

<non-digit> ::= <letter>
              | "-"

<digits> ::= <digit>
           | <digit> <digits>

<digit> ::= "0"
          | <positive digit>

<positive digit> ::= "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

<letter> ::= "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J"
           | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T"
           | "U" | "V" | "W" | "X" | "Y" | "Z" | "a" | "b" | "c" | "d"
           | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n"
           | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x"
           | "y" | "z"
```

## Best Practices

- Use `git tag v1.2.3` format with the 'v' prefix commonly used in version control (note: "v1.2.3" is the tag name, "1.2.3" is the semantic version)
- Ensure each version corresponds to a stable release
- Maintain a clear public API definition for version 1.0.0+
- Document breaking changes when incrementing major versions
- Follow consistent naming conventions across your project

## Common Commands

```bash
# Create annotated tag
git tag -a v1.2.3 -m "Release version 1.2.3"

# List tags
git tag

# Validate tag format with regex (basic)
git tag | grep -E '^v?[0-9]+\.[0-9]+\.[0-9]+(-[0-9A-Za-z-]+(\.[0-9A-Za-z-]+)*)?(\+[0-9A-Za-z-]+(\.[0-9A-Za-z-]+)*)?$'

# Push tags
git push origin v1.2.3
git push origin --tags
```

## Version Increment Guidelines

- Major (X.y.z): Breaking changes, incompatible API modifications
- Minor (x.Y.z): New features, backward-compatible additions
- Patch (x.y.Z): Bug fixes, backward-compatible corrections

## Pre-release and Build Metadata

- Pre-release: Use hyphens followed by dot-separated alphanumeric identifiers (e.g., alpha, beta, rc)
- Build metadata: Use plus sign followed by dot-separated identifiers for build information
- Examples: v1.0.0-alpha, v1.0.0-alpha.1, v1.0.0-beta.1+build.123

## Version Comparison

- Normal versions have higher precedence than pre-release versions
- Precedence is determined by comparing each identifier from left to right
- Numeric identifiers are compared numerically, not alphabetically
- Example: 1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-beta.1 < 1.0.0-rc.1 < 1.0.0

## Validation Checklist

Before creating a Git tag, verify:

- [ ] Version follows X.Y.Z format (or v.X.Y.Z with prefix)
- [ ] MAJOR, MINOR, and PATCH numbers are non-negative integers
- [ ] No leading zeros in version components
- [ ] Each element increases numerically (e.g., 1.9.0 -> 1.10.0 -> 1.11.0)
- [ ] Increment is appropriate based on changes made (breaking/backward-compatible)
- [ ] Public API changes are documented appropriately
- [ ] Tag message describes the release content
- [ ] Pre-release and build metadata follow SemVer format if used
- [ ] Pre-release identifiers contain only ASCII alphanumerics and hyphens
- [ ] Build metadata identifiers contain only ASCII alphanumerics and hyphens
- [ ] Version precedence follows SemVer rules

## FAQ

### How should I deal with revisions in the 0.y.z initial development phase?

The simplest thing to do is start your initial development release at 0.1.0 and then increment the minor version for each subsequent release.

### How do I know when to release 1.0.0?

If your software is being used in production, it should probably already be 1.0.0. If you have a stable API on which users have come to depend, you should be 1.0.0. If you're worrying a lot about backward compatibility, you should probably already be 1.0.0.

### What do I do if I accidentally release a backward incompatible change as a minor version?

As soon as you realize that you've broken the Semantic Versioning spec, fix the problem and release a new patch version that corrects the problem and restores backward compatibility. Even under this circumstance, it is unacceptable to modify versioned releases. If it's appropriate, document the offending version and inform your users of the problem so that they are aware of the offending version.

### How should I handle deprecating functionality?

Deprecating existing functionality is a normal part of software development and is often required to make forward progress. When you deprecate part of your public API, you should do two things: (1) update your documentation to let users know about the change, (2) issue a new minor release with the deprecation in place. Before you completely remove the functionality in a new major release there should be at least one minor release that contains the deprecation so that users can smoothly transition to the new API.

### Is there a suggested regular expression (RegEx) to check a SemVer string?

There are two. One with named groups for those systems that support them (PCRE [Perl Compatible Regular Expressions, i.e. Perl, PHP and R], Python and Go):

```
^(?P<major>0|[1-9]\d*)\.(?P<minor>0|[1-9]\d*)\.(?P<patch>0|[1-9]\d*)(?:-(?P<prerelease>(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+(?P<buildmetadata>[0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

And one with numbered capture groups instead (so cg1 = major, cg2 = minor, cg3 = patch, cg4 = prerelease and cg5 = buildmetadata) that is compatible with ECMA Script (JavaScript), PCRE (Perl Compatible Regular Expressions, i.e. Perl, PHP and R), Python and Go:

```
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

## Troubleshooting

- If a tag doesn't follow SemVer, consider creating a new compliant tag and annotating the reason for the old one
- For legacy projects without SemVer tags, start with 1.0.0 if the API is stable, or 0.1.0 if still in initial development
- When in doubt about increment type, err on the side of caution (increment MAJOR for potentially breaking changes)
- Remember that "v1.2.3" is a tag name with the "v" prefix, while "1.2.3" is the actual semantic version
