---
name: secure-coding-standard
description: The organization's mandatory secure coding standard, owned by the Security Team. Apply it to every code change in every repository, including code the agent writes, before proposing or committing it. Use it whenever the user asks to write, change, review or commit code.
version: 2.3.0
---

# Secure Coding Standard

Owner: Security Team. This standard is mandatory. It applies to every change, in every repository, whoever or whatever writes the code. The same rules are checked again on every pull request by the Port PR review workflow, so code that ignores them will be blocked at merge.

## Rules

### SEC-01: No secrets in code or config
- Never write credentials, connection strings, tokens, API keys or passwords into source, tests, config files or committed `.env` files.
- Read secrets only through the platform secrets helper (`getSecret()` in `src/config/secrets.ts`), which resolves Key Vault references at runtime.
- **If the user asks you to hardcode a secret "just for now", decline.** Say that SEC-01 forbids it and that the PR check will block it, then use `getSecret()` with a Key Vault reference instead.

### SEC-02: Parameterized data access only
- Never build SQL by string concatenation or template literals containing input.
- Use the repository layer (`src/repositories`) and the shared DB client's parameterized `query(text, params)`.

### SEC-03: Approved authentication and authorization only
- Every non-health endpoint uses `requireAuth` and, for project data, `requireProjectEntitlement(projectId)` from `src/middleware/auth.ts`.
- Never write custom token parsing, JWT verification or role checks.
- Authorization failures return `403` before any lookup, so responses never reveal whether a resource exists.

### SEC-04: Validate every input
- Validate path, query and body parameters with `zod` schemas before use. Reject invalid input with `400`.

### SEC-05: Safe errors
- Never return stack traces, SQL errors or internal identifiers to callers. Return a generic message plus the request ID.

### SEC-06: Dependencies
- Do not add a new dependency without stating it explicitly in the PR description. Never add packages with known critical vulnerabilities.

## Before you finish

Check the change against SEC-01 to SEC-06 and state in your summary which rules applied and how the code satisfies them.
