---
title: "AI Coding Agent House Rules"
permalink: /best-practices/agent-house-rules/
---

[← Back to Best Practices hub](best-practices/)

Principles for safe, effective AI agent operation: isolate tasks, comment thoroughly, enforce permissions, and run feedback loops.

## AI Coding Agent House Rules

**A good agent is like a developer in training mode, needs clear rules. These house rules ensure AI agents operate safely and effectively within your organisation.**

### Isolate Tasks
**Limit context window for focused execution**

- **Limit context window**: Longer chats become more confusing for AI
- **Custom instructions**: Provide specific, targeted guidance for each task
- **Task boundaries**: Define clear scope and limitations
- **Context management**: Keep conversations focused and relevant

### AI Comments
**Ensure generated code is readable and reviewable**

- **Comment everything**: AI should comment all the way through
- **Readable code**: Ensure generated code is self-documenting
- **Review manually**: Human verification of AI comments and logic
- **Documentation standards**: Maintain consistent commenting style
- **AI attribution**: Always mark AI-generated code sections clearly
- **Traceability**: Include model version, generation date, and reviewer information
- **Context preservation**: Document the prompt intent and business requirements

### AI Permission Control
**Strict access control for AI agents**

- **Clear boundary to operate under**: Define exactly what AI can and cannot do
- **Blacklisting/whitelisting**: Explicit allow/deny lists for commands and tasks
- **No access to production resources**: Strict separation of environments
- **Control tasks AI can and can't do**: Granular permission management

### AI Feedback Loop
**Continuous improvement through structured feedback**

- **Prompt → Code → Critique → Repeat**: Iterative refinement process
- **Quality gates**: Automated checks at each stage
- **Human oversight**: Regular human intervention points
- **Learning incorporation**: Use feedback to improve future prompts

### Audit Trails & Logging

What to Log
- **Prompt inputs** - What was requested
- **AI outputs** - What was generated
- **Human decisions** - What was approved/rejected
- **Modifications** - How outputs were changed
- **Deployment status** - What made it to production
- **Architectural Changes** - Critical changes made by the AI
- **Tag AI generated PRs separately** - Easier to audit later if anything goes wrong using AI generated code
