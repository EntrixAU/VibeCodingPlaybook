---
layout: page
title: "(03) Best Practices & Guardrails"
permalink: /best-practices/
---

# Best Practices & Guardrails - Do it Right!

Transform AI-assisted development from a risk into a competitive advantage with proven strategies, security measures, and governance frameworks. Your comprehensive guide to safe, effective AI-assisted development.

## 🎯 The Golden Rules

### Quick Reference: Do's and Don'ts

| ✅ **DO** | ❌ **DON'T** |
|-----------|-------------|
| Always review AI-generated code | Auto-merge AI commits |
| Version control your prompts | Put secrets in prompts |
| Run security scans on AI code | Trust AI output blindly |
| Use instruction files for context | Let AI access entire codebase |
| Test AI-generated code thoroughly | Skip human review steps |
| Log all AI interactions | Deploy without validation |

### Rule #1: AI Generates → Human Reviews → CI/CD Validates
**Never skip the human in the loop**


### Rule #2: Trust but Verify
**Every AI suggestion needs validation**

### The Golden Rules Quick Reference
```markdown
1. AI generates → Human reviews → CI/CD validates
2. Never auto-merge AI commits
3. Always version control prompts
4. Mask secrets before AI interaction
5. Log everything for audit trails
```

---

## 🏆 Essential Best Practices

### 🚫 Avoid Building Entire Features
**Focus on components, not complete systems**

- **Build repeated code**: utility functions, helper functions, reusable code, templates, CRUD functions
- **Don't build**: complete applications or complex business logic end-to-end
- **Why**: AI excels at patterns but struggles with complex system architecture and business rules

### 👥 Keep the Humans in the Loop
**Maintain human oversight at critical points**

- **Code reviews**: Every AI-generated piece needs human review
- **Security scans**: Automated scanning for vulnerabilities and secrets
- **CI/CD pipeline changes**: Human approval for deployment configurations
- **Command execution**: Human verification before running AI-suggested commands

### 🔍 Use Scanning Features
**Leverage automated security and quality checks**

- **Scan code for**: leaked secrets, vulnerabilities, dependency issues
- **Integrate into CI/CD**: Make scanning part of your automated pipeline
- **Regular audits**: Schedule periodic comprehensive scans
- **Alert on findings**: Immediate notification for critical issues

### 🎯 Craft Instructions Unique to You
**Customize AI prompts for your specific context**

- **Craft instruction files**: Create prompts tailored to your tech stack, app, and software standards
- **Include context**: Specify frameworks, coding standards, security requirements
- **Version control prompts**: Treat prompts like code - version and review them
- **Team consistency**: Share effective prompts across the team

### 🔒 Secure Prompting Patterns
**Train developers and AI to use secure practices**

- **Train developers**: Education on secure prompting techniques
- **Train AI via instructions**: Use system prompts to enforce security patterns
- **Consistent patterns**: Establish standard secure prompting templates
- **Regular updates**: Keep security patterns current with evolving threats

### 📋 Spec-Driven Prompts
**Break requirements into smaller, manageable specifications**

- **Break requirements**: Decompose large features into smaller, specific tasks
- **Requirement documents**: Create detailed specifications before prompting
- **Use in prompts**: Reference specs directly in AI instructions
- **Iterative refinement**: Improve specs based on AI output quality

### 🤖 AI for Non-Coding Tasks
**Leverage AI beyond just code generation**

- **Code explaining**: Use AI to document and explain existing code
- **Refactoring opportunities**: Identify areas for improvement
- **Architecture review**: Get suggestions for system design improvements
- **Documentation**: Generate and maintain technical documentation

### 📦 Modularise the Code
**Structure code for AI effectiveness**

- **Smaller segments**: Break codebase into manageable modules
- **Clear boundaries**: Define clear interfaces between components
- **Why**: LLMs struggle with large codebases and high line counts (LoC)
- **Better context**: Smaller modules provide clearer context for AI

### 🧠 Pick the Model That Works for the Task
**Choose the right AI tool for each specific need**

| Task Type | Recommended Model | Why |
|-----------|------------------|-----|
| **Code Generation** | GPT-4, Claude-3.5-Sonnet | Strong reasoning and code quality |
| **Code Explanation** | GPT-4, Claude-3 | Excellent at breaking down complex logic |
| **Refactoring** | Claude-3.5-Sonnet | Good at maintaining code structure |
| **Documentation** | GPT-4, Claude-3 | Natural language excellence |
| **Security Review** | Specialized tools + GPT-4 | Domain-specific knowledge needed |

### 📋 Plan Before Making Changes
**Use structured approaches for AI-assisted development**

- **Ask or chat mode first**: Clarify the plan of attack before implementation
- **Understand the approach**: Ensure AI's strategy aligns with your goals
- **Then use agent mode**: Execute the planned approach systematically
- **Iterative refinement**: Adjust plan based on results

### ⚠️ Fail Safe Mechanisms
**Build in controls and rollback capabilities**

- **Control mechanisms**: Ways to stop or pause AI agent workflows
- **Shutdown procedures**: Clear process to halt AI operations if needed
- **Enable logs**: Comprehensive logging of all AI actions
- **Rollback controls**: Ability to undo AI-generated changes quickly
- **Circuit breakers**: Automatic stops for suspicious or dangerous operations

---

## 🏠 AI Agent House Rules

A good agent is like a junior dev — needs clear rules. These house rules ensure AI agents operate safely and effectively within your organization.

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

### 🔒 Isolate Tasks
**Limit context window for focused execution**

- **Limit context window**: Longer chats become more confusing for AI
- **Custom instructions**: Provide specific, targeted guidance for each task
- **Task boundaries**: Define clear scope and limitations
- **Context management**: Keep conversations focused and relevant

### 💬 AI Comments
**Ensure generated code is readable and reviewable**

- **Comment everything**: AI should comment all the way through
- **Readable code**: Ensure generated code is self-documenting
- **Review manually**: Human verification of AI comments and logic
- **Documentation standards**: Maintain consistent commenting style

### 🔐 AI Permission Control
**Strict access control for AI agents**

- **Clear boundary to operate under**: Define exactly what AI can and cannot do
- **Blacklisting/whitelisting**: Explicit allow/deny lists for commands and tasks
- **No access to production resources**: Strict separation of environments
- **Control tasks AI can and can't do**: Granular permission management

### 🔄 AI Feedback Loop
**Continuous improvement through structured feedback**

- **Prompt → Code → Critique → Repeat**: Iterative refinement process
- **Quality gates**: Automated checks at each stage
- **Human oversight**: Regular human intervention points
- **Learning incorporation**: Use feedback to improve future prompts

---

## 🔒 DevSecOps Strategies for Safe Use of AI

Implementing comprehensive DevSecOps strategies ensures AI-assisted development maintains security, compliance, and quality throughout the development lifecycle.

### 🔍 Automated Security Scanning
**Comprehensive security validation at every stage**

#### SAST (Static Application Security Testing)
```yaml
# Example SAST configuration for AI code
sast_config:
  tools:
    - semgrep:
        rules: ["security", "ai-specific-rules"]
        fail_on: ["ERROR", "WARNING"]
    - sonarqube:
        quality_gate: "ai-code-standards"
        coverage_threshold: 80
    - codeql:
        languages: ["javascript", "python", "java"]
        queries: ["security-and-quality"]
```

#### Security Scanning Commands
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

#### Policy Validation
```bash
# Check OPA policies
conftest verify --policy .policies/ .

# Validate configuration
conftest test --policy security.rego deployment.yaml

# Check compliance
opa eval -d policies/ "data.security.allow" -i input.json
```

### 📋 Requirements as Code
**Codify and automate compliance requirements**

#### ADRs (Architecture Decision Records)
```markdown
# ADR-001: AI Code Generation Standards

## Status
Accepted

## Context
We need standardized approaches for AI code generation to ensure consistency and security.

## Decision
- Use GPT-4 for complex business logic generation
- Implement mandatory security review for AI authentication code
- Require 90% test coverage for AI-generated functions

## Consequences
- Improved code quality and security
- Increased review overhead initially
- Better long-term maintainability
```

### ➕ Augmented Reviews
**AI-enhanced code review processes**

#### AI PR Summaries
```markdown
# AI-Generated PR Summary

## Changes Overview
This PR implements user authentication using AI-generated code.

## AI Code Analysis
- **Files modified**: 3 (auth.py, tests/test_auth.py, docs/auth.md)
- **Security implications**: HIGH - implements password hashing and session management
- **Test coverage**: 92% (exceeds 80% requirement)
- **Complexity score**: Medium

## Review Focus Areas
1. **Security Review Required**: Password hashing implementation
2. **Performance Impact**: Database query optimisation needed
3. **Compliance Check**: GDPR compliance for user data handling

## Automated Checks Status
- ✅ SAST scan passed
- ✅ Secret scanning passed
- ✅ Dependency scan passed
- ⚠️ Performance test requires attention
```

### 🚫 Policy as Code
**Automated policy enforcement**

```python
# Example policy as code for AI compliance
class AICompliancePolicy:
    def validate_ai_code(self, code_metadata):
        violations = []
        
        # Check for mandatory security review
        if code_metadata.get('security_critical') and not code_metadata.get('security_reviewed'):
            violations.append("Security-critical AI code requires security team review")
        
        # Validate test coverage
        if code_metadata.get('test_coverage', 0) < 80:
            violations.append("AI-generated code requires minimum 80% test coverage")
        
        # Check for human review
        if code_metadata.get('ai_generated') and not code_metadata.get('human_reviewed'):
            violations.append("AI-generated code requires human review")
        
        return violations
```

---

## 📝 Prompt Templates & Examples

### Secure Code Generation Template
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

### Database Operations Template
```markdown
## Template: Database Code
Generate database operations for [ENTITY] with:
- Parameterized queries only
- Transaction management
- Connection pooling
- Input sanitization
- Error handling with audit logging
- Performance optimisation
- Include migration scripts if needed

Data sensitivity: [PUBLIC/INTERNAL/CONFIDENTIAL]
Compliance requirements: [GDPR/SOX/HIPAA/etc]
```

### API Development Template
```markdown
## Template: API Endpoint
Create REST API endpoint for [RESOURCE] with:
- OpenAPI/Swagger documentation
- Input validation middleware
- Authentication/authorisation checks
- Rate limiting
- Comprehensive error responses
- Audit logging for sensitive operations
- Unit and integration tests

Security level: [LOW/MEDIUM/HIGH]
Data classification: [PUBLIC/INTERNAL/RESTRICTED]
```

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

## 🔍 Code Review Checklist

### AI-Generated Code Review
```markdown
## Security Review
- [ ] No hardcoded secrets or credentials
- [ ] Input validation on all user inputs
- [ ] Proper error handling (no info leakage)
- [ ] Authentication/authorisation checks
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

## 📊 Daily Workflow Checklist

### Before Using AI
- [ ] Review AI suggestions before accepting
- [ ] Run security scans on AI-generated code
- [ ] Document prompt used for future reference
- [ ] Ensure test coverage meets standards
- [ ] Verify compliance with coding standards

### During Development
- [ ] Use appropriate AI model for the task
- [ ] Apply secure prompting patterns
- [ ] Maintain human oversight at critical points
- [ ] Log all AI interactions for audit

### After AI Generation
- [ ] Conduct thorough code review
- [ ] Run comprehensive security scans
- [ ] Validate against business requirements
- [ ] Update documentation and tests

---

## 🎯 Implementation Roadmap

### Phase 1: Foundation (Weeks 1-4)
- [ ] Implement basic SAST/SCA scanning
- [ ] Set up secret scanning and linting
- [ ] Create initial AI code review checklists
- [ ] Establish branch protection rules

### Phase 2: Enhancement (Weeks 5-8)
- [ ] Deploy DAST and infrastructure scanning
- [ ] Implement policy as code framework
- [ ] Create automated compliance checking
- [ ] Set up continuous monitoring

### Phase 3: Advanced Integration (Weeks 9-12)
- [ ] Deploy AI-augmented review processes
- [ ] Implement comprehensive SBOM generation
- [ ] Set up MCP addons for structured access
- [ ] Create advanced risk assessment algorithms

### Phase 4: Optimization (Ongoing)
- [ ] Refine policies based on learnings
- [ ] Optimize performance of security scans
- [ ] Enhance AI review assistance
- [ ] Continuous improvement of processes

---

## 💡 Pro Tips for Success

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

*This comprehensive guide combines best practices, security measures, and practical guidance for successful AI-assisted development. Bookmark this page for daily reference and continuous improvement of your AI development practices.*

---

**Navigation**: [← Common Habits](common-habits.html) | [Prompting Best Practices →](prompting-best-practices.html)