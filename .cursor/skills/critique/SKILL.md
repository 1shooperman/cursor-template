---
name: critique
description:  Use this skill before presenting any implementation plan to the user. Invoke the Critic MCP server to get an independent engineering review of the plan, then critically evaluate the feedback before responding.
disable-model-invocation: false
---

# Critic Plan Evaluation

Before presenting an implementation plan to the user, send it to the Critic MCP server for an independent engineering review. Do not skip this step.

## When to invoke

- You have a plan — even a provisional or iterative one — for an implementation, architecture, refactor, or feature
- You are about to present that plan to the user
- Plans don't need to be fully baked; a rough directional plan is worth reviewing early

## How to invoke

The Critic MCP server exposes its tools via HTTP at `http://localhost:3000/mcp`. Use Bash with curl (the MCP tools do not surface in Claude Code's tool registry). Use a 5-minute timeout.

```bash
curl -s \
  -H "Accept: application/json, text/event-stream" \
  -H "Content-Type: application/json" \
  -X POST http://localhost:3000/mcp \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
      "name": "critique_pipeline",
      "arguments": {
        "model": "gemini-2.5-flash",
        "pipeline": "engineering-review",
        "variables": {
          "plan": "<your full plan>",
          "user_ask": "<summary of what the user asked>",
          "tech_stack": "<comma-delimited tech stack>"
        }
      }
    }
  }'
```

All `variables` values must be strings. The response is SSE-framed JSON; extract the `.result.content[0].text` field.

For the Wethos project the baseline tech stack is: Vue 3, Vuex 4, Vue Router 4, Laravel 11, PHP 8.2, PostgreSQL, Redis, Stripe, Auth0, Cypress, Jest, PHPUnit. Extend with any additional context relevant to the specific task.

## How to handle the response

The pipeline runs multi-stage adversarial review. The final output includes a **"Final Verdict for Agent Consumption"** section — prioritize that. Evaluate the feedback on these dimensions:

**Incorporate** feedback that identifies:
- Missing edge cases or failure modes in the plan
- Architectural risks or coupling issues
- Scope gaps relative to what the user asked for
- Concrete alternatives that are simpler or more aligned with existing patterns

**Challenge** feedback that:
- Contradicts known project constraints (conventions, existing architecture, agreed scope)
- Demands implementation detail at a conceptual planning stage (premature over-specification)
- Suggests over-engineering beyond what the task requires
- Is based on assumptions that don't hold for this codebase

**Flag for the user** any point where you genuinely can't determine who is right — present both positions briefly and ask for direction.

**Discard silently** feedback that is clearly irrelevant, nitpicky, philosophical debate, or outside the scope of the ask.

**If the server is unreachable:** proceed without Critic review and note it briefly in your response.

## Output format

After processing Critic's feedback, present to the user:
- Your (potentially revised) plan
- A brief note on any meaningful changes you made based on Critic feedback, or any points where you disagreed and why
- Any questions you need answered before proceeding
