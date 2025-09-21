---
title: "Essential Best Practices"
permalink: /best-practices/essential/
---

[← Back to Best Practices hub](/best-practices/)

This section covers day‑to‑day practices: what to do (and avoid) when working with AI assistance.

## Essential Best Practices

### Avoid Building Entire Features or Modules
**Focus on components, not complete systems**

- **Build repeated code**: utility functions, helper functions, reusable code, templates, CRUD functions
- **Don't build**: complete applications or complex business logic end-to-end
- **Why**: AI excels at patterns but struggles with complex system architecture and business rules

### Keep the Humans in the Loop
**Maintain human oversight at critical points**

- **Code reviews**: Every AI-generated piece needs human review
- **Security scans**: Automated scanning for vulnerabilities and secrets
- **CI/CD pipeline changes**: Human approval for deployment configurations
- **Command execution**: Human verification before running AI-suggested commands

### Use Scanning Features
**Leverage automated security and quality checks**

- **Scan code for**: leaked secrets, vulnerabilities, dependency issues
- **Integrate into CI/CD**: Make scanning part of your automated pipeline
- **Regular audits**: Schedule periodic comprehensive scans
- **Alert on findings**: Immediate notification for critical issues

### Craft Instructions Unique to You
**Customize AI prompts for your specific context**

- **Craft instruction files**: Create prompts tailored to your tech stack, app, and software standards
- **Include context**: Specify frameworks, coding standards, security requirements
- **Version control prompts**: Treat prompts like code - version and review them
- **Team consistency**: Share effective prompts across the team

### Secure Prompting Patterns
**Train developers and AI to use secure practices**

- **Train developers**: Education on secure prompting techniques
- **Train AI via instructions**: Use system prompts to enforce security patterns
- **Consistent patterns**: Establish standard secure prompting templates
- **Regular updates**: Keep security patterns current with evolving threats

### Spec-Driven Prompts
**Break requirements into smaller, manageable specifications**

- **Break requirements**: Decompose large features into smaller, specific tasks
- **Requirement documents**: Create detailed specifications before prompting
- **Use in prompts**: Reference specs directly in AI instructions
- **Iterative refinement**: Improve specs based on AI output quality

_Example: GitHub Spec Flow Pattern_
Learn more from the official website (https://github.com/github/spec-kit)

## Master the Instruction files

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

### AI for Non-Coding Tasks
**Leverage AI beyond just code generation**

- **Code explaining**: Use AI to document and explain existing code
- **Refactoring opportunities**: Identify areas for improvement
- **Architecture review**: Get suggestions for system design improvements
- **Documentation**: Generate and maintain technical documentation

### Reuse prompts
**Ensure prompt best practices are documented and shared amongst the team to maximise reuse and consistency**

Create a reusable library where you store your documentation or create prompts as MD files stored in the docs folder of your repo. If stored outside, use MCP tools to grab them during prompting. Check your AI coding tool's ability to reuse prompts and be consistent to achieve the outcome you desire.

### Modularise the Code
**Structure code for AI effectiveness**

- **Smaller segments**: Break codebase into manageable modules
- **Clear boundaries**: Define clear interfaces between components
- **Why**: LLMs struggle with large codebases and high line counts (LoC)
- **Better context**: Smaller modules provide clearer context for AI

### Pick the Model That Works for the Task

**Selecting the appropriate AI tool for each task is essential for effective results.**

For code generation, opt for the latest, high‑quality models that excel in reasoning and code quality. When you need code explanation, choose models specifically optimised for understanding and breaking down complex logic. For refactoring, advanced models capable of code transformation are best, as they help maintain and improve code structure. Documentation tasks benefit from models with strong natural language capabilities, ensuring clarity and thoroughness. For security reviews, rely on specialised security tools and advanced AI models that possess domain‑specific knowledge to identify and address potential vulnerabilities.

Note: Use only organisation-approved models. Consider data residency, privacy and IP licensing constraints, and cost/latency trade-offs when selecting models for workflows.

### Plan Before Making Changes
**Use structured approaches for AI-assisted development, brainstorm before execution to ensure the model understands your intentions**

- **Ask or chat mode first**: Clarify the plan of attack before implementation
- **Understand the approach**: Ensure AI's strategy aligns with your goals
- **Then use agent mode**: Execute the planned approach systematically
- **Iterative refinement**: Adjust plan based on results

### Fail Safe Mechanisms
**Build in controls and rollback capabilities, understand your tool's capabilities to undo/restore code if it makes a mistake or accidentally executes a command you didn't wish for**

- **Control mechanisms**: Ways to stop or pause AI agent workflows
- **Shutdown procedures**: Clear process to halt AI operations if needed
- **Enable logs**: Comprehensive logging of all AI actions
- **Rollback controls**: Ability to undo AI-generated changes quickly
- **Circuit breakers**: Automatic stops for suspicious or dangerous operations

### Validate MCP Tools
**Ensure tool security and compliance**

- **Validate MCP tools for vulnerabilities**: Regular security assessments of all Model Context Protocol tools
- **Compliance verification**: Ensure tools meet applicable regulatory requirements
- **Ensure devs stick to governed set of tools**: Maintain approved tool registry and prevent shadow AI usage

**Implementation Framework**:
**MCP Tool Governance Framework**

- **Approval Process:**
  - All Model Context Protocol (MCP) tools must undergo a mandatory security assessment.
  - Compliance review is required to ensure alignment with regulatory standards.
  - Legal approval is necessary for any external tools.
  - Performance evaluation must be completed before tool adoption.

- **Approved Tools Registry:**
  - Only tools listed in the approved registry may be used for AI-assisted development.
    - *AI Development Environment* (approved version)
    - *Enterprise AI Assistant* (enterprise edition)
    - *AI API Services* (approved endpoints only)
    - *Other approved tools*

- **Monitoring:**
  - Usage tracking is enabled for all MCP tools to ensure proper oversight.
  - Continuous security scanning is performed to detect vulnerabilities.
  - Compliance auditing occurs quarterly to verify ongoing adherence to policies.
  

### Review Checklist

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