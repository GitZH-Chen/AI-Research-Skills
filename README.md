# AI-Research-Skills

This is a collection of personal AI agent skills for research targeting top-tier ML venues. This repository will grow to include additional skills for high-quality AI research. 

## Quick Update / Install

Use `update-skill.sh` to install or update skills from GitHub quickly.

Typical usage:

```bash
# Default: install/update ai-paper-writing from GitZH-Chen/AI-Research-Skills (main)
./update-skill.sh

# Specify repo/path/ref
./update-skill.sh --repo GitZH-Chen/AI-Research-Skills --path ai-paper-writing --ref main
```

It installs when missing, and backs up then updates when already installed.

## `ai-paper-writing`

Writing assistant for AI top-tier conference/journal papers in LaTeX. This skill is designed to work with a VSCode + LaTeX Workshop workflow. 

- Drafting and polishing for manuscript sections
- Rebuttal drafting and refinement
- Predefined command `grammar check`: grammar-only correction (no polishing or rewriting), plus LaTeX/template/equation rule checks, including optional checks against `Aux/Guidelines.pdf`
- Predefined command `bib check`: clean and standardize `.bib` entries for supported top-tier venues, keep core fields, normalize venue abbreviations, and report potential duplicate references

### Usage

You can structure your `AGENTS.md` as follows to include this skill in your workflow:

```md
Use $ai-paper-writing.

# Custom prompts
# add your own prompts below
```

### File Conventions

As defined in `ai-paper-writing/agents/openai.yaml`:

- `./Aux`: manuscript support materials
- `./Aux/Rebuttal`: rebuttal materials
- `./Aux/Rebuttal/{venue}-Reviews.md` or `./Aux/Rebuttal/{venue}-{reviewer}.md`: review file(s), either a single file for all reviewers or one file per reviewer (for example `ICLR26-Reviews.md` or `ICLR26-R1.md`)
- `preamble.tex`: predefined LaTeX macros
- `xx.bib`: bibliography source (`\bibliography{xx.bib}`)
- `Aux/Guidelines.pdf`: optional formatting guideline

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

### File Conventions

As defined in `openreview-rebuttal/agents/openai.yaml`:

- `./Aux`: manuscript support materials and references
- `./Aux/Rebuttal`: rebuttal materials
- `./Aux/Rebuttal/{venue}-Reviews.md` or `./Aux/Rebuttal/{venue}-{reviewer}.md`: review file(s), either a single file for all reviewers or one file per reviewer (for example `ICLR26-Reviews.md` or `ICLR26-R1.md`)
