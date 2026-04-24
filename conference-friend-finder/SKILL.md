---
name: conference-friend-finder
description: Track conference friends and research topics, keep affiliations and topic lists current, and export official main-conference schedule entries from venue sources. Use when attending a conference and trying to locate tracked friends' poster or oral sessions, find papers related to tracked research topics, update tracked people or topics, or switch the active conference target.
---

# Conference Friend Finder

Use this skill to maintain tracked people and research-topic lists, then find official main-conference schedule entries at the active conference.

## Default Project Files

This public skill must not bundle any private people list or private conference data.

Unless the user specifies other paths, use files in the user's current workspace:

- `./Friends.md` for the tracked people table
- `./TOPICS.md` for tracked research topics and search terms
- `./VENUE.md` for the active conference

If a file is missing and the user wants to initialize it, copy the schema or starter content from:

- `assets/Friends.template.md`
- `assets/TOPICS.template.md`
- `assets/VENUE.template.md`

Before conference search tasks, always read `VENUE.md` and the task-specific list: `Friends.md` for friend searches, `TOPICS.md` for topic searches.

## General Rules

- Search all main-conference papers by default.
- Exclude workshop papers unless the user explicitly requests them.
- Prefer sources in this order:
  1. official conference website
  2. official OpenReview venue page
  3. linked paper pages, arXiv/project pages, or author pages when official sources lack needed detail
- If a paper has both oral and poster schedule entries, include both.
- Do not invent affiliations, paper matches, topic matches, or schedule details.
- If affiliation, identity, name matching, topic relevance, surname parsing, or schedule details are ambiguous, ask the user before finalizing.

## File Contracts

The active `Friends.md` must remain a markdown table sorted by surname inferred from `Full Name`:

| Full Name | Institution / School | Last Verified |
|---|---|---|

Rules:

- `Full Name` preserves the display name the user is likely to search.
- `Institution / School` stores the clearest current primary affiliation.
- `Last Verified` uses ISO format: `YYYY-MM-DD`.

The active `TOPICS.md` must remain a categorized markdown bullet list of research topics and query variants:

- Keep a top-level `Last updated: YYYY-MM-DD` line.
- Use second-level headings (`## ...`) for broad categories.
- Under each category, use bullets for topic labels.
- Under each topic bullet, include nested bullets for `Search terms:` and `Scope:`.
- `Search terms:` should use semicolon-separated phrases, including common aliases and closely related technical terms.
- `Scope:` should describe the intended scope and any false-positive cautions.
- Prefer precise technical terms over generic words. Generic terms such as `metric`, `geometry`, or `brain` are not sufficient evidence by themselves.
- Deduplicate by normalized topic label; merge useful variants rather than creating near-duplicate bullets.

The active `VENUE.md` should stay concise and human-editable and include:

- conference name
- conference website
- OpenReview venue page
- target paper scope
- workshop exclusion rule

## Predefined Commands

### `search friends`

When the user says `search friends` or clearly asks to find tracked friends' main-conference sessions at the active conference:

1. Read `Friends.md` and `VENUE.md`.
2. Extract the full author names from `Friends.md`.
3. Search the active conference sources for papers whose author list contains any tracked name.
4. For each matched paper, collect:
   - paper title
   - full author list
   - matched friend name(s)
   - all official main-conference schedule entries for that paper
5. For each returned schedule entry, collect:
   - date
   - presentation type if available
   - time
   - location
   - for poster entries, keep both the room and poster-board position when the official site provides both
   - time in the conference venue's local timezone, even if the page is currently rendered in a different viewer timezone
6. Produce a real Excel workbook with:
   - one sheet per day
   - one row per returned schedule entry
   - both entries if a paper has an oral slot and a poster session slot
   - save the final workbook directly under `outputs/`
   - name the final workbook as `Venue-Year-friends.xlsx`, for example `ICLR-2026-friends.xlsx`
   - if `outputs/` already contains a file with the same name, overwrite it directly
   - columns in this order:
     1. `Paper Title + Authors`
     2. `Type`
     3. `Time`
     4. `Location`
7. In `Paper Title + Authors`, place the title and author list in the same cell, separate them with a line break, and bold the matched friend names.
8. Sort each sheet by time ascending, then location ascending.
9. If sources disagree, prefer the official conference site. If the conflict remains unresolved, ask the user.

Name matching rules:

- Match by full author name first.
- Use spacing or capitalization variants only when needed.
- Do not aggressively match initials-only names unless the identity is clearly supported.
- Avoid false positives for common names.

### `search topics`

When the user says `search topics` or clearly asks to find active-conference papers related to tracked research topics:

1. Read `TOPICS.md` and `VENUE.md`.
2. Extract all topic labels and search terms from the categorized bullet list.
3. Search the active conference sources for main-conference papers whose title, abstract, keywords, author-provided topic tags, or linked official paper page clearly match any tracked topic.
4. Use official conference search and paper pages first. Use the official OpenReview venue page next, then linked arXiv or project pages only when the official conference source lacks enough title or abstract detail.
5. For each matched paper, collect:
   - paper title
   - full author list
   - matched topic label(s)
   - the title, abstract, or keyword terms that support the match
   - all official schedule entries for that paper
6. For each returned schedule entry, collect:
   - date
   - presentation type if available
   - time
   - location
   - for poster entries, keep both the room and poster-board position when the official site provides both
   - time in the conference venue's local timezone, even if the page is currently rendered in a different viewer timezone
7. Produce a real Excel workbook with:
   - one sheet per day
   - one row per returned schedule entry
   - both entries if a paper has an oral slot and a poster session slot
   - save the final workbook directly under `outputs/`
   - name the final workbook as `Venue-Year-topics.xlsx`, for example `ICLR-2026-topics.xlsx`
   - if `outputs/` already contains a file with the same name, overwrite it directly
   - use the same four columns and formatting style as `search friends`
   - do not add extra columns for topic labels unless the user explicitly requests them
8. In `Paper Title + Authors`, place the title and author list in the same cell and separate them with a line break. Bold matched topic terms when they appear in the paper title; if the match is supported only by abstract or keywords, no bolding is required.
9. Sort each sheet by time ascending, then location ascending.
10. If sources disagree, prefer the official conference site. If the conflict remains unresolved, ask the user.

Topic matching rules:

- Match high-signal technical phrases first, including exact phrases and common aliases from `TOPICS.md`.
- Include spacing, hyphenation, capitalization, and plural variants when needed.
- Do not include papers solely because they contain a generic word like `metric`, `geometry`, `manifold`, `transport`, `brain`, or `signal`.
- EEG is an explicit tracked application area: include papers that mention `EEG` or `electroencephalography` even if no geometry method is used.
- For broad geometry terms, require supporting context such as Riemannian, differential, hyperbolic, Lorentz, Wasserstein, information geometry, SPD, Grassmannian, Lie group, optimal transport, or closely related mathematical geometry language.
- For application topics, include papers where a geometry, manifold, metric, Wasserstein/OT, hyperbolic, SPD, Grassmannian, Lie group, or information-geometric method is applied to a concrete domain or task.
- If a paper is borderline but likely relevant, ask the user before finalizing rather than silently including it.

### Shared Excel Formatting Rules

Location formatting:

- Keep the `Location` column as the physical location only; do not prepend presentation type there.
- For poster entries with both fields available, format as `Pavilion 3, P3-#122`.
- For non-poster entries, use the official room or location text shown by the conference site.

Time formatting:

- Keep the `Time` column in the conference local timezone.
- If an official page is rendered in the viewer's timezone, convert it back to the venue local timezone before writing the workbook.

Type formatting:

- Keep the `Type` column explicit for each schedule entry, using official labels such as `Oral` or `Poster`.
- Do not merge the presentation type into the `Location` column.

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

### `update topics`

When the user says `update topics` or clearly asks to refresh tracked research topics:

1. Read all topics from `TOPICS.md`.
2. Parse any topic names, keywords, paper titles, homepage URLs, profile URLs, or notes provided by the user in the same message.
3. If the user provides no additional topic information, use Ziheng Chen's homepage (`https://gitzh-chen.github.io`) as the default source, especially the research overview, selected publications, and publication titles.
4. Infer topic additions from the user's current research themes and publications, using this priority order:
   - Ziheng Chen's official homepage
   - linked official paper pages, OpenReview pages, or arXiv pages
   - linked project/code pages only when they clarify terminology
5. Add missing topic bullets and merge useful new search variants into existing bullets.
6. Keep `TOPICS.md` in the categorized bullet-list format.
7. Deduplicate topics by normalized topic label.
8. Set the top-level `Last updated:` date to the current date when topics are newly inserted or materially changed.
9. Preserve existing topics unless they are clear duplicates.
10. If a topic's scope is too broad or ambiguous, ask the user before finalizing that topic bullet.

Output expectation:

- return the updated `TOPICS.md`
- briefly summarize which topics were inserted or changed

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
