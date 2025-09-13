---
layout: page
title: "Cheat Sheet & Quick Reference"
permalink: /cheat-sheet/
---

# Interactive Cheat Sheet & Quick Reference

Your go-to reference for daily AI-assisted development. Bookmark this page for quick access to essential guidelines, commands, and best practices.

## ✅ Best Practices Quick Reference

### The Golden Rules
```markdown
1. AI generates → Human reviews → CI/CD validates
2. Never auto-merge AI commits
3. Always version control prompts
4. Mask secrets before AI interaction
5. Log everything for audit trails
```

### Daily Workflow Checklist
- [ ] Review AI suggestions before accepting
- [ ] Run security scans on AI-generated code
- [ ] Document prompt used for future reference
- [ ] Ensure test coverage meets standards
- [ ] Verify compliance with coding standards

---

## 🏠 House Rules for AI Agents

### Core Principles
| Rule | Description | Example |
|------|-------------|---------|
| **Never Self-Deploy** | AI suggests, humans deploy | AI creates PR, human merges |
| **No Secrets in Prompts** | Always mask sensitive data | Use `***` for passwords |
| **Mandatory Logging** | Log all AI interactions | Every prompt/response recorded |
| **Ask "Why" Not "What"** | Challenge AI reasoning | "Why this approach?" |
| **Least Privilege** | Minimal AI permissions | Read-only access by default |
| **Human in Loop** | Mandatory approval gates | No autonomous deployments |

### Quick Security Checks
```bash
# Before using AI assistance
✅ Are secrets masked?
✅ Is context appropriate?
✅ Will output be reviewed?
✅ Are logs being captured?
✅ Is this a sensitive system?
```

---

## 🛡️ Security Quick Commands

### Scan AI-Generated Code
```bash
# Vulnerability scanning
trivy fs --severity HIGH,CRITICAL .
grype .
snyk test

# Secret detection
truffleHog git file://. --only-verified
git-secrets --scan

# SAST scanning
semgrep --config=auto .
codeql database analyze
```

### Policy Validation
```bash
# Check OPA policies
conftest verify --policy .policies/ .

# Validate configuration
conftest test --policy security.rego deployment.yaml

# Check compliance
opa eval -d policies/ "data.security.allow" -i input.json
```

---

## 📝 Prompt Templates

### Secure Code Generation
```markdown
## Template: Authentication Code
Create [FUNCTIONALITY] with these security requirements:
- Input validation for all parameters
- Parameterized queries (no string concatenation)
- Proper error handling with logging
- Rate limiting: [X] requests per [TIME]
- Use environment variables for secrets
- Include comprehensive unit tests

Security considerations:
- [SPECIFIC SECURITY REQUIREMENTS]
- [COMPLIANCE REQUIREMENTS]
```

### Database Operations
```markdown
## Template: Database Code
Generate database operations for [ENTITY] with:
- Parameterized queries only
- Transaction management
- Connection pooling
- Input sanitization
- Error handling with audit logging
- Performance optimization
- Include migration scripts if needed

Data sensitivity: [PUBLIC/INTERNAL/CONFIDENTIAL]
Compliance requirements: [GDPR/SOX/HIPAA/etc]
```

### API Development
```markdown
## Template: API Endpoint
Create REST API endpoint for [RESOURCE] with:
- OpenAPI/Swagger documentation
- Input validation middleware
- Authentication/authorization checks
- Rate limiting
- Comprehensive error responses
- Audit logging for sensitive operations
- Unit and integration tests

Security level: [LOW/MEDIUM/HIGH]
Data classification: [PUBLIC/INTERNAL/RESTRICTED]
```

---

## 🔍 Code Review Checklist

### AI-Generated Code Review
```markdown
## Security Review
- [ ] No hardcoded secrets or credentials
- [ ] Input validation on all user inputs
- [ ] Proper error handling (no info leakage)
- [ ] Authentication/authorization checks
- [ ] SQL injection prevention
- [ ] XSS prevention measures
- [ ] CSRF protection where applicable

## Quality Review
- [ ] Code follows team standards
- [ ] Proper documentation/comments
- [ ] Unit tests included
- [ ] Performance considerations addressed
- [ ] Error scenarios handled
- [ ] Logging appropriately implemented

## Compliance Review
- [ ] Audit trail requirements met
- [ ] Data privacy regulations followed
- [ ] Retention policies respected
- [ ] Access controls properly implemented
```

---

## 🚨 Incident Response

### AI Code Security Incident
```markdown
## Immediate Actions (0-1 hour)
1. Identify affected systems and code
2. Assess potential data exposure
3. Implement temporary mitigations
4. Notify security team and stakeholders

## Investigation (1-24 hours)
1. Analyze AI interaction logs
2. Review prompt and output history
3. Identify root cause
4. Assess blast radius

## Resolution (24-72 hours)
1. Implement permanent fixes
2. Update AI policies if needed
3. Conduct lessons learned session
4. Update training materials
```

### Emergency Contacts
```yaml
Security Team: security@company.com
AI Governance: ai-governance@company.com
Compliance: compliance@company.com
Legal: legal@company.com
```

---

## 📊 Monitoring & Metrics

### Key Performance Indicators
```markdown
## Productivity Metrics
- Lines of code per hour (before/after AI)
- Feature delivery velocity
- Time to first working prototype
- Code review cycle time

## Quality Metrics
- Bug reports per 1000 lines (AI vs human)
- Security vulnerabilities detected
- Test coverage percentage
- Code complexity scores

## Governance Metrics
- AI code review completion rate
- Policy compliance percentage
- Audit finding resolution time
- Training completion rates
```

### Dashboard Queries
```sql
-- AI code generation rate
SELECT DATE(timestamp), COUNT(*) as ai_generations
FROM ai_audit_logs 
WHERE event_type = 'code_generation'
GROUP BY DATE(timestamp);

-- Security issue detection
SELECT severity, COUNT(*) as issue_count
FROM security_scans 
WHERE ai_generated = true
GROUP BY severity;

-- Review approval rates
SELECT review_status, COUNT(*) as count
FROM code_reviews 
WHERE ai_assisted = true
GROUP BY review_status;
```

---

## 🛠️ Tool Configuration Quick Setup

### Cursor IDE
```json
{
  "cursor.ai.enabled": true,
  "cursor.ai.maxContextLength": 8000,
  "cursor.ai.excludePatterns": [
    "**/.env*",
    "**/secrets/**",
    "**/*.key",
    "**/node_modules/**"
  ],
  "cursor.ai.requireExplicitApproval": true,
  "cursor.ai.logInteractions": true
}
```

### GitHub Copilot Enterprise
```yaml
# .github/copilot.yml
suggestions:
  enabled: true
  exclude_paths:
    - "secrets/"
    - "*.env"
    - "credentials/"
    - "private/"
audit:
  log_suggestions: true
  log_acceptances: true
  retention_days: 365
policies:
  require_review: true
  block_secrets: true
  enforce_licensing: true
```

### Security Pipeline
```yaml
# .github/workflows/ai-security.yml
name: AI Code Security
on: [pull_request]
jobs:
  security:
    if: contains(github.event.head_commit.message, 'ai-generated')
    steps:
      - name: Security Scan
        run: |
          trivy fs --exit-code 1 .
          semgrep --config=auto --error .
          conftest verify --policy .policies/ .
```

---

## 📚 Quick Reference Links

### Internal Resources
- [Company AI Policy](link-to-internal-policy)
- [Security Escalation Procedures](link-to-security-docs)
- [Code Review Guidelines](link-to-review-guidelines)
- [Compliance Requirements](link-to-compliance-docs)

### External Tools
- [Trivy Documentation](https://trivy.dev/)
- [Semgrep Rules](https://semgrep.dev/explore)
- [OPA Policy Examples](https://www.openpolicyagent.org/docs/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

### Training Materials
- [AI Safety Training Module](link-to-training)
- [Secure Coding Guidelines](link-to-secure-coding)
- [Prompt Engineering Best Practices](link-to-prompt-training)

---

## 🎯 Common Scenarios

### Scenario 1: AI Suggests Insecure Code
```markdown
1. STOP - Don't accept the suggestion
2. Identify the security issue
3. Modify prompt to include security requirements
4. Re-generate with security context
5. Review and validate the new output
6. Document the learning for future prompts
```

### Scenario 2: Compliance Audit Request
```markdown
1. Gather AI interaction logs for requested period
2. Export code review records
3. Compile security scan results
4. Document policy compliance evidence
5. Prepare executive summary
6. Schedule audit presentation
```

### Scenario 3: Performance Issue in AI Code
```markdown
1. Profile the problematic code
2. Identify performance bottlenecks
3. Create performance-focused prompt
4. Generate optimized version
5. Benchmark before/after performance
6. Update performance guidelines
```

---

## 💡 Pro Tips

### Prompt Engineering
- **Be specific**: Vague prompts = unpredictable results
- **Include context**: Mention frameworks, patterns, constraints
- **Specify security**: Always include security requirements
- **Request tests**: Ask for unit tests with code generation
- **Iterate**: Refine prompts based on output quality

### Code Review
- **Focus on logic**: AI is good at syntax, check business logic
- **Verify edge cases**: AI may miss unusual scenarios  
- **Check integrations**: Ensure AI code works with existing systems
- **Validate assumptions**: Don't assume AI understands your domain
- **Test thoroughly**: AI code needs the same testing rigor

### Security
- **Defense in depth**: Multiple security layers, not just AI checks
- **Regular updates**: Keep security tools and policies current
- **Team training**: Ensure everyone understands AI security risks
- **Incident preparation**: Have response plans ready
- **Continuous monitoring**: Watch for new AI-related threats

---

*Need more detailed guidance? Return to the main sections: [Best Practices](best-practices.html) | [Governance](governance.html) | [Tools](tools.html)*

---

[← Previous: Adoption Framework](adoption.html) | [Back to Home](index.html)
