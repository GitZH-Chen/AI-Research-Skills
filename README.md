# AI-Research-Skills

This is a collection of personal AI agent skills for research targeting top-tier ML venues. 

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

This repository will grow to include additional skills for high-quality AI research.

## `ai-paper-writing`

Writing assistant for AI top-tier conference/journal papers in LaTeX.

- Drafting and polishing
- Rebuttal
- `grammar check`, only fixes grammar and checks template/LaTeX/equation formatting against project rules (including `Aux/Guidelines.pdf`); no polishing or rewriting.
- `bib check`, cleans and standardizes `.bib` entries for target top-tier venues, keeping core fields and normalized venue abbreviations.

Path: `ai-paper-writing/`

### Usage

```md
Use $ai-paper-writing.
...
```

### File Conventions

As defined in `ai-paper-writing/agents/openai.yaml`:

- `./Aux`: manuscript support materials
- `./Aux/Rebuttal`: rebuttal materials
- `./Aux/Rebuttal/{venue}-Reviews.md`: review file (for example `ICLR26-Reviews.md`)
- `preamble.tex`: predefined LaTeX macros
- `xx.bib`: bibliography source (`\bibliography{xx.bib}`)
- `Aux/Guidelines.pdf`: optional formatting guideline
