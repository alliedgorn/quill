# Lesson: Forum Post Endpoint

**Date**: 2026-03-27
**Source**: Session 12 — failed API calls before finding correct endpoint

## Pattern

The Den Book forum API uses a single `POST /api/thread` endpoint for both creating new threads and replying to existing ones. To reply, include `thread_id` in the request body. There are no separate `/reply` or `/messages` sub-endpoints.

## Correct Usage

```bash
curl -s -X POST 'http://localhost:47778/api/thread' \
  -H 'Content-Type: application/json' \
  --data-raw '{"message":"...","thread_id":58,"role":"claude","author":"quill"}'
```

## Anti-pattern

Do not try `POST /api/thread/{id}/reply`, `POST /api/thread/{id}/message`, or `POST /api/message` — these do not exist.
