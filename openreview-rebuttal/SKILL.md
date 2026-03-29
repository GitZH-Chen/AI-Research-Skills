---
name: openreview-rebuttal
description: Assist with drafting, polishing, and final-checking OpenReview rebuttals for AI top-tier conferences or journals. Use when responding to reviewer comments, preparing concise evidence-driven rebuttal text, checking heading/reference rules, or converting math into OpenReview-compatible Markdown.
---

# OpenReview Rebuttal

## 1. Project Overview

Treat the rebuttal as submission-targeted work for an AI top conference or top journal using an OpenReview-style review and rebuttal workflow. If the user specifies a target venue and year, follow that target strictly. If unspecified, follow the rebuttal style of top-tier AI venues such as ICLR, NeurIPS, ICML, CVPR, ICCV, ECCV, AAAI, IJCAI, KDD, WWW, IEEE TPAMI, IEEE TMM, IEEE TIP, IEEE TNNLS, IJCV, JMLR, and TMLR.

At the beginning of each skill invocation, return the checklist below and ask the user to confirm or update each item before writing:

Treat the file layout below as a suggested default, not a requirement. If the user defines a different project structure in `# Custom prompts`, follow the user-defined structure instead.

- `./Aux`: manuscript support materials and references
- `./Aux/Rebuttal`: rebuttal materials
- `./Aux/Rebuttal/{venue}-Reviews.md` or `./Aux/Rebuttal/{venue}-{reviewer}.md`: review file(s), either a single file for all reviewers or one file per reviewer (for example `ICLR26-Reviews.md` or `ICLR26-R1.md`)

All checklist items are optional. If any item is missing, return a concise hint about its purpose and how to provide it, then continue with the available context.

---

## 2. Rebuttal and Formatting Requirements

### 2.1 Overall Goals

- Strengthen the clarity, rigor, and persuasiveness of the rebuttal.
- Address misunderstandings precisely.
- Correct factual errors concisely and politely.
- Reinforce the contribution, novelty, and significance.
- Maintain a confident but non-confrontational tone.
- Keep responses evidence-driven (theory, experiments, citations).

### 2.2 Rebuttal Response Formatting Rules (Structure-Only)

When generating rebuttal responses, strictly follow these structure and formatting rules:

Use `template.md` in this skill directory as the default rebuttal skeleton for each reviewer. Follow its section layout and placeholders unless the user provides a venue-specific format or a different target structure.

#### heading styles

1. **Top-Level Section Titles**
   - Use `#` heading.
   - Title must be a short summarizing sentence, not a repeat of the reviewer’s question.

2. **Subsection Headings**
   - Use bold paragraph headings, e.g., `**Geometry-level ablations**`.
   - No nested numbering (no 1.1, 1.2).
   - Bold is used only for these headings.

3. **Bullet Format**
   - Each bullet begins with `-`.
   - Bullet may start with a concise **bold summary phrase ending with a period**, but this is optional and can be omitted when the bullet is already short and clear.
   - The explanatory sentence follows **on the same line**, without line breaks.
   - Do not bold full sentences.

#### User-specific commands

- **final check**
  When asked to perform a final check on a rebuttal file, you must do all of the following:
  1. **Grammar check.** Only identify and correct grammatical errors without rephrasing or polishing the sentences. Do not alter style, structure, or word choice. Additionally, verify that all LaTeX commands comply with the project’s LaTeX Conventions.
  2. **Heading styles.** Check that all headings and bullets follow the rules specified in `heading styles`, and do not force adding a concise bold summary phrase when the original bullet is already short and clear without it.
  3. **Reference style.** Check that every `References` section follows the reference rules in **Style Requirements**: letter indices assigned in order of first appearance, listed in the same order at the end, and using the required Markdown quoting format. Report any issues and provide the corrected version.
  4. When you do final check, please directly revise the file that we target at.

### 2.3 Style Requirements

- Use **markdown**, especially:
  - when use bullet points for clarity, and considering **bold** for heading in the beginning of each bullet,
  - Use minimal bold (only for section headings, key numerical comparisons, or heading of a paragraph).
  - math in LaTeX syntax `$…$`,
  - headers to organize responses when helpful.
- Rebuttal must remain **within strict length limits** (concise, no redundancy).
- No unnecessary apologies. No emotional language.
- Avoid vague claims, provide precise statements.
- If the reviewer misunderstands, clarify factually without sounding defensive.
- For each response, the heading should summarize your response in one sentence, instead of repeating the question, unless I explicitly ask you to do so.
- Use our paper’s referencing style (e.g., `Sec. 3.2`, `Tab. A`, `Fig. 4`).
- When referring to our paper, **consider providing precise references**, such as:
  - `Sec. 3.2 on page 7`
  - `Fig. 4 on page 12`
  - `Tab. 3 on page 15`
- Note that for subscripts and superscripts in math mode, you leave proper blanks for OpenReview. For example, use `\lambda _{\max}` instead of `\lambda_{max}`.
- Reference style for rebuttal responses. When adding or editing a `References` section, always: (i) assign letter indices `[a], [b], [c], …` according to the order of first appearance in the response; (ii) list the references at the end in the same order; and (iii) use the following Markdown quoting format:

```markdown
# References
> [a] Hyperbolic Neural Networks
>
> [b] Hyperbolic Neural Networks++
>
> [c] Building Neural Networks on Matrix Manifolds: A Gyrovector Space Approach
>
> [d] Matrix Manifold Neural Networks++
```

- **Do not use prohibited spacing commands.** Avoid `\;`, `\!`, or `\,` at all times.
- **Adaptive brackets.** Always use `\left` and `\right` for scalable parentheses instead of `\big` or `\Big`.
- **Prohibition of sentence splitting dashes.** Do not use any long dash symbols such as `--` or `---` to separate or cut sentences. When you need a pause or a shift in tone, use commas, conjunctions, or split the content into two sentences instead. You are still allowed to use `-` and `--` in compound words, such as "manifold-valued point", "point-to-horosphere", "ResNet-18", `Beltrami--Klein` and so on, and to use standard dash notation where it is required.
- When writing or editing the manuscript, minimize the use of semicolons (`;`) unless strictly necessary, such as when enumerating multiple complex items within a single sentence (e.g., `(1); (2); (3); …`). If the sentence can be clearly expressed as two separate sentences without affecting logical continuity, prefer splitting it rather than connecting with a semicolon.
- Please convert all mathematical expressions into an OpenReview-compatible format:
  - use `\lbrace` and `\rbrace` instead of `\{` and `\}`,
  - use `\newline` instead of `\\` for line breaks,
  - and ensure there is a space before every subscript or superscript (e.g., `x _{i}`, `A ^{2}`) to prevent Markdown from swallowing backslashes.

### 2.4 Response Structure

For each reviewer (R1, R2, R3...), use:

**Reviewer X — Short Summary of Responses**
*(You list their concerns here)*

**Response:**
- Provide a direct, concise correction or clarification.
- When needed, cite sections / figures / tables using our paper’s notation (e.g., `Sec. 3.2`, `Fig. 4`).
- If new experiments or ablations can address the concern, propose them cleanly.
- In the response, use `Tab. A` not `Tab. 1` for the table shown in the response.
- For the ref, pls use the numbering like `[a]`, not `[1]` as in the original paper.
- Minimize the use of semicolons and em-dashes when linking clauses. Prefer independent sentences instead. Use commas or conjunctions when necessary, but default to clear, separate sentences to ensure readability.
- Minimize the use of bold text. Use bold only for: (i) paragraph-level headings at the start of a section; (ii) critical quantitative comparisons, such as “our method improves accuracy from 78% to **92%**”. Do not use bold for emphasis in ordinary sentences.

### 2.5 Refs

By default, all refs as well as the manuscript can be found in `./Aux`.
By default, rebuttal-specific materials can be found in `./Aux/Rebuttal`.
If the user defines a different layout in `# Custom prompts`, follow that layout instead.
If the user provides venue-specific core references or a preferred reference list, prioritize those papers when building the rebuttal and the `References` section.

---

## 3. Collaboration and Rebuttal Procedure

### 3.1 Rebuttal Writing Assistance

The user may provide reviewer comments, draft bullets, mathematical derivations, experiment notes, or partial response text. You will return a refined, venue-ready rebuttal that meets the formal style of top-tier AI venues while strictly following the structure and formatting rules above.

When working on reviewer comments or bullet points, convert them into coherent rebuttal text and ensure consistent terminology, notation, and logical flow with the manuscript.

If a citation, experimental result, definition, or formula is missing, explicitly note it so that the missing item can be added later.

### 3.2 Technical Depth

- Use accurate terminology from the paper.
- When objections are mathematical, respond with formal correctness.
- When objections relate to experiments, propose:
  *extra baselines*, *additional ablations*, *statistical interpretation*, or *clarification of protocol*.

### 3.3 If reviewers are wrong

- Correct misunderstandings respectfully.
- Use phrases like:
  *“We believe this originates from a misunderstanding; our intention was…”*
  *“We apologize for the ambiguity and will revise the text to clarify…”*
- Re-ground the explanation in equations or definitions from our method.

### 3.4 Forbidden

- No emotional words.
- No aggressive rebuttals.
- No excuses like “time limitation”.
- No overclaiming, strictly stay within provable statements.

### 3.5 Others

- Emoji like 😄 and 👍 are allowed.
