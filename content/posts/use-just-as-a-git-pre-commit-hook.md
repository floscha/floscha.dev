---
title: "Use just as a Git Pre-Commit Hook"
date: 2025-08-14
---

## Introduction

As a fan of [make](https://www.gnu.org/software/make/manual/make.html)-like tools such as [just](https://github.com/casey/just) or [Task](https://taskfile.dev/) and appreciating the guardrails provided by Git [pre-commit](https://pre-commit.com/), I dislike the redundancy of having to define commands like linting both in the `justfile`/`taskfile` as well as in the `.pre-commit-config.yaml`.
To solve this, I've built a simple Git-native [pre-commit hook](https://git-scm.com/book/ms/v2/Customizing-Git-Git-Hooks) which only delegates to a **just** [recipe](https://just.systems/man/en/introduction.html) (in our case called `lint`) where all the checks are defined.
While the following examples use **just**, they are easily adaptable to **Task** or even good old **make**.

## Define just Recipe

As a prerequisite, we need a `justfile` which contains all the checks we expect to pass (meaning the commands results in a `0` return code), before a new Git commit can be created.
For a Python codebase this could look like this:

```plaintext
lint:
    just --fmt --unstable --check
    uvx ruff check
    uvx ruff format --check
```

## Set Up Pre-commit Hook

Within your repository, create a new file for the pre-commit hook using the editor of your choice. For VS Code, run

```sh
touch .git/hooks/pre-commit
```

The `pre-commit` file should then have the following content.

```sh
#!/bin/sh

# Prevent commit if 'just lint' recipe fails.
if ! just lint; then
    echo "Pre-commit check failed!"
    exit 1
fi
```

Finally, set the permissions such that the hook can be executed.

```sh
chmod u+x .git/hooks/pre-commit
```

## Enable/Disable Hook

To disable the git hook, remove the permission to execute the script.

```sh
chmod u-x .git/hooks/pre-commit
```

Consequently, to re-enable the hook, add the permission again.

```sh
chmod u+x .git/hooks/pre-commit
```

## Bonus: Putting Everything in a justfile

To not set up the pre-commit hook manually, you can also do this via a `enable-pre-commit` recipe.

```plaintext
lint:
    just --fmt --unstable --check
    uvx ruff check
    uvx ruff format --check

@enable-pre-commit:
    echo '#!/bin/sh\n\n# Prevent commit if '\''just lint'\'' recipe fails.\nif ! just lint; then\n    echo "Pre-commit check failed!"\n    exit 1\nfi' > .git/hooks/pre-commit
    chmod u+x .git/hooks/pre-commit
    @echo "Pre-commit hook enabled."

@disable-pre-commit:
    chmod u-x .git/hooks/pre-commit
    @echo "Pre-commit hook disabled."
```
