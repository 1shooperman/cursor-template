# cursor-template

Template repository for a Cursor-enabled development environment. This repo is intentionally small: it primarily exists to version and share Cursor configuration (rules + reusable commands) that you can apply to other projects.

## What’s included

- **Global Cursor rules**: `.cursor/rules/global.mdc`
  - Applied to all files (see the frontmatter `alwaysApply: true`).
  - Encodes general engineering guidance (SOLID/DRY, consistency, error handling, dependency hygiene, etc.).
  - Contains placeholders you should customize:
    - `{{SOFTWARE_STACK}}`
    - `{{AGE_AND_DESCRIPTION}}`
    - `{{ADDITIONAL_DETAIL}}`

- **Reusable Cursor commands**: `.cursor/commands/`
  - `review.md`: prompts for a peer-style code review of the current diff (or last 10 commits on default branch).
  - `summarize.md`: prompts for a copy/pastable PR description snippet.

## How to use

### Option A: Use this repo as a template

1. Create a new repo from this template (or copy the contents into an existing repo).
2. Customize `.cursor/rules/global.mdc` placeholders to match your project and stack.
3. Commit the `.cursor/` directory so the rules/commands are shared with your team.

### Option B: Copy into an existing repo

Copy the `.cursor/` directory into the root of your target repository:

```bash
cp -R .cursor /path/to/your/other/repo/
```

Then edit `.cursor/rules/global.mdc` to match that project.

**Note**: Make sure to add `.cursor/*.log, change-summary.md, and code-review.md` to your `.gitignore`

## License

See `LICENSE`.
