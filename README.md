# Academic Skills for Claude

Reusable [Claude skills](https://docs.anthropic.com/en/docs/claude-code/skills) for academic researchers who use LLMs for research writing. Each skill is a standalone folder you can install independently.

## Skills

### source-grounding

Prevents LLM hallucination of citations, statistics, and source-attributed claims. Enforces a hard gate: read the source first, quote exact data, cite with a specific location. Never silently substitute training data for source material.

**What it does:**
- Enforces a read-first workflow before generating any source-attributed content
- Flags red-flag patterns (statistics, round numbers, attributed claims) as mandatory verification triggers
- Requires explicit gap-marking (`[UNVERIFIED]`) when sources are unavailable

### academic-latex-proofread

Read-only proofreading audit for LaTeX academic papers. Checks LaTeX mechanics, prose quality, grammar, and source grounding red flags. Reports violations with line numbers and suggested fixes without editing source files.

**What it does:**
- Checks universal LaTeX issues (dashes, quotes, brackets, subscripts, unclosed environments)
- Flags prose problems (throat-clearers, hackneyed adverbs, long paragraphs, dash overuse)
- Catches source grounding red flags (unsourced statistics, unlinked claims)
- Supports user-defined style conventions (semantic macros, banned terms, preferred terminology)

Includes an example configuration for CGEL-style linguistics in `references/example-linguistics-config.md`.

### More skills planned

- **session-management** -- Context persistence across sessions (shutdown protocol, startup checks, session logging)

## Installation

### Claude Code

Copy the skill folder into your Claude Code skills directory:

```bash
# Install a single skill
cp -r source-grounding/ ~/.claude/skills/source-grounding/
```

The skill activates automatically based on its description in the YAML frontmatter.

### Claude.ai

1. Download the skill folder (e.g., `source-grounding/`)
2. Zip it: `zip -r source-grounding.zip source-grounding/`
3. Go to Claude.ai > Settings > Skills
4. Upload the zip file

## License

MIT
