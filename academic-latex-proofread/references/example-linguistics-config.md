# Example: Linguistics Project Configuration

This shows how a CGEL-style linguistics project configures domain-specific rules for the academic-latex-proofread skill. Copy the structure and adapt for your own field.

These rules would go in your project's CLAUDE.md or a style configuration file.

---

## Semantic Macros

```
\term{}        -- Concepts and categories being defined (renders as small caps).
                  Example: The notion of \term{definiteness} is complex.

\mention{}     -- Words or expressions under discussion in body text (renders as italics).
                  Example: The determinative \mention{the} is not always definite.

\mentionhead{} -- Mentions inside section headings (renders as angle brackets).
                  Example: \subsection{The word \mentionhead{the}}
                  Rationale: Italics don't contrast in small-caps headings.

\olang{}       -- Object language / foreign words (renders as italics).
                  Example: German \olang{der Hund} 'the dog'

\enquote{}     -- Quotations (renders as locale-aware curly quotes).
                  Example: As \enquote{going forward} spread in business English...

Flag bare \textit{word} that should use \term{}, \mention{}, or \olang{}.
Flag \mention{} inside headings -- should be \mentionhead{}.
```

## Banned Terms

```
"crucially"          -- Find a better way to signal importance.
"wh-words"           -- Use "interrogative words" or specify: who, what, which.
"wh-fronting"        -- Use "interrogative fronting" or describe the construction.
"wh-movement"        -- Use "interrogative displacement" or "relative clause formation".
"wh-clause"          -- Use "interrogative clause", "relative clause", or "exclamative clause".
Contrastive "yet"    -- Use "but".
```

## Preferred Terminology

```
"data are/were/have"   -> singular agreement ("data is", "data shows")
"these data"           -> "this data"
"must" (modal)         -> prefer "have to" or softer phrasing (advisory)
```

## Category vs. Function Distinction

```
Determinative = lexical category (word class).
Determiner = syntactic function.
Flag any confusion between category and function labels.
"the dog" is an NP with a DP in determiner function (reject Abney's DP hypothesis).
```

## Cross-Linguistic Notation

```
\textsc{subject}\crossmark          -- Cross-linguistic comparative concept
\textsc{subject}\textsubscript{eng} -- Language-specific
\textsc{subject}\textsubscript{L}   -- Language-internal (generic)

Flag bare underscore subscripts in prose -- use \textsubscript{}.
```

## Citation Format

```
Parenthetical: \citep{key}
Narrative: \textcite{key}
With page: \citep[113--127]{key}
Flag bare \cite{} -- use \citep{} or \textcite{}.
```

## Additional Prose Rules

```
Contractions preferred (don't, isn't, won't).
Paragraph target: ~60 words, max 100.
No \paragraph{} headings -- use topic sentences and discourse markers.
No bold labels for enumeration ("(i) First point...") -- use prose transitions.
```
