# Testing

Every suite runs offline with a fake AI and a temporary database: no API key, no cost, no real data. Test data is fictional.

| Suite | Tests | Focus |
| --- | ---: | --- |
| `test_quick_replies` | 536 | Fixed messages, name capture, FAQ matching, form offer rules |
| `test_whatsapp` | 164 | Webhook parsing, HMAC signature, deduplication, unsupported media, sending errors |
| `test_form_fields` | 127 | Form question mapping and normalization of every field |
| `test_human_takeover` | 124 | Owner echo detection, final takeover, bot-sent message tracking, malformed payloads |
| `test_logic` | 113 | Database, persistence, state transitions, handoff and follow-up |
| `test_form` | 95 | Form intake endpoint, secret check, phone matching, mismatch handling |
| `test_replicability` | 51 | A second fictional brand runs from YAML only; no brand values in shared code |
| `test_human_notification` | 32 | Owner alert template building |
| `test_human_notification_integration` | 30 | Owner alert end to end through the engine |
| **Total offline** | **1,272** | **All passing** |

A separate suite runs 22 single questions and 27 full conversations against the real OpenAI API. Automatic checks catch dangerous mistakes (approving someone, wrong numbers, wrong action); tone is reviewed by a person.
