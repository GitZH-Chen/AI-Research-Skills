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

### Suggested File Conventions

As defined in `ai-paper-writing/agents/openai.yaml`:

These are suggested defaults, not requirements. You can define your own project layout in `# Custom prompts`, and the skill should follow your custom structure instead.

- `./Aux`: manuscript support materials
- `./Aux/Rebuttal`: rebuttal materials
- `./Aux/Rebuttal/{venue}-Reviews.md` or `./Aux/Rebuttal/{venue}-{reviewer}.md`: review file(s), either a single file for all reviewers or one file per reviewer (for example `ICLR26-Reviews.md` or `ICLR26-R1.md`)
- `preamble.tex`: predefined LaTeX macros
- `xx.bib`: bibliography source (`\bibliography{xx.bib}`)
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
├── sec/
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
