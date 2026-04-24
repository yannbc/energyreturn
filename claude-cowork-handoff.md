# Claude Cowork -- Outbound Application Guide

**Owner:** Yann Burden
**Purpose:** Workflow instructions for producing job application materials. Read `CLAUDE.md` in the repo root for the full candidate profile, writing rules, and framing rules.

---

## Read First

Before doing any work, read `CLAUDE.md` at the repo root. It contains:
- Who Yann Burden is (profile, career arc, contact details)
- Writing rules (Australian English, no em dashes, etc.)
- 10 framing rules you must follow exactly
- Key proof points and metrics
- Repo structure

Everything below assumes you've read and understood `CLAUDE.md`.

---

## Division of Labour: Tasklet vs Cowork

**Tasklet** (the agent running at tasklet.ai) handles the daily scan:
- Searches LinkedIn, Indeed and job aggregators at 7am AEST daily
- Scores and deduplicates roles against a pipeline database
- Emails a digest to yann@energyreturn.co with new matches
- Maintains the pipeline database and `job-search-log.md` in the repo

**Cowork** (you) handles application production:
- You read `job-search-log.md` from the repo to pick up new roles
- You produce the full application pack: brief, cover letter, tailored CV
- Follow the five-step workflow below for each role

**Do NOT duplicate the scanning work.** Tasklet has browser-based LinkedIn access and a deduplication database. Your job starts when Tasklet pushes updated roles to the repo.

### Automated Handoff via Repo

The repo file `job-search-log.md` is the handoff point. It contains an **Active Pipeline** table with columns including Status and Fit score.

Your scheduled task should:
1. Read `job-search-log.md` from the repo
2. Parse the Active Pipeline table
3. Process any role where **Status = "Queued"** and **Fit >= 7**
4. Skip roles you've already processed (maintain your own list of processed roles by company + role title)
5. For each new qualifying role, run the five-step workflow below

If no new qualifying roles are found, do nothing. No notification needed.

---

## Workflow for Each New Role

### Step 1: Research the Role

- Read the full job description carefully. Identify the top 3--5 requirements.
- Research the company: recent news, strategy shifts, leadership changes, funding rounds, M&A activity.
- Identify **why the role exists now**. What changed? New leader? New strategy? Someone left? Growth pressure?
- Research the hiring manager if named.

### Step 2: Assess Fit

- Score the role 1--10 against the profile in `CLAUDE.md`. Be honest about gaps.
- Identify the strongest 2--3 experience-to-requirement mappings.
- Flag any risks: salary, seniority mismatch, location, experience gaps.
- Identify the single most compelling angle.

### Step 3: Write the Cover Letter

- Max 4 paragraphs. No waffle.
- **Opening:** Something specific about the company/role that shows research. Reference the "why now" intelligence if possible.
- **Middle paragraphs:** Map experience to their specific requirements. Use concrete metrics from the Key Proof Points table in `CLAUDE.md`. Pick the 2--3 most relevant, not all of them.
- **Close:** Brief, confident. "I'd welcome the opportunity to discuss this further."
- **Sign off:** "Kind regards, Yann Burden"

### Step 4: Tailor the CV

- Start from the appropriate base CV in `cv/` (cv-base.md for AI/product roles, cv-consulting.md for advisory/GTM roles).
- Modify the **Highlights** section to emphasise what matters for this role.
- Modify the **Focus** keywords.
- Adjust bullet points to foreground the most relevant achievements.
- Do NOT invent experience or metrics.
- Do NOT change actual job titles.
- Keep to 2 pages maximum when rendered.

### Step 5: Write Application Notes

- Fit score with brief rationale
- Flags or risks
- Suggested next steps
- Priority ranking relative to other active applications if applicable

---

## Output Format

For each role, produce three files:

- `applications/{company-slug}/cover-letter.md`
- `applications/{company-slug}/cv.md`
- `applications/{company-slug}/brief.md`

Markdown only. Do not attempt to generate PDFs. Yann renders PDFs locally via `cv/tools/mdcv` using pandoc + weasyprint, which are not available in your environment. PDFs are not checked into the repo.

### Cover Letter Markdown Structure

Cover letters render through `mdcv --letter` and expect this structure:

```
# Yann Burden

+61 400 941 979 | yannburden@gmail.com | energyreturn.co/me | linkedin.com/in/yannburden

---

## {Company} — {Role Title}

*{Month Year}*

---

{Body paragraphs}

Cheers,
Yann
```

Letterhead (h1 + contact line) is required. Sign off with "Cheers, Yann" -- not "Kind regards".
