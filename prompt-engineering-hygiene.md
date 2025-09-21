---
title: "Prompt Engineering Hygiene"
permalink: /best-practices/prompt-engineering-hygiene/
---

[← Back to Best Practices hub](/best-practices/)

Keep secrets out of prompts, be concise and specific, sanitize outputs, and educate teams.

## Prompt Engineering Hygiene

#### Prompt Engineering Hygiene Best Practices

- **Never Include Secrets in Prompts**  
  Never paste passwords, API keys, tokens, or confidential information into prompts. Treat prompts as potentially visible to others or logged by systems.

- **Mask Sensitive Data**  
  Replace real user data, credentials, or internal identifiers with placeholders (e.g., `***`, `user@example.com`, `API_KEY_HERE`) before sharing with AI.

- **Use Synthetic or Anonymized Data**  
  When examples are needed, use fake or anonymized data that cannot be traced back to real users or systems.

- **Minimise Personally Identifiable Information (PII)**  
  Avoid including names, emails, addresses, or any PII in prompts. If necessary, redact or generalize such information.

- **Be Specific and Concise**  
  Write clear, focused prompts. Remove unnecessary context, code, or comments that could introduce risk or confusion.

- **Avoid Proprietary or Confidential Business Logic**  
  Do not share sensitive algorithms, trade secrets, or internal processes unless absolutely necessary and approved.

- **Review Prompts Before Submission**  
  Double-check prompts for accidental leaks of sensitive data or internal information before sending to an AI system.

- **Document Prompt Intent**  
  Clearly state the purpose of the prompt, especially if it will be reused or shared, to avoid misuse or misinterpretation.

- **Version and Track Prompts**  
  Store important prompts in version control, especially those used in automation or CI/CD, to ensure traceability and auditability.

- **Use Organisation‑Approved Templates**  
  Where possible, use standardised prompt templates vetted for security and compliance.

- **Limit Prompt Length and Scope**  
  Avoid overly long or complex prompts that may inadvertently include sensitive context or increase the risk of data leakage.

- **Sanitize Outputs**  
  Review AI-generated outputs for accidental inclusion of sensitive data before sharing or deploying.

- **Educate Team Members**  
  Train all users of AI tools on prompt hygiene and the risks of improper prompt construction.

- **Log Prompt Usage (Where Appropriate)**  
  Maintain an audit trail of prompts and responses for critical workflows, ensuring sensitive data is not logged.

- **Regularly Review and Update Prompting Practices**  
  Periodically audit prompt hygiene practices and update guidelines as threats and tools evolve.

