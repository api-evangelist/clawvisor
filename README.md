# Clawvisor

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Clawvisor is the authorization layer for AI agents — a Y Combinator (Spring 2026) security gateway that sits between an AI agent and the tools it acts on (Gmail, Calendar, Drive, Contacts, GitHub, Slack, Notion, Linear, Stripe, Twilio, iMessage). Agents never hold downstream credentials: they declare a task describing their purpose and the service/action pairs they need, the user approves that scope once, and Clawvisor enforces restrictions, task scope, and LLM intent verification on every request before injecting vaulted, short-lived credentials, executing through an adapter, and writing a full audit trail.

Ships open-core as a self-hostable Go daemon with a CLI, web dashboard, TUI, and an OAuth 2.1 MCP server, plus an official Claude Code plugin. Agents integrate over a plain HTTP gateway or MCP.

- Website: https://clawvisor.com
- Source: https://github.com/clawvisor/clawvisor
- Backed by: y-combinator

This profile was enriched by the API Evangelist enrichment pipeline from Clawvisor's public developer surface. The Gateway OpenAPI in `openapi/` is generated from the published README and agent protocol; Clawvisor does not publish an OpenAPI document.
