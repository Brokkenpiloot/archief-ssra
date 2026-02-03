# Authentication (draft)

Preparation for a future login system without committing to a stack yet.

## Goals

- Secure login for members and alumni
- Restrict archive content behind authentication
- Admin-managed access (invite or approval flow)

## Recommended direction (tooling)

Prefer a managed authentication provider to minimize security risk and
implementation effort. Options to consider:

- **Supabase Auth** (Postgres-backed, simple setup, good for small teams)
- **Auth0** (enterprise-grade, robust policies, higher cost at scale)
- **Clerk** (great developer experience, hosted UI, fast to ship)

If you want a self-hosted solution later, plan for **OpenID Connect** so the
app can switch providers without major rewrites.

**Current choice (draft):** Supabase Auth for email/password sign-in.

## Vendor lock-in avoidance (best practices)

- Implement a small auth adapter layer to isolate provider-specific SDK calls
- Store your own user records and keep authorization (roles) in your database
- Link users by `provider` + `provider_user_id` (`sub` claim)
- Avoid hard-coding provider-specific role or metadata claims
- Prefer OIDC-standard claims (`sub`, `email`, `iss`, `aud`) in the app layer
- Keep token validation and session handling in one module for easy swap

## Security requirements (non-negotiable)

- Passwords must be hashed with **Argon2id** or **bcrypt** if self-hosted
- Enforce strong passwords and rate limiting
- Email verification before activation
- Reset passwords via signed, expiring tokens
- Optional MFA for admins

## Authentication flow (draft)

1. User requests access or receives an invite
2. Admin approves and issues an invite token
3. User sets password and verifies email
4. User logs in to access archive content

## Integration notes

- Keep auth logic separate from content pages
- Use session-based auth with secure, httpOnly cookies
- Log access events for audit purposes
