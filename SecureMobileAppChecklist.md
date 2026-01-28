✅ Secure Mobile App Checklist (iOS & Android)
0) Threat Modeling (Before Writing Code)

- Identify assets (PII, tokens, payments, location, secrets)

- Define trust boundaries (device ↔ app ↔ backend ↔ third parties)

- Assume device compromise (root/jailbreak/emulator)

- Define attacker goals (data theft, account takeover, API abuse)

- Document mitigations per threat (not just “use HTTPS”)

Deliverable: 1–2 page threat model per app.

1) Authentication & Authorization

- No hardcoded credentials, API keys, or secrets in the app

- Tokens are short-lived (access) + rotated (refresh)

- Backend enforces authorization (never trust client roles/flags)

- Logout revokes server sessions

- Biometric auth is optional UX, not a security boundary

- Rate limiting & lockout handled server-side

Red flags: JWTs with long expiry, role checks only on client.

2) Secure Storage (At Rest)

- iOS: Use Keychain with correct accessibility class

- Android: Use Keystore + encrypted storage (not SharedPreferences)

- Never store:

- Plaintext tokens

- Passwords

- Full card numbers

- Cached files are non-sensitive or encrypted

- Clipboard usage reviewed (PII should not be copied)

Test: Dump app data on compromised device → no secrets readable.

3) Network Security (In Transit)

- TLS enforced everywhere (no HTTP fallback)

- Certificate pinning implemented correctly

- Graceful pin rotation strategy exists

- No custom TLS trust managers that accept all certs

- Sensitive APIs require server validation, not headers alone

- Replay protection for critical endpoints

Test: Intercept traffic with Burp → requests fail.

4) API & Backend Trust

- Backend treats mobile app as untrusted

- All inputs validated server-side

- No business logic enforced only in the app

- Object-level authorization enforced (IDOR protection)

- Pagination, filters, and sorting are validated

- API versioning & deprecation plan exists

Red flags: user_id sent from client and trusted.

5) Cryptography

- Use platform crypto APIs only (no custom crypto)

- Secure random generators (not predictable seeds)

- Keys stored in secure hardware where available

- No deprecated algorithms (MD5, SHA-1, ECB)

- Crypto used only when needed (avoid “security theater”)

Rule: If you can’t explain why crypto is needed, remove it.

6) Runtime Protections & Hardening

- Basic root/jailbreak detection (defense-in-depth)

- Emulator detection where appropriate

- Debug flags removed in release builds

- Anti-tampering checks for critical logic

- Sensitive logic executed server-side when possible

Note: These slow attackers; they don’t stop them alone.

7) Reverse Engineering Resistance

- iOS symbols stripped in release

- Android code obfuscated (R8/ProGuard)

- No sensitive strings in binaries

- Feature flags fetched securely from backend

- Crash logs sanitized (no secrets)

Test: Decompile app → no obvious secrets or logic exposure.

8) Input Handling & UI Security

- All inputs validated (length, format, type)

- WebViews:

- JavaScript bridges restricted

- No file:// access unless required

- Deep links validated (scheme + host + path)

- Screenshots disabled for sensitive screens (where appropriate)

9) Logging, Analytics & Crash Reporting

- No PII, tokens, or secrets in logs

- Production logging level reduced

- Crash reports scrubbed

- Analytics events reviewed for privacy leakage

Test: Force crash → inspect payload sent to crash service.

10) Permissions & Privacy

- Request minimum permissions

- Explain permission usage clearly

- Handle denied permissions safely

- No background access without explicit need

- Data retention policy implemented

11) Third-Party SDKs

- Inventory all SDKs (purpose, data accessed)

- Remove unused SDKs

- Review SDK network traffic

- Keep SDKs up to date

- Monitor SDK CVEs

Risk: SDKs are a common data-leak vector.

12) Build & Release Security

- Separate dev/staging/prod environments

- Secrets injected at build/runtime, not committed

- CI runs static analysis (SAST)

- App signing keys protected

- Reproducible builds enabled where possible

13) Security Testing (Before Every Release)

- Static analysis (MobSF / equivalent)

- Dynamic testing on real device

- Network interception attempt

- Root/jailbreak test

- Regression tests for past vulnerabilities

Definition of Done: Security checks pass.

14) Incident Readiness

- Ability to revoke tokens remotely

- Feature kill-switch available

- Logging sufficient for forensics

- Responsible disclosure process defined

🔁 How to Use This Checklist

Design phase: Sections 0–4

Implementation: Sections 5–10

Pre-release: Sections 11–13

Production: Section 14


a security review template
