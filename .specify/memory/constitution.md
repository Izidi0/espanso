# Espanso Constitution

## Core Principles

### I. Local Operation and Privacy

Espanso MUST preserve local text expansion without requiring an account, a remote service,
or network access. Core expansion MUST NOT introduce tracking or transmit typed text,
clipboard contents, snippets, or configuration to external services. Network operations,
such as package retrieval or user-configured scripts, MUST remain explicit and separate
from the core expansion requirement. New diagnostics MUST avoid recording sensitive
content by default. This protects users whose system-wide input may contain private data.

### II. Reliable Text Expansion

Changes to detection, matching, rendering, injection, or clipboard handling MUST preserve
unrelated user input and respect configured trigger and application boundaries. Changes
to these paths MUST describe expected behavior and failure behavior, including relevant
keyboard, Unicode, timing, and application interactions. Errors MUST provide actionable
diagnostics without exposing sensitive content. Regression fixes MUST include a focused
automated check where feasible, or a documented manual reproduction and verification
when operating-system interaction prevents reliable automation. Expansion correctness is
the primary measure of success for a system-wide text expander.

### III. Simple, Modular, Cross-Platform Architecture

Implementation MUST use the existing Cargo workspace and place behavior in the crate
responsible for that concern. Shared logic MUST remain separate from platform-specific
input, injection, clipboard, IPC, and UI code. New abstractions, crates, and dependencies
MUST solve a demonstrated requirement and explain why existing components are insufficient.
Changes MUST assess Windows, macOS, and Linux impact; platform-specific limitations and
experimental Wayland behavior MUST be stated explicitly. Native API and unsafe-code changes
MUST document their safety assumptions and resource ownership. This keeps the architecture
understandable while acknowledging real operating-system differences.

### IV. Compatible, Documented User Contracts

Changes MUST preserve existing configuration, trigger semantics, CLI behavior, and package
contracts unless an intentional breaking change is documented with its user impact and
migration instructions. Changes to configuration fields, types, defaults, or accepted
values MUST update the relevant documentation and applicable files in `schemas/` together
with implementation. Authored or updated project documentation, including specifications,
plans, tasks, and governance documents, MUST be written in English. Examples MUST reflect
actual supported behavior. These rules keep upgrades predictable and documentation useful
to the wider Espanso community.

### V. Evidence-Based Verification

Every change MUST be verified in proportion to the behavior and platforms it affects.
Behavior changes MUST include focused automated tests where feasible; tests MUST check
observable behavior rather than duplicate implementation details. Changes involving
expansion, input, or UI MUST also receive relevant manual checks using
`docs/src/ch05-01-manual-testing.md`. Verification reports MUST identify commands or manual
steps, results, platform coverage, and checks that could not run. Documentation-only
changes require content and consistency review, not an application build. A successful
check on one platform MUST NOT be presented as proof of another platform's behavior.

## Technology and Project Constraints

- Rust code MUST use the stable toolchain selected by `rust-toolchain.toml` and the
  workspace dependency conventions in `Cargo.toml`.
- Formatting MUST follow `rustfmt.toml`, `.editorconfig`, and `.clang-format`. Applicable
  Clippy checks MUST pass without warnings; workspace lint inheritance MUST NOT be
  assumed for crates that do not opt in.
- Native builds MUST follow the prerequisites in
  `docs/src/ch03-00-building-and-compilation.md` and the platform feature flags in
  `.github/workflows/ci.yml`. Cross-compilation MUST NOT be assumed to work; Windows
  application builds MUST use the documented native Windows toolchain.
- Changes MUST preserve the repository's GPL-3.0 license notices and attribution to
  Federico Terzi and other contributors. Dependency additions MUST be reviewed for
  license compatibility, maintenance impact, and platform requirements.
- User-facing configuration guidance MUST remain consistent with the official Espanso
  documentation. Reusable expansion packages MUST follow the existing package conventions;
  Espanso Hub remains the referenced package catalog.

## Development Workflow and Quality Gates

1. Before editing, inspect applicable `AGENTS.md` instructions, the current branch,
   pending changes, and affected components. Preserve unrelated work and keep scope focused.
2. Specifications MUST state user-visible outcomes and acceptance criteria. Plans MUST
   include a constitution compliance check, affected crates and platforms, compatibility
   impact, and verification strategy. Tasks MUST connect implementation to those checks.
   A small fix need not create a full specification set unless its workflow requires one.
3. Implement the smallest coherent change in the appropriate crate. Update affected
   documentation and schemas in the same change, and explain non-obvious tradeoffs.
4. For code changes, run `cargo test -p <affected-crate>` where supported, followed by
   applicable formatting, build, and Clippy checks from the repository root. Use
   `cargo fmt --all -- --check`, `cargo build`, and
   `cargo clippy --workspace -- --deny warnings` with platform adjustments required by CI.
   The CI workflow defines the applicable platform matrix: Linux tests exclude
   `espanso-modulo` and `espanso-ipc`; Windows CI currently formats, builds, and runs
   Clippy without a test step. Missing CI coverage does not replace relevant local
   verification. Record unavailable checks and the remaining verification needed.
5. Before integration, review constitution compliance, user impact, compatibility,
   documentation, and verification evidence. Pull requests MUST describe behavior changes
   and check results, link relevant issues when available, and include screenshots when
   useful for UI review. Do not claim unverified results or discard user work to pass checks.

## Governance

This constitution defines project governance for this checkout. Feature specifications,
plans, tasks, and implementation decisions MUST comply with it. `AGENTS.md` supplies
operational contributor guidance; conflicts between project documents MUST be identified
and resolved explicitly before the affected change is integrated. This document does not
claim adoption by upstream Espanso maintainers.

Amendments MUST identify the rule being changed, explain the rationale and compatibility
impact, describe migration or follow-up work, and update the version and amendment date.
They MUST be reviewed and accepted by the project owner or maintainers before integration.
The initial ratification date records adoption in this checkout and MUST remain unchanged
by later amendments.

Constitution versions MUST follow semantic versioning: MAJOR for incompatible principle
removals or redefinitions, MINOR for added principles or materially expanded guidance,
and PATCH for clarifications that do not change obligations. This version is independent
of the Espanso application version.

Reviewers MUST check compliance for each change. Any proposed exception MUST name the
affected rule, provide a concrete justification, define its scope and remaining checks,
and receive explicit owner or maintainer acceptance before integration. Repeated exceptions
MUST prompt a review of the governing rule rather than become undocumented practice.

**Version**: 1.0.0 | **Ratified**: 2026-10-20 | **Last Amended**: 2026-10-20
