---
layout: page
title: "Best Practices & Guardrails"
permalink: /best-practices/
---

# Best Practices & Guardrails

Implementing AI-assisted development safely requires a comprehensive approach combining technical controls, process improvements, and cultural changes.

## 🤝 Core Principle: Pair Programming with AI

### The Golden Rule
**AI generates → Human reviews → CI/CD validates**

This three-stage process ensures that AI assistance enhances rather than replaces human judgment.

### Implementation:
1. **AI Suggestion Phase** - Let AI generate code proposals
2. **Human Review Phase** - Developer evaluates, modifies, approves
3. **Automated Validation** - CI/CD pipeline runs comprehensive checks

---

## 📝 Prompt Version Control

### Why It Matters
Prompts are instructions—they should be treated like specifications and versioned accordingly.

### Best Practice:
Store prompts alongside source code in files like `spec.md` or `requirements.txt`:

```markdown
## AI Prompt for Authentication Module
Create a JWT-based authentication system with:
- Token expiration (24 hours)
- Refresh token mechanism
- Rate limiting (5 attempts per minute)
- Secure password hashing (bcrypt, cost 12)
```

### Benefits:
- **Reproducible results** - Same prompt, same output
- **Change tracking** - See how requirements evolved
- **Audit compliance** - Clear record of what was requested

---

## 🔒 Secure DevSecOps Pipelines

### Essential Security Controls

#### Static Analysis Security Testing (SAST)
```yaml
# Example GitHub Actions workflow
- name: Run SAST Scan
  uses: github/super-linter@v4
  with:
    default_branch: main
    github_token: ${{ secrets.GITHUB_TOKEN }}
```

#### Dependency Scanning
- **Software Bill of Materials (SBOM)** generation
- **Vulnerability scanning** with tools like Grype, Trivy
- **License compliance** checking

#### Dynamic Analysis
- **Runtime security testing** in staging environments
- **Penetration testing** of AI-generated endpoints
- **Performance impact assessment**

---

## 🧹 Prompt Engineering Hygiene

### Security Rules

#### Never Include Secrets in Prompts
```bash
# ❌ BAD
"Create a database connection using password 'prod_secret_123'"

# ✅ GOOD  
"Create a database connection using environment variable for password"
```

#### Mask Sensitive Data
```python
# Before sending to AI
user_data = {
    "email": "user@example.com",
    "ssn": "***-**-1234",  # Masked
    "account": "****5678"   # Masked
}
```

#### Use Synthetic Data
Generate realistic but fake data for AI training and testing scenarios.

---

## 📊 Audit Trails & Logging

### What to Log
- **Prompt inputs** - What was requested
- **AI outputs** - What was generated
- **Human decisions** - What was approved/rejected
- **Modifications** - How outputs were changed
- **Deployment status** - What made it to production

### Example Log Entry
```json
{
  "timestamp": "2025-01-15T10:30:00Z",
  "developer": "john.doe@company.com",
  "ai_model": "claude-3.5-sonnet",
  "prompt_hash": "sha256:abc123...",
  "output_hash": "sha256:def456...",
  "review_status": "approved_with_modifications",
  "modifications": ["added input validation", "updated error handling"],
  "deployment": "pending_qa"
}
```

---

## 🛠️ Tool-Specific Best Practices

### Using Cursor IDE

#### Context Management
- **Restrict context scope** - Don't expose entire repository
- **Use `.cursorignore`** to exclude sensitive files
- **Enable diff previews** before accepting suggestions

#### Safety Features
```bash
# .cursorignore example
.env
secrets/
credentials/
*.key
*.pem
```

### Using GitHub Copilot

#### Security Configuration
- **Disable for sensitive files** (credentials, compliance logic)
- **Use Copilot Chat** to understand reasoning, not just generate code
- **Enforce PR review policies** for all AI-assisted commits

#### Repository Policies
```yaml
# .github/branch_protection.yml
required_reviews: 2
dismiss_stale_reviews: true
require_code_owner_reviews: true
ai_generated_code_review: mandatory
```

---

## 🚫 Universal Rules (Tool-Agnostic)

### Never Auto-Merge AI Commits
Every AI-generated change must pass human review, regardless of how trivial it appears.

### Comprehensive Scanning Pipeline
```yaml
security_pipeline:
  - sast_scan: "sonarqube, semgrep"
  - dast_scan: "owasp-zap, burp"
  - dependency_scan: "grype, trivy, snyk"
  - license_check: "fossa, licensefinder"
  - sbom_generation: "syft, cyclonedx"
```

### Mandatory Security Gates
- All AI code must pass security scans
- Performance benchmarks must be maintained
- Test coverage cannot decrease
- Documentation must be updated

---

## 🎯 Implementation Checklist

### Phase 1: Foundation
- [ ] Establish AI code review process
- [ ] Configure security scanning tools
- [ ] Create prompt versioning system
- [ ] Set up audit logging

### Phase 2: Integration
- [ ] Integrate with CI/CD pipeline
- [ ] Train team on best practices
- [ ] Establish governance policies
- [ ] Create incident response procedures

### Phase 3: Optimization
- [ ] Monitor and measure effectiveness
- [ ] Refine processes based on experience
- [ ] Expand to additional teams
- [ ] Continuous improvement cycle

---

Ready to implement governance frameworks? Continue to [Governance Patterns](governance.html).

---

[← Previous: Perils](perils.html) | [Next: Governance →](governance.html)
