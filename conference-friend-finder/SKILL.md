---
name: conference-friend-finder
description: Track conference friends, keep their affiliations current, and find their main-conference poster sessions from official venue sources. Use when attending a conference and trying to locate friends' posters, updating a tracked people list, inserting new people, or switching the active conference target.
---

# Conference Friend Finder

Use this skill to maintain a tracked people list and find their poster sessions at the active conference.

## Default Project Files

This public skill must not bundle any private people list or private conference data.

Unless the user specifies other paths, use files in the user's current workspace:

- `./Friends.md` for the tracked people table
- `./VENUE.md` for the active conference

If either file is missing and the user wants to initialize it, copy the schema from:

- `assets/Friends.template.md`
- `assets/VENUE.template.md`

Always read both active project files before conference search tasks.

## General Rules

- Treat poster discovery as the primary goal of this skill.
- Search main-conference papers by default.
- Exclude workshop papers unless the user explicitly requests them.
- Prefer sources in this order:
  1. official conference website
  2. official OpenReview venue page
  3. linked paper pages or author pages
- If a paper has both oral and poster schedule entries, include both.
- Do not invent affiliations, paper matches, or schedule details.
- If affiliation, identity, name matching, surname parsing, or schedule details are ambiguous, ask the user before finalizing.

## File Contracts

The active `Friends.md` must remain a markdown table sorted by surname inferred from `Full Name`:

| Full Name | Institution / School | Last Verified |
|---|---|---|

Rules:

- `Full Name` preserves the display name the user is likely to search.
- `Institution / School` stores the clearest current primary affiliation.
- `Last Verified` uses ISO format: `YYYY-MM-DD`.

The active `VENUE.md` should stay concise and human-editable and include:

- conference name
- conference website
- OpenReview venue page
- target paper scope
- workshop exclusion rule

## Predefined Commands

### `search friends`

When the user says `search friends` or clearly asks to find tracked friends' posters at the active conference:

1. Read `Friends.md` and `VENUE.md`.
2. Extract the full author names from `Friends.md`.
3. Search the active conference sources for papers whose author list contains any of those names.
4. For each matched paper, collect:
   - paper title
   - full author list
   - matched friend name(s)
   - all official poster schedule entries
   - the oral entry too if the same paper officially has both oral and poster slots
5. For each returned schedule entry, collect:
   - date
   - time
   - location
   - presentation type if available
6. Produce a real Excel workbook with:
   - one sheet per day
   - one row per returned schedule entry
   - columns in this order:
     1. `Paper Title + Authors`
     2. `Time`
     3. `Location`
7. In `Paper Title + Authors`, place the title and author list in the same cell, separate them with a line break, and bold the matched friend names.
8. Sort each sheet by time ascending, then location ascending.
9. If sources disagree, prefer the official conference site. If the conflict remains unresolved, ask the user.

Name matching rules:

- match by full author name first
- use spacing or capitalization variants only when needed
- do not aggressively match initials-only names unless the identity is clearly supported
- avoid false positives for common names

### `update friends`

When the user says `update friends` or clearly asks to refresh the tracked affiliations:

1. Read all rows from `Friends.md`.
2. Verify whether each person's affiliation has changed.
3. Prefer current official affiliations using this priority order:
   - official personal homepage
   - official university, lab, or company profile
   - OpenReview profile
   - Google Scholar profile
   - other public sources only if higher-priority sources are unavailable
4. Update `Institution / School` and `Last Verified` where needed.
5. Rewrite the full file as a markdown table.
6. Keep the table sorted by surname inferred from `Full Name`.
7. Deduplicate entries by normalized `Full Name`.
8. Preserve names even if the affiliation cannot be confirmed.
9. If multiple active affiliations exist and the primary one is not obvious, ask the user before finalizing that row.

Output expectation:

- return the updated `Friends.md`
- briefly summarize which entries changed

### `insert friends`

When the user says `insert friends` or clearly asks to add new tracked people:

1. Parse each new author's raw information from the same message.
2. The raw information may include any combination of:
   - author name only
   - author name plus institution
   - personal homepage or profile URL
3. Use any provided institution or URL as a starting point, then verify and fill in missing fields when possible.
4. If only a homepage or profile URL is provided, infer the author's full name and current primary affiliation from that source or linked official sources.
5. If no institution is provided or the provided institution is outdated, search for the most relevant current institution.
6. If the identity or institution is ambiguous, or if the provided clues conflict, ask the user before finalizing that row.
7. Insert the new rows into `Friends.md`.
8. Deduplicate against existing rows by normalized `Full Name`.
9. Keep the file in the canonical schema.
10. Resort the full table by surname inferred from `Full Name`.
11. Set `Last Verified` to the current date for all newly inserted rows.

Output expectation:

- return the updated `Friends.md`
- briefly list the inserted names

### `update conf`

When the user says `update conf` or clearly asks to switch the active conference:

1. Read `VENUE.md`.
2. Parse the conference name and related URLs or scope information provided by the user.
3. Update `VENUE.md` with the new active conference information.
4. Keep the file concise and human-editable.
5. If a required conference field is missing or ambiguous, ask the user before finalizing.

Output expectation:

- return the updated `VENUE.md`
- briefly summarize which conference fields changed
