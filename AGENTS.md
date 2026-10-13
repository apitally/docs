# Agent guidance

Public documentation for Apitally at https://docs.apitally.io, built with Mintlify. Every push to `main` deploys to production.

## Writing standard

The readers are developers setting up and using Apitally. Write for them: clear, direct and technically accurate. No filler, marketing language, hedging or generic AI phrasing; every word must earn its place.

Treat every sentence in these docs as production code that thousands of developers rely on. Before writing any text, work out what the reader needs to know and why. Then write a draft, read it as that reader, and question every word: is it accurate, necessary and unambiguous, and is it consistent with the rest of the docs and the source it describes? Revise until you would sign off on it yourself. Only present text you believe is final, not a first attempt.

Read the whole page before editing it. Put new information where a reader working through the page would look for it, and integrate it into the existing text: rework, merge or reorder the surrounding sentences, or restructure the section if needed, instead of appending a sentence to the nearest paragraph. After every edit, reread the whole page and check that it still reads in a logical order, without repetition or contradiction, as if it had been written in one pass.

## Sources of truth

Verify every documented behavior, option, default, signature, version requirement and code example against the source repo, never against memory or other docs pages.

### SDKs

| Docs                                   | SDK repo                          | Local path                  |
| -------------------------------------- | --------------------------------- | --------------------------- |
| `sdk-reference/python/`                | `apitally/apitally-py`            | `../apitally-py`            |
| `sdk-reference/python-serverless/`     | `apitally/apitally-py-serverless` | `../apitally-py-serverless` |
| `sdk-reference/javascript/`            | `apitally/apitally-js`            | `../apitally-js`            |
| `sdk-reference/javascript-serverless/` | `apitally/apitally-js-serverless` | `../apitally-js-serverless` |
| `sdk-reference/go/`                    | `apitally/apitally-go`            | `../apitally-go`            |
| `sdk-reference/dotnet/`                | `apitally/apitally-dotnet`        | `../apitally-dotnet`        |

- `v1/` pages document the `main` branch of the SDK repo and `v0/` pages the `v0` branch. The serverless SDKs have only `v0/` pages, which document `main`.
- Each v1 SDK repo has a `MIGRATION.md` that mirrors `sdk-reference/<language>/v1/migration.mdx`. A change to one usually needs the same change in the other.

### App and CLI

| Docs                                                  | Repo                       | Local path |
| ----------------------------------------------------- | -------------------------- | ---------- |
| `api-metrics/`, `api-logs-traces/`, `api-monitoring/` | `apitally/cloud` (private) | `../cloud` |
| `api-reference.mdx`                                   | `apitally/cloud` (private) | `../cloud` |
| `agents/cli-and-skill.mdx`                            | `apitally/cli`             | `../cli`   |

The API reference endpoint pages are generated from https://api.apitally.io/openapi.json, which is served by `../cloud/apitally_cloud/api`.

## Editing pages

- `docs.json` defines navigation and redirects. Add new pages to the navigation, and add a redirect when moving or renaming a page.
- The v1 SDKs (built on OpenTelemetry) have the same set of pages for Python, JavaScript, Go and .NET. Keep headings, order and shared wording parallel across languages; a change to one language's page usually applies to the others.
- `snippets/` holds MDX shared between pages. Check every page that imports a snippet before changing it.
- Images are hosted at `https://assets.apitally.io`, not in this repo.
- Write each paragraph on a single line. Don't hard-wrap prose or run Prettier on MDX files.

## Checks

Mintlify doesn't support Node 25+, so run the CLI with the Node version from `.tool-versions` via mise:

```shell
mise exec -- mint validate
mise exec -- mint broken-links
mise exec -- mint dev  # local preview
```
