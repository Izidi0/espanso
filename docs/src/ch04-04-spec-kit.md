# Spec Kit

This repository uses GitHub Spec Kit to organize feature work as specifications, implementation plans, and task lists. The setup is configured for Codex on Windows and uses PowerShell scripts.

## Installation

Install `uv`, then reopen PowerShell so its updated `PATH` is loaded:

```powershell
winget install --id astral-sh.uv -e
```

Install the Specify CLI at the release used to generate this repository's Spec Kit files:

```powershell
uv tool install --python 3.12 specify-cli --from git+https://github.com/github/spec-kit.git@v1.2.2
specify version
```

The version command should report `1.2.2`. If PowerShell cannot find `uv` or `specify`, reopen the terminal; for `specify`, run `uv tool update-shell` if needed.

## Repository setup

From the repository root, initialize or refresh the project files with:

```powershell
specify init --here --force --non-interactive --integration codex --script ps --integration-options="--skills"
specify integration status
```

The `--skills` integration option installs Codex skills under `.agents/skills/`. Shared templates, PowerShell scripts, workflow metadata, and integration manifests are stored under `.specify/`. Review the Git diff after initialization, especially when upgrading the CLI, because managed templates can change.

Commit the generated skills and shared `.specify/` files with project changes. `.specify/.gitignore` excludes the CLI's machine-local feature pointer and local extension configuration. Keep private credentials or other local agent data out of the repository.

## Using the workflow

Start Codex with the repository open. For a new feature, invoke the skills in order from Codex chat:

1. `$speckit-constitution` to establish project principles based on existing repository guidance.
2. `$speckit-specify` to describe one bounded feature.
3. `$speckit-clarify` when the requirements need clarification.
4. `$speckit-plan` to design the change against the Rust workspace and its existing conventions.
5. `$speckit-tasks` to produce actionable implementation tasks.
6. `$speckit-analyze` to check consistency before implementation.
7. `$speckit-implement` to carry out the approved tasks, then `$speckit-converge` to check remaining work.

Review each artifact before continuing. The setup creates a constitution template at `.specify/memory/constitution.md`; replace its placeholders through `$speckit-constitution` before relying on it. Bug-fixing and idea-assessment workflows are optional extensions and are not installed by this setup.
