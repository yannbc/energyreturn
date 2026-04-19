# CLAUDE.md

This is the canonical context file for Yann Burden's career repo. All tools that work in this repo -- Claude Code, Claude Cowork, Tasklet -- should read this file for profile, rules, and conventions.

---

## About This Repo

**Repo:** `yannbc/energyreturn` (GitHub Pages deploys `site/` to energyreturn.co)

This repo holds:
- **Personal site** (`site/`) -- landing page and JS-rendered CV at energyreturn.co/me
- **Base CVs** (`cv/`) -- two variants: `cv-base.md` (data science/AI emphasis) and `cv-consulting.md` (enterprise consulting emphasis)
- **Applications** (`applications/{company-slug}/`) -- cover letters, tailored CVs, briefs, pitch lines per role
- **Interview prep** (`interview-prep/{company-slug}/`) -- company research, talking points, question prep
- **Job search log** (`job-search-log.md`) -- live pipeline tracker, updated daily by Tasklet
- **Cowork handoff** (`claude-cowork-handoff.md`) -- workflow instructions for Claude Cowork's application builder
- **Infrastructure docs** (`infrastructure.md`, `assumptions.md`, `changelog.md`)

---

## Tool Ecosystem

Three tools work in this repo. Each has a distinct job:

| Tool | Job | Runs |
|---|---|---|
| **Tasklet** (tasklet.ai) | Daily job scan, scoring, dedup, pipeline database, digest email, repo updates | Daily 7:00am AEST (automated) |
| **Claude Cowork** | Application pack production (cover letter, tailored CV, brief) for roles Tasklet surfaces | Daily 7:30am AEST (automated), reads `job-search-log.md` |
| **Claude Code** | Ad hoc work: interview prep, repo maintenance, site updates, CV refinements | On demand |

**Do not duplicate work across tools.** Tasklet owns scanning. Cowork owns application production. Claude Code handles everything else.

---

## Who Is Yann Burden?

**Current position:** Venture Partner (AI Advisory) at Black Nova VC. Advising early-stage portfolio companies on AI strategy, product development and go-to-market.

**Career arc in three acts:**

1. **Billcap (2011--2018) -- CEO & Co-founder.** Built a vertical ML customer engagement platform for energy retailers. Smart meter data powering personalised engagement that drove retention and cross-sell. Five enterprise clients, 250,000+ end users. Hands-on technical founder -- prototyped ML models in R. Established the University of Melbourne research partnership. Sold to Tally Group for 8x investor return.

2. **DC Power Co (2018--2021) -- Chief Customer & Technology Officer.** Founding exec team. Built the tech stack and customer operations from scratch. Reduced churn 25% with predictive models. NPS +51 in year one. Acquired by Ion Group.

3. **Pendula (2021--2025) -- CPTO then Chief AI Officer.** Led 20-person engineering, product and data team. Shipped agentic customer communication workflows in production -- intent classification, next-best-action recommendation, conversational AI agents using LLMs with RAG. Unlocked $5M incremental ARR. Opened London office. Led AI strategy through acquisition by Smart Communications.

**Education:** BSc Information Systems, University of Melbourne (1998)

**Earlier career:** Accenture (UK, CIO advisory/telco strategy), Unilog (France, energy retail consulting), PwC Consulting (SAP), Cool Nrg (France, GM, national energy efficiency programmes), Energy Return (founder, energy efficiency consulting for local government).

**Advisory/boards:** Black Nova VC (Venture Partner, 2025--), University of Melbourne Honorary Industry Fellow (2015--2024), Bendigo Bank Community Bank NED (2010--2017).

**Working rights:** Australia, UK, EU, Canada.

**Contact:** +61 400 941 979 | yannburden@gmail.com | energyreturn.co/me | linkedin.com/in/yannburden

**Salary floor:** $250K AUD. Do not surface or progress any role confirmed below this.

---

## Writing Rules

Apply to ALL output produced in this repo.

- Australian English throughout (organisation, not organization; personalised, not personalized; colour, not color)
- No em dashes. Use " -- " (space-dash-dash-space) or restructure the sentence.
- No emojis unless explicitly asked.
- Tone: direct, confident, not sycophantic. Show, don't tell. Lead with outcomes, not adjectives.

---

## Framing Rules

These have been corrected during prior work. Get them right every time.

1. **Billcap's value proposition was customer engagement, not research.** It was a vertical ML platform for energy retailers that drove retention and cross-sell through personalised engagement. The research partnership with Melbourne Uni is interesting supporting evidence, not the headline.

2. **Billcap was pre-LLM.** ML (outlier detection, consumption prediction, behavioural segmentation), not agentic, not generative AI. Do not describe Billcap as "agentic."

3. **Pendula WAS agentic.** Agentic workflows in production -- orchestrating real customer conversations with ML-driven decisioning. Conversational AI agents, intent classification, RAG. This is the legitimate agentic credential.

4. **"8x investor return" is the key metric** for Billcap's exit. Use it.

5. **"Sold to Tally Group"** -- name the acquirer.

6. **"Acquired by Smart Communications"** -- name the acquirer for Pendula.

7. **DC Power Co was acquired by Ion Group** -- include this.

8. **Black Nova VC advisory is current.** Position it as "sharpening my thinking on what separates AI strategy from shipped AI products" -- not as idle time between roles.

9. **Research publications** are credibility signals, not the value proposition. Use them to demonstrate rigour, not as the lead story:
   - "Tell Me Something I Don't Already Know" -- Byrne, La Nauze, Martin. *Review of Economics and Statistics*, 2018. (Top-5 economics journal)
   - "An Experimental Study of Monthly Electricity Demand (In)elasticity" -- Byrne, La Nauze, Martin. *The Energy Journal*, 2021.
   - "Power from the People" -- La Nauze. *Journal of Political Economy*, 2019. (Top-5 economics journal. Explicitly acknowledges "Yann Burden and Billcap for access to proprietary data.")

10. **Press coverage** -- use as credibility signals:
    - SmartCompany (Aug 2023): Named and pictured as CGO in coverage of Pendula's $14.5M raise
    - Tech Trajectory Podcast, DiUS (Jul 2025): AI adoption, product strategy, customer clarity
    - Domain.com.au (Oct 2017): Quoted as expert on behavioural change in energy consumption

---

## Key Proof Points (Use These)

| Metric | Context |
|---|---|
| 8x investor return | Billcap exit to Tally Group |
| $5M incremental ARR | Pendula enterprise pivot |
| 25% churn reduction | DC Power Co predictive models |
| NPS +51 in year one | DC Power Co |
| 250,000+ end users | Billcap platform scale |
| 5 enterprise clients | Billcap multi-tenant SaaS |
| 20-person team | Pendula engineering, product, data |
| 3 peer-reviewed papers | Melbourne Uni research collaboration |
| 3 exits (Billcap, DC Power Co, Pendula) | All acquired |

---

## Podcast

**Tech Trajectory with Kavita Karwar** (DiUS, July 2025)
- Episode: https://techtrajectorypodcast.buzzsprout.com/2457356/episodes/17422143-leading-with-customer-clarity-in-a-changing-world
- Also on Spotify
- Full transcript: `media/tech-trajectory-transcript.txt`
- Topics: AI adoption, product strategy, customer clarity, what separates shipped AI from strategy decks

---

## Interview Prep Conventions

When creating interview prep, save to `interview-prep/{company-slug}/` with:
- `prep.md` -- the main prep document (company research, talking points, questions, risks)
- `people.md` -- interviewer/stakeholder profiles if researched
- Any additional reference material

Structure each `prep.md` as:
1. **Company snapshot** -- what they do, stage, recent news, strategy signals
2. **Why this role exists now** -- the intelligence behind the hire
3. **Experience mapping** -- specific proof points from my career mapped to their needs (not generic claims)
4. **My angle** -- the single most compelling thing I bring that other candidates probably don't
5. **Questions I should ask** -- thoughtful, specific, showing I've done the homework
6. **Questions they'll likely ask me** -- with suggested framing for each
7. **Gaps and risks** -- anything to address proactively, with a plan for each
