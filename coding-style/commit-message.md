# Commit Message guidelines

I most of our projects, we follow the [*Conventional Commits* specification](https://www.conventionalcommits.org) for both clarity and automation purposes.

This document is a basic summary of the specification and how we use it, but you cna find more details on the [*Conventional Commits* specification](https://www.conventionalcommits.org) website.

## Usage

The [*Conventional Commits* specification](https://www.conventionalcommits.org) defines standards for full commit messages with complete descriptions, issue references and other details. But for common usage in our projects, since details are usually documented on *Issues* or in the documentation of our projects, we can just focus on the summaries. Because of this usage, please refer to the [*Conventional Commits* specification](https://www.conventionalcommits.org) for detailed commit message conventions.

As a general rule, a commit message can take two forms:

```text
type: short description
OR
type(scope): short description
```

### Types

Based on the [Angular team guidelines](https://github.com/angular/angular/blob/main/contributing-docs/commit-message-guidelines.md), the keyword can be one of the following:

- **`fix`**: a bug fix or a resolved issue
- **`docs`**: a change that occurs only in the documentation
- **`feat`**: a new feature
- **`refactor`**: a change that neither fixes a bug nor adds a feature
- **`ci`**: A change to the CI configuration (eg. GitHub Actions)
- **`test`**: Adding missing tests or correcting existing tests
- **`chore`**: A change with no impact for the users (eg. add version tag, deploy a Release, ...)

There's also some automatic behaviors related to the use of some of these keywords:

- **`fix`**: implies a *PATCH* in [Semantic Versioning](https://semver.org)
- **`feat`**: implies a *MINOR* in [Semantic Versioning](https://semver.org)
- Adding **`BREAKING CHANGE`** in the footer or a **`!`** after the type/scope: represents a breaking change and implies a *MAJOR* in [Semantic Versioning](https://semver.org) (eg: `fix!: removed v1 API calls`)

### Scopes

A scope is an optional part. It's a noun or an abbreviation for the subject of a change. Projects may have a shortlist of allowed scopes, but there's no common rules.

Here are some common examples:

- **`lang`**: related to translations
- **`core`**: related to the core systems
- **`common`**: related to utilities
- ...

We recommend to not specify a scope if you're not sure of the one to use, as it is an optional part.