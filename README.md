# Clawvisor

Clawvisor is the authorization layer for AI agents — a Y Combinator (Spring 2026) security gateway that sits between an AI agent and the tools it acts on (Gmail, Calendar, Drive, Contacts, GitHub, Slack, Notion, Linear, Stripe, Twilio, iMessage). Agents never hold downstream credentials: they declare a task describing their purpose and the service/action pairs they need, the user approves that scope once, and Clawvisor enforces restrictions, task scope, and LLM intent verification on every request before injecting vaulted, short-lived credentials, executing through an adapter, and writing a full audit trail.

Ships open-core as a self-hostable Go daemon with a CLI, web dashboard, TUI, and an OAuth 2.1 MCP server, plus an official Claude Code plugin. Agents integrate over a plain HTTP gateway or MCP.

- Website: https://clawvisor.com
- Source: https://github.com/clawvisor/clawvisor
- Backed by: y-combinator

This profile was enriched by the API Evangelist enrichment pipeline from Clawvisor's public developer surface. The Gateway OpenAPI in `openapi/` is generated from the published README and agent protocol; Clawvisor does not publish an OpenAPI document.
