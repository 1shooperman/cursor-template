# cursor-template

Template repository for a Cursor-enabled development environment. This repo is intentionally small: it primarily exists to version and share Cursor configuration (rules + reusable commands) that you can apply to other projects.

## What’s included

- **Global Cursor rules**: `.cursor/rules/global.mdc`
  - Applied to all files (see the frontmatter `alwaysApply: true`).
  - Encodes general engineering guidance (SOLID/DRY, consistency, error handling, dependency hygiene, etc.).
  - No project-specific placeholders; project context lives in `context.mdc`.

- **Tech stack / role context**: `.cursor/rules/context.mdc`
  - Defines only the project’s tech stack and role (e.g. “You are a senior Vue.js engineer…”).
  - Kept minimal so it stays in this repo while the rest of `.cursor/` can be symlinked into `~/.cursor/` for global use.
  - A template with placeholders lives at **`context.template.mdc`** (repo root): copy it to `.cursor/rules/context.mdc` and fill in `{{SOFTWARE_STACK}}`, `{{PROGRAMMING_LANGS_FRAMEWORKS}}`, `{{AGE_AND_DESCRIPTION}}`, and `{{ADDITIONAL_DETAIL}}`.

- **Cursor agents**: `.cursor/agents/`
  - Optional agent definitions (e.g. `security-auditor.md` for security audits) that you can invoke from Cursor.

- **Reusable Cursor commands**: `.cursor/commands/`
  - `review.md`: prompts for a peer-style code review of the current diff (or last 10 commits on default branch).
  - `summarize.md`: prompts for a copy/pastable PR description snippet.

## How to use

### Usage from this repo (recommended)

Use this pattern to apply this repo’s Cursor config globally on your machine:

1. **Symlink each folder in this repo’s `.cursor/` into `~/.cursor/`**  
   From this repo’s root, symlink every folder under `.cursor/` into your Cursor user directory:

   ```bash
   cd /path/to/cursor-template
   for dir in .cursor/*/; do
     name=$(basename "$dir")
     ln -sf "$(pwd)/$dir" "$HOME/.cursor/$name"
   done
   ```

   Or manually:

   ```bash
   ln -sf /path/to/cursor-template/.cursor/agents   ~/.cursor/agents
   ln -sf /path/to/cursor-template/.cursor/commands ~/.cursor/commands
   ln -sf /path/to/cursor-template/.cursor/rules    ~/.cursor/rules
   ```

2. **Keep project-only context in this repo**  
   In this repo’s `.cursor/rules/`, keep a `context.mdc` that only defines the tech stack and role, for example:

   ```markdown
   ## Role

   You are a senior Vue.js engineer that understands full stack architecture best practices for building cloud-based applications with PHP and Vue.
   ```

   Cursor will use the symlinked rules/commands/agents from `~/.cursor/` plus this repo’s `context.mdc` for project-specific context.

### Option A: Use this repo as a template

1. Create a new repo from this template (or copy the contents into an existing repo).
2. Copy `context.template.mdc` to `.cursor/rules/context.mdc` and fill in the placeholders, or add your own `.cursor/rules/context.mdc` with your tech stack and role.
3. Optionally review or tweak `.cursor/rules/global.mdc` to match your preferences.
4. Commit the `.cursor/` directory so the rules/commands/agents are shared with your team.

### Option B: Copy into an existing repo

Copy the `.cursor/` directory into the root of your target repository:

```bash
cp -R .cursor /path/to/your/other/repo/
```

Then copy `context.template.mdc` to `.cursor/rules/context.mdc` and fill in the placeholders (or add your own context.mdc). Edit `.cursor/rules/global.mdc` if you need to adjust the shared rules for that project.

**Note**: Make sure to add `.cursor/*.log, change-summary.md, and code-review.md` to your `.gitignore`

## License

See `LICENSE`.
