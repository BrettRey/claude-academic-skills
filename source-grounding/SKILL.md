---
name: source-grounding
description: >
  Prevents LLM hallucination of citations, statistics, and source-attributed claims
  in academic and research writing. Always active when generating content that references
  sources. Enforces a read-first workflow: read the source, quote exact data, cite with
  location. Flags red-flag patterns (statistics, round numbers, specific claims) that
  require source verification before output.
---

# Source Grounding

LLMs confidently fabricate citations, statistics, quotations, and other source-attributed data. The output looks authoritative. It is often wrong. This skill enforces a hard gate: **do not generate source-attributed claims from training data.**

## The Rule

### 1. Read Before You Write

Before generating any content that includes a factual claim attributed to a source, **read the source material first.** Do not paraphrase from memory. Do not reconstruct what you think it said. Do not synthesize from training data.

If the user has provided source files (PDFs, papers, data files, notes), read them. If the source is a URL, fetch it. If you cannot access the source, stop and say so (see "When You Can't Find It" below).

### 2. Quote Exact Data

Copy numbers, names, dates, and technical terms **directly from the source.** Do not round. Do not approximate. Do not clean up or normalize.

Cite with a specific location:
- Page number: `(Smith 2020: 45)`
- Section or heading: `(Smith 2020, Section 3.2)`
- Table or figure: `(Smith 2020, Table 1)`
- Data file: `(results.csv, row 12)`

If the source doesn't have page numbers, use the most specific locator available.

### 3. When Unavailable, Say So

Do not generate plausible-looking data to fill gaps. Instead, write:

> I don't have access to [source] to verify [claim]. Please provide the relevant file, or I'll mark this as [UNVERIFIED].

Use explicit markers for unverified claims:
- `[UNVERIFIED]` for claims you couldn't check
- `[TODO: verify from source]` for placeholders the user should fill

**Never silently substitute training data for source data.**

## Red Flags

Treat these as **mandatory verification triggers.** If you encounter any of these in your output, stop and verify against the source before continuing.

- Any statistic: percentage, count, p-value, effect size, sample size, confidence interval
- Round numbers that look suspiciously neat: 88%, 90%, "about 3x"
- Ranges that sound plausible but aren't sourced: 88--94%
- Any claim attributed to a specific author or publication
- Direct quotations
- Technical terminology outside your reliable knowledge
- Historical dates, names, or sequences of events tied to specific sources
- Data from tables, figures, or appendices

If you feel confident about any of these from training data, **that confidence is the problem.** Training-data confidence is indistinguishable from hallucination. Verify anyway.

## When You Can't Find It

### Source is available but the data point isn't

If you have the source material but can't locate the specific claim:

> I found [source] but couldn't locate [specific data point]. The closest relevant passage is: "[quote]" ([location]). Can you confirm this is what you're referring to, or point me to the right section?

Do not guess. Do not extrapolate from nearby passages.

### Source is not available

If the user hasn't provided the source and you can't access it:

> I don't have access to [source]. To avoid generating inaccurate data, I've marked this claim as [UNVERIFIED]. Please provide the file or confirm the [specific detail].

**Do not offer to "try from memory" or "approximate based on similar work."** These are hallucination with extra steps.

## Self-Check Before Output

Before finalizing any output that contains source-attributed claims, run this checklist:

- [ ] Every statistic traces to a specific source and location
- [ ] No round numbers unless the source itself uses them
- [ ] Every direct quote is verbatim from the source
- [ ] Every claim attributed to an author matches what that author actually wrote
- [ ] Gaps are flagged with `[UNVERIFIED]` or `[TODO]`, not filled with plausible guesses
