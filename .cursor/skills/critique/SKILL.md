---
name: critique
description: Follow the critic pattern, get a second opinion.
disable-model-invocation: true
---

Given the alternate agent "Critic" available via mcp server here: localhost:3000 that accepts the json input: 

```json
{
  "model": "gemini-2.5-flash",
  "pipeline": "engineering-review",
  "variables": { 
    "plan": /* YOU CURRENT PLAN GOES HERE */, 
    "user_ask": /* A SUMMARY OF MY ASK OF YOU */,
    "tech_stack": /* A COMMA DELIMITED LIST OF THE TECH STACK FOR THE CURRENT REPO */
  }
}
```

Have this mcp server evaluate your plan and incorporate feedback before asking the user for input. If you disagree with any part of the feedback, ask the user follow-up questions to clarify direction.