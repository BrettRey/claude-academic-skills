---
name: academic-latex-proofread
description: >
  Read-only proofreading audit for LaTeX academic papers. Use when user says
  "proofread", "audit", "check this file", or before finalizing a paper.
  Checks LaTeX mechanics (dashes, quotes, brackets), prose quality
  (paragraph length, throat-clearers, hackneyed adverbs), grammar, and
  source grounding red flags. Reports violations with line numbers and
  suggested fixes without editing source files. Supports user-defined
  style conventions.
---

# Academic LaTeX Proofread

A read-only audit for LaTeX academic papers. Produces a report of violations. **Does not edit source files.**

## Usage

`/academic-latex-proofread [filename]` -- if no filename given, ask which file to proofread.

## Workflow

### Step 1: Read Context

- Read the **full file** before reporting anything. Understand section argument arcs, not just individual sentences.
- Check if the project has a CLAUDE.md, style configuration file, or other conventions document. If so, read it and apply those rules alongside the defaults below.
- If the project defines custom macros, note which ones exist so you can flag inconsistent usage.

### Step 2: Audit

Work through the file systematically, section by section. Check every category listed below. Do not skip categories.

### Step 3: Report

For each issue found, report:

- **Location**: line number and surrounding context (enough to find it)
- **Category**: `latex` | `prose` | `grammar` | `grounding`
- **Severity**: `critical` (breaks compilation or changes meaning) | `major` (clear style violation) | `minor` (improvement suggestion)
- **Current text**: the problematic text
- **Suggested fix**: what it should say, or "verify against source" for grounding flags

Group issues by section of the paper, not by category. This makes it easier to fix them in order.

### Step 4: Do Not Edit

**Do not edit source files.** Report only. The user reviews the report and applies changes.

---

## Default Checks

These apply to any LaTeX academic paper regardless of field or conventions.

### LaTeX Mechanics

| Check | What to flag |
|-------|-------------|
| **Dashes** | Em-dashes (`---`) in prose. Should be en-dashes (`--`) for parentheticals and ranges. |
| **Quotes** | Straight quotes (`"text"`) or LaTeX backtick quotes (` `` text'' `). Should use `\enquote{}` or the project's quotation macro. |
| **Brackets in italics** | `\textit{(text)}` or `\emph{(text)}` where brackets inherit italics. Should be `(\textit{text})`. |
| **Subscripts in prose** | Bare underscore (`_eng`) outside math mode. Should use `\textsubscript{}`. |
| **Unclosed environments** | `\begin{X}` without matching `\end{X}`. |
| **Missing labels** | Numbered examples, sections, figures, or tables without `\label{}`. |
| **Overfull hbox candidates** | Long unbreakable strings (URLs, compound technical terms) that may cause layout warnings. |

### Prose Quality

| Check | What to flag |
|-------|-------------|
| **Throat-clearers** | "It is important to note that", "It should be mentioned that", "It is worth noting that", "Needless to say". Delete or rephrase. |
| **Hackneyed adverbs** | Moreover, furthermore, nevertheless, however (sentence-initial). Prefer simple coordinators (and, but). |
| **Paragraph length** | Paragraphs over 100 words. Target is ~60. Dense paragraphs should be broken into 2-4 shorter units. |
| **Dash overuse** | Dashes used for parenthetical asides where parentheses would be more appropriate. Most supplementary material is parenthetical, not dramatic. |

### Grammar

| Check | What to flag |
|-------|-------------|
| **Subject-verb agreement** | Including tricky cases (collective nouns, relative clauses). |
| **Article usage** | Missing or incorrect a/an/the. |
| **Tense consistency** | Shifts within a section without reason. |
| **Dangling modifiers** | Participial phrases that don't attach to the intended subject. |
| **Duplicated words** | "the the", "is is", "of of". |

### Source Grounding Red Flags

| Check | What to flag |
|-------|-------------|
| **Unsourced statistics** | Percentages, counts, p-values, effect sizes without citations or page references. |
| **Unlinked claims** | Claims attributed to specific authors without `\citep{}`, `\textcite{}`, or equivalent. |
| **Suspicious round numbers** | Round figures (88%, 90%, "about 3x") that may be approximated rather than sourced. |

---

## Custom Conventions

The default checks above are universal. Most academic projects also have field-specific or project-specific conventions. To use them with this skill, define your rules in your project's CLAUDE.md or a dedicated style file.

### What You Can Configure

**Semantic macros.** If your project defines macros for specific typographic functions, tell the skill what they are and when to use them. The skill will flag bare `\textit{}` or `\emph{}` that should use your macros instead.

Example:
```
Use \term{} for concepts being defined (renders as small caps).
Use \mention{} for words under discussion (renders as italics).
Flag bare \textit{word} that should use one of these macros.
```

**Banned terms.** Words or phrases that violate your project's conventions.

Example:
```
Flag "wh-words", "wh-fronting", "wh-movement" -- use precise functional terms instead.
Flag "crucially" -- find a better way to signal importance.
```

**Preferred terminology.** Substitution pairs for consistent usage.

Example:
```
"data are" -> "data is" (house style uses singular)
"which is to say" -> rephrase directly
```

**Citation format.** Which citation commands your project expects.

Example:
```
Parenthetical: \citep{}
Narrative: \textcite{}
Flag \cite{} (ambiguous) -- use \citep{} or \textcite{} instead.
```

### Where to Put Custom Rules

The skill looks for conventions in this order:
1. Project CLAUDE.md (most common)
2. A `style-rules.yaml` or `style-guide.md` in the project
3. A `.house-style/` directory

If no custom rules are found, the skill uses only the defaults above.

See `references/example-linguistics-config.md` for a complete example of how one project configures domain-specific rules.
