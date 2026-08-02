# Rust Reproducibility CI

This repository runs nightly reproducibility checks for the [Rust compiler](https://github.com/rust-lang/rust) using the [`repro-check`](https://github.com/rust-lang/rust/pull/149888) tool.

## What it does

Every night (or on manual trigger), this workflow:

1. Fetches the latest `rust-lang/rust` (or a branch/tag you choose)
2. Applies the `repro-check` patch from [PR #149888](https://github.com/rust-lang/rust/pull/149888)
3. Builds the Stage 2 compiler **twice** in separate directories
4. Compares the outputs byte-for-byte
5. Generates an HTML report of any differences

## Manual Trigger Options

Click **Actions → Rust Reproducibility Check → Run workflow** to:

- Select any upstream **branch or tag** (e.g. `main`, `1.79.0`, `beta`)
- Toggle **download-ci-llvm** (`true` = fast, `false` = hermetic but slow)
- Enable/disable **auto-bisect** on failure

## Artifacts

Each run produces:

| Artifact | Description | Retention |
|----------|-------------|-----------|
| `repro-check-&lt;sha&gt;-&lt;run_id&gt;` | HTML report, build log, metadata | 7 days |
| `repro-bisect-&lt;sha&gt;-&lt;run_id&gt;` | Bisect log (only on failure) | 7 days |

## Self-Hosted Runner

This runs on a private server (`repro-check` label) because building Rust Stage 2 twice requires ~50–100 GB disk and several hours of CPU.
