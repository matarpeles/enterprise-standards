---
name: api-service-standards
description: Coding standards for backend API services, covering structure, layering, error contracts, observability, traceability and mandatory tests. Use when adding or changing an endpoint, repository, or service module in a backend repository.
version: 3.1.0
---

# API and Service Coding Standards

Owner: Backend Team.

## Structure and layering

- Routes live in `src/routes/<resource>.ts` and only handle HTTP concerns: validation, auth middleware and status codes.
- Business logic lives in `src/services/`.
- Data access lives in `src/repositories/` and is the only layer that talks to the DB client.
- Map raw ledger or DB codes to API enums in the service layer, never in routes.

## API contract

- Version every public path under `/v1/`.
- Use `camelCase` JSON fields and ISO-8601 UTC timestamps.
- Error body is always `{ "error": { "code": string, "message": string, "requestId": string } }`.
- `403` for missing entitlement (checked first), `404` for not found, `400` for invalid input.

## Observability

- Every route records a latency histogram and a status counter through `src/lib/metrics.ts`.
- Unexpected but handled conditions (for example an unknown upstream status code) emit a warning metric instead of failing the request.

## Tests are mandatory

- Every new endpoint ships with unit tests for its service logic and integration tests covering at least the 200, 400, 403 and 404 paths.
- Coverage of new code must be at least **80%**. The PR check reads the coverage report.
- Run `npm test` and make sure it passes before declaring the work done.

## Traceability

- Branch name: `feature/<FEATURE-KEY>-short-name` (for example `feature/PFP-212-disbursement-status`).
- The PR title starts with the feature key, and the description links the Port work item and the ADO work item.
