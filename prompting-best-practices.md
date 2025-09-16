---
layout: page
title: "Prompting Best Practices"
permalink: /prompting-best-practices/
---

# The Art of AI Prompting for Developers

Master the craft of structured prompting to unlock AI's full potential while maintaining security, quality, and best practices. This guide focuses purely on prompting techniques - no tools, no tech, just the communication patterns that drive exceptional AI-assisted development.

## 🎯 The Prompting Mindset

**Think of AI as your most literal colleague** - one who follows instructions precisely but needs crystal-clear direction. The quality of your output is directly proportional to the quality of your input.

### Core Principle: CRISP Prompting Framework

Every effective prompt should be:
- **C**lear: Unambiguous instructions
- **R**elevant: Contextually appropriate
- **I**nstructive: Actionable guidance  
- **S**pecific: Detailed requirements
- **P**urposeful: Goal-oriented

---

## 📋 Universal Prompting Structure

### The 5-Layer Prompt Architecture

```markdown
## CONTEXT
[Who you are, what you're building, current situation]

## OBJECTIVE  
[Exactly what you want the AI to do]

## CONSTRAINTS
[Limitations, requirements, standards to follow]

## EXAMPLES
[Sample inputs/outputs or reference patterns]

## VALIDATION
[How to verify the output is correct]
```

### Example: Complete Structured Prompt

```markdown
## CONTEXT
I'm a senior developer working on a financial services application that processes sensitive customer data. Our codebase follows strict security standards and requires comprehensive error handling.

## OBJECTIVE
Create a Python function that validates and sanitizes user input for account numbers before database operations.

## CONSTRAINTS
- Must follow OWASP input validation guidelines
- Use regex patterns for validation
- Include comprehensive error handling
- Log security events without exposing sensitive data
- Follow our existing code style (see .cursorrules file)

## EXAMPLES
Valid input: "ACC-2024-001234" → sanitized and validated
Invalid input: "'; DROP TABLE users; --" → rejected with security log
Malformed input: "acc123" → rejected with format error

## VALIDATION
Test with: valid account numbers, SQL injection attempts, XSS payloads, empty strings, null values, and extremely long inputs.
```

---

## 🏛️ Instruction Files: Your Prompting Foundation

### The Power of Context Files

Instead of repeating project details in every prompt, create reusable instruction files:

#### `.cursorrules` - Project Context
```markdown
# Project: SecureFinance API
# Stack: Python 3.11, FastAPI, PostgreSQL, Docker
# Standards: PEP 8, OWASP Security Guidelines, SOC 2 Compliance

## Code Style
- Use type hints for all functions
- Maximum line length: 88 characters  
- Use descriptive variable names
- Include docstrings for all public functions
- Prefer composition over inheritance

## Security Requirements
- Validate all inputs using Pydantic models
- Use parameterized queries for database operations
- Log security events to audit trail
- Never log sensitive data
- Implement rate limiting on all endpoints

## Error Handling
- Use custom exception classes
- Provide user-friendly error messages
- Log detailed errors for debugging
- Return consistent error response format
```

#### `.ai-instructions` - AI-Specific Guidelines
```markdown
# AI Development Guidelines

## Code Generation Rules
1. Always include comprehensive error handling
2. Add security comments for sensitive operations
3. Include unit test examples
4. Provide performance considerations
5. Suggest monitoring and logging points

## Review Checklist
Before suggesting code, verify it addresses:
- Input validation and sanitization
- Error handling and edge cases
- Security implications
- Performance impact
- Testing requirements
- Documentation needs

## Preferred Patterns
- Use dependency injection for testability
- Implement circuit breaker pattern for external calls
- Use async/await for I/O operations
- Follow repository pattern for data access
```

### Leveraging Instruction Files in Prompts

```markdown
## SHORT PROMPT (leverages instruction files)
Following our project standards in .cursorrules, create a user authentication endpoint that handles login attempts with rate limiting.

## INSTEAD OF LONG PROMPT (without instruction files)  
Create a user authentication endpoint in Python using FastAPI that follows PEP 8 standards, uses type hints, implements comprehensive error handling, includes security logging, validates inputs using Pydantic, uses parameterized queries, implements rate limiting, includes unit tests, follows our custom exception pattern, returns consistent error formats, and complies with OWASP guidelines...
```

---

## 🎯 Best Practice Rules with Prompting Patterns

### Rule #1: AI Generates → Human Reviews → CI/CD Validates

**Prompting Pattern: Review-Ready Generation**
```markdown
Generate code that includes review markers and validation points:

Create a user registration function with:
- TODO comments marking areas needing human review
- Security checkpoints with validation requirements  
- Test cases that verify security assumptions
- Documentation explaining security decisions
- Clear separation between AI-generated and human-reviewed sections

Mark each security-critical section with "HUMAN-REVIEW-REQUIRED" comments.
```

### Rule #2: Never Auto-Merge AI Commits

**Prompting Pattern: Draft-Mode Generation**
```markdown
Generate code in draft mode for human review:

Create a password reset flow that:
- Includes "DRAFT: Requires Security Review" in all comments
- Lists specific security considerations for human validation
- Provides alternative implementation options
- Includes risk assessment comments
- Suggests specific test cases for security validation

Format as a pull request description with review checklist.
```

### Rule #3: Always Version Control Prompts

**Prompting Pattern: Self-Documenting Prompts**
```markdown
Create a commit message template that includes:

```
feat: implement user authentication

AI-Generated: Yes
Prompt Version: v2.1
Security Review: Required
Test Coverage: Unit tests included

Prompt Summary:
- Context: Financial services application
- Objective: Secure user authentication with MFA
- Constraints: OWASP compliance, audit logging
- Validation: Security scan passed, manual review pending

Human Review Checklist:
- [ ] Input validation comprehensive
- [ ] Error messages don't leak information  
- [ ] Audit logging captures required events
- [ ] Rate limiting properly implemented
```

### Rule #4: Mask Secrets Before AI Interaction

**Prompting Pattern: Secret-Safe Prompting**
```markdown
When working with configuration or code containing secrets:

"Review this authentication configuration, treating all actual values as [REDACTED]:

```python
DATABASE_URL = '[REDACTED]'  # PostgreSQL connection string
JWT_SECRET = '[REDACTED]'    # 256-bit secret key
API_KEY = '[REDACTED]'       # Third-party service key
```

Analyze the security patterns and suggest improvements without needing actual values."
```

### Rule #5: Craft Instructions Unique to You

**Prompting Pattern: Context-Rich Specification**
```markdown
Based on our microservices architecture (see architecture.md), create a service that:

ARCHITECTURAL CONTEXT:
- Event-driven communication via RabbitMQ
- Domain-driven design with bounded contexts
- CQRS pattern for read/write separation
- Distributed tracing with OpenTelemetry

SERVICE REQUIREMENTS:
[specific requirements here]

Ensure the implementation follows our established patterns and integrates seamlessly with existing services.
```

### Rule #6: Secure Prompting Patterns

**Prompting Pattern: Security-First Prompting**
```markdown
Security-conscious prompt structure:

"As a security-focused developer, create [functionality] that:

THREAT MODEL:
- Consider: [specific threats relevant to this component]
- Mitigate: [specific attack vectors]
- Validate: [input validation requirements]

SECURITY REQUIREMENTS:
- Authentication: [requirements]
- Authorization: [requirements]  
- Data Protection: [requirements]
- Audit Logging: [requirements]

Include security unit tests and threat analysis in your response."
```

### Rule #7: Spec-Driven Prompts

**Prompting Pattern: Specification-First Development**
```markdown
Implementation prompt based on specification:

"Using the attached API specification (see api-spec.yaml):

SPECIFICATION ADHERENCE:
- Implement exactly as specified in endpoints section 3.2
- Follow the error response format in section 5.1
- Use the authentication scheme defined in section 2.3
- Validate against the OpenAPI schema provided

IMPLEMENTATION REQUIREMENTS:
- Generate code that passes OpenAPI validation
- Include integration tests that verify spec compliance
- Add monitoring for spec deviation detection
- Document any assumptions or clarifications needed"
```

### Rule #8: Modularise the Code

**Prompting Pattern: Component-Focused Prompting**
```markdown
Component-specific prompt:

"Create a single, focused module for [specific functionality]:

SINGLE RESPONSIBILITY:
- Handle only: [specific responsibility]
- Dependencies: [list minimal required dependencies]
- Interface: [define clean interface]
- Testing: [specify testing approach]

INTEGRATION POINTS:
- How it connects to: [other modules]
- What it exposes: [public interface]
- What it consumes: [dependencies]

Keep the module under 200 lines and highly testable."
```

### Rule #9: Pick the Right Model for the Task

**Prompting Pattern: Task-Optimized Prompting**
```markdown
Task-specific prompting strategies:

FOR CODE GENERATION (use with GPT-4, Claude):
"Generate production-ready code with comprehensive error handling..."

FOR CODE EXPLANATION (use with any model):
"Explain this code's functionality, focusing on business logic and edge cases..."

FOR REFACTORING (use with Claude):
"Refactor this code to improve maintainability while preserving functionality..."

FOR SECURITY REVIEW (use with GPT-4, Claude):
"Perform a security analysis focusing on OWASP Top 10 vulnerabilities..."
```

### Rule #10: Plan Before Making Changes

**Prompting Pattern: Strategy-First Prompting**
```markdown
Two-phase prompting approach:

PHASE 1 - PLANNING:
"Analyze the current codebase and create an implementation strategy for [feature]:

- Current state assessment
- Proposed changes and their impact
- Risk analysis and mitigation strategies  
- Step-by-step implementation plan
- Testing and validation approach

Don't generate code yet - just the plan."

PHASE 2 - IMPLEMENTATION:
"Based on the approved plan above, implement step [X] of the strategy:
[specific implementation request]"
```

---

## 🔄 Advanced Prompting Techniques

### Chain-of-Thought Prompting for Complex Problems
```markdown
"Let's solve this step by step:

1. First, analyse the current authentication flow
2. Then, identify security gaps and requirements  
3. Next, design the improved architecture
4. Finally, implement the solution with tests

Walk me through each step with your reasoning before providing the final implementation."
```

### Few-Shot Prompting with Examples
```markdown
"Here are three examples of how we handle API errors in our codebase:

EXAMPLE 1: Validation Error
[code example]

EXAMPLE 2: Authorization Error  
[code example]

EXAMPLE 3: Rate Limit Error
[code example]

Following this same pattern, implement error handling for database connection failures."
```

### Role-Based Prompting for Context
```markdown
"As a senior security architect reviewing this authentication system:

1. Identify potential vulnerabilities
2. Assess compliance with security standards
3. Recommend specific improvements
4. Provide implementation guidance

Focus on practical, actionable recommendations."
```

### Constraint-Based Prompting for Quality
```markdown
"Generate a solution that must satisfy ALL constraints:

PERFORMANCE: Response time < 100ms
SECURITY: Zero trust architecture
SCALABILITY: Handle 10k concurrent users  
MAINTAINABILITY: Code coverage > 90%
COMPLIANCE: SOC 2 requirements

If any constraint cannot be met, explain why and suggest alternatives."
```

---

## 🎭 Persona-Driven Prompting Patterns

### The Security-Conscious Developer
```markdown
"As a paranoid security developer who assumes every input is malicious:

Create [functionality] that treats all data as potentially dangerous, implements defense in depth, logs security events comprehensively, and fails securely in all error conditions."
```

### The Performance-Obsessed Engineer
```markdown
"As a performance engineer optimising for milliseconds:

Implement [functionality] with focus on minimal latency, efficient memory usage, optimal algorithms, and comprehensive performance monitoring."
```

### The Compliance Officer
```markdown
"As a compliance officer ensuring regulatory adherence:

Review this implementation for GDPR, SOC 2, and industry-specific requirements, identifying any compliance gaps and suggesting remediation."
```

---

## 📊 Prompt Quality Checklist

Before sending any prompt, verify:

### ✅ Clarity Checklist
- [ ] Objective is unambiguous
- [ ] Success criteria are defined
- [ ] Constraints are specific
- [ ] Context is sufficient

### ✅ Security Checklist  
- [ ] No secrets or sensitive data included
- [ ] Security requirements specified
- [ ] Threat model considerations included
- [ ] Compliance requirements mentioned

### ✅ Quality Checklist
- [ ] Examples provided where helpful
- [ ] Validation criteria specified
- [ ] Error handling requirements included
- [ ] Testing expectations defined

### ✅ Efficiency Checklist
- [ ] Leverages instruction files
- [ ] Avoids unnecessary repetition
- [ ] Focuses on specific objective
- [ ] Includes relevant context only

---

## 🚀 Prompting Anti-Patterns to Avoid

### Quick Reference: Good vs Bad Prompting

| ✅ **GOOD PROMPTING** | ❌ **BAD PROMPTING** |
|----------------------|---------------------|
| Be specific and detailed | "Make this code better" |
| Provide context and constraints | No context or requirements |
| Use structured formats | Unclear, rambling requests |
| Include examples and patterns | Vague, open-ended questions |
| Specify security requirements | Ignore security considerations |
| Test and iterate prompts | Use one-size-fits-all prompts |

### ❌ The Vague Request
```markdown
BAD: "Make this code better"
GOOD: "Refactor this function to improve readability, add error handling for edge cases, and optimise the algorithm for better performance"
```

### ❌ The Context-Free Prompt
```markdown
BAD: "Create a login function"
GOOD: "For our financial services web application, create a secure login function that handles MFA, rate limiting, and audit logging according to our security standards"
```

### ❌ The Secret-Exposing Prompt
```markdown
BAD: "Fix this config: DATABASE_URL=postgresql://user:pass123@host:5432/db"
GOOD: "Review this database configuration pattern and suggest security improvements: DATABASE_URL=[REDACTED_CONNECTION_STRING]"
```

### ❌ The All-at-Once Request
```markdown
BAD: "Build a complete user management system with authentication, authorization, CRUD operations, and reporting"
GOOD: "Create the user authentication component following our established patterns, focusing on secure login/logout functionality"
```

---

## 🎯 Measuring Prompt Effectiveness

### Quality Indicators
- **First-try success rate**: How often does the AI generate usable code on the first attempt?
- **Review time**: How long does human review take?
- **Iteration count**: How many back-and-forth exchanges are needed?
- **Security pass rate**: How often does generated code pass security scans?

### Improvement Strategies
- **Prompt versioning**: Track which prompt variations work best
- **Context refinement**: Continuously improve instruction files
- **Pattern recognition**: Identify and reuse successful prompt patterns
- **Team sharing**: Share effective prompts across the development team

---

*Remember: Great prompting is like great documentation - it saves time, prevents errors, and enables better outcomes. Invest in your prompting skills as seriously as you invest in your coding skills.*

---

**Navigation**: [← Best Practices](best-practices.html) | [Tools & Examples →](tools-and-examples.html)
