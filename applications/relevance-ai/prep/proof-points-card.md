# Proof Points Card

One-line triggers. Story lives in your head. Glance, pick, tell.

---

**Pendula AI on rails** → deterministic workflow engine, marketers drop in LLM nodes, before-and-after measurement on whether the agent uplifts. Same architectural bet as Relevance.

**Pendula 100k loop** → agent did the right thing 100,000 times overnight. Burned a meaningful fraction of monthly LLM budget. Rebuilt observability with per-workflow cost ceilings, anomaly alerts, circuit breakers.

**Three-iteration permission contract** → permission model for agents took three goes. v1 too permissive (security said no). v2 too restrictive (agents couldn't work). v3 = scoped agent credentials, workflow author declares scope, admin approves, audit-logged separately.

**Graduated approval thresholds** → single threshold killed throughput when too high, caused agent-to-spam disasters when too low. Landed on confidence × action class × recipient class. Three variables, not one.

**Workflow versioning Google-Docs-not-Git** → marketers edit live workflows, break customer journeys mid-flight. Built version-on-publish with one-click rollback. Power users wanted Git. Mainstream wanted Google Docs.

**Pendula UK data residency** → expanded to UK in part for EU data residency. Easy: AWS regions. Hard: LLM provider chain (model, embeddings, eval logs all in-region). Built a routing layer that refuses out-of-region calls.

**Monthly PS-to-platform forum** → professional services forking the product per customer. Instituted monthly forum: top 3 customisations per month, three rules — needed by >2 customers = platform; vertical = template; truly bespoke = customer pays. Cleared the fork problem in two quarters.

**30-60-90 enterprise onboarding** → 3 months to close, 4-6 months to first production = killing expansion. Built FDE-pairing playbook: 30 days pair with customer RevOps lead, 60 days customer self-sufficient, 90 days QBR metrics. Cut time-to-meaningful-usage by more than half.

**Billcap multi-tenant** → 5 enterprise clients, each with distinct security/regulatory/operational reqs. 250k+ end users. 8x exit. The pattern Relevance is now scaling.

**Black Nova lens** → "build cost is falling, judgment premium is rising. What not to build is the binding constraint." Use this for AI thesis questions.

---

## The phrase
> "I have lived in each of these problems. Not abstractly — the version with the customer escalation that landed on a Friday afternoon."

Use once if it fits.
