# git-skills

A collection of agent skills for Git.

## Available Skills

- **add-commit**: Stage related changes and create a [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) message (type/scope/description, optional body/footer) with safe checks before committing.
- **semver-tag**: Create and validate [Semantic Versioning](https://semver.org/) tags from existing release history, then annotate and push tags safely.

Each skill ships with reference documentation under its `references/` directory.

## Installation

To use these skills, you can install them using the skills CLI:

```bash
npx skills add yshngg/git-skills
```

To list installed skills:

```bash
npx skills list
```

To remove a skill:

```bash
npx skills remove add-commit
```

For more information about skills, visit [skills.sh](https://skills.sh).
