# Convert Plan to PRD Prompt

Paste this in the **feedback field** when Claude asks to approve a plan.

---

## Prompt (copy this)

```
Don't implement yet. Convert this plan to plans/prd.json.

IMPORTANT: Plan mode creates multi-phase plans with explicit ordering.
PRD is a flat list - the LLM decides order by picking "highest priority" each iteration.
(It will naturally do foundational tasks first, like API before UI that calls it.)

Format:
{
  "features": [
    {
      "category": "functional",
      "description": "what the feature does (behavior, not implementation)",
      "steps": [
        "action to test",
        "verify expected outcome",
        "verify edge case"
      ],
      "passes": false
    }
  ]
}

Rules:
- At the top of the prd.json, add a comprehensive description of the feature.
- Flatten the plan: no explicit phases, just a list of tasks
- Focus on WHAT + behavior, not HOW to implement
- category: "functional" or "ui"
- steps = verification steps (how a human would test it works)
- CRITICAL: tasks must be SMALL (fit in one context window)
  - If a plan phase is too big, split into multiple PRD items
  - Better to have 5 small tasks than 1 big one
- All passes: false

Example task:
{
  "category": "functional",
  "description": "New chat button creates a fresh conversation",
  "steps": [
    "Navigate to main interface",
    "Click the 'New Chat' button",
    "Verify a new conversation is created",
    "Check that chat area shows welcome state",
    "Verify conversation appears in sidebar"
  ],
  "passes": false
}

After creating prd.json, exit plan mode without implementing.
```

---

## Workflow

1. Enter plan mode, describe feature
2. I explore, ask questions, write plan
3. When I ask "approve?" → paste prompt above as feedback
4. I convert plan → `plans/prd.json`
5. Run: `./plans/ralph.sh 5`
