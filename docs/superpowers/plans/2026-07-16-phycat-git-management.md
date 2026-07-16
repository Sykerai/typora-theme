# Typora Themes Git Management Implementation Plan

> **Execution note:** Follow the steps in order. Do not delete legacy backups until the baseline commit and repository checks succeed.

**Goal:** Replace per-change `.bak-*` files with local Git history for the complete Typora `themes` directory.

**Scope:** Initialize a standalone repository directly in the actual `themes` directory. Track every theme and bundled resource plus `.gitignore`. No remote is configured and nothing is pushed.

---

## Task 1: Prepare repository metadata

- Verify the root has no repository and identify any accidentally nested repository.
- Safely move the accidental `phycat` repository metadata aside during migration.
- Create a staged `.gitignore` containing `*.bak-*` so legacy backups cannot enter the baseline commit.
- Copy it into the actual `themes` directory.

## Task 2: Establish the recoverable baseline

- Initialize the actual `themes` directory as a Git repository on branch `main`.
- Stage all non-ignored files.
- Commit them as `chore: establish Typora themes baseline`.
- Confirm the commit exists, every non-backup theme file is tracked, the worktree is clean, and no remote is configured.

## Task 3: Remove legacy backups

- Enumerate files matching `*.bak-*` below the resolved `themes` directory.
- Validate every deletion target remains inside that exact directory tree.
- Delete those validated files only after Task 2 succeeds.

## Task 4: Verify and record the migration

- Confirm zero `*.bak-*` files remain.
- Confirm all tracked theme files remain unchanged relative to the baseline commit.
- Run repository integrity and worktree checks.
- Remove the displaced nested repository metadata after the root repository passes verification.
- Record the baseline commit and migration result in the existing Phycat maintenance log.

## Future workflow

- Continue staging edits under `D:\史永康\Documents` and applying them to the actual theme files.
- After validation, commit each requested change directly in the root `themes` repository.
- Do not create new per-change `.bak-*` files unless the user explicitly asks for one.
