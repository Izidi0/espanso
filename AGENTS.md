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
