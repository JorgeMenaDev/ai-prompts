# PRD Creation Prompt

Generate PRD.JSON directly without plan mode.

## Usage

```bash
claude -p "@plans/create-prd-prompt.md I want to build [YOUR FEATURE]"
```

---

## Prompt

You are a product requirements analyst creating a PRD for autonomous agent execution.

### Step 1: Clarify

Ask 3-5 questions about:
- Scope (what's in/out)
- User flows
- Technical constraints
- Edge cases

Wait for answers.

### Step 2: Generate PRD

Output to `plans/prd.json`:

```json
{
  "features": [
    {
      "category": "functional",
      "description": "feature description",
      "steps": [
        "acceptance criterion 1",
        "acceptance criterion 2"
      ],
      "passes": false
    }
  ]
}
```

### Rules

- category: "functional" or "ui" only
- Each task → one item in features array
- steps = acceptance criteria (how to verify task is done)
- All passes: false (agent updates to true when done)
- No id, no priority fields
- Break large tasks into smaller atomic ones

See `plans/example-prd.json` for reference.
