# OWASP Top 10 2025

## Key Terms

| Term | Definition |
|------|-----------|
| Identity | The unique account (e.g. user ID/email) representing a person or service |
| Authentication | Proving that identity (passwords, OTP, passkeys) |
| Authorisation | What that identity is allowed to do |
| Accountability | Recording and alerting on who did what, when, and from where |

---

## 1. Broken Access Control

Happens when the server doesn't properly enforce who can access what on every request.

**IDOR (Insecure Direct Object Reference)** - changing an ID like `?id=7` to `?id=6` to access someone else's data.

---

## 2. Authentication Failures

Happens when an application can't reliably verify a user's identity.

Common issues:
- Username enumeration
- Weak/guessable passwords with no lockout or rate limits
- Logic flaws in login/registration flow
- Insecure session or cookie handling

---

## 3. Logging and Alerting Failures

Good logging underpins accountability. Failures look like:
- Missing authentication events
- Vague error logs
- No alerting on brute-force or privilege changes
- Short retention periods
- Logs stored where attackers can tamper with them

---

## 4. Security Misconfigurations

Not code bugs - mistakes in how the environment, software, or network is set up.

**Common patterns:**
- Default credentials left unchanged
- Unnecessary services or endpoints exposed to the internet
- Misconfigured cloud storage (S3, Azure Blob, GCP buckets)
- Unrestricted API access or missing auth
- Verbose error messages exposing stack traces
- Outdated software with known vulnerabilities
- Exposed AI/ML endpoints without access controls

**Prevention:**
- Harden defaults, remove unused features
- Enforce least privilege across all systems
- Audit cloud configs regularly
- Integrate automated security checks into deployment pipeline

---

## 5. Software Supply Chain Failures

Happens when applications rely on components, libraries, or models that are compromised, outdated, or unverified.

**Common patterns:**
- Unverified or unmaintained libraries
- Auto-installing updates without verification
- Over-reliance on third-party AI models without auditing
- Insecure CI/CD pipelines
- Poor license or provenance tracking

**Prevention:**
- Verify all third-party components and AI models before use
- Sign, verify, and audit software updates
- Lock down CI/CD pipelines
- Implement runtime monitoring for dependencies

---

## 6. Cryptographic Failures

Happens when encryption is used incorrectly or not at all.

**Common patterns:**
- Weak/deprecated algorithms: MD5, SHA-1, ECB mode, DES
- Hard-coded secrets in code or config
- Poor key rotation or management
- No encryption for data at rest or in transit
- Self-signed or invalid TLS certificates
- Rolling your own cryptography instead of using vetted libraries

**Prevention:**
- Use AES-GCM, ChaCha20-Poly1305, or TLS 1.3
- Use key management services: Azure Key Vault, AWS KMS, HashiCorp Vault
- Rotate secrets regularly
- Hash passwords with bcrypt, scrypt, or Argon2
- Never embed credentials in source code or repos

---

## 7. Insecure Design

Flawed logic or architecture built into a system from the start. You can't patch an insecure design - it requires rethinking the system.

**Example:** Clubhouse assumed users would only interact via the mobile app. The backend API had no proper authentication, exposing user data and private conversations directly.

**Common patterns in 2025:**
- Weak business logic controls (recovery/approval flows)
- AI components with unchecked authority or access
- Missing guardrails for LLMs and automation agents
- Test or debug bypasses left in production

**AI-specific design failures:**
- Prompt injection - user input blended with system prompts, hijacking context
- Blind trust in model output - acting on AI decisions without validation
- Poisoned models - models from unverified sources with embedded backdoors

**Prevention:**
- Treat every model as untrusted until proven otherwise
- Separate system prompts from user content
- Require human review for high-risk AI actions
- Build threat modelling into every stage of development
- Apply least privilege across users, APIs, and services

---

## 8. Injection

Happens when user input is passed directly into a system that can execute commands or queries.

**Types:**
- SQL Injection
- Command Injection
- AI Prompt Injection
- Server Side Template Injection (SSTI)

**Prevention:**
- Treat all user input as untrusted
- Use prepared statements and parameterised queries for SQL
- Avoid functions that pass input directly to the system shell
- Validate, sanitise, and escape dangerous characters before processing

---

## 9. Software and Data Integrity Failures

Happens when an application trusts code, updates, or data without verifying their authenticity or origin.

**Examples:**
- Trusting software updates without verification
- Loading scripts from untrusted sources
- Accepting binaries, templates, or JSON files without checking if they were altered

**Prevention:**
- Use cryptographic checks (checksums) for update packages
- Ensure only trusted sources can modify critical artefacts
- Apply integrity and trust boundaries within CI/CD pipelines

---

## 10. (See Broken Access Control / Auth)

The 2025 list elevates injection to appear twice, reflecting the growing attack surface from AI prompts and automation systems.
