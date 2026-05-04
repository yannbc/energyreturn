# Workflow

Last updated: 5 May 2026

This document describes how the job search and career management system works end-to-end. It is the reference for rebuilding, debugging, or extending the system.

---

## Overview

Three tools share the `yannbc/energyreturn` repo as a single source of truth:

| Tool | Job | Runs |
|---|---|---|
| **Tasklet** (tasklet.ai) | Daily scan, scoring, dedup, pipeline database, email digest, repo updates | Daily 7:00am AEST (automated) + on demand |
| **Claude Cowork** | Application pack production (cover letter, tailored CV, brief) | On demand (scheduled task exists at 7:30am but not reliably delivering -- see notes) |
| **Claude Code** | Ad hoc: interview prep, site changes, CV edits, repo maintenance | On demand |

**Canonical context:** `CLAUDE.md` at repo root. All three tools read it. Profile, writing rules, framing rules, proof points -- everything lives there. Do not duplicate this information elsewhere.

---

## Tasklet -- The Daily Engine

Tasklet is the backbone of the automated pipeline. It runs on tasklet.ai with a persistent SQL database, file storage, connections to external services, and a reliable cron trigger.

### Trigger

- **Type:** Cron (`cronScheduler`)
- **Instance ID:** `cti_hw8qq397rhqe5kyndb7y`
- **Schedule:** `0 7 * * *` (7:00am daily)
- **Timezone:** `Australia/Sydney`
- **Invocation:** Runs the subagent at `/agent/subagents/daily-job-scan.md`

### What the daily scan does (7 steps)

**Step 1 -- Search for roles**

Uses the `jobspy` Python library to search LinkedIn and Indeed. Runs 9 query variants across 2 locations (Sydney and Melbourne):

Queries:
- "VP Product AI"
- "Head of AI"
- "CPTO" OR "Chief Product Technology Officer"
- "CTO AI" OR "CTO SaaS"
- "GM Digital" OR "General Manager Digital"
- "AI Strategy Director"
- "AI Consulting" OR "Fractional CTO AI"
- "Head of Product AI"
- "Director AI Engineering"

Filters:
- Location: Sydney, Australia OR Melbourne, Australia
- Remote roles: included
- Recency: last 24 hours (48 hours on Mondays to catch weekend postings)

Fallback: if jobspy fails, falls back to `web_search_web` with site-specific queries.

**Step 2 -- Deduplicate**

Checks every result URL against the `seen_roles` database table. Only new URLs proceed. Inserts new URLs into `seen_roles` to prevent reprocessing.

**Step 3 -- Score each new role (1-10)**

Scoring criteria:
- Title/seniority alignment (+3)
- AI relevance of company or role (+2)
- Location: Sydney (+2), Melbourne hybrid (+1), remote (+1)
- Company on watchlist (+1)
- Salary visible and above $250K (+1)
- Known company / interesting brand (+1)

Important scoring rules:
- Word-boundary matching for titles (regex `\b`), not substring. "CTO" must not match "director".
- "VP Product" matches "Vice President of Product", "SVP Product" and similar long-form variants.
- Salary floor: $250K AUD. Roles confirmed below this are not surfaced.

Watchlist companies: Canva, Atlassian, SafetyCulture, Rokt, Employment Hero, Culture Amp, Quantium, Harrison.ai, Eucalyptus, Earlybird, Buildkite, GO1, Immutable, Halter, Pet Circle, Airtasker, Zip Co, Seer Medical, Decidr, Relevance AI, McKinsey QuantumBlack, BCG X, Bain Vector, Accenture AI, PwC AI, Anthropic, Google DeepMind, Amazon AWS AI, Microsoft AI.

**Step 4 -- Update database**

Roles scoring 6+ are inserted into `job_pipeline` with status `queued`. The scan itself is logged in `scan_log`.

**Step 5 -- Check pipeline for follow-ups**

Queries all active pipeline items and flags:
- Applied > 7 days ago with no update: "Consider following up"
- Applied > 14 days ago: "Likely stale -- chase or close"
- Any next_step with a date that has passed

**Step 6 -- Update job-search-log.md**

Regenerates the Active Pipeline table from the database, adds today's scan to Scan History, and writes the file to `/agent/home/job-search-log.md`. Then pushes to GitHub via the `conn_6r02sw3ch60esa78wzrg__github_push_to_branch` tool (owner: `yannbc`, repo: `energyreturn`, branch: `main`).

**Step 7 -- Email digest**

Sends a markdown-formatted digest to `yannburden@gmail.com` via Tasklet's built-in `send_message` tool. Includes new matches (7+ highlighted, 6 as "also found"), pipeline updates, follow-up reminders, and scan stats.

If GitHub push fails, the digest still sends (with a note about the failure). If email fails, the digest is written to `/agent/home/latest-digest.md` as fallback.

### On-demand capabilities

Tasklet also handles ad hoc requests via chat:

- **Pipeline status:** Queries `job_pipeline` and presents the current state
- **Pipeline updates:** Status changes, new contacts, follow-up notes
- **Application production:** Can produce cover letters, CVs, and briefs (though this is being shifted to Cowork)
- **Interview prep:** Can research companies and build prep docs
- **LinkedIn profile updates:** Via the computer use connection (browser automation)
- **Site and repo changes:** Direct GitHub pushes
- **Email:** Can send and reply to messages on the owner's behalf
- **Gmail search:** Can search and read the owner's Gmail via the Gmail connection

### Connections

| Connection | ID | Active tools | Purpose |
|---|---|---|---|
| GitHub | `conn_6r02sw3ch60esa78wzrg` | `github_push_to_branch` | Push files to the energyreturn repo |
| Computer use | `conn_xggca7y49ahmtz2caqrz` | `computer` | Browser automation (LinkedIn profile updates, etc.) |
| Gmail | `conn_frhxh02cjxfspt7yzes7` | `gmail_search_threads`, `gmail_get_threads` | Search and read email |

### Database

Tasklet has a persistent SQL database (SQLite) with three working tables:

**`job_pipeline`** (30 rows as of 5 May 2026)
```sql
CREATE TABLE job_pipeline (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  company TEXT NOT NULL,
  role_title TEXT NOT NULL,
  source TEXT,
  source_url TEXT,
  status TEXT NOT NULL DEFAULT 'queued',
  salary_range TEXT,
  location TEXT,
  fit_score INTEGER,
  date_found TEXT NOT NULL,
  date_applied TEXT,
  date_last_action TEXT,
  last_action TEXT,
  next_step TEXT,
  notes TEXT,
  contact_name TEXT,
  contact_email TEXT,
  created_at TEXT DEFAULT (datetime('now')),
  updated_at TEXT DEFAULT (datetime('now'))
);
```

Status values: `queued`, `materials_ready`, `applied`, `interviewing`, `outreach`, `awaiting_response`, `awaiting_followup`, `rejected`, `declined`, `withdrawn`, `closed`, `closed_warm`, `closed_cold`

**`seen_roles`** (678 rows as of 5 May 2026)
```sql
CREATE TABLE seen_roles (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  source TEXT NOT NULL,
  source_url TEXT NOT NULL UNIQUE,
  role_title TEXT,
  company TEXT,
  first_seen TEXT DEFAULT (datetime('now'))
);
```

Dedup table. Every URL ever seen in a scan is stored here. Prevents the same listing from being rescored or re-added to the pipeline.

**`scan_log`** (tracks daily scan history)
```sql
CREATE TABLE scan_log (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  scan_date TEXT NOT NULL,
  roles_scanned INTEGER DEFAULT 0,
  new_matches INTEGER DEFAULT 0,
  high_fit_matches INTEGER DEFAULT 0,
  notes TEXT,
  created_at TEXT DEFAULT (datetime('now'))
);
```

### File storage

Persistent files on Tasklet's filesystem (`/agent/home/`):

- `job-search-log.md` -- pipeline tracker (also pushed to repo)
- `CLAUDE.md` -- canonical context (also pushed to repo)
- `claude-cowork-handoff.md` -- Cowork application workflow (also pushed to repo)
- `cowork-prompt-template.md` -- Cowork setup prompts (also pushed to repo)
- `infrastructure.md` -- hosting/DNS/email docs (also pushed to repo)
- `changelog.md` -- session history (also pushed to repo)
- `assumptions.md` -- flagged items needing validation (also pushed to repo)
- `linkedin-copy.md` -- prepared LinkedIn section copy
- `linkedin-audit.md` -- full LinkedIn profile audit
- `job-search-plan.md` -- original job search automation plan
- `role-briefs-18-apr.md` -- research briefs for Prezzee, Nine, Mastercard, Derwent
- `applications/{company}/` -- application materials (mirrored to repo)

Subagent:
- `/agent/subagents/daily-job-scan.md` -- instructions for the daily scan subagent

### Contact methods

- `yannburden@gmail.com` (workspace member email -- always available, no verification needed)

Additional contact methods (e.g. for texting) would need to be added and verified via `add_contact_method`.

---

## Claude Cowork

Cowork's role is on-demand application production. It reads `CLAUDE.md` from the repo for profile and rules, and follows the five-step workflow in `claude-cowork-handoff.md`.

### Scheduled task (status: unreliable)

A `daily-application-builder` task exists at 7:30am AEST. It is supposed to:
1. Read `job-search-log.md` from the repo (via blob URL -- raw.githubusercontent.com is blocked)
2. Pick up roles where Status = Queued and Fit >= 7
3. Run the five-step application workflow
4. Save to `applications/{company-slug}/`

**In practice:** This task has not reliably produced application packs. The old `daily-job-search` scanning task has been disabled. Recommendation is to use Cowork on demand only and let Tasklet handle all automated work.

### URLs (raw is blocked)

Cowork's sandbox blocks `raw.githubusercontent.com`. Use blob URLs instead:
- `https://github.com/yannbc/energyreturn/blob/main/CLAUDE.md`
- `https://github.com/yannbc/energyreturn/blob/main/claude-cowork-handoff.md`
- `https://github.com/yannbc/energyreturn/blob/main/job-search-log.md`

### MCP connections

- Indeed MCP: connected (search_jobs, get_job_details)
- Exa MCP: connected (better public LinkedIn job post indexing)
- LinkedIn: no first-party connector (ToS prohibit scraping; no public Jobs API)

### Application workflow (five steps)

Documented in `claude-cowork-handoff.md`:
1. Research the company and role
2. Assess fit against profile (flag dealbreakers before writing)
3. Write cover letter
4. Tailor CV
5. Write brief (one-page summary with pitch line)

Output: `applications/{company-slug}/cover-letter.md`, `cv.md`, `brief.md`

---

## Claude Code

Ad hoc use only. Reads `CLAUDE.md` automatically from the repo root.

Used for:
- Interview prep (saved to `interview-prep/{company-slug}/`)
- Site updates (`site/`)
- CV base edits (`cv/`)
- Repo maintenance
- Any one-off task

### Interview prep prompts

Saved in `claude-code-prompts.md` in the repo. Structure follows the conventions in `CLAUDE.md`.

---

## The Repo -- Single Source of Truth

**Repo:** `https://github.com/yannbc/energyreturn`
**Owner:** `yannbc`
**Default branch:** `main`
**Deployed via:** GitHub Pages (`.github/workflows/pages.yml` deploys `site/` on push to main)
**Custom domain:** `energyreturn.co`

### Structure

```
energyreturn/
  CLAUDE.md                      -- Canonical context (profile, rules, proof points)
  WORKFLOW.md                    -- This file
  README.md                      -- Repo readme
  job-search-log.md              -- Live pipeline tracker (updated daily by Tasklet)
  claude-cowork-handoff.md       -- Cowork application workflow
  cowork-prompt-template.md      -- Cowork setup prompts
  claude-code-prompts.md         -- Claude Code interview prep prompts
  infrastructure.md              -- Hosting, DNS, email routing
  changelog.md                   -- Session history
  assumptions.md                 -- Flagged items needing validation
  backlog.md                     -- Backlog items

  site/
    index.html                   -- Landing page (energyreturn.co)
    me/index.html                -- JS-rendered CV with highlight switcher
    cv/index.html                -- Redirect to /me
    og-card.png                  -- 1200x630 social card
    CNAME                        -- energyreturn.co

  cv/
    cv-base.md                   -- Base CV (data science/AI emphasis)
    cv-consulting.md             -- Consulting/advisory variant

  applications/
    {company-slug}/
      cover-letter.md
      cv.md
      brief.md (if produced)

  interview-prep/
    {company-slug}/
      prep.md
      people.md (optional)

  media/
    tech-trajectory-transcript.txt  -- Cleaned podcast transcript

  .github/workflows/
    pages.yml                    -- Deploys site/ to GitHub Pages
```

### Push convention

All pushes to main go through:
- **Tasklet:** `conn_6r02sw3ch60esa78wzrg__github_push_to_branch` tool (automated daily + on demand)
- **Claude Code:** `git push origin HEAD:main` (manual)
- **Cowork:** Does not push to repo (no reliable Git connection)

---

## Email Infrastructure

- **Domain:** energyreturn.co
- **DNS:** Cloudflare (Zone ID: `e65983df27f4e960943c5f338a26e293`)
- **Inbound:** Cloudflare Email Routing forwards `yann@energyreturn.co` and `hello@energyreturn.co` to `yannburden@gmail.com`
- **Outbound from Gmail:** Requires manual "Send mail as" setup (App Password + verification email). See `infrastructure.md` for steps.
- **Outbound from Tasklet:** Sends from Tasklet's own email address via `send_message`. Used for daily digests.

---

## Key Accounts and Services

| Service | Purpose | Account |
|---|---|---|
| GitHub | Repo hosting, GitHub Pages | yannbc |
| Cloudflare | DNS, email routing | yannburden@gmail.com |
| Gmail | Primary email | yannburden@gmail.com |
| LinkedIn | Profile, networking, job search | yannburden@gmail.com |
| Tasklet | Automation agent | Yann's private workspace |
| Claude (Max) | Cowork, Code, Projects | yannburden@gmail.com |

---

## Rebuilding the System

If starting from scratch on a new Tasklet agent:

1. **Clone context:** Read `CLAUDE.md` and this file from the repo
2. **Create database tables:** Run the three `CREATE TABLE` statements above
3. **Backfill `seen_roles`:** Import from `job-search-log.md` or accept that the first scan will re-find known roles
4. **Set up connections:** GitHub (push), Gmail (search/read), optionally Computer Use (LinkedIn browser automation)
5. **Create subagent:** Write `/agent/subagents/daily-job-scan.md` following the seven-step process documented above
6. **Create trigger:** Cron, `0 7 * * *`, timezone `Australia/Sydney`, invocation message pointing to the subagent
7. **Verify:** Run a test scan, confirm digest email arrives, confirm `job-search-log.md` is pushed to repo

For Cowork: paste the prompts from `cowork-prompt-template.md` in the repo.
For Claude Code: clone the repo and start working -- `CLAUDE.md` is picked up automatically.
