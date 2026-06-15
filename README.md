# VysionAI GTM Outreach Agent

> Signal-based outreach pipeline built entirely in Claude Code.  
> Find companies → research ICP fit → find decision makers → write personalised sequences.  
> One command. One output folder.

---

## What It Does

This system runs a 5-stage outreach pipeline inside Claude Code:

```
Signal Finding → Company Research → Contact Finding → Email Writing → OUTREACH-READY.csv
```

You give it a signal prompt. It finds companies showing buying intent, scores them against your ICP, identifies the right decision maker at each company, writes a personalised 3-email sequence per contact, and compiles everything into a single CSV ready to load into Smartlead or PlusVibe.

No Apollo subscription doing the research. No Zapier. No n8n for the research layer.

**One command:**
```
@agents/gtm-outreach-agent.md "B2B SaaS companies hiring Head of Growth in the US"
```

---

## Requirements

- Claude Code (v2.0+) — [install guide](https://docs.anthropic.com/en/docs/claude-code)
- Claude Pro or Max subscription (or Anthropic API key)
- VS Code (recommended) or any terminal
- Node.js (for Claude Code CLI)
- Python 3.8+ (for scripts — optional at start, needed for scale)

---

## Quick Start

**1. Clone the repo**
```bash
git clone https://github.com/yourusername/vysionai-gtm-agent
cd vysionai-gtm-agent
```

**2. Edit your templates** ← do this before anything else
```
templates/icp-criteria.md      ← your ICP: industry, size, stage, tech stack
templates/email-framework.md   ← your proof points, tone rules, CTA format
```

**3. Start Claude Code**
```bash
claude
```

**4. Test with one company first**
```
@skills/gtm-qualify/SKILL.md https://somecompany.com
```

**5. Run the full pipeline**
```
@agents/gtm-outreach-agent.md "your signal prompt here"
```

**6. Check outputs**
```
outputs/OUTREACH-READY.csv       ← load this into Smartlead or PlusVibe
outputs/sequences/               ← individual email sequences per company
```

---

## Folder Structure

```
vysionai-gtm-agent/
│
├── CLAUDE.md                          ← loads every session automatically
│
├── agents/
│   └── gtm-outreach-agent.md          ← main orchestrator, runs all 4 stages
│
├── skills/
│   ├── signal-finder/
│   │   └── SKILL.md                   ← finds companies from job posts, funding, LinkedIn
│   ├── company-research/
│   │   └── SKILL.md                   ← 19-dimension GTM analysis + ICP scoring
│   ├── contact-finder/
│   │   └── SKILL.md                   ← finds decision maker + personalisation anchors
│   ├── email-writer/
│   │   └── SKILL.md                   ← writes personalised 3-email sequence per contact
│   └── gtm-qualify/
│       └── SKILL.md                   ← standalone quick ICP check on one company
│
├── scripts/
│   ├── company_scorer.py              ← ICP scoring at scale (100+ companies)
│   ├── contact_enricher.py            ← contact data processing
│   └── csv_builder.py                 ← compiles all outputs into OUTREACH-READY.csv
│
├── templates/
│   ├── icp-criteria.md                ← ⚠️ edit this first — your ICP definition
│   ├── email-framework.md             ← ⚠️ edit this first — your email rules + tone
│   └── signal-sources.md             ← signal search sources and query templates
│
└── outputs/                           ← all results save here automatically
    ├── SIGNALS-FOUND.md
    ├── COMPANIES-SCORED.csv
    ├── CONTACTS-FOUND.csv
    ├── OUTREACH-READY.csv
    └── sequences/
        └── {company-name}-sequence.md
```

---

## How It Works

### File Types Explained

| File | What It Is | Auto-loads? |
|---|---|---|
| `CLAUDE.md` | Your context file — who you are, ICP, stack, rules | ✅ Every session |
| `agents/*.md` | Orchestrator — manages the full pipeline | ❌ Invoke with `@agents/` |
| `skills/*/SKILL.md` | Task instructions — one skill = one job | ❌ Invoke with `@skills/` |
| `scripts/*.py` | Python code — data processing at scale | ❌ Claude runs when needed |
| `templates/*.md` | Reference files — ICP, email rules, sources | ❌ Claude loads mid-task |
| `outputs/` | Results folder | ✅ Auto-populated by skills |

### Skills vs Agents vs Scripts

**A skill** does one specific task. One input, one output, invoke it standalone.

**An agent** manages multiple skills in sequence. It launches signal-finder, waits for the output, passes it to company-research, and so on — all the way to the final CSV.

**A script** handles data processing at scale — scoring 200 rows, calling an API, merging CSV files. Claude Code writes and runs these when a skill needs them. You don't write scripts manually.

### The Pipeline

```
Stage 1 → signal-finder skill
          Searches job boards, funding news, LinkedIn content
          Output: outputs/SIGNALS-FOUND.md

Stage 2 → company-research skill
          Fetches each company website, 19-dimension GTM analysis, ICP scoring
          Output: outputs/COMPANIES-SCORED.csv (A/B/C/Skip grades)

Stage 3 → contact-finder skill
          Finds decision maker at each A/B grade company
          Identifies personalisation anchors (recent posts, job changes, funding)
          Output: outputs/CONTACTS-FOUND.csv

Stage 4 → email-writer skill
          Writes 3-email sequence per contact using their anchor
          Applies rules from templates/email-framework.md
          Output: outputs/sequences/{company}-sequence.md

Stage 5 → csv_builder.py
          Merges all data into one send-ready file
          Output: outputs/OUTREACH-READY.csv
```

---

## Signal Types Supported

The signal-finder skill searches across four signal types:

| Signal Type | What It Finds | Quality |
|---|---|---|
| Hiring signals | Companies posting GTM/RevOps/Growth roles | ⭐⭐⭐⭐⭐ |
| Funding signals | Series A/B raises in last 90 days | ⭐⭐⭐⭐ |
| Content signals | Founders posting about GTM pain on LinkedIn | ⭐⭐⭐⭐⭐ |
| Tech stack signals | Job posts revealing automation gaps | ⭐⭐⭐ |

**Example signal prompts:**
```
"B2B SaaS companies hiring Head of Growth in the US"
"GTM tools startups that raised Series A in last 90 days"
"RevOps SaaS companies posting Head of Growth roles"
"Founders writing about building outbound from scratch"
```

---

## ICP Scoring

Every company is scored 0–100 across five dimensions:

| Dimension | Weight | What It Measures |
|---|---|---|
| ICP Firmographic Match | 25% | Industry, size, stage, geography |
| GTM Pain Visibility | 20% | Evidence of the problem we solve |
| Budget Signals | 20% | Funding, tech stack, hiring |
| Tech Stack Fit | 20% | HubSpot, Apollo, modern tools |
| Outreach Readiness | 15% | Can we find the right person? |

**Grade bands:** A (80–100) → hot. B (60–79) → warm. C (40–59) → nurture. Skip (<40) → pass.

Only Grade A and B companies proceed to contact finding and email writing.

---

## Email Output Format

For each Grade A/B company, the email-writer produces a sequence file:

```
outputs/sequences/company-name-sequence.md
├── Subject line options (2)
├── Email 1 — The Opener (60–80 words, signal-based first line, yes/no CTA)
├── Email 2 — The Value Add (50–70 words, new insight, softer ask)
└── Email 3 — The Breakup (30–40 words, warm close, no pressure)
```

All emails are copy-paste ready. No placeholders. Every Email 1 opens with the specific personalisation anchor found for that contact.

---

## Customisation

### Edit Your ICP
Open `templates/icp-criteria.md` and update:
- Target industries and company sizes
- Funding stage preferences
- Tech stack signals (which tools indicate fit)
- Scoring weights per dimension
- Buyer persona profiles

### Edit Your Email Voice
Open `templates/email-framework.md` and update:
- Your proof points and outcomes you deliver
- Tone rules and words to avoid
- CTA format preferences
- Anchor quality standards

### Add New Signal Sources
Open `templates/signal-sources.md` and add:
- New job boards to search
- Industry-specific funding newsletters
- LinkedIn search queries that work for your ICP

---

## Standalone Skill Commands

Use individual skills without running the full pipeline:

```bash
# Quick ICP check on one company (fastest test)
@skills/gtm-qualify/SKILL.md https://company.com

# Find signals only
@skills/signal-finder/SKILL.md "B2B SaaS hiring RevOps"

# Research a specific company
@skills/company-research/SKILL.md https://company.com

# Find contacts at a company
@skills/contact-finder/SKILL.md https://company.com

# Write email sequence (reads from CONTACTS-FOUND.csv)
@skills/email-writer/SKILL.md
```

---

## Output Files

| File | Contents | Use For |
|---|---|---|
| `SIGNALS-FOUND.md` | Raw list of companies found with signal type and source | Review before committing to full research |
| `COMPANIES-SCORED.csv` | All researched companies with ICP scores, GTM gap, tech stack | Understand why each company was prioritised |
| `CONTACTS-FOUND.csv` | Decision makers with personalisation anchors and email patterns | Input for email writing |
| `OUTREACH-READY.csv` | All contacts + Email 1 copy in one file | Load directly into Smartlead or PlusVibe |
| `sequences/*.md` | Full 3-email sequence per company | Review before sending, or load into sequences manually |

---

## Limitations

This pipeline handles the **research and writing layer** only.

It does not:
- Auto-send emails
- Auto-load contacts into your CRM
- Run on a schedule without you present

For scheduled, unattended automation: connect `OUTREACH-READY.csv` to an n8n workflow that loads contacts into Smartlead and triggers the sequence. Claude Code builds. n8n runs.

---

## Stack

Built with Claude Code only — no external tools required for the research pipeline.

| Tool | Role |
|---|---|
| Claude Code | Research, scoring, writing, orchestration |
| Python (optional) | Data processing at scale (100+ companies) |
| n8n (optional) | Scheduled sending after pipeline runs |
| Smartlead / PlusVibe | Email delivery (load OUTREACH-READY.csv) |

---

## About

Built by [Saswati Gorai](https://vysionai.com) — Growth AI Strategist at VysionAI.

VysionAI builds AI automation and GTM systems for B2B SaaS companies.

- Website: [vysionai.com](https://vysionai.com)
- Newsletter: VysionAI on Substack
- Contact: contact@vysionai.com

---

## Licence

MIT — fork it, adapt it, build on it. If you share what you build, a mention is appreciated.
