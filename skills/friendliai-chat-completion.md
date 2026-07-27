---
name: Generate a chat completion with Friendli Model APIs
description: Send a conversation to a FriendliAI serverless model and get a response, with optional streaming.
api: openapi/friendliai-openapi-original.yml
operations: [serverlessModelList, serverlessChatComplete, serverlessChatStream]
---

# Generate a chat completion with Friendli Model APIs

FriendliAI's Model APIs are OpenAI-compatible. Base URL: `https://api.friendli.ai`.

## Auth
Send `Authorization: Bearer <FRIENDLI_TOKEN>`. Create a token in Friendli Suite → Personal settings.
Optionally scope to a team with the `X-Friendli-Team` header.

## Steps
1. (Optional) Discover available models with **serverlessModelList** (`GET /serverless/v1/models`).
2. Call **serverlessChatComplete** (`POST /serverless/v1/chat/completions`) with a `model` id and a
   `messages` array (roles: system/user/assistant). Set `response_format` for structured JSON output.
3. For token-by-token output, call **serverlessChatStream** (`POST /serverless/v1/chat/completions#stream`)
   and consume the server-sent events.

## Rules
- Errors return custom JSON envelopes, not RFC 9457 (see `errors/friendliai-problem-types.yml`);
  handle 401 (bad token), 422 (validation), 429 (tier rate limit).
- No idempotency key is supported; retries create new completions.
- Because the surface is OpenAI-compatible, the official OpenAI SDKs work by pointing `base_url` here.
