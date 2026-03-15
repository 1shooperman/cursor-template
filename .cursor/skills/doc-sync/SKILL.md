---
name: doc-sync
description:  Keeps the agent files up to date
disable-model-invocation: true
---

# doc-sync skill
The .cursor/rules/*.mdc, CLAUDE.md, and .github/copilot-insutructions.md files need to be kept in sync with each other, the README, and the code.

## Constraints / outputs
- only update agent related files: .cursor/rules/*.mdc, CLAUDE.md, and .github/copilot-insutructions.md. this list is exhaustive
- Capture the current system time (e.g., on Linux/macOS run `date`) and record it in your internal context for time-sensitive reasoning and web searches.
- Per recent research (12.FEB.2026, https://arxiv.org/pdf/2602.11988): Agent generated md (e.g. AGENTS.md, CLAUDE.md) files should: limit instructions to non-inferable details, such as highly specific tooling or custom build commands.