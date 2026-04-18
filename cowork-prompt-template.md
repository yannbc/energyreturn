# Claude Cowork -- Prompt Template

## Initial Setup Prompt

Use this once when starting a new Cowork project. It loads the playbook and establishes the working relationship.

```
Read the full application guide from the repo:
https://raw.githubusercontent.com/yannbc/energyreturn/main/claude-cowork-handoff.md

This is your reference for all job applications. It contains:
- My profile and career history
- Writing rules (Australian English, no em dashes, no emojis)
- 10 framing rules you must follow exactly
- Two base CV variants (data science and consulting)
- A five-step workflow for each role

Confirm you've loaded it and summarise the framing rules back to me.

IMPORTANT: You are handling application production only. Job scanning and sourcing is handled by a separate agent (Tasklet) which emails me a daily digest. Your job starts when I give you a role to process. Do not set up or run job searches.
```

_If Cowork can't fetch the URL, upload `claude-cowork-handoff.md` as project knowledge and replace the URL with "Read the uploaded guide."_

---

## Single Role Prompt

Paste this each time a new role needs processing:

```
New role to process:

**Company:** [name]
**Title:** [title]
**Location:** [location]
**Listing URL:** [url]
**Source:** [LinkedIn / Seek / etc]

Full job description:
[paste JD or link]

Run the five-step workflow from the guide. Flag anything that's a dealbreaker before writing materials.

Output:
- applications/{company-slug}/brief.md
- applications/{company-slug}/cover-letter.md
- applications/{company-slug}/cv.md
```

---

## Batch Prompt

When forwarding multiple roles from the Tasklet digest:

```
New roles from today's scan. Process each one through the five-step workflow.
Flag dealbreakers before writing materials.
Prioritise them relative to each other and to any active applications.

Role 1:
- Company: [name]
- Title: [title]
- Location: [location]
- URL: [url]
- JD: [paste or link]

Role 2:
- Company: [name]
- Title: [title]
- Location: [location]
- URL: [url]
- JD: [paste or link]
```

---

## Refinement Prompts

After reviewing materials:

```
For [company]: [specific feedback, e.g. "lead with the Pendula agentic angle instead of Billcap", "tone down the consulting language", "they care about governance more than technical depth"]

Revise the cover letter and CV accordingly.
```

---

## Disable the Daily Job Search

Cowork should NOT be running its own job search. If a scheduled daily-job-search task exists, disable or delete it. Tasklet handles scanning.

To confirm: ask Cowork to list its scheduled tasks and remove any job search automation.
```
List your scheduled tasks. If there's a daily-job-search or similar scanning task, delete it. Job scanning is handled by Tasklet. You only produce application materials when I give you a role.
```
