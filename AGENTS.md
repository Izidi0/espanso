# Repository Guidelines

## Project Structure

Espanso is a Rust Cargo workspace. The `espanso/` crate is the application; `espanso-*` crates handle configuration, matching, rendering, input detection and injection, packages, IPC, clipboard, and UI. Rust tests usually live beside code under `src/`. Native platform code is grouped under `espanso-ui/src/{win32,mac,linux}` and `espanso-modulo/src/sys`. Contributor documentation is in `docs/src/`, schemas in `schemas/`, assets in `images/`, and build scripts in `scripts/`, `nix/`, and `snap/`.

## Build and Checks

Run Cargo from the repository root with the stable toolchain in `rust-toolchain.toml`. `cargo build` builds the workspace; `cargo fmt --all -- --check` checks formatting; `cargo clippy --workspace -- --deny warnings` checks lints. Cross-compilation is not guaranteed; follow `.github/workflows/ci.yml` for platform feature flags and tests. Linux CI excludes `espanso-modulo` and `espanso-ipc` from tests; Windows CI formats, builds, and runs Clippy without tests. See `docs/src/ch03-00-building-and-compilation.md` for prerequisites.

## Style and Testing

Follow `rustfmt.toml`, `.editorconfig`, and `.clang-format`: Rust uses four spaces and 100-character lines; C/C++ use 80. Use clear Rust names: `snake_case` for functions and modules, `UpperCamelCase` for types. Workspace Clippy rules apply only to crates opting in with `[lints] workspace = true`. Add focused tests for behavior changes, usually beside code in a `#[cfg(test)]` module. Run `cargo test -p <crate>`, then applicable CI checks. Manually check changes to expansion, input, or UI behavior using `docs/src/ch05-01-manual-testing.md`. Documentation-only changes need documentation review.

## Commits and Pull Requests

Keep pull requests focused. Describe user impact and checks; link related issues and include UI screenshots when useful. Commit subjects sometimes use a scope, such as `fix(render): ...`; this is an example, not a requirement. Pull request commits are usually squashed when merged.

## Documentation, Packages, and License

Use the [official getting-started documentation](https://espanso.org/docs/get-started/) for user-facing behavior and configuration guidance. The [Espanso Hub](https://hub.espanso.org/) is the catalog for reusable expansion packages. When configuration fields, types, or accepted values change, update the relevant documentation and files in `schemas/`. Preserve the repository's license and attribution: espanso was created by [Federico Terzi](http://federicoterzi.com/) and is licensed under the [GPL-3.0 license](https://github.com/espanso/espanso/blob/dev/LICENSE).

## Spec Kit Integration and Workflow Policy

### Assess the Request Before Selecting a Workflow

For each development request, inspect applicable instructions, the current branch and pending
changes, affected components, `.specify/memory/constitution.md`, and relevant existing feature
artifacts. Preserve user work. Select the smallest workflow that addresses the request's scope,
uncertainty, compatibility impact, and verification needs; briefly explain that choice before
editing. Do not require the user to name Spec Kit or choose individual skills.

The constitution governs project decisions; this section supplies operational guidance without
replacing its principles or the checks above. Identify conflicts explicitly and resolve them
before the affected work proceeds. Never weaken the constitution to accommodate an implementation.
Reserve `$speckit-constitution` for deliberate governance changes under its amendment procedure,
not routine tasks. Write project artifacts in English.

### Choose a Proportionate Workflow

- **Simple tasks:** Handle typos, small documentation changes, and obvious local adjustments
  through the normal workflow when behavior and compatibility are clear. Do not generate feature
  specifications, plans, or task sets solely to satisfy a process.
- **Bug fixes:** Investigate reproduction steps, expected versus observed behavior, root cause,
  and affected components before proposing a scoped correction. Verify the original reproduction
  and relevant regressions with focused automated tests where feasible, or document manual
  reproduction and verification. A narrow, understood repair may use the normal workflow.
  For bugs needing recorded diagnosis or coordinated verification, use the installed bug workflow:
  `$speckit-bug-assess` → `$speckit-bug-fix` → `$speckit-bug-test`, retaining the same bug slug.
  Review the assessment before fixing; only the fix stage changes application source. Record
  assessment, changes, and verification under `.specify/bugs/<slug>/`. Escalate to the feature
  workflow when the repair entails a contract change, broad redesign, or unresolved consequential
  decisions. Do not require a feature specification merely because a request reports a bug.
- **Features and structural changes:** Use `$speckit-specify` → `$speckit-plan` →
  `$speckit-tasks` → `$speckit-implement` → `$speckit-converge` for new capabilities or changes
  to behavior, configuration, CLI contracts, architecture, or multiple components that require
  coordinated decisions. Define observable outcomes and acceptance criteria before design;
  connect implementation tasks to the constitution's compatibility and verification requirements.
- **Contextual quality steps:** Before planning a feature, review ambiguity with
  `$speckit-clarify`; ask only questions that materially affect the result. Respect its documented
  handling of an explicit decision to skip clarification. Use `$speckit-checklist` when a domain
  needs a dedicated review of requirement completeness, clarity, or consistency. It evaluates
  requirements, not runtime correctness. For complex or higher-risk features, run
  `$speckit-analyze` after a complete `tasks.md` exists and before implementation; address critical
  conflicts before proceeding.

### Resume, Execute, and Verify

Reuse valid artifacts for the same initiative and resume at the first incomplete prerequisite
rather than recreating the workflow. Confirm the intended feature using
`SPECIFY_FEATURE_DIRECTORY` or `.specify/feature.json`; a Git branch alone does not identify the
active feature. If the feature is unclear, resolve that ambiguity before writing artifacts.
Reconcile scope changes with the specification, plan, and tasks before continuing implementation;
do not silently expand the requested work or overwrite existing artifacts.

Read each selected `.agents/skills/speckit-<name>/SKILL.md` before invoking it and follow its
prerequisites, scope, hooks, and approval requirements within the user's authorization and
applicable higher-priority instructions. Run steps separately and inspect each result before
advancing. Missing prerequisites require the appropriate earlier step, not invented artifacts
or claims of successful execution.

Respect the distinct write boundaries: `$speckit-analyze` is read-only; `$speckit-converge` may
only append remaining tasks and leaves `tasks.md` unchanged when no gaps remain. Custom
checklists belong to their reviewer: generating them does not approve them or complete
implementation. `$speckit-implement` reads checklist state without changing it and asks before
proceeding with unchecked items as its instructions require. The built-in
`checklists/requirements.md` is maintained by specify/clarify, not by implement.

After feature implementation, run `$speckit-converge`. Complete appended tasks through
`$speckit-implement` and repeat convergence while remaining work is within the authorized scope.
Report blockers or scope decisions instead of repeatedly running an unchanged failing step.
Convergence does not replace tests: follow the constitution and repository checks above, report
commands or manual steps and actual results, and distinguish artifact compliance from verified
behavior and unavailable platform coverage. Documentation-only edits require content and
consistency review rather than a Rust build.

### Installed Capabilities and Official References

Verified on 2026-10-20: Specify CLI and project integration version **1.2.2**, Codex skills mode
(`--skills`), and PowerShell scripts (`ps`). The explicitly authorized **bug extension 1.0.1**
adds assess, fix, and test skills. These are observations, not permanent version requirements.
Recheck `.specify/integration.json`, installed extensions, local skill files, session skill
availability, and `specify version` when availability or behavior may have changed.

Invoke skills in the Codex chat as `$speckit-<name>`; these are not terminal commands.
Run CLI commands such as `specify version` and applicable PowerShell scripts from the repository
root in PowerShell. If a newly installed skill is not exposed in the current session, report that
limitation and refresh the session's skill discovery before claiming it was invoked.

Do not assume that an extension described online is installed locally. If a required skill is
unavailable, explain the limitation and use a proportionate normal workflow where possible;
do not invent a command or silently bypass a required gate. Do not install, upgrade, or
reinitialize Spec Kit merely because a workflow was selected; setup changes require an explicit
request. Do not create issues automatically. The local `$speckit-taskstoissues` skill is
deprecated; its GitHub extension replacement requires separate availability and authorization
checks before use.

Consult the [official Spec Kit documentation](https://github.github.com/spec-kit/),
[SDD workflow](https://github.github.com/spec-kit/quickstart.html),
[Codex integration and invocation reference](https://github.github.com/spec-kit/reference/integrations.html),
and [bug workflow](https://github.github.com/spec-kit/guides/bugfix.html) for current guidance.
Distinguish current upstream guidance from the capabilities of this installation, and resolve
material differences explicitly instead of assuming the latest documentation matches local files.
