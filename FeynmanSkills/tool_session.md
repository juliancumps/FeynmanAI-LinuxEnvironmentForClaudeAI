# TOOL: Session Management & Utility Commands

## Purpose
Defines behavior for session-level commands: `/log`, `/search`, `/outputs`, `/help`, and general session hygiene. Substitutes for Feynman's native session-search package and REPL utilities.

---

## /log — Session Log

**What it does:** Writes a structured summary of everything accomplished in the current session. Useful at the end of a research session to capture findings, open threads, and next steps.

**Output format:**
```markdown
# Session Log — [Date]
**Topic(s) covered:** [list]

## Completed Work
- [Workflow run]: [brief summary of what was found/produced]
- [Workflow run]: ...

## Key Findings
- [Finding 1] — source: [URL]
- [Finding 2] — source: [URL]
- ...

## Open Questions
- [Question that came up but wasn't resolved]
- ...

## Artifacts Produced
- [Document/draft title or description]
- ...

## Suggested Next Steps
- [Concrete next action, e.g., "run /audit on arXiv:2401.12345"]
- [Next action]
- ...

## Sources Consulted This Session
[All URLs fetched or referenced during this session]
```

---

## /search — Session Search

**What it does:** Searches prior messages in the current conversation for relevant past findings. Substitute for Feynman's indexed session-search package.

**Protocol:**
1. Receive a search query from the user
2. Scan backwards through the current conversation
3. Identify: prior research outputs, findings, citations, decisions made
4. Return relevant excerpts with context

**Output format:**
```markdown
# Session Search Results: "[query]"

## Match 1
**Context:** [What workflow/discussion this came from]
**Excerpt:** [Relevant content]
**Location:** [Approximate — e.g., "from /deepresearch on scaling laws, earlier in this session"]

## Match 2
...

## No matches found for: [query]
[If nothing relevant — suggest running a new workflow]
```

**Cross-session note:** Claude Projects retain context across conversations within the project. For cross-session search, the user should save key findings to a running notes document in the Project's files.

---

## /outputs — Artifact Browser

**What it does:** Lists all research artifacts (briefs, drafts, reviews, audit reports, replication plans, comparison matrices) generated during the current session.

**Output format:**
```markdown
# Session Artifacts

| # | Type | Topic | Status |
|---|------|-------|--------|
| 1 | Research Brief | [topic] | Complete |
| 2 | Lit Review | [topic] | Complete |
| 3 | Peer Review | [paper title] | Complete |
| 4 | Draft | [title] | Complete |
| ... | ... | ... | ... |

To retrieve any artifact, ask: "Show me artifact #N" or "Show the [type] on [topic]"
```

---

## /help — Command Reference

**Output:**
```markdown
# Feynman — Command Reference

## Research Workflows
| Command | Description |
|---------|-------------|
| `/deepresearch <topic>` | Full multi-agent investigation → cited research brief |
| `/lit <topic>` | Literature review → consensus, debates, open questions |
| `/review <paper/draft>` | Simulated peer review → severity-graded feedback |
| `/audit <arXiv ID>` | Paper vs. codebase mismatch audit |
| `/replicate <paper>` | Replication plan with risk assessment |
| `/compare <topic/papers>` | Side-by-side source comparison matrix |
| `/draft <topic>` | Paper-style document from research findings |
| `/autoresearch <idea>` | Iterative hypothesis → experiment → analysis loop |
| `/watch <topic>` | Fresh check for new developments on a topic |

## Session Commands
| Command | Description |
|---------|-------------|
| `/log` | Write a structured session log |
| `/search <query>` | Search prior findings in this session |
| `/outputs` | List all artifacts produced this session |
| `/help` | Show this reference |

## Tips
- Commands can be typed naturally: "do a deep dive on X" = `/deepresearch X`
- After `/deepresearch` or `/lit`, use `/draft` to turn findings into a document
- After `/draft`, use `/review` to get feedback on the draft
- Chain workflows: `/lit` → `/compare` → `/draft` is a natural Related Work pipeline
- Provide arXiv IDs directly: "review arxiv:2401.12345"
```

---

## General Session Hygiene

**At the start of a new research topic:**
- Announce what workflow is being run
- State the search angles being used
- Confirm with the user if the scope needs adjusting before diving in

**During long workflows:**
- Announce each agent phase transition explicitly
- Surface intermediate findings before synthesizing, so the user can redirect

**At the end of a workflow:**
- Always suggest a natural next step (e.g., "Run `/review` on this draft?" or "Run `/audit` to check if the code matches?")
- Flag any unresolved questions or sources that couldn't be accessed

**Handling ambiguous requests:**
- If the user's intent is ambiguous between two workflows, ask one clarifying question before starting
- If a topic is too broad, suggest narrowing it or ask which angle to prioritize
