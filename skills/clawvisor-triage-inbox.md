---
name: clawvisor-triage-inbox
description: >-
  Triage a user's inbox through the Clawvisor gateway — fetch the service
  catalog, declare a read-scoped task, wait for the user to approve it, list and
  read messages under scope, and complete the task. The agent never holds any
  credentials; Clawvisor injects them per request.
api: openapi/clawvisor-gateway-openapi.yml
operations:
  - fetchCatalog
  - createTask
  - getTask
  - gatewayRequest
  - expandTask
  - completeTask
generated: '2026-07-18'
method: generated
source: >-
  Grounded in openapi/clawvisor-gateway-openapi.yml operationIds and the
  Clawvisor agent protocol (skills/clawvisor-agent-protocol.md).
---

# Clawvisor: triage an inbox safely

You act through Clawvisor, the authorization layer between you and external
services. You never hold credentials — you declare a task, the user approves the
scope once, and every request is checked and credential-injected by Clawvisor.
Authenticate every call with `Authorization: Bearer $CLAWVISOR_AGENT_TOKEN`
(the catalog endpoint uses the `X-Clawvisor-Agent-Token` header instead).

## Steps

1. **Fetch the catalog** — `fetchCatalog` (`GET /api/skill/catalog`). Confirm the
   Gmail service is active for the user and that `list_messages` / `get_message`
   are not restricted. Do not proceed with a restricted action.

2. **Declare a read-scoped task** — `createTask` (`POST /api/tasks?wait=true`).
   Set a clear `purpose` ("triage the inbox — read only, ask before sending"),
   and `authorized_actions` with `auto_execute: true` for the read actions and
   `auto_execute: false` for anything that sends. Use `?wait=true` to block until
   the user approves.
   ```json
   {
     "purpose": "Triage the inbox: list and read recent messages to surface what needs attention. Read-only.",
     "authorized_actions": [
       {"service": "google.gmail", "action": "list_messages", "auto_execute": true,
        "expected_use": "List and search recent messages to build a triage summary."},
       {"service": "google.gmail", "action": "get_message", "auto_execute": true,
        "expected_use": "Read individual message bodies surfaced by list_messages."}
     ],
     "expires_in_seconds": 1800
   }
   ```

3. **Confirm approval** — if the task did not resolve on the create call, long-poll
   `getTask` (`GET /api/tasks/{id}?wait=true`) until `status` is `active` (or
   `denied` — stop if denied).

4. **List and read under scope** — call `gatewayRequest`
   (`POST /api/gateway/request`) for each action, always passing `task_id`,
   a unique `request_id`, a truthful `reason`, and the action `params`. Read the
   response `status`:
   - `executed` → use `result.summary` / `result.data`.
   - `restricted` → intent verification rejected it; adjust `params`/`reason` and
     retry with a **new** `request_id`.
   - `blocked` → a restriction forbids it; do not retry.
   - `pending_scope_expansion` → the action is outside scope; see step 5.

5. **Expand only if needed** — if the user then asks you to reply, call
   `expandTask` (`POST /api/tasks/{id}/expand`) for `send_message` with
   `auto_execute: false`, so the send still goes to the user for approval.

6. **Complete the task** — `completeTask` (`POST /api/tasks/{id}/complete`) when
   done. This cleans up chain-context facts.

## Conventions to respect
- One task per workflow purpose; size the scope to the task (see conventions/clawvisor-conventions.yml).
- On **standing** tasks you MUST send a stable `session_id` on every gateway
  request, or the call is rejected with `MISSING_SESSION_ID`.
- Never log the agent token or downstream response bodies.
- Error/decline semantics are in errors/clawvisor-problem-types.yml.
