---
name: appstore-update-workflow
description: Use for every request in the AppStore-updata repository, including questions, implementation, reviews, debugging, and Excel workbook changes. Load the project workflow and accumulated rules before work, then retain only durable project guidance learned from the user. Do not use outside this repository.
---

# AppStore-updata Workflow

Apply this workflow before doing any work in this repository.

## Start Every Request

1. Read [references/learned-rules.md](references/learned-rules.md) completely and apply every active rule whose scope matches the request.
2. Read [references/project-context.md](references/project-context.md) when the request depends on architecture, build commands, Excel structure, App Store Connect behavior, or file ownership. For a trivial repository question, read only the relevant section.
3. Read [references/iap-generation-rules.md](references/iap-generation-rules.md) completely for every request involving coins, coin bags, VIP memberships, recharge-item generation, Product IDs, prices, entitlements, or their Excel workbooks. Apply its source-priority and request-expansion rules before proposing or editing rows.
4. Inspect the current worktree and the files involved. Current code and tests are authoritative when this reference has become stale.
5. Identify whether the task concerns the macOS app, an Excel fixture/template, documentation, or more than one layer. Trace changes through all affected layers before editing.

## Work With The Repository

- Preserve unrelated worktree changes and keep edits within the request's scope.
- For Excel-driven behavior, check the workbook sheet and column contract, `IAPExcelParser`, the model, the upload controller, API helpers, tests, fixtures, and user documentation as applicable. Do not update only one layer when the behavior crosses several.
- Preserve workbook formatting, formulas, sheet names, column order, and unrelated rows. Validate edited `.xlsx` files as ZIP/OOXML packages and, when behavior depends on them, parse them through the app's parser or a structured workbook library.
- For a minimal product request such as a type and one price, expand all implied normal/abnormal prices, supported variants, suffixes, entitlements, linked workbook rows, and backend mappings defined by the IAP rules. Show unsupported price-point candidates and rule conflicts instead of silently omitting or guessing them.
- Treat App Store Connect operations as live external mutations. Do not upload products, change prices, submit reviews, use stored credentials, or perform releases unless the current user request explicitly authorizes that operation.
- Validate changes in proportion to risk. Prefer focused XCTest coverage, then run the `AppleParty` scheme from `AppleParty/AppleParty.xcworkspace`. Report any verification that could not be run.
- Do not commit, push, publish, or release unless explicitly requested.

## Learn Durable Rules

At the end of each request, evaluate the user's messages and any corrections made during the task. Update [references/learned-rules.md](references/learned-rules.md) in the same task only when a candidate is all of the following:

- Project-specific and likely to affect future decisions.
- Durable across more than the current request.
- Explicitly stated as an ongoing instruction, repeated across requests, or confirmed by clear repository evidence after a user correction.
- Precise enough to follow and compatible with higher-priority instructions.

Do not retain one-off deliverables, guesses, temporary state, dates that will expire, generic engineering advice, credentials, tokens, account identifiers, personal data, or conclusions unsupported by the repository. A repeated example is not automatically a general rule.

When admitting a rule:

1. Use the narrowest correct scope.
2. Add one concise active entry with the next `RULE-NNN` identifier, the rule, its basis, and the date.
3. If it conflicts with an older rule, amend or retire the older entry instead of accumulating contradictions. The newest explicit user instruction wins within the same authority level.
4. Avoid duplicating guidance already present. Update an existing entry when the meaning is the same.
5. Mention the learned-rule change briefly in the final response. If no candidate qualifies, do not edit the file and do not mention rule learning.

Update [references/project-context.md](references/project-context.md) only when verified architecture, commands, or file ownership change. Do not store user preferences there.
