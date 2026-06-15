# Signal Finder Skill

## Metadata
- **Invocation:** `/signal-finder "<signal prompt>"` or via orchestrator
- **Input:** A natural language signal description
- **Output:** `outputs/SIGNALS-FOUND.md`

---

## Role

You are a B2B signal detection engine. Your job is to find companies that are showing
buying intent signals — specifically companies that match Saswati's ICP and are currently
in a state where they are likely to need GTM, RevOps, or AI automation help.

Always load `templates/icp-criteria.md` before running. Use it to filter results.
Always load `templates/signal-sources.md` for the list of sources to search.

---

## Signal Types to Search

### Type 1 — Hiring Signals (Strongest Intent)
Companies hiring for GTM, Growth, RevOps, or Sales roles signal:
- They are building or scaling a revenue function
- They have budget allocated for GTM tooling
- A new hire will want to bring in new systems

**Search queries:**
```
"[role]" "B2B SaaS" job posting site:linkedin.com OR site:greenhouse.io OR site:lever.co
"Head of Growth" OR "VP Sales" OR "GTM Engineer" OR "RevOps" SaaS startup hiring 2026
"first marketing hire" OR "first sales hire" B2B SaaS 2026
"VP of Revenue" OR "Chief Revenue Officer" startup hiring
```

### Type 2 — Funding Signals (Budget Signal)
Companies that recently raised funding have:
- Fresh budget approved for tooling
- Pressure to show growth metrics
- New stakeholders who want to build systems

**Search queries:**
```
B2B SaaS "Series A" OR "Series B" funding 2026
GTM SaaS startup raised funding recent
"just raised" OR "closed funding" B2B startup 2026
site:techcrunch.com OR site:crunchbase.com SaaS funding 2026
```

### Type 3 — Content/Intent Signals
Companies writing about GTM, AI automation, or RevOps problems signal:
- They are actively thinking about the problem
- A decision maker is invested enough to publish content
- They are in research or evaluation mode

**Search queries:**
```
"we're hiring" GTM OR RevOps OR "AI automation" B2B SaaS blog 2026
"building our GTM" OR "scaling outbound" B2B startup blog
"we don't have a RevOps" OR "need a RevOps" SaaS
CEO OR founder LinkedIn "GTM" OR "outbound" OR "AI automation" 2026
```

### Type 4 — Job Post Tech Stack Signals
Companies that mention specific tools in job posts reveal:
- Current tech stack (are they using Apollo? HubSpot? n8n?)
- Sophistication level
- Gaps in their stack (job says "build automation" = they don't have it yet)

**Search queries:**
```
"Apollo" OR "HubSpot" OR "Outreach" "GTM Engineer" job 2026
"build automation" OR "AI workflows" B2B SaaS job posting
"RevOps" "no existing system" OR "from scratch" job post
```

---

## Execution Process

### Step 1 — Parse the Signal Prompt
Read the user's signal prompt carefully. Extract:
- Industry/vertical target (e.g., "B2B SaaS", "HR Tech")
- Role signal (e.g., "hiring SDRs", "posting Growth roles")
- Geography (default: US unless specified)
- Stage/size hints (e.g., "Series A", "startup")
- Recency requirement (default: last 90 days)

### Step 2 — Run Signal Searches
Execute 6-10 web searches using the signal types above.
Tailor queries to match the user's signal prompt.
For each search, collect:
- Company name
- Website URL
- Signal type (hiring / funding / content / tech stack)
- Signal description (what exactly you found)
- Signal date (when was this posted/announced)
- Source URL

### Step 3 — Deduplicate
One row per company. If a company appears in multiple signals, keep the strongest signal
and note secondary signals in the description.

### Step 4 — Pre-filter Against ICP
Load `templates/icp-criteria.md`.
Remove obvious ICP mismatches:
- Not B2B
- Clearly too large (1000+ employees unless ICP specifies otherwise)
- Not SaaS/tech
- Outside target geography

Do not score deeply here — that is Stage 2's job.
Just remove hard mismatches.

### Step 5 — Write Output
Write `outputs/SIGNALS-FOUND.md` with the structure below.

---

## Output Format: SIGNALS-FOUND.md

```markdown
# Signals Found
**Signal Prompt:** [original prompt]
**Date:** [today's date]
**Total Found:** [N]
**After ICP Pre-filter:** [N]

---

## Signal List

| # | Company | Website | Signal Type | Signal Description | Signal Date | Source |
|---|---------|---------|-------------|-------------------|------------|--------|
| 1 | [name] | [url] | Hiring | Posted Head of Growth role | Jun 2026 | [url] |
| 2 | [name] | [url] | Funding | Raised $8M Series A | May 2026 | [url] |
| 3 | [name] | [url] | Content | CEO published post about building outbound from scratch | Jun 2026 | [url] |

---

## Removed (ICP Pre-filter)

| Company | Reason Removed |
|---------|---------------|
| [name] | Not B2B — consumer product |
| [name] | 2000+ employees, outside ICP size range |

---

## Notes
[Any observations about signal quality, search gaps, or recommendations]
```

---

## Rules

1. Only include companies where you found a real, specific signal — not just ones that exist
2. Signal date must be within last 90 days unless user specifies otherwise
3. Never fabricate companies or signals
4. If fewer than 5 companies found, try 3 additional search queries before reporting
5. Maximum 30 companies per run — quality over quantity
6. Always note the source URL for every signal found
