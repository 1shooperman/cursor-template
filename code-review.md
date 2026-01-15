# Code Review

## Scope reviewed

- Working tree diff on `main`: `README.md`
- Untracked files: `.cursor/` (commands, rules, and `debug.log`)
- Repo commit history: 1 commit (`Initial commit`)

## Summary

- The updated `README.md` is a clear improvement and matches the current intent of the repo (Cursor configuration + reusable commands).
- The biggest concern is that `.cursor/debug.log` looks like real project debug output and is not appropriate to commit into a “template” repo.

## Findings

### Must-fix (before committing)

- **Don’t commit `.cursor/debug.log`**
  - It contains JSON log lines referencing unrelated app files (e.g. `CompleteUploadProfile.vue`, `lazyLoadCropper.js`) and timestamps.
  - Recommendation: remove it from the repo and add a `.gitignore` entry (or equivalent) so it can’t be accidentally committed again.

### Should-fix (polish / correctness)

- **Typo in `.cursor/rules/global.mdc` placeholder**
  - The line `This is an {{AGE_AND_DESCRIPTION}}} application...` has an extra `}`.

- **Minor typos**
  - “never user the --legacy-peer-deps flag” → “never use the --legacy-peer-deps flag”
  - “updated this document…” should probably be “Update this document…” (imperative) if it’s an instruction.

### Nice-to-have

- **README: mention what *not* to include**
  - A short note like “Don’t commit Cursor debug logs / local artifacts” could prevent future noise in this repo.
  - Not required, but helpful given the presence of `debug.log`.

## Test / verification notes

- No runtime behavior changes; doc-only change in `README.md`.
- Verification is visual: ensure the README renders cleanly on GitHub and the paths match the repo structure.

