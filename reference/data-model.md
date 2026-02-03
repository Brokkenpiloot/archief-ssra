# Data model (draft)

High-level entities for future implementation. This is intentionally minimal
until the stack is chosen.

## Users

Core fields:

- `id` (uuid)
- `email` (unique)
- `password_hash` (if self-hosted)
- `role` (member, alumni, admin)
- `status` (invited, active, suspended)
- `created_at`
- `last_login_at`

Access control fields:

- `invite_token` (nullable, expiring)
- `invite_expires_at` (nullable)
- `email_verified_at` (nullable)

Notes:

- If using a managed auth provider, user records may be synced from the provider
- Use a separate table/collection for access logs if needed
