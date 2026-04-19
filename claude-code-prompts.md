# Claude Code Prompts

Prompts for use in Claude Code. CLAUDE.md at the repo root is read automatically -- no need to reference it explicitly.

---

## Interview Prep

### hipages -- SVP Product (Monday 21 April)

```
Prepare interview prep for hipages Group, Senior Vice President of Product.

Context on the conversation:
- This is an initial conversation, not a formal interview. I reached out to Jeremy Burton (CTO) via LinkedIn with a personal connection (we both attend CTO chinwags). He's responded and we're having a conversation tomorrow.
- The current CPO, Herry Wiputra, is leaving on 1 July 2026. This role likely exists because of that departure.
- hipages is an ASX-listed marketplace connecting homeowners with tradies. They've been investing in AI for matching and pricing.
- My angle: I've built and scaled product orgs through exactly this kind of leadership transition, and I bring AI product experience from Pendula that maps directly to marketplace personalisation.

Read my existing application materials if any exist in applications/hipages/.
Read my Relevance AI cover letter and CV in applications/relevance-ai/ for reference on how I position enterprise platform experience.

Research:
- hipages recent earnings, strategy, AI initiatives, product roadmap signals
- Jeremy Burton's background (CTO, hipages)
- Herry Wiputra's tenure and what they built
- Competitive landscape (Airtasker, ServiceSeeking, etc.)

Produce interview-prep/hipages/prep.md following the conventions in CLAUDE.md.
Also produce interview-prep/hipages/people.md with profiles for Jeremy Burton and Herry Wiputra.
```

### Relevance AI -- Technical PM/Lead, Enterprise (Monday 21 April)

```
Prepare interview prep for Relevance AI, Technical PM/Lead (Enterprise).

Context on the conversation:
- I applied via LinkedIn around 1 April. Status is "in review" -- this will be a first conversation.
- I've positioned myself as likely more senior than the role as written, but interested in where they're heading on enterprise product. See my cover letter in applications/relevance-ai/cover-letter.md.
- Relevance AI builds an agentic AI platform (AI workforce / AI agents). They're at the stage where enterprise product leadership is the growth bottleneck.
- My angle: I've built exactly this -- agentic workflows in production at Pendula, enterprise readiness (governance, auditability, access controls) for regulated clients. The Pendula-to-Relevance mapping is very direct.

Read my existing application materials in applications/relevance-ai/ (cover letter, pitch, tailored CV).

Research:
- Relevance AI's current product, recent launches, funding, team size, key people
- Their enterprise push -- who are their customers, what verticals, what stage
- The competitive landscape (CrewAI, LangChain, AutoGen, custom agent frameworks)
- Who might be interviewing me (founder/CEO Daniel Vassilev, or enterprise team leads)

Produce interview-prep/relevance-ai/prep.md following the conventions in CLAUDE.md.
Also produce interview-prep/relevance-ai/people.md with profiles for likely interviewers.
```

---

## General Interview Prep (Template)

```
Prepare interview prep for {company}, {role title}.

Context:
- {Stage of conversation: initial chat / first interview / technical / final}
- {Who am I speaking with, if known}
- {How I got here: applied, referred, headhunted, outreach}
- {Any specific angle or positioning notes}

Read my application materials in applications/{company-slug}/ if they exist.

Research the company, the role, the people, and the competitive landscape.

Produce interview-prep/{company-slug}/prep.md and people.md following the conventions in CLAUDE.md.
```

---

## Other Useful Prompts

### Update CV after feedback

```
Read cv/cv-base.md. Apply these changes: {describe changes}. Write the updated version back to cv/cv-base.md.
```

### Add a new application

```
Read CLAUDE.md and the job description below. Follow the Cowork five-step workflow (documented in claude-cowork-handoff.md) to produce applications/{company-slug}/brief.md, cover-letter.md, and cv.md.

{paste JD}
```

### Site update

```
Read site/me/index.html. {Describe the change}. Update the file. Make sure the change doesn't break the JS-rendered CV switcher.
```
