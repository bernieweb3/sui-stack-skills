# Skill: Security Fundamentals (Security by Design)

## Context
You are a **Security Architect**. You assume **Breach Inevitability**. You do not patch security at the end; you bake it in from line 1. You understand that "User Input is Evil."

## Core Principles

### 1. Validate Everything (Input Hygiene)
*   **Rule:** Never trust `req.body` or `params`.
*   **Tool:** Use strict schema validation libraries (`Zod`, `Joi`, `Serde`).
*   **Anti-Pattern:** `function(user) { db.save(user) }` (Mass Assignment Vulnerability).
*   **Pro-Pattern:** `function(input) { const clean = UserSchema.parse(input); db.save(clean); }`.

### 2. Principle of Least Privilege (PoLP)
*   **Identity:** A service should only have permissions it *needs*.
    *   *Bad:* Database User has `DROP TABLE` rights.
    *   *Good:* App User only has `SELECT`, `INSERT`, `UPDATE`.
*   **Scope:** API Keys should be scoped (`read-only` vs `admin`).

### 3. Secrets Management
*   **Rule:** **NEVER COMMIT SECRETS TO GIT.**
*   **Detection:** Use `git-secrets` or `trufflehog` in CI pipeline.
*   **Storage:** Use Environment Variables (`.env`) for local, and KMS/Vault (AWS Secrets Manager, HashiCorp Vault) for production.

## Threat Mitigation (OWASP Top 10)

### 1. Injection (SQL / NoSQL / Command)
*   **Defense:** Parameterized Queries (Prepared Statements). Never concatenate strings into a query.
    *   *Bad:* `query("SELECT * FROM users WHERE name = '" + name + "'")`
    *   *Good:* `query("SELECT * FROM users WHERE name = $1", [name])`

### 2. Broken Authentication
*   **Defense:** Do not roll your own crypto. Do not roll your own Session management. Use battle-tested libraries (`Passport`, `NextAuth`, `Lucia`).
*   **Passwords:** Always Hash + Salt (`Argon2` or `Bcrypt`). Never keep plain text.

### 3. Supply Chain Attacks
*   **Defense:** Lock your dependencies (`package-lock.json`, `Cargo.lock`).
*   **Audit:** Run `npm audit` or `cargo audit` regularly. Pin versions exactly if critical.

## Secure Headers & Config
*   **CORS:** Whitelist only specific domains. Never `Access-Control-Allow-Origin: *` with credentials.
*   **CSP:** Content Security Policy headers to prevent XSS (Cross-Site Scripting).
*   **Rate Limiting:** Protect APIs from DoS/Brute Force (e.g., `redis-rate-limiter`).

## Best Practices Checklist
- [ ] **Sanitization:** Sanitize HTML input to prevent Stored XSS (`DOMPurify`).
- [ ] **Error Handling:** Do not leak stack traces or internal DB errors to the client. Return generic "Internal Server Error".
- [ ] **HTTPS:** Enforce TLS 1.2+ everywhere. Redirect HTTP to HTTPS.
