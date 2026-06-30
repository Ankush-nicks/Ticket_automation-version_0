# Ticket Resolver System — Interactive Prototype

A single-file, zero-dependency HTML prototype simulating an AI-assisted ticket resolution workflow: POC notification, SLA escalation, AI-suggested replies, and weighted AI evaluation of resolver comments.

Built as a V0 demo to validate the workflow concept before wiring it into a real ticketing portal (Zoho Creator) and chat platform (Microsoft Teams / Slack / Telegram).

## What this demonstrates

This prototype walks through the full lifecycle of a support ticket:

1. **Ticket creation** — an admin/dispatcher fills in ticket details (title, department, priority, resolver, SLA deadline) and sets the AI evaluation weights.
2. **Resolver notification** — a Teams-style notification card appears showing the ticket, with Accept and Escalate actions.
3. **SLA simulation** — buttons let you simulate 50% SLA elapsed (triggers a nudge with guiding questions) and 100% SLA breached (triggers auto-escalation to the reporting manager).
4. **AI-suggested reply** — once the resolver accepts the ticket, an AI-generated draft resolution comment appears, tuned to the configured evaluation weights. The resolver can use it as-is, edit it, regenerate it, or write their own from scratch.
5. **AI evaluation** — the submitted comment is scored against four configurable dimensions, with an overall weighted score, per-dimension breakdown, feedback to the resolver, and a separate instructor/manager summary.

## Evaluation dimensions

Each dimension has an adjustable weight (sliders, must total 100%):

- **Quality & completeness** — how thorough and complete the resolution is
- **Tone & professionalism** — communication quality
- **Root cause addressed** — whether the underlying cause is explained, not just the symptom
- **SLA acknowledgment** — whether the resolver shows awareness of timing/urgency

## Running it

No build step, no server, no install. Just open the file in a browser:

```bash
open ticket-resolver.html
# or double-click the file in your file explorer
```

Works fully offline using built-in heuristic scoring. No API key required to try it.

## Using real AI evaluation (optional)

By default the app scores comments using local keyword heuristics so it works out of the box. To use real AI evaluation and suggestion generation instead:

1. Click **Set OpenAI key** (top right)
2. Paste an OpenAI API key (`sk-...`)
3. All suggestions and evaluations now run through `gpt-4o-mini`

> The key is stored only in memory for the current browser tab/session. It is sent directly to OpenAI's API and is never logged, saved, or sent anywhere else. Reloading the page clears it.

## Project status

This is a **V0 / proof-of-concept prototype**, built to validate the workflow and get stakeholder buy-in before integrating with real systems. It is not connected to any ticketing backend.

### What's simulated vs. real
| | This prototype | Production version |
|---|---|---|
| Ticket source | Manual form input | Zoho Creator webhook |
| Notifications | In-browser Teams-style card | Real Teams / Slack bot |
| SLA timer | Manual "simulate" buttons | Cron-based polling |
| AI evaluation | Optional OpenAI key, or local heuristic | OpenAI API (server-side) |
| Data persistence | None (resets on reload) | Database (e.g. Google Sheets / proper DB) |

### Planned next steps
- Wire ticket creation to a real Zoho Creator webhook
- Replace the in-browser notification with a real chat bot (Teams Adaptive Cards or Slack Block Kit, depending on what IT approves)
- Move SLA checks to a scheduled backend job instead of manual simulation
- Persist tickets and evaluation history to a real datastore
- Add a daily/end-of-shift department summary report

## Tech notes

- Single HTML file — all CSS and JavaScript inline, no build tooling
- No external dependencies or CDN calls except the optional direct call to `api.openai.com`
- All state is held in memory (vanilla JS, no framework) and resets on page reload
- Designed to be readable and easy to extend — every screen, scoring function, and prompt template lives in one file

## License

Internal prototype — not licensed for external distribution.
