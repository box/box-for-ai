---
name: box-integration-best-practices
description: Build, debug, or review repeatable Box API integrations and agent workflows for efficient calls, bulk work, and 429 handling.
---

# Box integration best practices

For bulk work or 429s, estimate calls per completed item and peak request rate by Box actor and endpoint. Check the current [rate limits](https://developer.box.com/guides/api-calls/permissions-and-errors/rate-limits); user, endpoint, enterprise, and temporary quality-of-service limits may apply.

## Reduce calls

- Reuse cached access tokens. Request fields needed for a decision in the list response when supported, and reuse returned data instead of fetching each item again. `fields` reduces call count only when it replaces a follow-up read.
- Use the largest useful supported page size. For large or changing collections, use [marker pagination](https://developer.box.com/guides/api-calls/pagination/marker-based) where supported; follow `next_marker` to completion and do not treat a marker as a durable checkpoint.
- Use webhooks or events with a catch-up path instead of repeated full scans. Query indexed metadata template fields instead of fetching metadata for every file.
- When content stays in Box, prefer Box-side copy, move, or update operations over download and re-upload.

## Control bulk work and retries

- Bound concurrency across workers sharing a Box actor, accounting for endpoint and enterprise limits. On 429, honor `Retry-After` across affected workers. Current official SDKs retry 429s; check the installed version before adding another retry layer. For direct HTTP, use bounded backoff with jitter.
- For long-running jobs, persist per-item outcomes so work can resume. Reconcile uncertain writes before retrying them.

## Check the result

Compare calls per completed item and 429s by actor and endpoint on a representative workload. Use the [Platform Activity report](https://docs.box.com/en/box-admin-tools/reporting-and-insights/platform-activity-report) when enterprise-level attribution is needed. If a token reaches a browser or other client-side component, [downscope it](https://developer.box.com/guides/authentication/tokens/downscope).
