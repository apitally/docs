---
directory: /Users/simon.gurcke/Repos/apitally/docs
implemented_at: null
---

# Docs updates for v1 SDK GA

Make the v1 SDKs (Python, JavaScript, Go, .NET) the default and remove beta mentions. Then update general pages that still describe v0 behaviour.

Baseline: docs `40c9184`. Facts checked on 2026-10-14 against apitally-py `53a4d57`, apitally-js `1297654`, apitally-go `0f17a37`, apitally-dotnet `5fb2d92` and cloud `6bc15859` (including `cloud/docs/sdks/spec.md` and `design.md`). Line numbers refer to the baseline.

## Approach

General pages describe how Apitally works with the current SDKs. Keep copy to what a developer needs. SDK internals (buffer sizes, limits, per-framework activation) stay out unless a page's purpose requires them.

### Collaboration workflow

Follow this workflow with the user when continuing this plan.

1. Work through one item at a time, in order, unless the user explicitly defers it. Treat a paragraph or section as a coherent piece of writing rather than adding sentences mechanically for checklist items.
2. Agree on the message before drafting wording. Verify relevant behaviour against source code, then discuss what the reader needs to know and what belongs in the text. Use verification to support a focused editorial discussion. Plan items and verification findings are inputs to that discussion, not automatic additions to the docs.
3. Once the message is agreed, present three wording options in context, with enough surrounding text to judge how they read. Use independent subagents for drafting and polishing, then review their work before presenting it. Present finished wording, not raw drafts.
4. Edit only after the user approves the wording. Apply the selected option and requested adjustments without adding unapproved claims or qualifications.
5. Write for the reader. Keep text clear, direct, accurate and necessary. Describe current behaviour without migration framing. Avoid unnecessary internals, unsolicited caveats, generic AI phrasing, em dashes, semicolons and mid-sentence colons. Follow the detailed writing rules below.
6. Review the integrated result. Reread the whole page after editing, check flow and repetition, run appropriate checks, and update the plan to reflect the actual decisions.
7. Leave changes uncommitted unless the user asks to commit. Push only when asked. Explicitly deferred work stays deferred.

### Writing rules for copy changes

These rules apply to every copy edit in Part 2, in addition to the writing standard in `AGENTS.md`.

**The result must read as if it had always been written this way.**

- Describe current behaviour as the behaviour. Never frame it as a change, with words such as "now", "no longer", "new", "previously", "has been replaced", "starting with" or "unlike before".
- Don't mention v0, v1 or SDK versions on general pages. Refer to "the Apitally SDKs" or "the SDK for <language>". Comparisons with v0 belong only in the migration guides.
- The plan items contain context for the editor, such as notes like "(also for v0)", "pre-existing" or "v0 did X", and → instructions. They explain why an edit is needed and which facts to convey. None of it goes into the copy, and the wording of an item is not the wording to use.

**Edit pages, not sentences.**

- Before editing, read the entire page and the pages it links to. Understand what each section is for and the order in which a reader works through it.
- Treat all plan items for a page as one revision of that page. When a fact changes the premise of a section (e.g. a section titled "Enable logging" when logging is on by default), rewrite the section from the new premise, including its heading, opening sentence, body and links. Never fix one sentence and leave neighbouring sentences that still rest on the old premise.
- Put new facts where a reader would look for them. Merge them into existing sentences and lists, and reorder or restructure when needed. Don't append sentences to the nearest paragraph, and don't add Note, Tip or Warning callouts to patch in information.
- Remove what no longer has a reason to exist. A revised section is usually about as long as the original, or shorter. Any extra length needs a clear reason.
- When a fact changes, text that only explained or qualified the old behaviour goes with it. Don't replace it with a statement of what a reader would expect anyway. For example, CPU values above 100% needed an explanation, but values up to 100% don't, so that sentence is deleted and not rewritten as "values can't exceed 100%". Never describe the absence of a limitation, caveat or required step.

**Match the page and the rest of the docs.**

- Keep the page's voice, tense, level of detail, heading style, list style, callout use and formatting of code and option names.
- Use terms the docs already use (request logs, consumers, environments, endpoints). Keep OpenTelemetry vocabulary (span, OTLP, resource attributes) off product pages unless the page needs that precision, as data-privacy does for listing collected data, or already uses it, as traces.mdx does for "span". Link to the SDK reference for details instead of repeating them.
- State a fact the same way on every page where it appears, e.g. what response time measures or what is captured by default. Settle the wording where the fact first appears and reuse it.
- Mention a per-language difference only when a reader would otherwise act wrongly (e.g. Go needs `apitally.NewSlogHandler` to capture logs). Keep it to one clause and link to the SDK reference.

**Write precisely.**

- Verify every fact against the sources of truth in `AGENTS.md` at the time of editing. The facts in this plan were checked against the baseline commits and may have changed since.
- No hedging unless the behaviour really is conditional. No marketing words and no filler such as "It's worth noting", "This ensures that", "seamless", "comprehensive" or "robust".
- No em dashes, and no colons or semicolons in the middle of a sentence. Split the sentence or rephrase it. A colon is fine at the end of a sentence that introduces a list or code block.

**Check before handing over.**

1. Reread the whole page top to bottom as a first-time reader. It must flow, contain no repetition or contradictions, and refer to nothing that was removed. Each heading must still describe its section.
2. Read the diff on its own and ask whether a reader who never saw the old page would notice anything patched in.
3. Search the changed text for change language and version references (`now`, `no longer`, `new`, `previously`, `v0`, `v1`), and for semicolons and mid-sentence colons.
4. When a heading changes, search this repo and the cloud and website repos for links to the old anchor.
5. Run `mise exec -- mint broken-links`.
6. Work on one page at a time and leave changes uncommitted for review. Tick items only after the review.

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

### Remove beta mentions (v1 pages)

- [x] Overview pages (4): remove `<Warning>The Apitally SDK v1 is currently in beta.</Warning>` (L29 in `python|javascript|go|dotnet/v1/overview.mdx`).
- [x] Setup guides (21: Python 7, JavaScript 9, Go 4, .NET 1): remove `<Info>This setup guide is for v1 of the ... SDK (currently in beta).</Info>` (L9 or L10). Recommendation: remove it entirely.
- [x] `javascript/v1/setup-guides/h3.mdx` L12 and `javascript/v0/setup-guides/h3.mdx` L20: "H3 v2 (currently in beta)". H3 2.x is now stable (`latest` on npm is 2.0.2). → Remove "(currently in beta)". The Requirements line (`2.0.1-rc.26+`) matches the SDK peer dependency, so keep it.

### Install commands (v1 pages)

- [x] Python: `python/v1/overview.mdx` and the 7 setup guides. → `pip install --pre` → `pip install`, `uv add --prerelease allow` → `uv add`, `poetry add --allow-prereleases` → `poetry add`.
- [x] JavaScript: `javascript/v1/overview.mdx` and the 9 setup guides. → `apitally@next` → `apitally` for npm, yarn, pnpm and bun. Keep `@opentelemetry/api` on the yarn lines.
- [x] Go: `go/v1/setup-guides/{chi,echo,fiber,gin}.mdx` and `go/v1/migration.mdx` L18-38. → Drop `@v1.0.0-beta.2`. A plain `go get` resolves to the latest release and also upgrades an existing v0 requirement.
- [x] .NET: `dotnet/v1/overview.mdx` L38, `setup-guides/aspnet-core.mdx` L38, `migration.mdx` L17. → Drop `--prerelease`.
- [x] Python migration (`python/v1/migration.mdx` L14): the page sends users to the setup guides for installation, but those commands don't upgrade an existing 0.x install. → Added upgrade commands: `pip install -U`, `uv add "...>=1"` (a plain `uv add` keeps the existing requirement) and `poetry add ...@latest`. The `MIGRATION.md` mirror is in Part 3.

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
- [x] L6: "lightweight middleware or plugin". → v1 SDKs are built on OpenTelemetry and export OTLP directly to Apitally, with no collector or agent. They work alongside an existing OpenTelemetry setup.
- [ ] L17-28: the diagram (light and dark) shows "Apitally Hub" and "syncs with Apitally in 1 minute intervals". → Replace it with a v1 diagram (Part 3). If none is ready by GA, remove the images.
- [x] L30-36: the functionality list ends with a "Non-blocking client" and lists "OpenTelemetry integration to capture traces" as an add-on. → Describe the v1 parts at the same level of detail (4-5 bullets): request instrumentation, log capture, metrics aggregation, background export.
- [x] L44-47 "On startup": "introspects ... sent in a once-off request". → Describe application metadata collection and background delivery, with endpoint details and version information as examples. Keep configuration and OpenAPI collection details for the data-privacy page.
- [x] L51: "times the invokation of the request handler". → Describe response time and request/response metadata capture. Avoid a universal streaming-completion claim because Fiber finalizes known-length streams when the handler returns.
- [ ] Response-time wording follow-up: align the runtime paragraph with the approved data-privacy definition, "Time taken by your application to respond to the request." The user rejected "server-side response time" as documentation terminology.
- [x] L52: "If the response is a validation error (status 400 or 422), the middleware inspects the response body". → Omit the validation-error explanation from the runtime section. Keep the section focused on request observation, local aggregation, traces and background delivery.
- [x] L54-55: "synchronizes the aggregated data with Apitally in one minute intervals". → Describe background delivery as normally about once a minute. Omit the faster schedule during the first hour of an app environment.
- [x] L57-58: "If request logging is enabled, the client writes ... to a compressed temporary file". → Describe traces, associated application logs and local buffering as part of the whole-section rewrite. Keep configuration defaults and controls in the SDK reference. Integrate background delivery into the prose and remove the redundant runtime note.
- [x] L66: the queue statement is still accurate (the OTLP receiver publishes to NATS JetStream). No change.
- [x] L68 (pre-existing): CPU/memory data is kept for 32 days only, not aggregated for 13 months. → Scope the 32-day and 13-month retention periods to request metrics, without implying delayed aggregation. State 15-day retention for request logs and traces, with longer retention available on request. Keep CPU/memory retention details on the data-privacy page.

### data-privacy.mdx

Customers use this page for vendor and GDPR assessments, and it claims to be a complete overview of collected data. v1 collects more by default than v0: traces, application logs, client IP and user agent.

- [x] L9: "Require explicit user opt-in for features that could capture sensitive data". This no longer holds, because traces and application logs are on by default. → Remove the opt-in principle and retain the existing control and transparency principles. Explain capture defaults in the data collection sections.
- [ ] L20-21: says option names are in Python `snake_case`. → Keep, but use v1 Python names throughout.
- [ ] L24: the legend "Potentially sensitive and not collected by default" no longer matches application logs and traces. → Replace shields with toggle icons for configurable collection. Use primary-colored toggle-on for collected by default and red toggle-off for not collected by default. The approved legend is applied. Update each entry's icon alongside its capture defaults as its section is revised; leave items without a capture setting unmarked.
- [x] L26-58 "Application metadata":
  - Describe the collected metadata without a startup-only claim. Include endpoint summaries and descriptions where available.
  - Replace Versions with Framework and versions, covering the framework identifier, runtime and framework package versions, and optional application version. Remove the Apitally version from the example; it belongs to resource attributes.
  - Add SDK configuration, including capture, sampling, masking and exclusion settings. Configured callbacks are sent as `true`, not their code. The write token is excluded.
  - Add the OpenAPI specification from FastAPI, Litestar and Django (DRF/Ninja), where available.
  - Add resource attributes covering service name, random instance identifier, environment, SDK names and versions, and custom attributes from environment variables or an existing provider.
  - Keep collection descriptions focused on what is collected. Omit unsolicited masking caveats.
- [x] L60-131 "Metrics & analytics" (reviewed; broken-link and build validation checks passed):
  - Replace Metrics & analytics with peer-level Request metrics and Application metrics sections, alongside Application metadata and Request logs. Retain all existing request entries and links.
  - Describe size and response-time measurements as histograms without bucket implementation details.
  - Use the approved response-time definition, "Time taken by your application to respond to the request." Reuse this wording on related pages.
  - Add consumer custom `attributes` to the description and example. Remove the shield without adding a toggle.
  - Update validation error fields to `source`, `field`, `message` and `type`. Remove the invalid-value exclusion claim without adding a warning.
  - Add exception type and remove the restriction to unhandled exceptions. Retain the HTTP 500 condition and optional Sentry event ID.
  - Add periodic application-process CPU utilization, resident memory usage and uptime.
  - Keep the existing copy wherever possible. Describe collected data neutrally, without alarming clauses or unnecessary caveats.
  - Review the three introductions together in page order: Application metadata, Application metrics, Request metrics. Approved wording describes application details, periodic per-process measurements, and request measurements combined into counts and histograms within the application before delivery to Apitally. Avoid repeated "The Apitally SDKs collect the following" introductions.
- [x] L133-277 "Request logs" (reviewed; broken-link and build validation checks passed):
  - The user authorized selecting and integrating the wording directly for review instead of choosing between bundled options. Preserve existing fields, examples and links where possible.
  - Introduce the contents of individual request records, including associated application logs and traces. Keep default-state announcements and sampling controls out of the introduction. Sampling belongs in Data filtering.
  - L146: remove the obsolete query-string stripping instruction. Retain the full URL, query-parameter masking description and example.
  - L159, L170, L184, L194: `log_request_headers`, `log_request_body`, `log_response_headers` and `log_response_body` → `capture_*`. `mask_request_body_callback`/`mask_response_body_callback` → `mask_request_body`/`mask_response_body`.
  - L203: the response time definition. → As above.
  - L207-221 Consumer: as in metrics.
  - L222-235 Application logs: describe log records and their metadata and attributes. Correct the field-level `capture_logs` default and use toggle-on. Keep Go setup and integration details out of this privacy inventory.
  - L236-263 Traces: remove the obsolete `capture_traces` instruction and shield. Describe the request span and associated spans with their attributes, including outgoing URLs and database queries supplied by instrumentation. Keep per-language instrumentation details out.
    - Custom attributes from `set_request_attribute` are included.
    - The example's root span uses `"kind": "INTERNAL"`; in v1 it is `SERVER`.
  - L264-276 Exception info: describe type, message and stack trace recorded in the request's trace. Remove `log_exception` and the unhandled/HTTP 500 restriction, which does not apply to all trace exceptions. Retain the optional Sentry event ID and example.
  - Missing: client IP (`client.address`) is always sent, with no opt-out (v0 only derived it from captured headers). The user agent is also always sent. → Add "Client IP" and "User agent" items.
- [x] L279-321 "Data masking" (reviewed; broken-link and build validation checks passed):
  - L287/L300: `api-?key` → `api[-_]?key`.
  - L310-319: add `cvv` and `cvc` to body fields, retaining `ccv`. Describe the scope as string-valued fields in JSON request and response bodies.
  - L321: use `mask_request_body` and `mask_response_body` for the callbacks.
  - Add a short Application logs subsection identifying `mask_log_record` for masking log messages.
  - Scope the case-insensitive, substring matching description to the default patterns. Preserve the existing structure and omit per-language differences and warning paragraphs.
  - The user explicitly excluded the proposed explanation of query-parameter masking in outgoing trace URLs.
- [x] L323-349 "Data filtering" (reviewed; broken-link and build validation checks passed):
  - Use the user's exact two paragraphs explaining selective exclusion of request logs and associated traces and application logs, using paths or sampling callbacks. Exclusions happen within the application before delivery, while request metrics remain collected.
  - Follow with compact links to the Python, JavaScript, Go and .NET Sampling pages.
  - Keep default exclusion inventories, sampling-rate instructions and dashboard filtering instructions out of this privacy section.
  - Move the verified default path and user-agent patterns into `snippets/v1/default-exclusions.mdx`, imported by all four SDK Sampling pages in place of their links back to the privacy page. Correct trailing-slash matching in the health, ping, ready and live patterns. Place Default exclusions at the bottom of all four pages, after Exclude paths.
  - Follow-up pending approval: `snippets/default-masking-and-exclusion.mdx`, used by six v0/serverless pages, still promises default exclusion lists on the privacy page.
  - Scope approval is not wording approval. Present independently drafted and polished options, then wait for explicit wording approval before editing.
- [x] L351-353 "Data transit": verified HTTPS/TLS behavior across all four v1 SDKs. Keep the existing wording unchanged. The user excluded local temporary-file storage as irrelevant to this page; do not add it under Data transit or Data storage.
- [x] L363 (pre-existing): retention (approved wording applied; whitespace, broken-link and build validation checks passed).
  - Preserve "allowing you to analyze long-term trends" and the existing deletion and backup paragraphs.
  - Specify aggregated request metrics for 13 months; CPU utilization, memory usage and uptime monitoring data for 32 days; request logs, application logs and traces for 15 days by default.
  - Reuse "with longer retention available on request" from `how-it-works.mdx` for logs and traces.
- [x] L357-359 storage and L371 compliance: no change. The OTLP receiver runs in the same DigitalOcean cluster.

### introduction.mdx

- [ ] L56-57: "You'll see the first data in the dashboard within a few seconds". Many frameworks (e.g. Express, Flask, Go) only start sending data on the first request. → "within a few seconds of the first request".
- [x] L63: the setup guide list is fixed by the accordion change in Part 1.

### usage-limits.mdx

- [ ] L26 Tip: "exclude certain requests from logging using custom exclusion rules". → Use `sample_rate`, the sampling callbacks and `exclude_paths`.

### api-metrics/errors.mdx

- [ ] L36-50: the list of supported validation frameworks. → Update to v1:
  - Express: add Zod (express-validator, celebrate/Joi, Zod).
  - Add H3 (Zod / Standard Schema).
  - Add ASP.NET Core (MVC `ModelState`, `ProblemDetails`).
  - Chi: requires passing the error to `apitally.CaptureError`, as in v0.
  - Python is unchanged (FastAPI, Django Ninja, Litestar). Koa and Hapi: none.
- [ ] L67-69 Sentry: v1 supports Sentry linking in Python and JavaScript only. v0 Go and .NET advertised it. → State the supported SDKs.

### api-metrics/performance.mdx

- [ ] L9: "measure the time it takes your route handler functions to respond". → Use the approved data-privacy definition, "Time taken by your application to respond to the request."

### api-metrics/endpoints.mdx

- [ ] L64-65: "These are based on the `Content-Length` headers in the requests and responses." Sizes are the actual body sizes, including streamed and chunked bodies, which is what a reader expects. → Delete the sentence.

### api-metrics/resources.mdx

- [ ] L17-18: "measured per-process and not relative to available cores, the value can exceed 100%". → Delete the second sentence. Nothing replaces it.

### api-logs-traces/request-logs.mdx

- [ ] L15-21 "Enable logging": "By default, our SDKs don't capture request logs." → On by default. Request headers and bodies are opt-in, response headers are on by default. Sampling reduces volume. Rename the section (e.g. "Configure logging").

### api-logs-traces/application-logs.mdx

- [ ] L10: "Log capture must be explicitly enabled alongside request logging". → On by default in Python, JavaScript and .NET. Go requires `apitally.NewSlogHandler`.
- [ ] L29-30 Supported loggers, Go: "slog (must use context-aware logging)". → Also requires wrapping the handler with `apitally.NewSlogHandler`.

### api-logs-traces/traces.mdx

- [ ] L10: "If enabled, you'll find traces in the request details". → Drop "If enabled".
- [ ] L19-24 "Enable tracing": "must be explicitly enabled ... alongside request logging". → On by default. The SDKs are built on OpenTelemetry, create the request span automatically and work alongside an existing OpenTelemetry setup. Link to the per-language v1 tracing pages for instrumenting libraries.

## Part 3: Outside this repo (coordinate at GA)

- [ ] cloud dashboard setup instructions: v0 is the default and v1 is labelled "v1 beta" (`AppSetupInstructions.tsx` L102, L207-229). Beta install commands appear in:
  - `app-setup-instructions/shared.tsx` L92-98, L117
  - `python/*-v1.tsx` (`prerelease` prop)
  - `javascript/*-v1.tsx` (`apitally@next`)
  - `go/*-v1.tsx` (`@v1.0.0-beta.2`)
  - `dotnet/aspnet-core-v1.tsx` L26 (`--prerelease`)
- [ ] cloud `utils/frameworks.ts` L82 `getSetupGuideUrl` links to `/setup-guides/<slug>`, which now redirects to v1. The anchor `#identify-consumers` (`Consumers.tsx` L102) exists in v1 guides. `#configure-request-logging` (`RequestLogs.tsx` L152) does not; the nearest v1 sections are "Capture headers and bodies" and "Sample requests".
- [ ] cloud `Resources.tsx` L218: tooltip "Can be > 100%", which is wrong for v1.
- [ ] SDK repos: `apitally-go/MIGRATION.md` L17-18 (`@v1.0.0-beta.2`) and `apitally-dotnet/MIGRATION.md` L15 (`--prerelease`). Add the Python upgrade commands from `python/v1/migration.mdx` to `apitally-py/MIGRATION.md`.
- [ ] website blog posts (FastAPI and Hono logging guides, tracing release announcement) show v0 code and options, but their docs links now lead to v1 pages. → Update the posts, or label them as v0.
