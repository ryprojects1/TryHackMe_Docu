# OWASP Top 10 2025

## Key Terms

- **Identity** - the unique account (e.g., user ID/email) that represents a person or service.
- **Authentication** - proving that identity (passwords, OTP, passkeys).
- **Authorisation** - what that identity is allowed to do.
- **Accountability** - recording and alerting on who did what, when, and from where.

## Broken Access Control

Broken Access Control happens when the server doesn't properly enforce who can access what on every request. A common occurrence of this is **IDOR (Insecure Direct Object Reference)**: if changing an ID (like `?id=7` → `?id=6`) lets you see or edit someone else's data, access control is broken.

## Authentication Failures

Authentication Failures happen when an application can't reliably verify or bind a user's identity. Common issues include:

- Username enumeration
- Weak/guessable passwords (no lockout/rate limits)
- Logic flaws in the login/registration flow
- Insecure session or cookie handling

## Logging & Alerting Failures

Good logging underpins accountability (being able to prove who did what, when, and from where). In practice, failures look like:

- Missing authentication events
- Vague error logs
- No alerting on brute-force or privilege changes
- Short retention
- Logs stored where attackers can tamper with them

# OWASP Top 10 2025: Application Design Flaws

Security Misconfigurations
What It Is
Security misconfigurations happen when systems, servers, or applications are deployed with unsafe defaults, incomplete settings, or exposed services. These are not code bugs but mistakes in how the environment, software, or network is set up. They create easy entry points for attackers.
Why It Matters
Even small misconfigurations can expose sensitive data, enable privilege escalation, or give attackers a foothold into the system. Modern applications rely on complex stacks, cloud services, and third-party APIs. A single exposed admin panel, an open storage bucket, or misconfigured permissions can compromise the entire system
Common Patterns

Default credentials or weak passwords left unchanged
Unnecessary services or endpoints exposed to the internet
Misconfigured cloud storage or permissions (S3, Azure Blob, GCP buckets)
Unrestricted API access or missing authentication/authorisation
Verbose error messages exposing stack traces or system details
Outdated software, frameworks, or containers with known vulnerabilities
Exposed AI/ML endpoints without proper access controls
How To Prevent It

Harden default configurations and remove unused features or services
Enforce strong authentication and least privilege across all systems
Limit network exposure and segment sensitive resources
Keep software, frameworks, and containers up to date with patches
Hide stack traces and system information from error messages
Audit cloud configurations and permissions regularly
Secure AI endpoints and automation services with proper access controls and monitoring
Integrate configuration reviews and automated security checks into your deployment pipeline
What It Is
Software supply chain failures happen when applications rely on components, libraries, services, or models that are compromised, outdated, or improperly verified. These weaknesses are not inherent in your code, but rather in the software and tools you depend on. Attackers exploit these weak links to inject malicious code, bypass security, or steal sensitive data.

Why It Matters
Modern applications are built from many third-party packages, APIs, and AI models. One compromised dependency can compromise your entire system, allowing attackers to gain access without ever touching your own code. Supply chain attacks can be automated and distributed, making them hard to detect and very damaging
Common Patterns

Using unverified or unmaintained libraries and dependencies
Automatically installing updates without verification
Over-reliance on third-party AI models without monitoring or auditing
Insecure build pipelines or CI/CD processes that allow tampering
Poor license or provenance tracking for components
Lack of monitoring for vulnerabilities in dependencies after deployment
How To Protect The Supply Chain

Verify all third-party components, libraries, and AI models before use
Monitor and patch dependencies regularly
Sign, verify, and audit software updates and packages
Lock down CI/CD pipelines and build processes to prevent tampering
Track provenance and licensing for all dependencies
Implement runtime monitoring for unusual behaviour from dependencies or AI components
Integrate supply chain threat modelling into the SDLC, including testing, deployment, and update workflows
Cryptographic Failures
What It Is
Cryptographic failures happen when encryption is used incorrectly or not at all. This includes weak algorithms, hard-coded keys, poor key handling, or unencrypted sensitive data. These flaws let attackers access information that should be private.

Why It Matters
Web applications rely on cryptography everywhere: protecting network traffic, securing stored data, verifying identities, and safeguarding secrets. When these controls fail, sensitive data such as passwords, tokens, or personal information can be exposed, leading to account takeovers or full-scale breaches.

Attackers can exploit these flaws through man-in-the-middle attacks, brute-force attacks on weak keys, or by simply discovering secrets that were never properly protected.

Common Patterns 

Using deprecated or weak algorithms like MD5, SHA-1, or ECB mode
Hard-coded secrets in code or configuration
Poor key rotation or management practices
Lack of encryption for sensitive data at rest or in transit
Self-signed or invalid TLS certificates
Using AI/ML systems without proper secret handling for model parameters or sensitive inputs
How To Prevent It

Use strong, modern algorithms such as AES-GCM, ChaCha20-Poly1305, or enforce TLS 1.3 with valid certificates
Use secure key management services like Azure Key Vault, AWS KMS, or HashiCorp Vault
Rotate secrets and keys regularly, following defined crypto periods
Document and enforce policies and standard operating procedures for key lifecycle management
Maintain a complete inventory of certificates, keys, and their owners
Ensure AI models and automation agents never expose unencrypted secrets or sensitive data
The web application in this room contains a weakness of this type for you to explore.
