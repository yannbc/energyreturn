# WORKFLOW.md

**Owner:** Yann Burden
**Purpose:** The operating model for the job search system. How Tasklet, Cowork, and Claude Code split the work and hand off to each other. `CLAUDE.md` is the candidate profile and writing rules; this is how the work flows.

Last updated: 5 May 2026

---

## The split

Three tools, three jobs:

| Tool | What it does | When it runs | Where its state lives |
|---|---|---|---|
| **Tasklet** (tasklet.ai) | Scans LinkedIn and Indeed daily, scores roles, dedupes, maintains the pipeline database, emails the digest, pushes pipeline state to GitHub | Automated, 7:00am AEST daily | Tasklet-internal SQL database plus `job-search-log.md` in this repo |
| **Claude Cowork** | Produces the application pack (brief, cover letter, tailored CV) for one specific role at a time | On demand. Yann starts a Cowork conversation when he picks a role | `applications/{company-slug}/` in this repo |
| **Claude Code** | Ad hoc work: interview prep, site updates, CV refinements, repo maintenance, framing fixes | On demand, in the terminal | This repo |

**Cowork is no longer scheduled.** A previous setup ran Cowork daily at 7:30am AEST to auto-produce packs for any role with Fit >= 7. That produced inventory ahead of demand. As of 5 May 2026, six Cowork-produced packs were sitting in `applications/` for roles still marked "Queued" with no submission. Killed in favour of demand-pull -- materials are written when Yann commits to applying, not before.

---

## The handoff

`job-search-log.md` is the single source of truth between tools.

1. **Tasklet writes to it daily.** Updates the Active Pipeline table and appends to Scan History.
2. **Yann reads** the morning email digest (sent by Tasklet to yann@energyreturn.co) or opens `job-search-log.md` directly.
3. **Yann picks a role to apply to.** Status moves manually from "Queued" to "In progress" or directly to "Applied" once submitted.
4. **Cowork is invoked on-demand for that one role.** Yann opens a Cowork conversation, points it at the role, and Cowork follows `claude-cowork-handoff.md`. Output lands in `applications/{company-slug}/`.
5. **Claude Code handles everything around the application** -- interview prep into `interview-prep/{company-slug}/`, CV refinements, site changes, framing corrections.

---

## What lives where

### In this repo

| Path | Owner | Purpose |
|---|---|---|
| `CLAUDE.md` | Yann (with Claude Code) | Profile, writing rules, framing rules. Read first by all three tools. |
| `WORKFLOW.md` | Yann (with Claude Code) | This file. Operating model. |
| `claude-cowork-handoff.md` | Yann (with Claude Code) | Cowork's instructions for producing application packs. |
| `claude-code-prompts.md` | Yann (with Claude Code) | Prompt templates for interview prep and ad hoc work. |
| `infrastructure.md` | Yann (with Claude Code) | Hosting, DNS, email routing. |
| `job-search-log.md` | Tasklet writes, Yann reads | Live pipeline state and scan history. |
| `cv/` | Yann (with Claude Code) | Base CVs. `cv-base.md` (data/AI), `cv-consulting.md` (advisory). |
| `applications/{slug}/` | Cowork writes, Yann reviews | Per-role pack: brief + cover letter + tailored CV. |
| `interview-prep/{slug}/` | Claude Code | Per-role interview prep. |
| `site/` | Yann (with Claude Code) | Personal site. Deployed to energyreturn.co via GitHub Pages on push to `main`. |
| `changelog.md`, `assumptions.md`, `backlog.md` | Yann (with Claude Code) | Session notes, flagged assumptions, repo backlog. |

### Outside the repo

| What | Where | Why outside the repo |
|---|---|---|
| Pipeline SQL database | Tasklet-internal | Tasklet owns dedup state and full scan history. Only the live pipeline is mirrored to `job-search-log.md`. |
| Email digest | yann@energyreturn.co inbox | Daily Tasklet output. |
| GitHub Pages deploy | github.com/yannbc/energyreturn | See `infrastructure.md`. |
| DNS, email routing | Cloudflare | See `infrastructure.md`. |
| PDF rendering | Local only (`cv/tools/mdcv` -> pandoc + weasyprint) | Not in CI. PDFs not committed. |

---

## Triggers and connections

### Tasklet
- **Schedule:** 7:00am AEST daily
- **Source of role data:** LinkedIn, Indeed (via jobspy queries), other aggregators as configured in Tasklet
- **Outputs:** Email digest to yann@energyreturn.co; commit to `main` updating `job-search-log.md`
- **GitHub:** Tasklet has push access to `main` for `job-search-log.md` updates
- **Salary floor enforced:** $250K AUD. Do not surface or progress roles below this.

### Cowork
- **Schedule:** None. Demand-pull only.
- **Trigger:** Yann starts a Cowork conversation when he picks a role.
- **Inputs:** `CLAUDE.md`, `claude-cowork-handoff.md`, `cv/`, the specific role from `job-search-log.md`
- **Outputs:** `applications/{slug}/{brief, cover-letter, cv}.md`

### Claude Code
- **Schedule:** None. Ad hoc.
- **Trigger:** Terminal session in this repo.
- **Scope:** Anything that isn't scanning (Tasklet) or application pack production (Cowork).

---

## Daily loop

1. Read the Tasklet email digest.
2. Open `job-search-log.md`. Scan the Active Pipeline.
3. For each "Queued" role you want to apply to: start a Cowork conversation, point it at the role, let it produce the pack.
4. Review the pack in `applications/{slug}/`. Edit if needed. Render PDFs locally with `cv/tools/mdcv`.
5. Submit. Update `job-search-log.md` Status to "Applied".

That's the whole loop.

---

## Rebuilding on a fresh agent

If you handed this system to a successor agent (replacement for Cowork, replacement for Tasklet, or a brand new tool playing one of these roles), here's the minimum it needs:

1. **Read `CLAUDE.md` first.** Profile, writing rules, framing rules. Non-negotiable.
2. **Read `WORKFLOW.md`** (this file) for the operating model.
3. **Read the tool-specific handoff doc** for the role they're playing:
   - Cowork-equivalent: `claude-cowork-handoff.md`
   - Interview prep / ad hoc: `claude-code-prompts.md`
   - Tasklet-equivalent: there is no `tasklet-handoff.md` in this repo today. Tasklet's instructions live inside Tasklet. Document them here if you ever switch scanners.
4. **Read `job-search-log.md`** for current pipeline state.
5. **If replacing Tasklet:** the harder problem. You need to reproduce the dedup database and scan history. Either rebuild from URL plus normalised (company + title), or accept some duplicate scans during transition.

---

## Decisions captured here

- **2026-05-05** -- Cowork's scheduled task killed. Cowork now runs on demand only. Reason: scheduled production produced packs ahead of demand; six packs sat unsubmitted while the bottleneck was Yann's review-and-submit step, not generation throughput.
