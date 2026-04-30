---
name: reimbursement
description: Organize reimbursement folders, receipts, supporting PDFs, and Excel summaries for research, travel, grants, events, purchasing, and other reimbursable projects. Use when preparing or auditing reimbursement materials, especially projects with 0_Aux, food, and transportation_accomodation folders, and when the user asks to run org receipts or sum excel.
---

# Reimbursement Organizer

## Scope

Use this skill to organize reimbursement evidence for research, travel, grants, events, purchasing, visitor programs, collaboration projects, or other reimbursement packages. Follow any local `AGENTS.md`, funder, institution, or project-specific instructions first.

Keep the skill itself public and reusable. Do not add personal names, emails, addresses, bank details, private local paths, or specific private travel details to this skill repository.

## Default Layout

Use these default folders unless local instructions define a different structure:

```text
0_Aux/
food/
transportation_accomodation/
```

- `0_Aux/`: official rules, terms and conditions, approval letters, reimbursement templates, unit-cost decisions, and other reference files. Do not put ordinary receipts here.
- `food/`: supermarket, grocery, canteen, restaurant, and other meal or beverage receipts. Do not assume these are reimbursable; check the rules or flag them for confirmation.
- `transportation_accomodation/`: travel, local transport, accommodation invoices, contracts, booking confirmations, and payment proofs.

Other folders may be user-created project structure. Do not move, rename, or reorganize user-created folders outside the default categories or their obvious variants unless the user explicitly asks you to modify those folders.
Treat obvious variants such as `transport/`, `transportation/`, `travel/`, `accommodation/`, `accomodation/`, `hotel/`, `meals/`, or `food_receipts/` as category folders when the mapping is clear.
Additional category folders such as `registration/`, `supplies/`, `equipment/`, `services/`, or `other/` are allowed when the reimbursement project needs them, but do not repurpose an existing user-named folder for these categories unless the folder is clearly named for that category or the user explicitly says so.

`output/` is not required initially. Create it only for generated summaries or filled forms.

At the project root, keep a `readme.md` for project-specific instructions. If it is missing during `org receipts`, create it with `Use $reimbursement` and a `# Customized Requirement` section for the user to fill later.

If filling a reimbursement form, copy the template from `0_Aux/` before editing. Never modify the original template in `0_Aux/`.

## Workflow

1. Read local instructions: root `AGENTS.md`, project `readme.md` or `README.md`, funding-specific `AGENTS_*.md`, and any user-provided reimbursement guidance.
2. Inspect the folder tree with `rg --files` or `find`; create missing default folders and `readme.md` when running `org receipts`.
3. Read the official rules and forms in `0_Aux/` enough to identify eligible categories, deadlines, required proof, limits, and exclusions.
4. Build an inventory of receipts: file, category, date or period, amount and currency, vendor, route or stay period, proof of payment, and possible excluded line items.
5. Create reimbursement-ready copies or merged PDFs when useful, while preserving originals unless the user asks for in-place renaming.
6. For reimbursement forms or Excel summaries, enter each actual expense once. Use comments to point to the supporting PDF and capture date or period, amount, currency, purpose, included source documents, and any eligibility caveats.
7. Keep uncertain or excluded costs out of reimbursable totals unless the rules clearly allow them. When creating an Excel summary, put unresolved eligibility items in a separate open-questions sheet.

## Naming Rules

Name receipt files as:

```text
[class]-[date]-[note].[ext]
```

- `class`: use `food`, `transportation`, or `accommodation`.
- For other reimbursement categories, use a clear lowercase class such as `registration`, `supplies`, `equipment`, `services`, or `other`.
- `date`: use `YYYY-MM-DD` for a one-day receipt, ticket, invoice, or trip.
- `date`: use `YYYY-MM-DD_to_YYYY-MM-DD` when the receipt covers multiple days, a stay period, a ticket validity period, or a bundle of receipts across a range.
- Prefer the service, travel, stay, or receipt date over booking or payment date. If only the invoice or payment date is available, use that date and mention the uncertainty in the Excel `Comments` column when creating a summary.
- `note`: optional, lowercase English words joined with hyphens. Keep it short and descriptive.

Examples:

```text
food-2026-01-15-canteen-receipt.pdf
food-2026-01-01_to_2026-01-31-grocery-receipts.pdf
transportation-2026-01-10-train-ticket-seat-reservation.pdf
transportation-2026-02-01_to_2026-02-28-local-public-transport.pdf
accommodation-2026-01-10_to_2026-01-24-hotel-invoice.pdf
```

## Eligibility Checks

- Do not promise that a cost is reimbursable. The official rules and reimbursement office, funder, or project administrator decisions override the folder category.
- For travel or long-term stays, reimbursement offices may cover only the journey to and from the start and end of the stay. Local monthly passes and daily local transport during the stay may need confirmation.
- Meal, grocery, restaurant, canteen, laundry, and other daily-living expenses may be excluded for long-term stays. If rules or office feedback exclude them, keep the evidence but remove them from reimbursable totals.
- If an invoice contains both eligible and ineligible line items, include the invoice as support but subtract excluded items in the reimbursement calculation.
- Do not claim the same expense under multiple funding streams.

## Commands

### `org receipts`

Initialize missing structure and organize receipt files in the reimbursement project.

1. Ensure `0_Aux/`, `food/`, and `transportation_accomodation/` exist. Create missing folders.
2. Ensure root `readme.md` exists. If missing, create it with:

   ```md
   Use $reimbursement

   # Customized Requirement

   ```

   If `readme.md` already exists, preserve the user's content. Only add `Use $reimbursement` or the `# Customized Requirement` section when they are missing.
3. Use the project requirements from `readme.md`, `AGENTS.md`, official rules, and user instructions to map files into categories. If no custom requirement exists, use the default folders, their obvious variants, and allowed additional category folders.
4. Inspect the project root, default category folders, and obvious category variants. Move clearly identifiable reimbursement evidence from those locations into the appropriate category folder and create a suitable category folder when the project requirement or root-level file content calls for it.
5. Leave `0_Aux/` for rules, approvals, templates, and official reference files only. Move clearly identifiable rules, approvals, templates, and reference files there; do not put ordinary receipts in `0_Aux/`.
6. Do not move, rename, or reorganize unrelated user-created folders. If an arbitrary user-created folder contains possible reimbursement evidence, leave it in place and report it as a candidate for user confirmation.
7. For every receipt or supporting reimbursement file under category folders, rename it according to `[class]-[date]-[note].[ext]`.
8. Avoid overwriting files. If a normalized target filename already exists, add a short disambiguating suffix such as `-2`, `-invoice`, or `-payment-proof`.
9. Preserve ambiguous files. If a file cannot be categorized confidently, leave it in place and report what needs user confirmation.

### `sum excel`

Create an Excel reimbursement summary in `output/`, following the structure of a general reimbursement package.

1. Create `output/` if missing.
2. Write a workbook such as `output/reimbursement-summary.xlsx`. If the workbook already exists, overwrite it.
3. Include a main expense sheet with columns:
   - `Nr.`
   - `Date`
   - `Category`
   - `Name`
   - `Amount in currency`
   - `Currency`
   - `Comments`
   - `Supporting file`
4. Use `Comments` for concrete explanations, such as the purpose, route, stay period, validity period, payment proof, or why multiple source files support one expense.
5. Use one row per actual expense. Do not double-count ticket, invoice, confirmation, and payment proof files that support the same cost.
6. Sort the main expense sheet before assigning `Nr.`: first by `Category`, then by `Date` ascending. Use the category order `交通`/transportation, `住宿`/accommodation, `meal`/food, `其他`/other, while preserving the project's preferred category labels.
7. Add a compact summary sheet with totals by category and, when relevant, a separate list of open questions or items that may need eligibility confirmation.
8. Do not modify templates in `0_Aux/` while creating the summary.
