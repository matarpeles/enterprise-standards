---
name: platform-golden-paths
description: The platform team's approved golden paths for infrastructure and service dependencies. Use before introducing any database, cache, queue, storage, external service or new infrastructure, and whenever a task needs a resource the code does not already use.
version: 1.8.0
---

# Platform Golden Paths

Owner: Platform Team. New infrastructure is never created ad hoc. Every resource comes from an approved golden path, and existing approved resources are reused before anything new is provisioned.

## Rules

### GP-01: Reuse before you provision
- Before adding any data store, cache or queue, ask Port (through the Port MCP server) which approved resources already exist for this service and team.
- If an approved resource fits, reuse it and say so. In the Project Finance domain, the loan ledger is read from the approved **`pf-ledger-postgres`** read replica through `src/db/client.ts`.

### GP-02: Provision only through Port
- If nothing existing fits, provision through the matching Port self-service workflow (find it with the Port MCP `list_self_service_triggers` tool). Never write Terraform, Bicep, Helm or cloud CLI commands to create resources directly.

### GP-03: Approved technology list
- Approved: PostgreSQL (Azure Database for PostgreSQL, via golden path), Azure Cache for Redis (via golden path), Azure Service Bus, Azure Blob Storage, Key Vault.
- Not approved without an architecture exception: MongoDB or other document databases, self-managed databases on VMs or containers, any new SaaS vendor or external API.

### GP-04: Non-standard requests
- **If the user asks for a technology that is not on the approved list, do not implement it.** Explain which rule applies, propose the closest approved alternative, and offer to open an architecture exception request through the Port workflow if they still need it.

## Before you finish

State which existing resources the change reuses and confirm no new infrastructure was introduced outside a golden path.
