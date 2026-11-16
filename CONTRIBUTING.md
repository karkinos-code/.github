# Contributing to Karkinos

Thanks for helping out! This guide covers how work flows through the repository.

## Branches

- `main` is the released, always-working branch. Nobody pushes to it directly.
- `karkinos-<crate>` branches (for example `karkinos-core`) are long-lived
  integration branches, one per crate.
- Day-to-day work happens on short-lived branches named `type/scope-description`,
  branched from the relevant `karkinos-*` branch:

  | Type       | Use for                                |
  |------------|----------------------------------------|
  | `feat`     | new functionality                      |
  | `fix`      | bug fixes                              |
  | `docs`     | documentation only                     |
  | `refactor` | restructuring without behavior changes |
  | `perf`     | performance improvements               |
  | `test`     | adding or fixing tests                 |
  | `ci`       | workflows and tooling                  |
  | `chore`    | maintenance (dependencies, cleanup)    |

  Example: `feat/integrators-boris`. Use lowercase letters, numbers, and hyphens.
  CI checks the branch name on every pull request.

## Workflow

1. Branch from the `karkinos-*` branch for the crate you are working on.
2. Open a pull request back into that `karkinos-*` branch. Squash merging is fine here.
3. When a crate branch is ready, open a pull request from `karkinos-<crate>` into
   `main`. Use a **merge commit**, not a squash, so the long-lived branch stays in
   sync with `main`.
4. After anything merges into `main`, merge `main` back into the other
   `karkinos-*` branches so changes to shared crates (especially `karkinos-core`)
   reach everyone quickly.

## Before you push

Run the same checks CI runs:

```bash
cargo fmt --all -- --check
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo test --workspace --all-features
RUSTDOCFLAGS="-D warnings" cargo doc --workspace --no-deps --all-features
```

For documentation changes, preview the site locally:

```bash
uvx --with-requirements docs/requirements.txt properdocs serve
```

## Pull requests

- Keep pull requests focused on one change.
- Every public item needs a doc comment.
- If you change a public trait or the overall structure, update
  `docs/developer/architecture.md` in the same pull request.
- At least one review from a code owner is required before merging.
