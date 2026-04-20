# Surface Area Card

The seven components of the enterprise product surface. Pick what the conversation invites. Ask the shake-out question.

---

**1. Permissioning & access control**
> "How is your permission model layered between user and agent? Is admin approval per-agent, per-workflow, or per-action?"

**2. Audit, observability, cost attribution**
> "What's your circuit breaker pattern when an agent goes into a loop or burns budget unexpectedly? Cost attribution real-time or end-of-day?"

**5. Multi-region deployment & data residency**
> "How are you handling LLM provider data residency in your multi-region story — provider-by-provider, or a uniform routing layer?"

**3. Approval workflows & human-in-the-loop**
> "How is your Approval Mode configured today — single threshold, graduated, or fully custom? What feedback loop do you have for customers who set it wrong?"

**4. Version control, rollback, change management**
> "Is version control surfaced to the canvas user, or only to power users? What happens to in-flight agent runs when an agent gets updated?"

**6. Forward Deployed Engineer to platform feedback loop**
> "Is there a cadenced forum where FDE work converts into product roadmap, or is it ad hoc?"

**7. Enterprise deployment & onboarding**
> "What does your average enterprise time-to-first-production-agent look like, and where is the friction? My read is that's a leading indicator for enterprise NRR."

---

## My read on the highest-leverage two

**6 (FDE feedback loop) and 4 (version control)** unblock everything else.
**3 (approval workflows)** is the L2-to-L3 unlock for SuperGTM.
**7 (enterprise deployment)** is the highest commercial leverage.

If asked "what would you focus on?" → 6 + 3, framed as "I'd want to test that against your roadmap."
