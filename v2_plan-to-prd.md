A complete multi-phase plan already exists in the current context (ref:  ~/.claude/plans/transient-sniffing-lemur.md).
That plan is authoritative and final.

🚫 CRITICAL CONSTRAINT

You must NOT:

re-plan

refine the plan

question the plan

suggest alternatives

add scope

remove scope

ask clarifying questions

Your task is pure transformation only.

Your task

Convert the existing multi-phase plan into a flat, execution-ready plans/prd.json file.

This PRD will be consumed by an automated loop that:

executes one feature at a time

selects work based on priority, blockers, and effort

commits each feature independently

Therefore:

No phases

No ordering

No implementation details

No new ideas

Output

Create plans/prd.json only.

Do not implement anything.
Do not propose improvements.
After writing the file, exit plan mode immediately.

PRD JSON structure
{
  "feature_overview": {
    "problem": "",
    "user_story": "",
    "happy_path": [],
    "error_states": [],
    "edge_cases": [],
    "non_goals": [],
    "assumptions": []
  },
  "features": [
    {
      "id": "PRD-001",
      "category": "functional",
      "priority": "P1",
      "effort": "S",
      "blocked_by": [],
      "description": "",
      "success_criteria": [],
      "steps": [],
      "passes": false
    }
  ]
}

Transformation rules (STRICT)
1. Treat the plan as frozen

Every PRD item must map directly to something already present in the plan

Do not invent features

Do not collapse or expand scope

If something is unclear, represent it as-is

2. Flatten the plan

Remove all explicit phases

Convert each meaningful unit of work into a small PRD item

If a phase is large:

split it into multiple PRD items

each item must be independently executable

3. Feature overview (derived, not invented)

Populate feature_overview by summarizing the plan, not rethinking it:

problem: what the plan is solving

user_story: who benefits and why

happy_path: main flow implied by the plan

error_states: failures explicitly mentioned in the plan

edge_cases: edge cases explicitly mentioned in the plan

non_goals: anything the plan explicitly excludes

assumptions: assumptions stated or implied by the plan

⚠️ Do not add new assumptions.

4. PRD items (features array)

Each PRD item must include:

Required fields

id: stable identifier (PRD-001, PRD-002, …)

category: "functional" or "ui"

priority:

P0 = core / blocking

P1 = important

P2 = optional

effort:

S = small

M = medium

L = large (split if possible)

blocked_by:

encode dependencies already implied by the plan

use PRD ids when possible

description: WHAT the system does (behavior only)

success_criteria: observable outcomes explicitly implied by the plan

steps: how a human would verify it works

passes: always false

5. Verification steps

Steps must be testable by a human

Prefer Given / When / Then

Only include behaviors mentioned or implied by the plan

Do not “improve” test coverage beyond the plan

6. Task size constraints

Each PRD item must:

fit in one LLM execution context

touch one primary surface area

avoid compound responsibilities

If something is too large → split it.

7. Ambiguity handling (NO DECISIONS)

If the plan contains ambiguity:

Create a PRD item titled "Decision: …"

Reflect the ambiguity faithfully

Do not resolve it

Keep passes: false

8. Defaults

All passes = false

Default priority = P1

Default effort = S

Default blocked_by = []

Final instruction

After creating plans/prd.json:

Do not implement.
Do not ask questions.
