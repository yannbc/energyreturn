# Claude Cowork -- Prompt Template

## Step 1: Load the Playbook

Use this once when starting a new Cowork project.

```
Read the full application guide from the repo:
https://github.com/yannbc/energyreturn/blob/main/claude-cowork-handoff.md

This is your reference for all job applications. It contains my profile, writing rules, framing rules, base CVs, and the five-step workflow.

Confirm you've loaded it and summarise the 10 framing rules back to me.
```

_If Cowork can't fetch the URL, upload `claude-cowork-handoff.md` as project knowledge instead._

---

## Step 2: Kill the Existing Job Search Task

Tasklet handles all scanning. Cowork should not be running its own searches.

```
List your scheduled tasks. If there's a daily-job-search or similar scanning task, delete it. Job scanning is handled by Tasklet. You only produce application materials.
```

---

## Step 3: Set Up the Daily Application Task

This replaces the old search task. Cowork reads the repo log that Tasklet pushes each morning and produces application packs for new high-fit roles.

```
Set up a daily scheduled task at 7:30am AEST. Name it "daily-application-builder". Each run:

1. Read the job search log from: https://github.com/yannbc/energyreturn/blob/main/job-search-log.md
2. Read the application guide from: https://github.com/yannbc/energyreturn/blob/main/claude-cowork-handoff.md
3. Parse the Active Pipeline table. Identify any roles where Status = "Queued" and Fit score >= 7.
4. Skip any roles you've already processed in a previous run (maintain your own list by company + role title to avoid re-processing).
5. For each new qualifying role, run the five-step workflow from the guide:
   a. Research the company and role (web search for context, news, strategy signals)
   b. Assess fit (1-10 with reasoning)
   c. Write a tailored cover letter
   d. Tailor the appropriate base CV (data science variant for product/AI roles, consulting variant for advisory/GTM roles)
   e. Write application notes (key angles, referral opportunities, risks)
6. Save outputs as:
   - applications/{company-slug}/brief.md
   - applications/{company-slug}/cover-letter.md
   - applications/{company-slug}/cv.md
7. Present all new application packs for my review.

If no new qualifying roles are found, do nothing silently.

IMPORTANT: Do not run any job searches yourself. The pipeline data comes from Tasklet via the repo. Your job is application production only.
```

---

## Ad Hoc: Manual Role Submission

For roles you find yourself outside the daily scan:

```
New role to process:

**Company:** [name]
**Title:** [title]
**Location:** [location]
**Listing URL:** [url]

Full job description:
[paste JD or link]

Run the five-step workflow from the guide. Flag dealbreakers before writing materials.
```

---

## Refinement

After reviewing materials:

```
For [company]: [specific feedback, e.g. "lead with the Pendula agentic angle", "tone down the consulting language"]

Revise the cover letter and CV accordingly.
```
