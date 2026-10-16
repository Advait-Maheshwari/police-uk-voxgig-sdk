# Voxgig mini task 1: Police UK Forces SDK

## Result and scope

A small, unofficial TypeScript SDK was generated with the official Voxgig SDK tooling for the public Police UK API. It covers only:

- `GET /forces` → `client.Force().list()`
- `GET /forces/{id}` → `client.Force().load({ id })`

The input is a minimal OpenAPI document at `.sdk/def/openapi.json`, based on the provider's [force list](https://data.police.uk/docs/method/forces/) and [individual force](https://data.police.uk/docs/method/force/) documentation. This is not a complete Police UK API SDK or an official Police UK product. The endpoints need no account, API key or paid trial.

The Voxgig SDK organisation catalogue was checked on 16 October 2026 at 10:53:38 UTC: 804 repository names/descriptions across the paginated API, with no match for `police`, `police.uk` or `crime`. That is a catalogue check, not a source-level inspection of every repository.

## How it was built

The project was scaffolded using the official `@voxgig/create-sdk` workflow, with a TypeScript target and test feature. Generation ran through the resulting `.sdk` project using `@voxgig/sdkgen` 4.34.3 and `@voxgig/apidef` 8.25.0. The project overlay records the repository owner and package metadata. The generated SDK, model, templates and OpenAPI input are retained so the result can be regenerated.

## Verification actually performed

Checks were run by the AI assistant in its execution environment on 16 October 2026, using Node.js 24.19.0:

- TypeScript build: passed.
- Generated test suite: 219 tests reported, 210 passed, 9 skipped, 0 failed. These are mostly generated runtime/mock tests, not 210 real API integration checks.
- Four added contract tests: all passed. They check list URL and method, loading by ID without a duplicate query parameter, HTTP 404 rejection, and transport-error rejection.
- Live read-only smoke test: passed. The force list returned 44 records; loading `leicestershire` returned `Leicestershire Police`; a deliberately invalid force ID rejected.

The live checks demonstrate the two selected endpoints at the time of testing. They do not establish production readiness, exhaustive schema coverage or future API availability. No credentials or personal customer data were used.

## Developer-experience observations

The entity-style interface is compact: list/load operations map cleanly to the two endpoints. Keeping the API input and generated project together makes it possible to understand where the client comes from. The built-in test transport is useful for repeatable offline checks, and the generated documentation explains that list results are entity instances whose payload is read with `.data()`.

For this small exercise, the generated scaffold is considerably larger than the API-specific code: it includes reusable pipeline utilities, documentation machinery and workflows for multiple targets. A minimal single-target quickstart would make the first successful API call easier to find. The generated test count should also be clearly separated from API-specific integration coverage. Explicit examples for 404/error handling and adding a timeout would help a new user judge what the default client handles.

## Limits and next improvements

Only two read-only force endpoints are modelled. Crime search, neighbourhoods, pagination/rate-limit behaviour, retries and a wider set of malformed responses were not investigated. The smoke script has not been validated across Node versions. No npm package has been published and no hosted service is deployed. GitHub-hosted CI is separate from the successful local checks.

Next steps would be to add more documented endpoints, expand response/error fixtures, add a bounded timeout to the live example and validate the generated package in a clean installation. Those are proposed improvements, not completed work.

## AI assistance and attribution

Advait requested and directed an AI-assisted implementation. The assistant selected the API, prepared the specification, ran Voxgig generation, wrote the additional checks and this report, and executed the tests. The technical observations above come from that automated work. This report does not claim that Advait personally wrote, ran or reviewed those steps. Any later human review or changes should be described only after they occur.

The exercise is subject to Richard's maximum of 30 minutes of human work. Machine execution time is not asserted to be human work time, and this report does not invent a personal time log. Work is intentionally limited to the small scope above.

## Run it

From the repository root:

```sh
cd ts
npm ci
npm test
cd ..
node --test checks/contract.test.cjs
node examples/live-smoke.cjs
```

To regenerate from the retained specification/model:

```sh
cd .sdk
npm ci
npm run generate
```

The SDK is MIT-licensed; see `LICENSE`. API data and service usage remain subject to the upstream provider's terms.
