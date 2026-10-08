---
name: box-integration-best-practices
description: Design, build, review, or fix Box API integrations, Box-powered apps, and repeatable agent workflows for efficient API use, safe bulk work, and reliable rate-limit handling. Use for code or architecture that calls Box; not for one-off Box content operations.
---

# Box integration best practices

Use this skill to change implementation decisions, not to restate Box documentation. Preserve the project's architecture when it works. Verify current endpoint and SDK behavior in the [Box Developer Docs](https://developer.box.com/llms.txt) and the installed SDK before coding; limits, signatures, and supported features can change.

## Plan the request budget

- Identify the Box actor, app, scopes, endpoints, data volume, and frequency. Estimate calls per item, per page, per run, and at peak concurrency. Include authentication, retries, preflight checks, and verification calls. Remember that user, endpoint, quality-of-service, and enterprise limits can all matter; published rates are planning inputs, not guaranteed capacity.
- If reviewing code, trace the actual call graph and find repeated reads, polling, token requests, per-item metadata fetches, and unconstrained workers. Prefer evidence from traces or metrics over guessing which call dominates.

## Remove avoidable calls

- Reuse valid access tokens; use the official SDK's token cache when available. Do not request a token per item or add a second cache or retry layer without a demonstrated need.
- Request only needed `fields`. Reuse data already returned by a list or write response before fetching the same item again. Check the endpoint's response shape before relying on nested fields.
- Use optional per-item preflight checks only when their result changes how the job handles a likely failure.
- Paginate every collection to its documented end. Use a larger supported page size and marker pagination when the endpoint supports it and the dataset warrants it; do not retain markers as durable inventory.
- For change detection, prefer webhooks or events with a catch-up path over repeated full scans. Use metadata queries when searching indexed template fields is suitable; account for their access and template limits. Use conditional requests where supported to avoid transferring unchanged data.
- Choose Box-side copy, move, or update operations when content stays in Box. Do not download and re-upload solely to change its Box location or metadata. Check whether the specific workflow has a real batch endpoint; do not assume one exists.

## Make high-volume work controlled and recoverable

- Use a bounded queue and a shared rate controller for workers using the same actor or app. Set initial concurrency from the relevant endpoint's limits, then adjust from observed latency and 429s. Never start an unbounded fan-out.
- On 429, honor `Retry-After` and pause affected workers. Current official SDKs handle 429 retries; inspect the installed version before adding application retries. For direct HTTP or older SDKs, use bounded backoff with jitter. Reconcile uncertain writes before retrying them; diagnose ordinary permission and validation errors instead of repeatedly sending the same request.
- For bulk jobs, checkpoint discovery and per-item outcomes, make restart behavior explicit, and reconcile totals with Box state. Choose direct versus chunked upload and individual versus ZIP download from file sizes, failure isolation, and supported limits, not from request count alone.

## Keep the integration safe and measurable

- Use the intended actor and least privilege. Keep server credentials out of browsers and logs; downscope tokens passed to client-side components. Preserve access controls and correct write semantics when reducing calls.
- Record calls by app, actor, endpoint, status, request ID, and job where safe. Track calls per completed item, 429 rate, retry delay, and backlog; use the [Platform Activity report](https://docs.box.com/en/box-admin-tools/reporting-and-insights/platform-activity-report) to identify high-usage apps. Validate the call budget and completion behavior with a representative workload, including a throttled response.

Use the relevant official guides for details: [rate limits and usage reduction](https://developer.box.com/guides/api-calls/permissions-and-errors/rate-limits), [authentication best practices](https://developer.box.com/guides/authentication/best-practices), [pagination](https://developer.box.com/guides/api-calls/pagination/marker-based), [events](https://developer.box.com/guides/events), [webhooks](https://developer.box.com/guides/webhooks), [metadata queries](https://developer.box.com/guides/metadata/queries/limitations), and [consistency headers](https://developer.box.com/guides/api-calls/ensure-consistency).
