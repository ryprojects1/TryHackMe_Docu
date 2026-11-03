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
