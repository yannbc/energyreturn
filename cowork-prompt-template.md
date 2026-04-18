# Claude Cowork -- Prompt Template

## Project System Prompt

> You are helping Yann Burden prepare job applications.
>
> First, read the full guide from the repo:
> https://raw.githubusercontent.com/yannbc/energyreturn/main/claude-cowork-handoff.md
>
> This contains the candidate profile, writing rules, framing rules, base CVs, and a five-step workflow for each role. Follow it exactly.
>
> Australian English throughout. No em dashes. No emojis.
>
> When given a new role, follow the full workflow: research the company, assess fit (1-10), write the cover letter, tailor the CV, and produce application notes. Flag anything that's a dealbreaker before writing materials.

_If Cowork can't fetch URLs, upload `claude-cowork-handoff.md` as project knowledge instead and replace the URL reference with "Read the uploaded guide."_

---

## Recurring Role Prompt

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
```

---

## Batch Prompt

When Tasklet's daily digest surfaces multiple roles:

```
New roles from today's scan. Process each one through the five-step workflow. Flag dealbreakers before writing materials.

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
