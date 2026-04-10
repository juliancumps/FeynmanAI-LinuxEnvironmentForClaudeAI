---
name: feynman-session
description: Feynman session management and utility commands. Activate when user types /log, /search, /outputs, or /help, or asks to "summarize what we've done", "what have we found so far", "list all the artifacts from this session", "search our prior findings", or "show me the command list". Handles session logging, searching conversation history, listing artifacts, and displaying help.
---

# TOOL: Session Management & Utility Commands

---

## /log — Session Log

Writes a structured summary of everything accomplished in the current session.

**Output format:**
```markdown
# Session Log — [Date]
**Topics covered:** [list]

## Completed Work
- [Workflow run]: [brief summary of what was found/produced]

## Key Findings
- [Finding 1] — source: [URL]
- [Finding 2] — source: [URL]

## Open Questions
- [Question that came up but wasn't resolved]

## Artifacts Produced
- [Document/draft title or description]

## Suggested Next Steps
- [Concrete next action, e.g., "run /audit on arXiv:2401.12345"]

## Sources Consulted This Session
[All URLs fetched or referenced]
```

---

## /search — Session Search

Searches prior messages in the current conversation for relevant past findings.

**Protocol:**
1. Receive search query
2. Scan backwards through conversation
3. Identify: prior research outputs, findings, citations, decisions made
4. Return relevant excerpts with context

**Output format:**
```markdown
# Session Search: "[query]"

## Match 1
**Context:** [What workflow/discussion this came from]
**Excerpt:** [Relevant content]
**Location:** [e.g., "from /deepresearch on scaling laws, earlier this session"]

## No matches for: [query]
[If nothing — suggest running a new workflow]
```

**Cross-session note:** Claude Projects retain context across conversations. For cross-session search, save key findings to a running notes document in the Project's files.

---

## /outputs — Artifact Browser

Lists all research artifacts generated during the current session.

**Output format:**
```markdown
# Session Artifacts

| # | Type | Topic | Status |
|---|------|-------|--------|
| 1 | Research Brief | [topic] | Complete |
| 2 | Lit Review | [topic] | Complete |
| 3 | Peer Review | [paper title] | Complete |
| 4 | Draft | [title] | Complete |

To retrieve any artifact: "Show me artifact #N" or "Show the [type] on [topic]"
```

---

## /help — Command Reference

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

## Power User Tips
- Chain workflows: `/lit` → `/compare` → `/draft` = complete Related Work pipeline
- After `/deepresearch`, type `/draft --from-session` to turn findings into a paper
- After `/draft`, run `/review` to get peer feedback on the draft
- Provide arXiv IDs directly: "review arxiv:2401.12345" or "/audit 2401.12345"
- Constrain searches: "focus on papers from 2024–2025" or "only empirical results"
```

---

## General Session Hygiene

**At the start of a new research topic:**
- Announce what workflow is running
- State the search angles being used
- Confirm scope with user before diving in if ambiguous

**During long workflows:**
- Announce each agent phase transition explicitly
- Surface intermediate findings so the user can redirect if needed

**At the end of a workflow:**
- Always suggest a natural next step
- Flag unresolved questions or sources that couldn't be accessed

**Handling ambiguous requests:**
- If intent is unclear between two workflows, ask one clarifying question before starting
- If topic is too broad, suggest narrowing or ask which angle to prioritize
