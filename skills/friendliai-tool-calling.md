---
name: Call tools with a Friendli model
description: Use OpenAI-compatible tool calling so a Friendli model can invoke your functions.
api: openapi/friendliai-openapi-original.yml
operations: [serverlessChatComplete]
---

# Call tools with a Friendli model

FriendliAI supports OpenAI-compatible tool calling with strict schema enforcement and parallel calls.
(The older Model APIs "Tool Assisted API" was deprecated 2026-07-06 — use tool calling below.)

## Steps
1. Call **serverlessChatComplete** (`POST /serverless/v1/chat/completions`) with your `messages` and a
   `tools` array (each a JSON-schema function definition). Optionally set `tool_choice`.
2. If the response contains `tool_calls`, execute each function in your own code.
3. Append a `role: tool` message per call (with its `tool_call_id` and result) and call
   **serverlessChatComplete** again to let the model produce the final answer.

## Rules
- Auth: `Authorization: Bearer <FRIENDLI_TOKEN>` (see `authentication/friendliai-authentication.yml`).
- Tool schemas are enforced strictly; malformed requests return 422.
- Respect tier rate limits (429); see `rate-limits/friendliai-rate-limits.yml`.
