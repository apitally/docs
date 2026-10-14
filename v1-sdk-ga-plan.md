---
directory: /Users/simon.gurcke/Repos/apitally/docs
implemented_at: null
---

# Docs updates for v1 SDK GA

Make the v1 SDKs (Python, JavaScript, Go, .NET) the default and remove beta mentions. Then update general pages that still describe v0 behaviour.

Baseline: docs `40c9184`. Facts checked on 2026-10-14 against apitally-py `53a4d57`, apitally-js `1297654`, apitally-go `0f17a37`, apitally-dotnet `5fb2d92` and cloud `6bc15859` (including `cloud/docs/sdks/spec.md` and `design.md`). Line numbers refer to the baseline.

Approach: general pages describe v1 behaviour. Mention v0 only where dashboard data visibly differs (CPU utilization) or where a v0 user would otherwise be misled.

## Part 1: SDK reference

### docs.json navigation

- [x] Python, JavaScript, Go, .NET (L101-345): v0 comes first in `versions`, so it is the default. → Move v1 first (or set `"default": true` on it) and remove `"tag": "beta"` (L131, L199, L264, L321). Optional: tag v0 as `"Legacy"`.

### docs.json redirects

- [x] 78 of 108 redirects point to v0. → Point the 68 that cover the standard SDKs to the v1 equivalent, so that "unversioned URL = current version":
  - `/frameworks/<framework>` (22) and `/setup-guides/<framework>` (22) → `/sdk-reference/<lang>/v1/setup-guides/<framework>`. Every slug exists in v1, including `django`.
  - `/reference/<lang>` (4), `/sdk-reference/<lang>` (4) and `/sdk-reference/<lang>/overview` (4) → `/v1/overview`.
  - `/sdk-reference/<lang>/configuration` and `/tracing` (8) → `/v1/configuration` and `/v1/tracing`.
  - `/sdk-reference/<lang>/masking-filtering` (4) → `/v1/masking`, the closest v1 page.
  - The 10 serverless redirects stay on v0.
- [ ] Who links to these URLs:
  - website: `/setup-guides/<framework>` for every framework (`utils/frameworks.ts`, `content/FAQSection.tsx`, `components/HeroFrameworks.tsx`); `/sdk-reference/<lang>/tracing` (tracing release blog post); `/sdk-reference/<lang>/overview` (FastAPI and Hono logging guide blog posts)
  - dashboard: `/setup-guides/<slug>` (see Part 3)
  - v0 SDK READMEs (as published on PyPI, npm, NuGet and pkg.go.dev for v0 releases): `/setup-guides/<framework>`, `/sdk-reference/<lang>`, `/frameworks/aspnet-core`

  v0 readers who arrive this way land on v1 pages and rely on the version selector.
- [ ] The redirects are permanent (308) but are served with `cache-control: no-store`, so browsers do not keep the old v0 target. After deploying, use `curl -sI` on a few sources to confirm the Cloudflare edge (`cf-cache-status: HIT`) serves the new `location`. Keep explicit entries: a wildcard such as `/sdk-reference/python/:slug*` would also match the versioned paths.

### Remove beta mentions (v1 pages)

- [x] Overview pages (4): remove `<Warning>The Apitally SDK v1 is currently in beta.</Warning>` (L29 in `python|javascript|go|dotnet/v1/overview.mdx`).
- [x] Setup guides (21: Python 7, JavaScript 9, Go 4, .NET 1): remove `<Info>This setup guide is for v1 of the ... SDK (currently in beta).</Info>` (L9 or L10). Recommendation: remove it entirely.
- [x] `javascript/v1/setup-guides/h3.mdx` L12 and `javascript/v0/setup-guides/h3.mdx` L20: "H3 v2 (currently in beta)". H3 2.x is now stable (`latest` on npm is 2.0.2). → Remove "(currently in beta)". The Requirements line (`2.0.1-rc.26+`) matches the SDK peer dependency, so keep it.

### Install commands (v1 pages)

- [x] Python: `python/v1/overview.mdx` and the 7 setup guides. → `pip install --pre` → `pip install`, `uv add --prerelease allow` → `uv add`, `poetry add --allow-prereleases` → `poetry add`.
- [x] JavaScript: `javascript/v1/overview.mdx` and the 9 setup guides. → `apitally@next` → `apitally` for npm, yarn, pnpm and bun. Keep `@opentelemetry/api` on the yarn lines.
- [x] Go: `go/v1/setup-guides/{chi,echo,fiber,gin}.mdx` and `go/v1/migration.mdx` L18-38. → Drop `@v1.0.0-beta.2`. A plain `go get` resolves to the latest release and also upgrades an existing v0 requirement.
- [x] .NET: `dotnet/v1/overview.mdx` L38, `setup-guides/aspnet-core.mdx` L38, `migration.mdx` L17. → Drop `--prerelease`.
- [ ] Python migration (`python/v1/migration.mdx` L14): the page sends users to the setup guides for installation, but those commands don't upgrade an existing 0.x install. pip reports "Requirement already satisfied" and poetry reports "already present"; check uv. → Add explicit upgrade commands, e.g. `pip install -U ...` and `poetry add ...@latest`. Mirror this in `apitally-py/MIGRATION.md`. Docs page done; it uses `uv add "...>=1"`, because a plain `uv add` keeps the existing requirement. `MIGRATION.md` is still to do.

### v0 pages

- [x] v0 install commands pull v1 after GA. → Pin them to v0 on all standard-SDK v0 overviews and setup guides. Check the syntax for each tool:
  - Python (overview + 7 guides): `"apitally[<extra>]<1"` for pip, uv and poetry.
  - JavaScript (overview + 9 guides, including `fastify-plugin` lines): `apitally@0`.
  - Go: overview (`{yourFramework}`), chi-v5, echo-v4/v5 and fiber-v2/v3 → `@v0`. The v0 `gin` module path is separate from v1's `gin-v1`, so it is unaffected.
  - .NET (overview + aspnet-core): `--version "0.*"`.
- [x] v0 pages give no hint that a newer version exists. Decided against a notice on v0 pages; the version selector is enough.

### SDK landing pages and shared snippets

- [x] `sdk-reference/overview.mdx` L14-29: the standard SDK cards link to `/v0/overview`. → Link to `/v1/overview`. Optional: mention OpenTelemetry in "These SDKs include a client that sends data to Apitally in the background" (L8).
- [x] `snippets/setup-guides-accordion.mdx` L1-4 imports the `snippets/v0/setup-guides-*.mdx` card lists. The accordion is used by `introduction.mdx` and `sdk-reference/setup-guides/overview.mdx`, and the general pages link to the latter as "the setup guide for your framework". → Import `snippets/v1/...` instead; the lists are otherwise identical. Keep the Cloudflare Workers import (v0).

## Part 2: General pages

### how-it-works.mdx

- [x] L9-12: SDK cards link to `/v0/overview`. → Link to v1.
- [ ] L6: "lightweight middleware or plugin". → v1 SDKs are built on OpenTelemetry and export OTLP directly to Apitally, with no collector or agent. They work alongside an existing OpenTelemetry setup.
- [ ] L17-28: the diagram (light and dark) shows "Apitally Hub" and "syncs with Apitally in 1 minute intervals". → Needs a new v1 diagram, hosted on assets.apitally.io outside this repo.
- [ ] L30-36: the functionality list ends with a "Non-blocking client" and lists "OpenTelemetry integration to capture traces" as an add-on. → Describe the v1 parts:
  - framework instrumentation that creates a span per request
  - log capture
  - in-process metrics aggregation
  - a background export worker with a disk buffer
- [ ] L38-42: "will retry sending data for up to 1 hour". The figure is still roughly right, but the mechanism has changed:
  - Telemetry is buffered in gzip files in the OS temp dir, capped at 50 MB. If the temp dir isn't writable, it falls back to memory, capped at 10 MB.
  - Retries happen on every export cycle. A file is dropped 59 min after its first send attempt.
  - Telemetry is flushed on graceful shutdown. Go requires `apitally.Shutdown`.
- [ ] L44-47 "On startup": "introspects ... sent in a once-off request". → The v1 startup event contains endpoints, runtime and framework versions, the resolved SDK configuration and the OpenAPI spec where available. It is sent through the regular export pipeline, once the process starts serving:
  - at startup completion for ASGI with lifespan, Litestar, BlackSheep, Fastify, Hapi, AdonisJS, Fiber and ASP.NET Core
  - on the first request otherwise (WSGI, Express, Koa, Hono, H3, Elysia, Chi, Echo, Gin)

  Test runs and scripts that never serve send nothing.
- [ ] L51: "times the invokation of the request handler". → Time runs from middleware entry until the response is complete, including streaming. In Go it runs until the handler chain returns. Also fix the typo "invokation".
- [ ] L52: "If the response is a validation error (status 400 or 422), the middleware inspects the response body". → Detection is framework-specific: exception hooks, framework error objects, or response bodies. Validation and server errors are aggregated per process and sent as separate events, independent of sampling.
- [ ] L54-55: "synchronizes the aggregated data with Apitally in one minute intervals". → Metrics are aggregated in-process as exponential histograms. They are exported every 15 s during an environment's first hour and every 60 s after that; the server can set the interval anywhere from 5 to 60 s. The first export goes out ~2 s after activation.
- [ ] L57-58: "If request logging is enabled, the client writes ... to a compressed temporary file". → Request logging is on by default. Each request becomes a trace (request span plus child spans) with its application logs, buffered until the response completes. It is subject to exclusions and sampling, and metrics always count every request. Traces and logs are batched into the gzip spool and sent each export cycle.
- [x] L66: the queue statement is still accurate (the OTLP receiver publishes to NATS JetStream). No change.
- [ ] L68 (pre-existing): CPU/memory data is kept for 32 days only, not aggregated for 13 months. → Clarify that the retention statement applies to request metrics.

### data-privacy.mdx

Customers use this page for vendor and GDPR assessments. v1 collects more by default than v0: traces, application logs, client IP and user agent.

- [ ] L9: "Require explicit user opt-in for features that could capture sensitive data". This no longer holds: traces and application logs are on by default. → Restate the principle: payloads (bodies) and request headers still require opt-in.
- [ ] L20-21: says option names are in Python `snake_case`. → Keep, but use v1 Python names throughout.
- [ ] L24: the legend "Potentially sensitive and not collected by default" no longer matches application logs and traces. → Separate "sensitive" from "off by default", or reword the legend and re-mark each item.
- [ ] L26-58 "Application metadata":
  - The Versions example `"apitally": "0.21.3"` is v0. v1 sends the runtime, framework packages and app version. The SDK version travels in the `telemetry.distro.version` resource attribute.
  - Add the resolved SDK configuration (`config`). Callbacks appear as `true`, user patterns are included, and the write token is never included.
  - Add the OpenAPI spec: FastAPI, Litestar and Django (DRF/Ninja) only. The .NET endpoint list includes summaries and descriptions.
  - Add resource attributes: `service.name` (default `unknown_service:<executable>`), `service.instance.id` (random UUID), environment, `telemetry.*`, and anything in `OTEL_RESOURCE_ATTRIBUTES`. With a user-owned tracer provider, spans carry that provider's resource.
- [ ] L60-131 "Metrics & analytics":
  - L76/L81: "histogram in 1 KB buckets" and L86: "10 ms buckets". → Base-2 exponential histograms (scale 3).
  - L86: the response time definition. → Same as the how-it-works L51 fix.
  - L90-103 Consumer: metrics carry only the identifier. → Name, group and the new custom `attributes` are sent as separate consumer-update events.
  - L105-116 Validation error info: the example uses v0 `loc`/`msg`/`type`. → The v1 event has `method`, `path`, `source`, `field`, `message`, `type` and per-consumer counts. It still contains no input values.
  - L118-130 Exception info: also captured via the `capture_exception` helper. Only counted when the final status is 500. Sent as aggregated events with per-consumer counts.
  - Missing (also for v0): CPU utilization and memory usage. → Add them.
- [ ] L133-277 "Request logs":
  - L135: "if request logging is enabled". → On by default, for requests kept by sampling (`sample_rate`, `sample_on_request`, `sample_on_response`) and exclusions.
  - L146: `log_query_params` was removed and query strings are always captured. → Recommend `mask_query_params=[r".*"]` to mask all values.
  - L159, L170, L184, L194: `log_request_headers`, `log_request_body`, `log_response_headers` and `log_response_body` → `capture_*`. `mask_request_body_callback`/`mask_response_body_callback` → `mask_request_body`/`mask_response_body`.
  - Bodies are captured only for JSON/text content types, up to 50,000 bytes; larger bodies become `[BODY_TOO_LARGE]`. → State this next to the body items.
  - L203: the response time definition. → As above.
  - L207-221 Consumer: as in metrics.
  - L222-235 Application logs: "disabled by default". → On by default in Python, JavaScript and .NET (`capture_logs`). Go requires wrapping the slog handler with `apitally.NewSlogHandler`. Messages are truncated to 2,048 characters, with at most 1,000 records per request. Add the `mask_log_record` callback.
  - L236-263 Traces: "Only collected if enabled via `capture_traces`". → On by default. The trace includes spans from any OpenTelemetry instrumentation active during the request, with their attributes, such as outgoing URLs and DB queries. These attributes are not masked, except query parameters in URL attributes. Limits: 1,000 spans per request; spans outside requests are never exported.
    - The JavaScript SDK auto-enables undici/fetch instrumentation. The .NET SDK, when it owns the tracer provider, adds HttpClient instrumentation and all ActivitySources.
    - Custom attributes from `set_request_attribute` are included.
    - The example's root span uses `"kind": "INTERNAL"`; in v1 it is `SERVER`.
  - L264-276 Exception info: `log_exception` was removed. → Exceptions are always recorded in traces.
  - Missing: `client.address` is always sent, with no opt-out. v0 only derived the IP from captured headers. Also missing: `user_agent.original` and `server.address`. → Add a "Client IP" item and a "User agent" item.
- [ ] L279-321 "Data masking":
  - L287/L300: `api-?key` → `api[-_]?key`.
  - L310-319: add `cvv` and `cvc` to body fields. `card[-_ ]?number` is unchanged.
  - L321: callback names (as above).
  - Add:
    - `mask_log_record` for application logs
    - query params inside captured `Location`/`Content-Location` headers are masked
    - query params in span URL attributes from other instrumentations are masked
    - failed masking yields `[REDACTED]`, never the raw value
- [ ] L323-349 "Data filtering":
  - L328-333: health, ping, ready and live patterns now accept a trailing slash (e.g. `/_?healthz?/?$`).
  - Missing (also for v0): the default user-agent exclusions `health[-_ ]?check`, `microsoft-azure-application-lb`, `googlehc` and `kube-probe`.
  - L325: excluded requests are dropped from request logs, traces and application logs, but metrics still count them. OPTIONS and websocket requests are never logged, and OPTIONS is not counted in metrics.
  - L345: `exclude_callbacks` (typo; v0 used `exclude_callback`) was removed. → Describe `sample_rate`, `sample_on_request` and `sample_on_response`. `exclude_paths` matches the request path with the query string stripped.
- [ ] L351-353 "Data transit": → v1 uses OTLP/HTTP (protobuf, gzip) over HTTPS to `otlp.apitally.io`. Add that telemetry is buffered on the host in gzip files in the OS temp dir until delivered (max 50 MB, memory fallback), and that the standard proxy env vars are honoured.
- [ ] L363 (pre-existing): retention. → Add traces/spans (15 days). CPU/memory are kept 32 days, not 13 months.
- [x] L357-359 storage and L371 compliance: no change. The OTLP receiver runs in the same DigitalOcean cluster.

### introduction.mdx

- [ ] L56-57: "You'll see the first data in the dashboard within a few seconds". The first export goes out ~2 s after activation, and many frameworks activate on the first request (see how-it-works). → "within a few seconds of the first request".
- [ ] L43-50: "Add middleware to your application". → Optional rewording, since v1 setup is "install the SDK and initialize it" (e.g. `apitally.init`).
- [x] L63: the setup guide list is fixed by the accordion change in Part 1.

### usage-limits.mdx

- [ ] L26 Tip: "exclude certain requests from logging using custom exclusion rules". → Use `sample_rate`, the sampling callbacks and `exclude_paths`. Add that v1 captures request logs, traces and application logs by default, so quotas are used without any configuration.
- [ ] L24: "stops ingesting new request logs, application logs or spans (depending on which quota was exceeded)". For v1:
  - Exceeding the request log quota rejects whole trace exports (HTTP 402), so spans stop too.
  - Exceeding the application logs and spans quota stops spans and application logs only; request logs are still stored.

  → Clarify.

### api-metrics/errors.mdx

- [ ] L36-50: the list of supported validation frameworks. → Update to v1:
  - Express: add Zod (express-validator, celebrate/Joi, Zod).
  - Add H3 (Zod / Standard Schema issues).
  - Add ASP.NET Core (MVC `ModelState`, `ProblemDetails`).
  - Chi: no automatic capture; requires passing the error to `apitally.CaptureError`. Echo, Fiber and Gin need the validator error returned to the framework.
  - Python is unchanged (FastAPI, Django Ninja, Litestar). Koa and Hapi: none.
- [ ] L54-55 Server errors: also captured via the `capture_exception` / `CaptureError` helpers. Counted only when the final status is 500.
- [ ] L67-69 Sentry: v1 supports Sentry linking in Python and JavaScript only, not Go or .NET. → State the supported SDKs.

### api-metrics/performance.mdx

- [ ] L9: "measure the time it takes your route handler functions to respond". → Same as the how-it-works L51 fix.

### api-metrics/endpoints.mdx

- [ ] L64-65: "based on the `Content-Length` headers". → v1 uses `Content-Length` when present and otherwise counts body bytes (streamed and chunked bodies).

### api-metrics/resources.mdx

- [ ] L17-18: "measured per-process and not relative to available cores, the value can exceed 100%". → v1 normalizes across available CPUs (0-100%). Keep the >100% statement as v0-only; both versions feed the same charts.
- [ ] L22: "resident set size (RSS)". → Still correct; .NET reports the working set, which is equivalent. No change unless we want precision.

### api-logs-traces/request-logs.mdx

- [ ] L15-21 "Enable logging": "By default, our SDKs don't capture request logs." → On by default. Request headers and bodies are opt-in, response headers are on by default. Sampling reduces volume. Rename the section (e.g. "Configure logging").
- [ ] Optional: v1 shows requests without a matched route (e.g. 404s) in request logs with an empty endpoint path, but excludes them from metrics.

### api-logs-traces/application-logs.mdx

- [ ] L10: "Log capture must be explicitly enabled alongside request logging". → On by default in Python, JavaScript and .NET. Go requires `apitally.NewSlogHandler`.
- [ ] L18-32 Supported loggers:
  - Go: "slog (must use context-aware logging)" → also requires wrapping the handler with `apitally.NewSlogHandler`.
  - hapi: capture uses `request.log()` and is skipped when `hapi-pino` is registered.
  - .NET: captures the rendered message only.
  - Others unchanged.
- [ ] Add: logs are captured only for requests kept by sampling and exclusions. Messages are truncated to 2,048 characters, with at most 1,000 records per request. `mask_log_record` can modify or drop records.

### api-logs-traces/traces.mdx

- [ ] L10: "If enabled". → Traces are captured for every logged request by default.
- [ ] L19-24 "Enable tracing": "must be explicitly enabled ... alongside request logging". → On by default.
  - The SDKs are built on OpenTelemetry and create the request span automatically.
  - Library spans need instrumentation: Python `apitally.otel.instrument_*`; JavaScript auto-instruments undici/fetch; .NET adds HttpClient and all ActivitySources when it owns the provider; Go picks up any OpenTelemetry instrumentation.
  - Existing OpenTelemetry setups keep their provider; Apitally adds its span processor.
  - Max 1,000 spans per request.

  → Link to the v1 tracing pages.

### api-monitoring/uptime.mdx

- [ ] L9-10: "sending heartbeats every minute". This is still roughly right: v1 metrics exports are the heartbeat, every 15 s in the first hour and 60 s after that, and an environment is offline after 180 s without data. → Optional rewording.
- [ ] Undocumented v1 behaviour: on frameworks that activate on the first request (Express, Koa, Hono, H3, Elysia, Flask/WSGI, Chi, Echo, Gin), a restarted process sends no heartbeats until it handles a request. Low-traffic apps can appear offline, and may trigger uptime alerts (sent after ~5 min without data) after a deploy. → Decide whether to document this or raise it as a product issue.

### No changes needed

`support.mdx`, `api-reference.mdx`, `agents/cli-and-skill.mdx`, `api-metrics/overview.mdx`, `api-metrics/traffic.mdx`, `api-metrics/consumers.mdx` (custom consumer attributes are stored but not shown in the dashboard), `api-monitoring/alerts.mdx`, `sdk-reference/setup-guides/cloudflare-workers.mdx`, and the serverless SDK pages.

## Part 3: Outside this repo (coordinate at GA)

- [ ] cloud dashboard setup instructions: v0 is the default and v1 is labelled "v1 beta" (`AppSetupInstructions.tsx` L102, L207-229). Beta install commands appear in:
  - `app-setup-instructions/shared.tsx` L92-98, L117
  - `python/*-v1.tsx` (`prerelease` prop)
  - `javascript/*-v1.tsx` (`apitally@next`)
  - `go/*-v1.tsx` (`@v1.0.0-beta.2`)
  - `dotnet/aspnet-core-v1.tsx` L26 (`--prerelease`)
- [ ] cloud `utils/frameworks.ts` L82 `getSetupGuideUrl` links to `/setup-guides/<slug>`, which redirects to v0 (see the Part 1 redirects). The anchor `#identify-consumers` (`Consumers.tsx` L102) exists in v1 guides. `#configure-request-logging` (`RequestLogs.tsx` L152) does not; the nearest v1 sections are "Capture headers and bodies" and "Sample requests".
- [ ] cloud `Uptime.tsx` L242: "heartbeats every minute". cloud `Resources.tsx` L218: tooltip "Can be > 100%", which is wrong for v1.
- [ ] SDK repos: `apitally-go/MIGRATION.md` L17-18 (`@v1.0.0-beta.2`) and `apitally-dotnet/MIGRATION.md` L15 (`--prerelease`). Plus the Python upgrade-command addition to `apitally-py/MIGRATION.md`. The Python and JavaScript READMEs have no beta install commands.
- [ ] website blog posts (FastAPI and Hono logging guides, tracing release announcement) show v0 code and options. Once the redirects send their docs links to v1, the code in the posts no longer matches the docs they link to. → Update the posts, or label them as v0.
- [ ] assets.apitally.io: new how-it-works diagrams (`how-it-works-light.png`, `how-it-works-dark.png`).
