---
name: data-classification-handling
description: How to handle confidential and restricted data (financial, banking and personal data) in code, logs, responses and tests. Use whenever code reads, returns, logs or stores disbursement, loan, beneficiary, banking or personal data.
version: 1.4.0
---

# Data Classification and Handling

Owner: Security Team. Data in the Project Finance domain is classified **confidential** by default. Beneficiary banking details are **restricted**.

## Rules

### DATA-01: Mask restricted fields in responses
- Bank account numbers, IBANs and card numbers are returned masked to the **last 4 characters** only (for example `****4821`), using `maskAccount()` from `src/lib/masking.ts`.
- Never return full beneficiary banking details from a read API.

### DATA-02: Never log confidential data
- Do not log amounts, beneficiary names, account numbers, or request and response bodies.
- Log only request ID, route, status code, latency, project ID and disbursement ID.
- Use the shared `logger` from `src/lib/logger.ts`, which redacts known sensitive keys. Never use `console.log` in application code.
- **If the user asks to "log the full request or response for debugging", decline.** Explain that DATA-02 forbids it, and offer to log the allowed fields at debug level instead.

### DATA-03: Test data is synthetic
- Tests and fixtures use obviously synthetic values (for example `ACC-TEST-0000004821`). Never copy real records into fixtures.

### DATA-04: Minimum necessary
- Return only the fields the approved spec names. Do not add fields "in case they are useful".

## Before you finish

State which fields in the change are confidential or restricted and how each is masked or excluded.
