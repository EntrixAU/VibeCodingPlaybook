---
title: Vibe Coding – Let's Do It Right
author: Sam Fernando (CTO, Opex Consulting)
theme: default
paginate: true
---

# Slide 1: Vibe Coding  
### Let's Do It Right

- Presented by **Sam Fernando**  
- CTO, Opex Consulting  
- ACS Forum  

---

# Slide 2: Agenda

1. What is Vibe Coding?  
2. The Evolution of Developer Assistance  
3. The Perils & Pitfalls of AI-assisted Development  
4. Best Practices & Guardrails  
5. Governance Patterns  
6. House Rules for AI Agents  
7. Practical Principles in Action  
8. The Tools & Ecosystem  
9. Adoption Framework for Enterprises  
10. Cheat Sheet Recap & Key Takeaways  

---

# Slide 3: What is Vibe Coding?

- A **methodology**: blending AI-driven suggestions with **human creativity**  
- Goes beyond autocomplete: structured collaboration between devs & AI  
- Benefits:  
  - Accelerates **productivity**  
  - Unlocks **new ways of thinking**  
  - Helps enforce **standards** when governed correctly  
- Challenges:  
  - Risk of blind trust in AI  
  - Governance overhead if ignored  

> Speaker Note: Think of vibe coding like pair programming, but your partner is an AI that never sleeps and knows “something” about everything — but not always the right thing.  

---

# Slide 4: The Evolution of Developer Assistance

- **2000s: IntelliSense & IDE Tooling**  
  - Context-aware autocomplete in Visual Studio / Eclipse  
  - Static analyzers like ReSharper, SonarLint  

- **2010s: Smarter Code Generation**  
  - StackOverflow copy-paste culture  
  - Early code snippets and boilerplate generators  

- **2020s: AI Pair Programming**  
  - GitHub Copilot, Cursor, Replit Ghostwriter  
  - Contextual completions trained on billions of lines  

- **Now (2025): Agentic Development**  
  - Model Context Protocol (MCP) for tool orchestration  
  - Agents with memory, multi-step reasoning, and tool use  
  - Integrating with Jira, GitHub, AWS pipelines  

- **Future:** AI dev teams → self-managing, but with human oversight  

---

# Slide 5: Peril 1 – False Sense of Security

- **Misconception:** “AI always knows best”  
- Reality: AI generates plausible, not always correct, solutions  
- Risks:  
  - Security flaws (e.g., SQL injection, weak crypto)  
  - Performance bottlenecks (inefficient queries, poor scaling)  
  - Compliance gaps (GDPR / Privacy / Financial regulations)  

**Example:**  
AI suggests logging sensitive PII into console output → works in dev, but catastrophic in production.  

---

# Slide 6: Peril 2 – Shadow Code & IP Risks

- AI may generate **code snippets of unknown origin**  
- **IP Ownership Uncertainty**: Who owns AI-generated code?  
- OSS Licensing issues: GPL vs MIT vs proprietary  
- “Invisible contributions” → future legal disputes  

**Scenario:**  
An AI introduces a function very similar to GPL-licensed code. Six months later, during due diligence for investment, lawyers flag it → halts funding.  

---

# Slide 7: Peril 3 – Security Blind Spots

- Attack vectors introduced via AI coding:  
  - **Prompt Injection**: malicious text hijacks agent instructions  
  - **Data Leakage**: secrets exposed in context windows  
  - **Dependency Pollution**: AI picks insecure NPM/PyPI packages  
- Multi-agent risks:  
  - Over-permissioned bots acting like “insider threats”  
  - No monitoring of agent-to-agent chatter  

**Real-world risk:**  
AI chooses `leftpad-2.0` from an unknown repo because it “looked useful.” Package later injects crypto-miners.  

---

# Slide 8: Peril 4 – Governance & Compliance Drift

- AI-driven workflows **break traditional governance models**:  
  - How do you evidence **ISO 27001** alignment if AI wrote 50% of code?  
  - SOC2 / CPS234 auditors require traceability → who authored what?  
- Challenges:  
  - Prompts aren’t versioned  
  - Outputs not logged  
  - Test coverage inconsistent  
- Risk of **compliance audit failures**  

---

# Slide 9: Best Practices to Overcome Perils

- **Pair Programming with AI**  
  - AI generates → Human reviews → CI/CD validates  
- **Prompt Version Control**  
  - Store prompts alongside source code (like spec.md)  
- **Secure DevSecOps Pipelines**  
  - Static analysis, dependency scanning, SBOMs  
- **Prompt Engineering Hygiene**  
  - Avoid secrets in prompts  
  - Mask data before passing to agents  
- **Audit Trails & Logging**  
  - Capture what AI suggested and who approved it  

---

# Slide 10: Practical Best Practices (Examples)

**Using Cursor IDE:**  
- Restrict context: avoid exposing entire repo  
- Use “Explain Code” mode before trusting suggestions  
- Enable diff previews before committing AI code  

**Using GitHub Copilot:**  
- Disable Copilot for sensitive files (e.g., credentials, compliance logic)  
- Use Copilot Chat to query reasoning, not just generate code  
- Enforce repo policies that mandate PR review of AI commits  

**Tool-agnostic Rules:**  
- Never auto-merge AI commits  
- Run SAST/DAST scans on AI-generated code  
- Add SBOM scanning to pipelines (e.g., Grype, Trivy)  

---

# Slide 11: AI Agent House Rules

A good agent is like a junior dev — needs clear rules.  

1. **Never self-deploy.** Only suggest code.  
2. **No secrets in prompts.** Redact, mask, or use synthetic data.  
3. **Always log actions.** Capture queries, outputs, decisions.  
4. **Ask “why” not just “what.”** Encourage devs to challenge outputs.  
5. **Restrict permissions.** Agents = least-privilege accounts.  
6. **Human in the loop.** Mandatory sign-off before merging or shipping.  

---

# Slide 12: Governance Patterns

- **Spec-Driven Development**  
  - GitHub **spec.md / spec flow**  
  - Jira stories linked to specs for traceability  
- **Policy as Code**  
  - Use tools like OPA (Open Policy Agent) or Conftest  
- **Audit-Ready Logs**  
  - Keep prompt-output pairs for compliance review  
- **Connector Patterns**  
  - Securely integrate AI agents with external APIs (OAuth2, API gateways)  
- **Review Boards**  
  - Treat AI output like external vendor deliverables → needs review sign-off  

---

# Slide 13: Adoption Framework

**Stage 1 – Experiment**  
- Pilot Copilot, Cursor, Claude with dev volunteers  

**Stage 2 – Define Guardrails**  
- Establish house rules, prompt patterns, do/don’t examples  

**Stage 3 – Governance Layer**  
- Logging, auditing, review boards, security controls  

**Stage 4 – Integration**  
- Embed AI into CI/CD  
- Ensure DevSecOps pipelines validate all AI code  

**Stage 5 – Maturity**  
- AI becomes trusted co-developer  
- Agents integrated across Jira, GitHub, AWS, testing, deployment  

---

# Slide 14: Tools & Ecosystem

- **IDE Plugins**: Cursor, Copilot, Codeium  
- **Agent Frameworks**: MCP, Bedrock Agents, LangChain  
- **Governance Tools**: Trivy, Grype, Snyk, OpenPolicyAgent  
- **Audit & Logging**: Loki, ELK, Supabase activity logs  
- **Secure Context Managers**: Secrets Manager, Vault  

---

# Slide 15: Interactive Cheat Sheet

✅ **Best Practices**  
- Always review AI code  
- Version-control prompts  
- Secure pipelines (SAST/DAST/SBOM)  

✅ **House Rules**  
- No secrets in prompts  
- Mandatory logging  
- Least privilege for agents  

✅ **Governance Patterns**  
- Spec-driven Jira workflows  
- Policy as code  
- Audit-ready logs  

✅ **Peril Mitigation**  
- Red-team prompts  
- Run dependency scans  
- Human-in-loop approvals  

---

# Slide 16: Closing Thoughts

- Vibe coding is **inevitable** → either you shape it, or it shapes you  
- With guardrails, it can **supercharge teams**  
- Without governance, it creates **shadow risks**  
- The future is **humans + AI agents** → co-pilots, not replacements  

---

# Slide 17: Thank You

**Sam Fernando**  
CTO, Opex Consulting  
📧 sam@opexconsulting.com.au  

---
