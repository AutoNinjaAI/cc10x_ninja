---
name: github-research
description: "Internal skill. Use cc10x-router for all development tasks."
allowed-tools: WebFetch, WebSearch, mcp__octocode__githubSearchCode, mcp__octocode__githubSearchRepositories, mcp__octocode__githubViewRepoStructure, mcp__octocode__githubGetFileContent, mcp__octocode__githubSearchPullRequests, mcp__octocode__packageSearch, mcp__context7__resolve-library-id, mcp__context7__query-docs, Read, Write, Edit, Bash
---

# External Code Research

## Overview

Research external patterns, documentation, and best practices from GitHub, library docs, and web when AI knowledge is insufficient. 

**Tool Priority:**
1. **WebSearch + WebFetch** - Built-in, always available, best for docs and StackOverflow
2. **Octocode MCP** - GitHub-specific searches (code, repos, PRs)
3. **Context7 MCP** - Library documentation lookup

**Core principle:** Research BEFORE building, not during. External research enhances planning, not execution.

**This skill is ONLY for external research.** Local codebase search is handled by other cc10x tools (Grep/Glob/Read).

## The Iron Law

```
NO EXTERNAL RESEARCH WITHOUT CLEAR AI KNOWLEDGE GAP OR EXPLICIT USER REQUEST
```

If AI training knowledge covers the technology well, skip external research - UNLESS user explicitly asks. This skill is for NEW technologies, complex integrations, unfamiliar APIs, and explicit user requests.

## When to Use

**ALWAYS invoke when:**
- User explicitly requests research ("research X", "how do others", "best practices", "find on github")
- Technology released after 2024 (AI knowledge cutoff)
- Complex integration patterns (auth, payments, real-time)
- Local debugging failed 3+ times with external service errors
- Error message is unfamiliar or library-specific

**NEVER invoke when:**
- User says "quick" or "simple"
- Standard patterns AI knows well (CRUD, REST, React basics)
- Code review or refactoring tasks
- Technology released before 2024 (unless user explicitly asks)

## Web Research (PRIMARY - Always Available)

**WebSearch + WebFetch are built-in Claude Code tools. Use them FIRST.**

### For Documentation
```
# Search for official docs
WebSearch(query="React useEffect cleanup function official docs")

# Fetch specific documentation page
WebFetch(url="https://react.dev/reference/react/useEffect", prompt="How to properly clean up effects and avoid memory leaks?")
```

### For Error Messages
```
# Search for error solutions
WebSearch(query="TypeError: Cannot read property 'map' of undefined React")

# Fetch StackOverflow answer
WebFetch(url="https://stackoverflow.com/questions/...", prompt="What is the recommended solution?")
```

### For Best Practices
```
# Search for current best practices
WebSearch(query="authentication patterns Next.js 2025 best practices")

# Fetch authoritative source
WebFetch(url="https://nextjs.org/docs/authentication", prompt="What is the recommended auth pattern?")
```

### Pre-Approved Documentation Domains (Faster Extraction)
These domains get optimized content extraction:
- react.dev, angular.io, vuejs.org, nextjs.org
- docs.python.org, nodejs.org, go.dev
- pytorch.org, tensorflow.org, pandas.pydata.org
- platform.claude.com, code.claude.com
- developer.mozilla.org, learn.microsoft.com

## MCP Research (SECONDARY - If Available)

### Octocode MCP - GitHub Research

**Check availability first:**
```
mcp__octocode__packageSearch(name="express", ecosystem="npm")
```

**If available, use for:**
- Searching GitHub code patterns
- Finding real-world implementations
- Exploring repository structures

### Context7 MCP - Library Docs

**For library-specific documentation:**
```
mcp__context7__resolve-library-id(libraryName="prisma")
mcp__context7__query-docs(libraryId="prisma", query="how to handle transactions")
```

## Fallback Chain

```
TIER 1: WebSearch + WebFetch (ALWAYS AVAILABLE)
   ↓ (if need GitHub code specifically)
TIER 2: Octocode MCP
   ↓ (if need structured library docs)  
TIER 3: Context7 MCP
```

**NO LOCAL SEARCH** - This skill is for external research only. Local codebase search uses Grep/Glob/Read directly.

## Research Process

### Phase 1: Confirm Need

Before using any research tools, verify:
1. User explicitly requested research? → Proceed
2. Technology is post-2024? → Proceed  
3. Complex integration with uncertainty? → Proceed
4. Error message is unfamiliar? → Proceed
5. None of the above? → STOP. Use AI knowledge.

### Phase 2: Execute Research

**For most queries, start with WebSearch:**
```
WebSearch(query="[specific technical question] [year if relevant]")
```

**Then fetch relevant pages:**
```
WebFetch(url="[URL from search results]", prompt="[specific question about this page]")
```

**Only use Octocode if you need:**
- Actual code implementations from GitHub
- Repository structure exploration
- Pull request patterns

### Phase 3: Summarize for cc10x Memory

Extract only what's needed for the task at hand:
- Key patterns/approaches found
- Gotchas or warnings discovered
- Specific code snippets (minimal)
- Links for future reference

**DO NOT dump entire files - summarize and cite.**

## Output Format

```markdown
## External Research Summary

**Knowledge Gap**: [What we didn't know]

**Research Conducted**:
- Source: [URL or repo]
- Query: [what we searched]

**Key Findings**:
1. [Pattern/approach found]
2. [Gotcha or warning]
3. [Relevant code snippet - minimal]

**Application to Task**:
[How this applies to current work]

**References Saved**:
- [URL for future debugging]
```

## Save Research (MANDATORY)

**Research insights are LOST after context compaction unless saved.** This section is NON-NEGOTIABLE.

### Step 1: Save Research File

```
# Create directory (permission-free)
Bash(command="mkdir -p docs/research")

# Save research using Write tool (permission-free for new files)
Write(file_path="docs/research/YYYY-MM-DD-<topic>-research.md", content="[full research from Output Format above]")
```

**File naming convention:** `YYYY-MM-DD-<topic>-research.md`
- Use today's date
- Use kebab-case for topic (e.g., `claude-code-tasks-system`, `react-server-components`)

### ATOMIC CHECKPOINT (DO NOT PROCEED UNTIL BOTH COMPLETE)

**The next two operations MUST complete in sequence with NO agent invocations between them:**
- ✓ Research file saved to docs/research/
- ⏸️ IMMEDIATELY proceed to Step 2 (do not invoke agents, do not pass go)

**Why this is critical:** If context compaction occurs between Step 1 and Step 2, the research file becomes orphaned (exists but not indexed in memory). These two operations must be atomic.

### Step 2: Update Memory (Links Research to Memory)

**CRITICAL: This must happen in the same execution block as Step 1**

**Use Edit tool (permission-free):**

```
# First read existing content
Read(file_path=".claude/cc10x_ninja/activeContext.md")

# Then append to Research References section using Edit
Edit(file_path=".claude/cc10x_ninja/activeContext.md",
     old_string="## Research References",
     new_string="## Research References
| [Topic] | docs/research/YYYY-MM-DD-topic-research.md | [Key insight from findings] |")
```

**If Research References section doesn't exist, add it:**
```
Edit(file_path=".claude/cc10x_ninja/activeContext.md",
     old_string="## Last Updated",
     new_string="## Research References
| Topic | File | Key Insight |
|-------|------|-------------|
| [Topic] | docs/research/YYYY-MM-DD-topic-research.md | [Key insight] |

## Last Updated")
```

### Step 3: Extract Patterns (Auto-Promote Learnings)

**If research found gotchas or reusable patterns, add to patterns.md:**

```
Read(file_path=".claude/cc10x_ninja/patterns.md")

Edit(file_path=".claude/cc10x_ninja/patterns.md",
     old_string="## Common Gotchas",
     new_string="## Common Gotchas
- [Gotcha from research]: [Solution] (Source: docs/research/YYYY-MM-DD-topic-research.md)")
```

**What to extract:**
- Error patterns and their solutions
- API quirks and workarounds
- Integration gotchas
- Best practices discovered

**What NOT to extract:**
- Task-specific implementation details
- One-time findings not applicable to future work
- Raw code snippets without context

### Step 4: Commit Research (Optional but Recommended)

```
Bash(command="git add docs/research/*.md")
Bash(command="git commit -m 'docs: add <topic> research'")
```

## Red Flags - Research NOT Complete

If you finish research WITHOUT:
- [ ] Saving to `docs/research/YYYY-MM-DD-topic-research.md`
- [ ] Updating `activeContext.md` with research reference
- [ ] Extracting patterns to `patterns.md` (if applicable)

**STOP. Research is NOT complete. Go back and save.**

## Why This Matters

```
WITHOUT SAVE:
Research → Context compaction → LOST FOREVER

WITH SAVE:
Research → docs/research/ → Memory reference → PERSISTS ACROSS SESSIONS
         → patterns.md → LEARNINGS COMPOUND
```

**Research without documentation is wasted effort.**
