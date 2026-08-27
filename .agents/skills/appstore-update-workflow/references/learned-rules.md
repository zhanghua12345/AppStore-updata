# Learned Project Rules

This is the durable rule store for future Codex work in this repository. The workflow skill owns admission, deduplication, amendment, and retirement of entries.

## Active Rules

### RULE-001

- Scope: Every Codex request in this repository.
- Rule: Load and apply the project workflow and active learned rules before starting the requested work.
- Basis: Explicit user instruction requesting a rule-first workflow for every Codex question.
- Added: 2026-08-27.

### RULE-002

- Scope: Rule maintenance for this repository.
- Rule: After each request, independently screen the user's durable project guidance and persist only rules that meet the workflow's admission criteria.
- Basis: Explicit user instruction to let Codex filter and add rules over successive questions.
- Added: 2026-08-27.

### RULE-003

- Scope: Coins, coin bags, VIP memberships, recharge items, and their Excel workbooks.
- Rule: Treat a minimal product request as a request for the complete connected plan: infer all unambiguous normal/abnormal price pairs, supported price variants, suffixes, entitlements, linked workbook rows, and backend mappings from the maintained IAP rules. Validate every candidate against exact price points, and report unsupported candidates or conflicts instead of silently omitting or guessing them.
- Basis: Explicit user instruction that a request such as a `9.99` coin bag should automatically cover normal and abnormal recurring prices, nearby variants, `_1`/other suffix logic, and all connected logic.
- Added: 2026-08-27.

### RULE-004

- Scope: Operator-facing workflows for this repository.
- Rule: Prefer a double-clickable, no-Xcode entry point for routine preparation and guidance. Keep a static HTML console offline and credential-free, and clearly distinguish it from live App Store Connect or operations-backend uploads, which require the compiled app or an authorized local backend.
- Basis: Explicit user preference to operate through an `index.html`-like interface instead of opening Xcode.
- Added: 2026-08-27.

## Retired Rules

Keep superseded entries here only when their history is useful. Record the replacement rule ID and retirement date. There are no retired rules yet.
