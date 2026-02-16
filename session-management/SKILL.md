---
name: session-management
description: >
  Prevents context loss across LLM sessions for multi-session projects.
  Use at session end ("shutdown", "wrap up", "done for now") and session
  start ("pick up where I left off", "what did we do last time"). Captures
  decisions, state changes, and open questions to STATUS.md and
  session-log.jsonl. Startup protocol reads these files to restore context.
  Also helps recover context during long sessions.
---

# Session Management

LLM sessions are stateless. When a session ends, everything learned -- decisions, preferences, project state, open questions -- evaporates. This skill creates a persistence layer using two files so the next session can pick up where the last one left off.

## Persistence Files

**`STATUS.md`** -- Human-readable project status. Dated entries with bullet points. This is the primary file for context recovery.

**`session-log.jsonl`** -- Machine-readable session log. One JSON line per session. Useful for scanning recent activity across many sessions.

Both files are created automatically if they don't exist.

---

## Shutdown Protocol

Use when the user says "shutdown", "wrap up", "done for now", or is ending a session.

### Step 1: Session Summary

Review what was accomplished this session. Identify:

- **Decisions made** that should persist (design choices, rejected approaches, user preferences)
- **Context learned** (constraints discovered, patterns noted, things that didn't work)
- **State changes** (files created or modified, tasks completed, blockers found)
- **Open questions** or next steps that the next session should start with

### Step 2: Update STATUS.md

Add a dated entry to STATUS.md in the project root. Create the file if it doesn't exist.

Format:
```markdown
### YYYY-MM-DD

- [Decision or state change]
- [What was accomplished]
- [Open questions or next steps]
```

Most recent entries go at the top, below the heading.

### Step 3: Append to Session Log

Append one line to `session-log.jsonl` in the project root. Create the file if it doesn't exist.

```json
{"timestamp": "YYYY-MM-DDTHH:MM:SSZ", "project": "project-name", "summary": "one-line summary of session"}
```

The summary should be a single sentence capturing the session's main accomplishment.

### Step 4: Context File Interview

Ask the user one question before closing:

> "Anything from this session that should be added to a context file (CLAUDE.md or similar)?"

If yes, make the updates. If no, proceed. **Do not skip this step.** It is the highest-value action for preventing context loss -- the user often knows things that aren't captured in the summary.

### Step 5: Confirm Exit

> "Session summary saved to STATUS.md and session-log.jsonl. Ready to exit?"

---

## Startup Protocol

Use when a new session begins, or the user says "pick up where I left off", "what did we do last time", or similar.

### Step 1: Read STATUS.md

Find STATUS.md in the project root. Read the most recent dated entry. Note decisions, state changes, and open questions.

### Step 2: Read Session Log

Read the last 5-10 entries of `session-log.jsonl`. If multiple sessions have happened since the user last worked on this project, summarize the arc (not just the last session).

### Step 3: Check for Open Items

If the most recent STATUS.md entry mentions blockers, open questions, or explicit next steps, collect them.

### Step 4: Brief the User

> "Last session (YYYY-MM-DD): [summary]. Open items: [list or 'none']. Ready to continue?"

Keep the briefing short. The user can ask for details if needed.

---

## Long Session Recovery

In long sessions, early context fades as the conversation grows. If the user seems disoriented, asks "where were we", or has been working for a long time without a clear thread, re-read STATUS.md and session-log.jsonl to recover the session's starting state and trajectory. Summarize what's been done and what remains, then resume.
