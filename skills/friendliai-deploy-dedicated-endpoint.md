---
name: Deploy a Dedicated Endpoint and run inference
description: Create a FriendliAI Dedicated Endpoint on GPUs, wait until it is ready, then send inference.
api: openapi/friendliai-openapi-original.yml
operations: [dedicatedCreateEndpoint, dedicatedGetEndpointStatus, dedicatedChatComplete]
---

# Deploy a Dedicated Endpoint and run inference

Dedicated Endpoints run any open-source or custom model on dedicated GPUs. Base URL: `https://api.friendli.ai`.

## Steps
1. Create the endpoint with **dedicatedCreateEndpoint** (`POST /dedicated/beta/endpoint`), specifying the
   model (e.g. a Hugging Face repo) and GPU instance configuration. Capture the returned `endpoint_id`.
2. Poll **dedicatedGetEndpointStatus** (`GET /dedicated/beta/endpoint/{endpoint_id}/status`) until the
   endpoint reports a running/ready status.
3. Run inference with **dedicatedChatComplete** (`POST /dedicated/v1/chat/completions`), passing the
   endpoint's model id in `model` and your `messages`.

## Rules
- Auth: `Authorization: Bearer <FRIENDLI_TOKEN>`; scope with `X-Friendli-Team` when acting for a team.
- Billing is per-second by GPU type; sleep/terminate the endpoint when idle
  (`dedicatedSleepEndpoint` / `dedicatedTerminateEndpoint`) to control cost.
- Endpoints support version history and rollback (see `lifecycle/friendliai-lifecycle.yml`).
