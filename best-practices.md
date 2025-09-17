---
title: "(03) Best Practices & Guardrails"
permalink: /best-practices/
---

_Practical rules and guardrails to make AI assistance safe, effective, and auditable._

Transform AI-assisted development from a risk into a competitive advantage with proven strategies, security measures, and governance frameworks. Your comprehensive guide to safe, effective AI-assisted development.

## Table of Contents

### Essential Best Practices
- [🚫 Avoid Building Entire Features](#-avoid-building-entire-features)
- [👥 Keep the Humans in the Loop](#-keep-the-humans-in-the-loop)
- [🔍 Use Scanning Features](#-use-scanning-features)
- [🎯 Craft Instructions Unique to You](#-craft-instructions-unique-to-you)
- [🔒 Secure Prompting Patterns](#-secure-prompting-patterns)
- [📋 Spec-Driven Prompts](#-spec-driven-prompts)
- [📝 AI-Generated Code Documentation](#-ai-generated-code-documentation)

### Implementation Guidance
- [🧾 Instruction Examples](#-instruction-examples)
- [🤖 AI for Non-Coding Tasks](#-ai-for-non-coding-tasks)
- [📦 Modularise the Code](#-modularise-the-code)
- [🧠 Pick the Model That Works for the Task](#-pick-the-model-that-works-for-the-task)
- [📋 Plan Before Making Changes](#-plan-before-making-changes)
- [⚠️ Fail Safe Mechanisms](#️-fail-safe-mechanisms)

### AI Coding Agent House Rules
- [🏠 AI Coding Agent House Rules](#-ai-coding-agent-house-rules)
- [🔒 Isolate Tasks](#-isolate-tasks)
- [💬 AI Comments](#-ai-comments)
- [🔐 AI Permission Control](#-ai-permission-control)
- [🔄 AI Feedback Loop](#-ai-feedback-loop)

### Security & Operations
- [🔒 DevSecOps Strategies for Safe Use of AI](#-devsecops-strategies-for-safe-use-of-ai)
- [🧹 Prompt Engineering Hygiene](#-prompt-engineering-hygiene)
- [📊 Audit Trails & Logging](#-audit-trails--logging)
- [🔍 Review Checklist](#-review-checklist)

### Practical Resources
- [💡 Pro Tips for Success](#-pro-tips-for-success)

---

## Essential Best Practices

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

## 🧾 Instruction Examples

Design a lightweight instruction file that sets boundaries, embeds context, and defines how the AI should behave. Keep it short, version it with your code, and link to it from prompts.

### What to include
- **Project/file/folder structure**: Where things live, naming conventions, and what goes where.
- **Architecture principles**: Non-negotiable patterns, layering rules, and non‑functional priorities.
- **Logging changes**: What the AI must report after making edits or proposing diffs.
- **Permission controls**: What the AI can and cannot do; environments it must not touch.
- **Clarify-if-uncertain policy**: Ask questions instead of assuming when requirements are unclear.
- **Role-driven workflow**: Humans guide and approve; AI proposes and executes within guardrails.
- **Comments**: What the AI can and cannot do; environments it must not touch.

### Example: Repository instruction file (YAML)
```yaml
# .ai/instructions.yml
context:
  language: typescript
  framework: nextjs
  testing: vitest
  formatting: prettier+eslint
  repo_structure:
    - src/: application code (domain → app → ui)
    - tests/: unit and integration tests
    - docs/: architecture and ADRs

architecture:
  principles:
    - dependency_inversion: ui → app → domain (no upward imports)
    - pure_functions_preferred: side effects isolated at boundaries
    - error_handling: never swallow errors; propagate with context
  non_functionals:
    - performance_first_render: avoid blocking data calls in critical paths
    - security_inputs: validate and sanitize all external inputs

permissions:
  allowed:
    - propose diffs via PR only
    - create tests and docs alongside code changes
  prohibited:
    - push to main
    - modify production infra or secrets
    - run destructive commands

workstyle:
  clarify_if_uncertain: true
  ask_before_installing_dependencies: true
  review_gates:
    human_review_required: true

logging:
  require:
    - list of files created/modified/deleted
    - rationale and risks for changes
    - follow-up actions and test coverage
```

### Example Instructions to create and use (or paste into prompts)

#### Project/file/folder structure
```markdown
Use this structure:
- src/domain/: business logic (no framework imports)
- src/app/: orchestration, services, adapters
- src/ui/: components and pages
Place new code accordingly and keep imports directional (ui→app→domain only).
```

#### Architecture principles
```markdown
Apply these rules:
- Prefer dependency inversion; depend on interfaces in domain
- Isolate side effects; pure functions by default
- Validate inputs at boundaries; never trust request data
Explain any trade-offs if you must break a rule.
```

#### Logging changes
```markdown
After changes, output:
1) Architectural decisions or changes done
2) Files changed (added/modified/deleted)
3) Summary of changes and rationale
4) Tests added/updated and results
5) Risks/assumptions and follow-ups
```

#### Permission controls
```markdown
Do not write to protected branches or production configs. Propose a PR only.
Never run destructive commands or modify secrets. Ask before installing deps.
```

#### Ask for clarity if in doubt
```markdown
If requirements are ambiguous, ask targeted questions (max 5) before coding.
Show your plan briefly and wait for confirmation.
```

#### Role-driven workflow
```markdown
You propose; humans decide. Provide diffs, tests, and a rollback plan.
Wait for approval gates before executing follow-up steps.
```

#### AI-Generated Code Documentation
```markdown
Ensure AI-generated code is clearly identified and well-documented
```


- **Mark AI-generated code**: Clearly identify AI-generated sections with comments
- **Comprehensive documentation**: AI should document all functions, classes, and complex logic
- **Instruction file guidance**: Create specific instructions for AI commenting standards
- **Review and validation**: Human review of AI comments for accuracy and completeness

**Instruction file example for commenting**:
```yaml
# .ai/commenting-standards.yml
commenting_rules:
  ai_generated_marker: "// AI-GENERATED: [description] - Generated by [model]"
  human_review_marker: "// Human review: [reviewer], [date]"
  documentation_required:
    - all_functions: true
    - complex_logic: true
    - business_rules: true
    - security_code: true
  comment_style:
    - explain_why_not_what: true
    - include_assumptions: true
    - note_limitations: true
    - reference_specs: true
```

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

Selecting the appropriate AI tool for each task is essential for effective results. For code generation, opt for the latest, high-quality models that excel in reasoning and code quality. When you need code explanation, choose models specifically optimized for understanding and breaking down complex logic. For refactoring, advanced models capable of code transformation are best, as they help maintain and improve code structure. Documentation tasks benefit from models with strong natural language capabilities, ensuring clarity and thoroughness. For security reviews, rely on specialized security tools and advanced AI models that possess domain-specific knowledge to identify and address potential vulnerabilities.

Note: Use only organisation-approved models. Consider data residency, privacy and IP licensing constraints, and cost/latency trade-offs when selecting models for workflows.

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

## 🏠 AI Coding Agent House Rules

A good agent is like a developer in training mode, needs clear rules. These house rules ensure AI agents operate safely and effectively within your organization.

### Core Principles
| Rule                     | Description                | Example                        |
|--------------------------|----------------------------|--------------------------------|
| **Never Self-Deploy**    | AI suggests, humans deploy | AI creates PR, human merges    |
| **No Secrets in Prompts**| Always mask sensitive data | Use `***` for passwords        |
| **Mandatory Logging**    | Log all AI interactions    | Every prompt/response recorded |
| **Ask "Why" Not "What"** | Challenge AI reasoning     | "Why this approach?"           |
| **Least Privilege**      | Minimal AI permissions     | Read-only access by default    |
| **Human in Loop**        | Mandatory approval gates   | No autonomous deployments      |

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
- **AI attribution**: Always mark AI-generated code sections clearly
- **Traceability**: Include model version, generation date, and reviewer information
- **Context preservation**: Document the prompt intent and business requirements

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
#### Security Scanning Commands
#### Policy Validation

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
- Use approved AI models for complex business logic generation
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
3. **Compliance Check**: Data protection compliance for user data handling

## Automated Checks Status
- ✅ SAST scan passed
- ✅ Secret scanning passed
- ✅ Dependency scan passed
- ⚠️ Performance test requires attention
```

### 🚫 Policy as Code
**Automated policy enforcement**

---

## 🧹 Prompt Engineering Hygiene

#### Security Rules
#### Never Include Secrets in Prompts
#### Mask Sensitive Data
#### Use Synthetic Data

---

## 📊 Audit Trails & Logging

### What to Log
- **Prompt inputs** - What was requested
- **AI outputs** - What was generated
- **Human decisions** - What was approved/rejected
- **Modifications** - How outputs were changed
- **Deployment status** - What made it to production
- **Architectural Changes** - Critical changes made by the AI

---

## 🔍 Review Checklist

_If you don't have tools (highly recommended to have some), then you can use this simple checklist as a way of validation. Not a perfect list or a one size fits all but a good starting point._

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