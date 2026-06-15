# VysionAI GTM Outreach Agent — Project Context

## What This Project Is

This is the VysionAI GTM Outreach Agent — a signal-based outreach pipeline
built entirely in Claude Code. No external tools required to run the research
and writing pipeline.

**Purpose:** Find B2B SaaS companies showing GTM buying signals →
research and score them against Saswati's ICP → find decision makers →
write personalised 3-email sequences → output OUTREACH-READY.csv

---

## How to Run the Full Pipeline

```
@agents/gtm-outreach-agent.md "<your signal prompt>"
```

Example:
```
@agents/gtm-outreach-agent.md "B2B SaaS companies hiring Head of Growth in the US"
```

---

## Key Files — Load These Before Any Task

| File | Purpose | Load When |
|---|---|---|
| `templates/icp-criteria.md` | ICP definition and scoring weights | Before any scoring task |
| `templates/email-framework.md` | Email rules, proof points, tone | Before any email writing task |
| `templates/signal-sources.md` | Where to find signals | Before any signal search |

**Rule: Always load the relevant template before starting a skill.**

---

## Folder Structure

```
vysionai-gtm-agent/
├── CLAUDE.md                          ← this file
├── agents/
│   └── gtm-outreach-agent.md         ← main orchestrator
├── skills/
│   ├── signal-finder/SKILL.md        ← Stage 1
│   ├── company-research/SKILL.md     ← Stage 2
│   ├── contact-finder/SKILL.md       ← Stage 3
│   ├── email-writer/SKILL.md         ← Stage 4
│   └── gtm-qualify/SKILL.md          ← standalone quick qualify
├── scripts/
│   ├── company_scorer.py             ← ICP scoring from CSV
│   ├── contact_enricher.py           ← contact data processing
│   └── csv_builder.py                ← final output compiler
├── templates/
│   ├── icp-criteria.md               ← EDIT THIS with your ICP
│   ├── email-framework.md            ← EDIT THIS with your proof points
│   └── signal-sources.md             ← sources to search
└── outputs/                          ← ALL outputs go here
    ├── SIGNALS-FOUND.md
    ├── COMPANIES-SCORED.csv
    ├── CONTACTS-FOUND.csv
    ├── OUTREACH-READY.csv
    └── sequences/
        └── {company-name}-sequence.md
```

---

## Output Rules

- ALL outputs go to `outputs/` folder — never write to root
- CSV files use UPPERCASE names with hyphens
- Sequence files named: `{company-name}-sequence.md` (lowercase, hyphens)
- Never truncate output files — always write complete files
- Confirm to user what was written and where after every file creation

---

## Skill Invocation Quick Reference

| Task | How to Invoke |
|---|---|
| Full pipeline | `@agents/gtm-outreach-agent.md "<signal>"` |
| Find signals only | `@skills/signal-finder/SKILL.md "<signal>"` |
| Research companies only | `@skills/company-research/SKILL.md` |
| Find contacts only | `@skills/contact-finder/SKILL.md` |
| Write emails only | `@skills/email-writer/SKILL.md` |
| Quick qualify one company | `@skills/gtm-qualify/SKILL.md <url>` |

---

## This Project's Context

**Owner:** Saswati Gorai | VysionAI (vysionai.com)
**Stack:** Claude Code only — no n8n, no Apollo, no external tools for research
**Audience:** B2B SaaS companies, 10–200 employees, US-based
**Goal:** Signal-based outreach that feels human and converts

**Note for Claude:**
When in this project folder, you are operating as Saswati's outreach system.
All scoring, research, and writing should be calibrated to her ICP and voice
as defined in the templates. Do not default to generic B2B practices —
always check the templates first.
