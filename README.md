# Infisical substrate — 2026-05-15

Provisioned by the 2026-05-15 session:
- 7 workspaces (1 per consumer product/concern)
- 11 consumer Machine Identities (universal-auth method, viewer role on single workspace)
- 1 bootstrap-admin identity (org-admin scope, revoke when no longer needed)
- 54 secrets seeded into prod env across shared-infrastructure, backend-shared, wambesh, wambesha, ops-infrastructure

## Files in this directory

| File | What |
|---|---|
| `workspaces.txt` | Workspace name → UUID map. Operationally useful for the backend wrapper script. Not sensitive. |
| `seed-mapping.tsv` | TSV of every seeded secret's (workspace, env, path, key, source). KEY NAMES only — no values. Reference for "what's where". |

## NOT in this directory (intentionally)

- Consumer Machine Identity client-secrets — regeneratable in seconds via Infisical UI or API; not worth the leak surface.
- Bootstrap-admin client-secret — same.
- Prod backend env dumps — sensitive; were shredded after seeding.
- Admin JWT — short-lived; expires in 30d anyway.

## Recovery procedure

If you ever need to re-bootstrap consumer identity credentials:

1. Log into https://infisical.wambesha.com as admin
2. Org Settings → Access Control → Identities
3. For each identity in `workspaces.txt` (named pattern: `{dev-server,prod}-{workspace-shortname}-{env}`):
   a. Click identity → Authentication tab → + Add Client Secret
   b. Copy the new secret, paste into the consumer's config
   c. (Optional) Revoke the previous secret via the same UI

## Encryption key (separate concern)

The Infisical `ENCRYPTION_KEY` value is in your other private-repo backup (the one filed 2026-05-15 after the transcript leak). Do NOT lose that one — it's forever-coupled to the PG ciphertext.

## Bootstrap-admin identity

`bootstrap-admin-2026-05` Machine Identity still exists in Infisical with org-admin scope. Revoke when no longer needed for API-driven automation; mint a fresh one if/when needed.
