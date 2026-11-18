# Security and privacy

## Assets

- Candidate personal data collected by the form (national ID numbers, address, income, references) and stored in SQLite
- API credentials: WhatsApp access token, app secret, OpenAI key, form webhook secret
- The brand's reputation: what the bot promises in its name

## Threats and controls

| Threat | Control |
| --- | --- |
| Forged WhatsApp webhook | HMAC-SHA256 over the raw body using the Meta App Secret; invalid signatures are refused |
| Forged form submission | Shared secret (minimum 16 characters) compared in constant time |
| Webhook verification hijack | Verify token compared in constant time; missing token returns 403 |
| Credential leak | Secrets only in `.env`; `.env` and `data/*.db` are in `.gitignore`; no secret in YAML or prompt |
| Prompt injection / hallucinated promises | Deterministic rules handle routine paths; the AI may state only facts from the brand YAML; commission numbers are cross-checked at startup |
| Automated decisions about people | The AI has no approve or reject action; any future prioritization must exclude protected or sensitive attributes and be approved in writing |
| Bot and human talking at the same time | Owner takeover is detected from message echoes and is final |
| External API outage | 20 s timeout on OpenAI, 10 s on WhatsApp; failure sends one safe message and routes to a human |
| Replayed or duplicated webhooks | Message IDs are stored; duplicates are ignored |
| Cost abuse | Per-contact AI call budget, then handoff to a human |
| Cross-brand data exposure | One instance and one database per brand |
| Personal data in logs | Values and raw form payloads are never logged; error messages carry no values |
| Wrong form linked to a candidate | Linking only by an exact normalized phone match to exactly one contact; no name or time-based guessing |

## Open items (documented, not yet implemented)

- Retention policy for registrations that were not approved
- Column-level encryption for national ID numbers before scaling beyond the pilot
- Self-service data deletion requests under Brazil's LGPD (manual today)
- Opaque per-conversation code in the form link for stronger form-to-chat correlation

## Process

Before the live pilot the full codebase went through a read-only security audit. Only a scoped patch was approved: closing the human-takeover path and handling AI failures safely. Unrelated refactors were deliberately left out of that change.
