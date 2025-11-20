You MUST conduct a complete research phase BEFORE producing the plan. The final response visible to the user must ONLY follow the Output Format specification (no extra prose, no research dump). Perform all research internally; summarize findings implicitly through the chosen changes.

### Research Methodology (Internal Only – DO NOT OUTPUT VERBATIM)

1. Scope & Intent
   - Restate THE TASK internally; define in/out of scope boundaries.
2. Codebase Discovery
   - Perform semantic searches for concepts, then exact searches for filenames, symbols, hooks, components, types, query keys.
   - Target areas: existing features, relevant React Query hooks, API client usage (`api`, `safeApiCall`), reusable UI components, types in `@/types`, zod schemas, config under `/src/lib`, `/src/features`, `/src/components`.
3. Evidence Collection
   - For each relevant file encountered: note path + purpose internally; DO NOT output full quotes.
   - Prefer reuse over creating new artifacts.
4. Pattern Extraction
   - Identify established patterns (React Query key style, component composition, naming conventions, error handling via `safeApiCall`).
5. Gap Analysis & Reuse Matrix
   - Decide what must be added vs what can be reused; prefer minimal footprint.
6. Validation
   - Ensure no hallucinated files/components; only list actual existing paths in the plan.
7. Clarifications
   - If critical ambiguity exists, ask targeted questions with options (a/b/c) and STOP. Otherwise proceed.

### Non-Negotiable Rules (Enforced Internally)

- Never invent API client names: ONLY `api`, `safeApiCall`.
- All API calls use pattern: `safeApiCall(api.method<ApiResponse<Type>>(url))`.
- `ApiResponse` is global; do not import it.
- Use `'use client'` for any new API/hook file.
- No `useEffect` for data fetching (React Query only).
- Reuse existing components/hooks before creating new ones.
- Desktop-only; no mobile/responsive requirements.
- No toasts unless explicitly requested.
- Keep solution minimal, coherent, componentized.

### Output Constraints

After research is complete, output ONLY the sections defined below. Do NOT output the research details. Do NOT add extra sections, commentary, or prefatory text. Keep each line concise. Avoid paragraphs longer than two lines.

---

## OUTPUT FORMAT (VISIBLE TO USER)

```
Plan: <Short Imperative Title>

Changes Required:
1. <change #1>
2. <change #2>
3. ... (Keep total ≤10)

Files to Modify (N):
- path/one – very short change
- path/two – very short change

Files to Create (M):
- path/new-file – purpose
If none: None

Implementation Steps:
1. Verb-led action (single responsibility)
2. ... (≤10 steps total, each ≤1–2 lines)

Expected Result:
- Bullet outcome 1
- Bullet outcome 2
- Bullet outcome 3
- Bullet outcome 4 (optional)
- Bullet outcome 5 (optional)

Estimated Time: <range>

Would you like to proceed?

❯ 1. Yes, and auto-accept edits
   2. Yes, and manually approve edits
   3. No, keep planning
```

### Additional Formatting Rules

- Use backticks for file paths ONLY if needed for clarity; otherwise plain text is fine.
- Do not include code snippets unless absolutely required for disambiguation (prefer not to).
- No analysis sections, no risk blocks, no verbose research output.
- If ambiguity prevented a plan: output only clarification questions (each with options a/b/c + recommendation) instead of the plan format.
- Ensure all listed files actually exist (except legitimately new ones in Files to Create).
- Keep tone neutral, direct, implementation-focused.


—————————
THE TASK:
<task_details>

<task_details>
