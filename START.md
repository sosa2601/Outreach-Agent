# VysionAI GTM Agent — Quick Start

## How to Start Every Session

Open terminal in this folder and run:
```
claude
```
That's it. CLAUDE.md loads automatically. You're ready.

---

## Quick Command Reference

Paste any of these into the Claude Code chat to start:

### Run Full Pipeline
```
@agents/gtm-outreach-agent.md "B2B SaaS companies hiring Head of Growth in the US"
```

### Quick Qualify One Company
```
@skills/gtm-qualify/SKILL.md https://companywebsite.com
```

### Find Signals Only
```
@skills/signal-finder/SKILL.md "GTM SaaS startups that raised Series A in last 90 days"
```

### Research a Specific Company
```
@skills/company-research/SKILL.md https://companywebsite.com
```

### Find Contacts at a Company
```
@skills/contact-finder/SKILL.md https://companywebsite.com
```

### Write Email Sequence
```
@skills/email-writer/SKILL.md
```
(Reads from outputs/CONTACTS-FOUND.csv automatically)

---

## What Loads Automatically Every Session

- `CLAUDE.md` — your project context and rules
- Your name, brand, ICP, stack, and output preferences

## What You Invoke Manually

- All skills — using @skills/
- All agents — using @agents/
- All scripts — Claude runs them when a skill needs them

---

## Output Files Location

All outputs save to the `outputs/` folder automatically:
- `outputs/SIGNALS-FOUND.md`
- `outputs/COMPANIES-SCORED.csv`
- `outputs/CONTACTS-FOUND.csv`
- `outputs/OUTREACH-READY.csv`
- `outputs/sequences/` — individual email sequences

---

## Session Flow (What to Do Each Time)

1. Open VS Code → open terminal
2. `cd` into this folder (already done if VS Code opened here)
3. Type `claude` → session starts
4. Paste a command from above → pipeline runs
5. Check `outputs/` folder for results

---

## If Claude Code Asks You to Log In Again

It shouldn't. But if it does, run this in PowerShell before starting:
```powershell
$env:ANTHROPIC_API_KEY = "your-key-here"
```
Or set it permanently (one time only):
```powershell
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "your-key-here", "User")
```
