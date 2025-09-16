Vibe Coding Playbook
====================

What this is
-------------
The Vibe Coding Playbook is a practical, security‑minded guide for adopting AI‑assisted development in real teams. It focuses on safe workflows, measurable outcomes, and enterprise alignment. The site is built with Jekyll and published via GitHub Pages.

Why this exists
---------------
AI coding tools are everywhere, but good habits and guardrails are not. This playbook curates patterns that work in practice and highlights risks that teams repeatedly encounter (security, compliance, quality, and maintainability).

What’s inside
-------------
- (01) What’s Vibe Coding: methodology and context
- (02) Common Habits: patterns to amplify and pitfalls to avoid
- (03) Best Practices & Guardrails: practical rules, checklists, and workflows
- (04) Enterprise Alignment: governance, compliance, audit‑ready practices
- (05) Adoption Framework: a six‑step roadmap to scale safely

How to use this repo
--------------------
- Browse the sections at the top of the site or via the links on the home page
- Treat examples as starting points; adapt to your stack, policies, and risk appetite
- Measure locally: establish baselines (lead time, PR cycle time, change failure rate, defect density) and validate outcomes

Contributing
------------
Contributions are very welcome—from fixes and examples to entirely new sections. Please:

1. Open an issue describing the problem/opportunity and proposed change
2. Fork the repo and create a topic branch
3. Make focused edits with clear commit messages
4. Include rationale, references, and before/after context where helpful
5. Open a pull request and link the issue

Contribution guidelines (quick version)
--------------------------------------
- Prefer actionable guidance over theory; show small, copy‑pasteable examples
- Keep security, compliance, and auditability in view (note trade‑offs)
- Avoid vendor lock‑in wording when a generic pattern will do
- Use inclusive, clear language; keep sections concise and skimmable
- Don’t include secrets or proprietary code

Sharing knowledge
-----------------
If you’ve tried these practices, learned something new, or built internal tooling that others could replicate, please share:

- Real‑world case studies (what worked, what didn’t)
- Guardrail patterns (policy‑as‑code, CI gates, review workflows)
- Prompting patterns and instruction files that improved quality or safety
- Metrics you track and how you measure impact

Local development
-----------------
This is a Jekyll site compatible with GitHub Pages. To run locally:

```bash
bundle install
bundle exec jekyll serve
```

Then open http://localhost:4000/VibeCodingPlaybook/ (or your baseurl) in a browser.

License
-------
Copyright (c) 2025. Licensed under the MIT License. See LICENSE for details.

Acknowledgements
----------------
Thanks to everyone contributing examples, critiques, and improvements. Your feedback makes the guidance sharper and safer for all.

