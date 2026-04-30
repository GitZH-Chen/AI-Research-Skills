# AI-Research-Skills

This is a collection of personal AI agent skills for research targeting top-tier ML venues. This repository will grow to include additional skills for high-quality AI research. 

## Quick Update / Install

Use `update-skill.sh` to install or update skills from GitHub quickly.

Typical usage:

```bash
# Default: install/update all skills discovered in this repo from GitZH-Chen/AI-Research-Skills (main)
./update-skill.sh

# Install/update one specific skill
./update-skill.sh --path ai-paper-writing

# Specify repo/path/ref
./update-skill.sh --repo GitZH-Chen/AI-Research-Skills --path ai-paper-writing --ref main
```

Without `--path`, the script discovers all local skill directories and processes each one. It installs when missing, and backs up then updates when already installed.

## `conference-friend-finder`

Conference companion skill for tracking people and research topics, then exporting official main-conference schedule entries into Excel.

- Search tracked friends across the active conference's main papers
- Search tracked research topics across titles, abstracts, keywords, and official paper pages
- Include both oral and poster entries when a matched paper has both
- Refresh affiliations in the user's tracked friends list
- Insert new people from names, institutions, or profile URLs
- Update tracked research topics from user-provided terms, papers, profile URLs, or notes
- Switch the active conference target by updating the user's venue file

### Usage

```md
Use $conference-friend-finder.
```

### Default Active Files

- `./Friends.md`
- `./TOPICS.md`
- `./VENUE.md`

This public skill does not ship with any private people or conference data.

### Starter Templates

- `conference-friend-finder/assets/Friends.template.md`
- `conference-friend-finder/assets/TOPICS.template.md`
- `conference-friend-finder/assets/VENUE.template.md`

### Typical Commands

- `search friends`: writes `outputs/Venue-Year-friends.xlsx`
- `search topics`: writes `outputs/Venue-Year-topics.xlsx`
- `update friends`
- `insert friends: Jane Doe`
- `update topics`
- `update conf for ICLR 2027, conference website https://..., OpenReview venue page https://...`

For `search friends` and `search topics`, existing same-name files under `outputs/` are overwritten directly.

### Excel Format

Both search commands produce one sheet per day and one row per official schedule entry, with columns:

1. `Paper Title + Authors`
2. `Type`
3. `Time`
4. `Location`

The `Time` column uses the conference venue's local timezone. The `Location` column stores the physical location only; poster entries with room and board are formatted like `Pavilion 3, P3-#122`.

## `reimbursement`

Organizer for reimbursement materials across research, travel, grants, events, purchasing, and other reimbursable project folders.

- Use default folders: `0_Aux/`, `food/`, and `transportation_accomodation/`
- Keep official rules, approval letters, and templates in `0_Aux/`
- Normalize receipt names as `[class]-[date]-[note].[ext]`
- Use `YYYY-MM-DD` for one-day receipts and `YYYY-MM-DD_to_YYYY-MM-DD` for multi-day receipts or validity periods
- Preserve or add other category folders such as `registration/`, `supplies/`, `equipment/`, `services/`, or `other/` when needed
- Create `output/reimbursement-summary.xlsx` for Excel summaries
- Preserve user-created folders and avoid moving them unless explicitly requested
- Keep eligibility decisions tied to the official rules and relevant office or funder feedback

### Usage

```md
Use $reimbursement.
```

### Example File Structure

```text
reimbursement-project/
├── 0_Aux/
│   ├── reimbursement-rules.pdf
│   └── reimbursement-template.xlsx # official template for reimbursement summary
├── food/
│   └── food-2026-01-15-canteen-receipt.pdf
├── transportation_accomodation/
│   ├── transportation-2026-01-10-train-ticket.pdf
│   └── accommodation-2026-01-10_to_2026-01-24-hotel-invoice.pdf
├── registration/
│   └── registration-2026-01-08-conference-fee.pdf
└── output/
    └── reimbursement-summary.xlsx
```

### Typical Commands

- `init reimbersement`: initialize a reimbursement project with `0_Aux/`, `food/`, `transportation_accomodation/`, and a `readme.md` containing `Use $reimbursement` plus `# Customized Requirement`.
- `org receipts`: organize receipt files into reimbursement categories and normalize names as `[class]-[date]-[note].[ext]`.
- `sum excel`: create `output/reimbursement-summary.xlsx` with one row per actual expense, supporting files, comments, and category totals.

## `ai-paper-writing`

Writing assistant for AI top-tier conference/journal papers in LaTeX. This skill is designed to work with a VSCode + LaTeX Workshop workflow. 

- Drafting and polishing for manuscript sections
- Rebuttal drafting and refinement
- Predefined command `init latex`: create `Aux/`, `Figs/`, and `Sec/` if missing, then write fixed `.latexmkrc`, `.vscode/settings.json`, and `.gitignore` files exactly as specified by the skill; default to `main.tex` when present, otherwise ask which `.tex` file is the main entry before initialization
- Predefined command `grammar check`: grammar-only correction (no polishing or rewriting), plus LaTeX/template/equation rule checks, including optional checks against `Aux/Guidelines.pdf`
- Predefined command `bib check`: clean and standardize `.bib` entries for supported top-tier venues, keep core fields, normalize venue abbreviations, and report potential duplicate references

### Usage

You can structure your `AGENTS.md` as follows to include this skill in your workflow:

```md
Use $ai-paper-writing.

# Custom prompts
# add your own prompts below
```

### Suggested File Conventions

As defined in `ai-paper-writing/agents/openai.yaml`:

These are suggested defaults, not requirements. You can define your own project layout in `# Custom prompts`, and the skill should follow your custom structure instead.

- `./Aux`: manuscript support materials
- `./Aux/Rebuttal`: rebuttal materials
- `./Figs`: figure assets
- `./Sec`: section `.tex` files
- `./Aux/Rebuttal/{venue}-Reviews.md` or `./Aux/Rebuttal/{venue}-{reviewer}.md`: review file(s), either a single file for all reviewers or one file per reviewer (for example `ICLR26-Reviews.md` or `ICLR26-R1.md`)
- `preamble.tex`: predefined LaTeX macros
- `xx.bib`: bibliography source (`\bibliography{xx.bib}`)
- `.vscode/settings.json`: fixed VS Code LaTeX Workshop settings
- `.latexmkrc`: fixed local latexmk configuration
- `.gitignore`: fixed local ignore rules aligned with the LaTeX workflow
- `Aux/Guidelines.pdf`: optional formatting guideline

Typical project tree example, which comes from https://arxiv.org/abs/2602.18858:

```text
your-paper-project/
├── AGENTS.md
├── main.tex
├── preamble.tex
├── abbreviation.tex
├── ref.bib
├── cvpr.sty
├── Figs/
│   ├── teaser.pdf
│   ├── method_overview.pdf
│   └── results.pdf
├── Sec/
│   ├── 0_abstract.tex
│   ├── 1_introduction.tex
│   ├── 2_preliminaries.tex
│   ├── 3_method.tex
│   ├── 4_experiments.tex
│   ├── 5_conclusion.tex
│   └── X_suppl.tex
├── Aux/
│   ├── Guidelines.pdf
│   ├── Rebuttal/
│   │   ├── CVPR26-Reviews.md
│   │   └── CVPR26-R1.md
```

This is only a representative layout, and the skill should follow your project-specific structure when you define one in `AGENTS.md` or `# Custom prompts`.

## `openreview-rebuttal`

Writing assistant for OpenReview rebuttals targeting AI top-tier conferences and journals.

- Drafting and polishing reviewer responses
- Rebuttal-specific structure, heading, and reference-format checks
- Predefined command `final check`: grammar-only correction, heading/bullet validation, and rebuttal reference-style checking

### Usage

```md
Use $openreview-rebuttal.

# Custom prompts
# add your own prompts below
```

### Suggested File Conventions

As defined in `openreview-rebuttal/agents/openai.yaml`:

These are suggested defaults, not requirements. You can define your own project layout in `# Custom prompts`, and the skill should follow your custom structure instead.

- `./Aux`: manuscript support materials and references
- `./Aux/Rebuttal`: rebuttal materials
- `./Aux/Rebuttal/{venue}-Reviews.md` or `./Aux/Rebuttal/{venue}-{reviewer}.md`: review file(s), either a single file for all reviewers or one file per reviewer (for example `ICLR26-Reviews.md` or `ICLR26-R1.md`)
