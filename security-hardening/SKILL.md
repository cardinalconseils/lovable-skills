---
name: security-hardening
description: "Security hardening and OWASP Top 10 prevention for production applications. Use when: reviewing security, handling user input, managing secrets, configuring authentication, setting up HTTPS, adding CSP headers, auditing dependencies, or preparing for production deployment."
---

# Security Hardening

## OWASP Top 10 Prevention

**1. Injection** — Use parameterized queries or ORM — never concatenate user input into SQL. Apply least-privilege database users.

**2. Broken Authentication** — Hash passwords with bcrypt (cost ≥ 10) or argon2id. Rate-limit login endpoints (max 5 attempts per minute). Offer MFA; enforce it for admin accounts.

**3. Sensitive Data Exposure** — HTTPS everywhere. Encrypt sensitive data at rest (AES-256). Never log PII, passwords, tokens, or credit card numbers.

**4. Broken Access Control** — Check permissions on every endpoint — server-side, not just UI. Default deny: no permission = no access. Verify resource ownership: user A cannot access user B's data by changing an ID.

**5. Security Misconfiguration** — Disable debug mode and verbose errors in production. Set security headers on all responses. Keep frameworks updated.

**6. Cross-Site Scripting (XSS)** — Output-encode all user-generated content. Implement Content Security Policy (CSP) headers. Use framework auto-escaping.

**7. Known Vulnerabilities** — Run `npm audit` / `pip-audit` / `bundler-audit` in CI. Update dependencies monthly.

**8. Insufficient Logging** — Log auth failures, access control denials, and input validation failures. Do NOT log sensitive data. Send security logs to a centralized system with alerting.

## Secrets Management

- Store secrets in environment variables — never in code, config files, or git
- Use `.env.example` (with placeholder values) in the repo, never `.env`
- Rotate secrets on a schedule and after any suspected compromise

## HTTP Security Headers

| Header | Value | Purpose |
|--------|-------|-------|
| `Content-Security-Policy` | `default-src 'self'` (minimum) | Prevents XSS |
| `X-Frame-Options` | `DENY` | Prevents clickjacking |
| `X-Content-Type-Options` | `nosniff` | Prevents MIME sniffing |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` | Forces HTTPS |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Limits referrer leakage |

## CORS Configuration

- Whitelist specific origins — never use `*` in production
- Only allow necessary HTTP methods and headers
- Validate the Origin header server-side

## Input Validation

- Validate type, length, range, and format at every system boundary
- Use schema validation libraries (Zod, Joi, Pydantic)
- Validate on both client (UX) and server (security) — server is authoritative

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We're just a small app" | Automated scanners hit everything. Size is irrelevant. |
| "Security can wait until production" | A breach in pilot destroys user trust permanently. |
| "We use a framework, it handles security" | Frameworks provide tools, not guarantees. Misconfiguration defeats framework protections. |

## Red Flags

- SQL queries built with string concatenation
- Secrets committed to git
- Debug mode or stack traces in production
- No CSP or security headers configured
- `CORS: *` in production
- Dependencies with known CVEs
- No rate limiting on authentication endpoints

## Verification

- [ ] All database queries use parameterized queries or ORM
- [ ] Passwords hashed with bcrypt or argon2id
- [ ] HTTPS enforced with HSTS header
- [ ] Security headers set (CSP, X-Frame-Options, X-Content-Type-Options)
- [ ] CORS whitelist uses specific origins, not wildcard
- [ ] No secrets in code, config files, or git history
- [ ] `npm audit` shows zero high/critical vulnerabilities
- [ ] Input validated at every system boundary
- [ ] Authentication endpoints rate-limited
- [ ] Security-relevant events logged
