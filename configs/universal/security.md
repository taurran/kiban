# Security Policy

Security principles and scanning workflows that apply to all projects.
This is a policy reference -- it states WHAT to do and WHY, with links
to external resources for HOW.

---

## Automated Security Scanning

### Snyk Code Scan

- Always run Snyk on new first-party code in Snyk-supported languages
- If security issues are found on newly introduced or modified code: fix using Snyk result context
- Rescan after fixes to confirm resolution and check for newly introduced issues
- Repeat scan-fix-rescan cycle until no new issues are found
- Applies to: Python, JavaScript, TypeScript, and other Snyk-supported languages
- If the scanner is unavailable, the toolchain is unsupported, or a scan cannot converge: record a one-line deferred-security note and proceed -- do NOT block the task or shim tooling to force a scan
- Residual findings may be accepted instead of fixed when documented with a reason and an expiry (e.g., a `.snyk` policy entry), rather than left silently unresolved

### Bandit (Python SAST)

- Run Bandit alongside Snyk on all Python and FastAPI projects
- Bandit catches Python-specific issues Snyk may miss (e.g., use of eval, hardcoded passwords, weak crypto)
- Run with: `bandit -r src/` (recursive scan of source directory)
- Fix all HIGH and MEDIUM severity findings before merging

---

## Core Principles

- No hardcoded secrets, credentials, or API keys in source code -- use environment variables or secret managers
- Validate at system boundaries: all user input, external API responses, and file uploads
- Dependency hygiene: keep dependencies updated, audit with `pip audit` (Python) or `npm audit` (Node)
- Principle of least privilege: services and API keys get minimum permissions needed
- Defense in depth: do not rely on a single security control

---

## OWASP Top 10 (Web / API Security)

Awareness checklist for web and API projects (reference: https://owasp.org/www-project-top-ten/).

- **Injection** -- parameterize queries, never concatenate user input into SQL/commands
- **Broken Authentication** -- use established auth libraries, enforce strong password policies
- **Sensitive Data Exposure** -- encrypt data in transit (HTTPS) and at rest, minimize data collection
- **Broken Access Control** -- enforce server-side authorization on every request, deny by default
- **Security Misconfiguration** -- disable debug modes in production, review default configs

---

## OWASP AI Top 10

Awareness checklist for projects using AI/LLM integrations like Claude API
(reference: https://owasp.org/www-project-top-10-for-large-language-model-applications/).
Relevant to Mise (recipe parsing via Claude) and Kagami (Guide agent).

- **Prompt Injection** -- validate and sanitize inputs before passing to LLM; separate system prompts from user content
- **Insecure Output Handling** -- never trust LLM output as safe; sanitize before rendering or executing
- **Excessive Agency** -- limit what actions an LLM can take; require human confirmation for destructive operations
- **Model Denial of Service** -- implement rate limiting and input size limits on LLM API calls
- **Sensitive Information Disclosure** -- do not include secrets, PII, or credentials in prompts sent to external LLM APIs
