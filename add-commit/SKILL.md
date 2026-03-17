---
name: add-commit
description: Perform git add and git commit operations with properly formatted commit messages following Conventional Commits specification. Use when you need to stage and commit changes to a Git repository with well-structured commit messages that communicate intent clearly.
---

# Conventional Commits 1.0.0 with Git Add/Commit Best Practices

## Summary

The Conventional Commits specification is a lightweight convention on top of commit messages.
It provides an easy set of rules for creating an explicit commit history;
which makes it easier to write automated tools on top of.
This convention dovetails with [SemVer](http://semver.org),
by describing the features, fixes, and breaking changes made in commit messages.

The commit message should be structured as follows:

---

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

---

<br />
The commit contains the following structural elements, to communicate intent to the
consumers of your library:

1. **fix:** a commit of the _type_ `fix` patches a bug in your codebase (this correlates with [`PATCH`](http://semver.org/#summary) in Semantic Versioning).
1. **feat:** a commit of the _type_ `feat` introduces a new feature to the codebase (this correlates with [`MINOR`](http://semver.org/#summary) in Semantic Versioning).
1. **BREAKING CHANGE:** a commit that has a footer `BREAKING CHANGE:`, or appends a `!` after the type/scope, introduces a breaking API change (correlating with [`MAJOR`](http://semver.org/#summary) in Semantic Versioning).
   A BREAKING CHANGE can be part of commits of any _type_.
1. _types_ other than `fix:` and `feat:` are allowed, for example [ @commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional) (based on the [Angular convention](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)) recommends `build:`, `chore:`,
   `ci:`, `docs:`, `style:`, `refactor:`, `perf:`, `test:`, and others.
1. _footers_ other than `BREAKING CHANGE: <description>` may be provided and follow a convention similar to
   [git trailer format](https://git-scm.com/docs/git-interpret-trailers).

Additional types are not mandated by the Conventional Commits specification, and have no implicit effect in Semantic Versioning (unless they include a BREAKING CHANGE).
<br /><br />
A scope may be provided to a commit's type, to provide additional contextual information and is contained within parenthesis, e.g., `feat(parser): add ability to parse arrays`.

## When to use

Use this skill when you need to:

- Stage and commit changes to a Git repository
- Create well-structured commit messages that follow Conventional Commits specification
- Ensure commit messages communicate the intent and impact of changes clearly
- Follow best practices for atomic commits and meaningful commit messages
- Automatically generate changelogs or determine semantic version bumps
- Communicate the nature of changes to teammates and other stakeholders

## Conventional Commits Examples

### Commit message with description and breaking change footer

```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

### Commit message with `!` to draw attention to breaking change

```
feat!: send an email to the customer when a product is shipped
```

### Commit message with scope and `!` to draw attention to breaking change

```
feat(api)!: send an email to the customer when a product is shipped
```

### Commit message with both `!` and BREAKING CHANGE footer

```
feat!: drop support for Node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

### Commit message with no body

```
docs: correct spelling of CHANGELOG
```

### Commit message with scope

```
feat(lang): add Polish language
```

### Commit message with multi-paragraph body and multiple footers

```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

## Conventional Commits Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

1. Commits MUST be prefixed with a type, which consists of a noun, `feat`, `fix`, etc., followed
   by the OPTIONAL scope, OPTIONAL `!`, and REQUIRED terminal colon and space.
1. The type `feat` MUST be used when a commit adds a new feature to your application or library.
1. The type `fix` MUST be used when a commit represents a bug fix for your application.
1. A scope MAY be provided after a type. A scope MUST consist of a noun describing a
   section of the codebase surrounded by parenthesis, e.g., `fix(parser):`
1. A description MUST immediately follow the colon and space after the type/scope prefix.
   The description is a short summary of the code changes, e.g., _fix: array parsing issue when multiple spaces were contained in string_.
1. A longer commit body MAY be provided after the short description, providing additional contextual information about the code changes. The body MUST begin one blank line after the description.
1. A commit body is free-form and MAY consist of any number of newline separated paragraphs.
1. One or more footers MAY be provided one blank line after the body. Each footer MUST consist of
   a word token, followed by either a `:<space>` or `<space>#` separator, followed by a string value (this is inspired by the
   [git trailer convention](https://git-scm.com/docs/git-interpret-trailers)).
1. A footer's token MUST use `-` in place of whitespace characters, e.g., `Acked-by` (this helps differentiate
   the footer section from a multi-paragraph body). An exception is made for `BREAKING CHANGE`, which MAY also be used as a token.
1. A footer's value MAY contain spaces and newlines, and parsing MUST terminate when the next valid footer
   token/separator pair is observed.
1. Breaking changes MUST be indicated in the type/scope prefix of a commit, or as an entry in the
   footer.
1. If included as a footer, a breaking change MUST consist of the uppercase text BREAKING CHANGE, followed by a colon, space, and description, e.g.,
   _BREAKING CHANGE: environment variables now take precedence over config files_.
1. If included in the type/scope prefix, breaking changes MUST be indicated by a
   `!` immediately before the `:`. If `!` is used, `BREAKING CHANGE:` MAY be omitted from the footer section,
   and the commit description SHALL be used to describe the breaking change.
1. Types other than `feat` and `fix` MAY be used in your commit messages, e.g., _docs: update ref docs._
1. The units of information that make up Conventional Commits MUST NOT be treated as case-sensitive by implementors, with the exception of BREAKING CHANGE which MUST be uppercase.
1. BREAKING-CHANGE MUST be synonymous with BREAKING CHANGE, when used as a token in a footer.

## Git Commit Best Practices

### Basic Rules

#### Commit Related Changes

A commit should be a wrapper for related changes. For example, fixing two different bugs should produce two separate commits. Small commits make it easier for other developers to understand the changes and roll them back if something went wrong.
With tools like the staging area and the ability to stage only parts of a file, Git makes it easy to create very granular commits.

#### Commit Often

Committing often keeps your commits small and, again, helps you commit only related changes. Moreover, it allows you to share your code more frequently with others. That way it's easier for everyone to integrate changes regularly and avoid having merge conflicts. Having large commits and sharing them infrequently, in contrast, makes it hard to solve conflicts.

#### Don't Commit Half-Done Work

You should only commit code when a logical component is completed.
Split a feature's implementation into logical chunks that can be completed quickly so that you can commit often. If you're tempted to commit just because you need a clean working copy (to check out a branch, pull in changes, etc.) consider using Git's «Stash» feature instead.

#### Test Your Code Before You Commit

Resist the temptation to commit something that you «think» is completed. Test it thoroughly to make sure it really is completed and has no side effects (as far as one can tell). While committing half-baked things in your local repository only requires you to forgive yourself, having your code tested is even more important when it comes to pushing/sharing your code with others.

#### Write Good Commit Messages

Begin your message with a short summary of your changes (up to 50 characters as a guideline). Separate it from
the following body by including a blank line. The body of your message should provide detailed answers to the following questions:
– What was the motivation for the change? – How does it differ from the previous
implementation?
Use the imperative, present tense ("change", not "changed" or "changes") to be consistent with generated messages from commands like git merge.
Having your files backed up on a remote server is a nice side effect of having a version control system. But you should not use your VCS like it was a backup system. When doing version control, you should pay attention to committing semantically (see "related changes") - you shouldn't just cram in files.

#### Use Branches

Branching is one of Git's most powerful features - and this is not by accident: quick and easy branching was a central requirement from day one. Branches are the perfect tool to help you avoid mixing up different lines of development. You should use branches extensively in your development workflows: for new features, bug fixes, ideas...

#### Agree on A Workflow

Git lets you pick from a lot of different workflows: long-running branches, topic branches, merge or rebase, git-flow... Which one you choose depends on a couple of factors: your project, your overall development and deployment workflows and (maybe most importantly) on your and your teammates' personal preferences. However you choose to work, just make sure to agree on a common workflow that everyone follows.

The following document is based on experience doing code development, bug troubleshooting and code review across a number of projects using GIT, including libvirt, QEMU and OpenStack Nova. Examination of other open source projects such as the Kernel, CoreUtils, GNULIB and more suggested they all follow a fairly common practice. It is motivated by a desire to improve the quality of the Nova GIT history. Quality is a hard term to define in computing; one man's "Thing of Beauty" is another man's "Evil Hack". We can, however, come up with some general guidelines for what to do, or conversely what not to do, when publishing GIT commits for merge with a project, in this case, OpenStack.

This topic can be split into two areas of concern

- The structured set/split of the code changes
- The information provided in the commit message

## Formatting Rules

- Capitalized, short **(50 chars or less)** summary

- More detailed explanatory text, if necessary. **Wrap it to about 72 characters**. In some contexts, the first line is treated as the subject of an email and the rest of the text as the body. The blank line separating the summary from the body is critical (unless you omit the body entirely); tools like rebase can get confused if you run the two together.

- **Always leave the second line blank.**

- Write your commit message in the imperative: "Fix bug" and not "Fixed bug" or "Fixes bug." This convention matches up with commit messages generated by commands like git merge and git revert.

- Further paragraphs come after blank lines.
  - Bullet points are okay, too
  - Typically a hyphen or asterisk is used for the bullet, preceded by a single space, with blank lines in between, but conventions vary here
  - Use a hanging indent

### Examples of good practice

**Example 1** (no description, only summary)

```
  commit 3114a97ba188895daff4a3d337b2c73855d4632d
  Author: [removed]
  Date:   Mon Jun 11 17:16:10 2012 +0100

    Update default policies for KVM guest PIT & RTC timers
```

**Example 2** (description as bullet points)

```
  commit ae878fc8b9761d099a4145617e4a48cbeb390623
  Author: [removed]
  Date:   Fri Jun 1 01:44:02 2012 +0000

    Refactor libvirt create calls

     - Minimize duplicated code for create

     - Make wait_for_destroy happen on shutdown instead of undefine

     - Allow for destruction of an instance while leaving the domain
```

**Example 3** (description as plain text)

```
  commit 31336b35b4604f70150d0073d77dbf63b9bf7598
  Author: [removed]
  Date:   Wed Jun 6 22:45:25 2012 -0400

    Add CPU arch filter scheduler support

    In a mixed environment of running different CPU architectures,
    one would not want to run an ARM instance on a X86_64 host and
    vice versa.

    This scheduler filter option will prevent instances running
    on a host that it is not intended for.

    The libvirt driver queries the guest capabilities of the
    host and stores the guest arches in the permitted_instances_types
    list in the cpu_info dict of the host.

    The Xen equivalent will be done later in another commit.

    The arch filter will compare the instance arch against
    the permitted_instances_types of a host
    and filter out invalid hosts.

    Also adds ARM as a valid arch to the filter.

    The ArchFilter is not turned on by default.
```

## Why Use Conventional Commits

- Automatically generating CHANGELOGs.
- Automatically determining a semantic version bump (based on the types of commits landed).
- Communicating the nature of changes to teammates, the public, and other stakeholders.
- Triggering build and publish processes.
- Making it easier for people to contribute to your projects, by allowing them to explore
  a more structured commit history.

## FAQ

### How should I deal with commit messages in the initial development phase?

We recommend that you proceed as if you've already released the product. Typically _somebody_, even if it's your fellow software developers, is using your software. They'll want to know what's fixed, what breaks etc.

### Are the types in the commit title uppercase or lowercase?

Any casing may be used, but it's best to be consistent.

### What do I do if the commit conforms to more than one of the commit types?

Go back and make multiple commits whenever possible. Part of the benefit of Conventional Commits is its ability to drive us to make more organized commits and PRs.

### Doesn't this discourage rapid development and fast iteration?

It discourages moving fast in a disorganized way. It helps you be able to move fast long term across multiple projects with varied contributors.

### Might Conventional Commits lead developers to limit the type of commits they make because they'll be thinking in the types provided?

Conventional Commits encourages us to make more of certain types of commits such as fixes. Other than that, the flexibility of Conventional Commits allows your team to come up with their own types and change those types over time.

### How does this relate to SemVer?

`fix` type commits should be translated to `PATCH` releases. `feat` type commits should be translated to `MINOR` releases. Commits with `BREAKING CHANGE` in the commits, regardless of type, should be translated to `MAJOR` releases.

### How should I version my extensions to the Conventional Commits Specification, e.g. ` @jameswomack/conventional-commit-spec`?

We recommend using SemVer to release your own extensions to this specification (and
encourage you to make these extensions!)

### What do I do if I accidentally use the wrong commit type?

#### When you used a type that's of the spec but not the correct type, e.g. `fix` instead of `feat`

Prior to merging or releasing the mistake, we recommend using `git rebase -i` to edit the commit history. After release, the cleanup will be different according to what tools and processes you use.

#### When you used a type _not_ of the spec, e.g. `feet` instead of `feat`

In a worst case scenario, it's not the end of the world if a commit lands that does not meet the Conventional Commits specification. It simply means that commit will be missed by tools that are based on the spec.

### Do all my contributors need to use the Conventional Commits specification?

No! If you use a squash based workflow on Git lead maintainers can clean up the commit messages as they're merged—adding no workload to casual committers.
A common workflow for this is to have your git system automatically squash commits from a pull request and present a form for the lead maintainer to enter the proper git commit message for the merge.

### How does Conventional Commits handle revert commits?

Reverting code can be complicated: are you reverting multiple commits? if you revert a feature, should the next release instead be a patch?

Conventional Commits does not make an explicit effort to define revert behavior. Instead we leave it to tooling
authors to use the flexibility of _types_ and _footers_ to develop their logic for handling reverts.

One recommendation is to use the `revert` type, and a footer that references the commit SHAs that are being reverted:

```
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

## Instructions for Git Add/Commit Operations

1. **Analyze the changes**: Review the differences in the working directory using `git diff` to understand what changes are being committed.

2. **Stage appropriate files**: Use `git add` to stage only related changes. You can stage specific files with `git add <filename>` or use `git add -p` to interactively stage parts of files to create granular commits.

3. **Determine the appropriate commit type**: Based on the Conventional Commits specification, determine if the changes represent a `feat`, `fix`, or another appropriate type. Consider if the changes include any breaking changes that need to be marked with `!` or a BREAKING CHANGE footer.

4. **Write a concise, imperative commit message**: Craft a commit message following the Conventional Commits format with an appropriate type, optional scope, and descriptive summary. Keep the summary under 50 characters and use imperative mood.

5. **Add a detailed body if needed**: If the change is complex, add a detailed explanatory text after a blank line, wrapping to about 72 characters.

6. **Include footers for special cases**: Add footers for breaking changes, issue references (e.g., `Closes #123`), or other metadata as appropriate.

7. **Execute the commit**: Run `git commit` with the prepared message, or use `git commit -m` for simple commits.
