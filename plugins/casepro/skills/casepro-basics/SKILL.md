---
description: Core CasePro concepts and how to drive the connector's tools. Use whenever the user asks about their CasePro matters, clients, parties, injuries, providers, bills, liens, demands, or other legal-CRM data, or when you first connect to CasePro.
---

# CasePro Basics

CasePro is a legal-CRM for personal-injury law firms. You are connected via the **casepro** MCP connector and act on behalf of the signed-in user, automatically scoped to their organization. You never need a URL, API key, or org id — the OAuth connection handles all of that.

## Before you start: connection & sign-in

This skill tells you HOW to use CasePro, but the actual work happens through the **casepro connector tools** — `execute_operation`, `query_entities`, `get_entity`, `list_schema`, `search_documents`, `get_document`, `discuss_document`, and similar.

- **If those tools are NOT available in this chat:** the CasePro connector is installed but not turned on/connected for this conversation. Tell the user plainly: *"Please enable the CasePro connector for this chat and sign in to CasePro."* In Claude web/desktop they enable it from the connectors/tools menu for the conversation; the first call prompts a browser sign-in. **Do NOT try to do CasePro work through a web browser or any other tool** — only the connector tools.
- **If a tool call comes back not-signed-in / unauthorized:** tell the user to sign in to CasePro (the connector shows a Connect / sign-in prompt; approving it in the browser is all that's needed), then retry. Never fall back to browsing the CasePro website.

## Core terminology

- **Matter** — a case/claim. The central record. Has a `matter_name`, `matter_number` (auto, e.g. `CP-2026-001`), `status`, an immutable link to an intake questionnaire, and arrays of parties.
- **Party** — a person or entity on a matter. `record_type` is one of `Individual`, `Business`, `Medical Provider`, `Insurance Company`, `Vendor`.
- **Client** — the firm's client on a matter (`client_id` → a party).
- **Plaintiff / Defendant** — arrays of party ids on the matter (client side vs at-fault side).
- **Injury** — an injury recorded on a matter.
- **Medical Provider / Bill** — a provider party and its bills (with reductions).
- **Insurance** — insurance records on a matter. **Lien / Negotiation / Demand** — financial-recovery records.

## How the tools work

The connector exposes generic, schema-driven tools — you drive them with the entity **name** and fields, not raw SQL:

- **execute_operation** — the primary tool. Give it a plain-English instruction ("Get matter X and return all fields", "Create a party…"). It discovers the right tables, fetches the schema, and runs the plan.
- **query_entities / get_entity / aggregate_data** — structured reads.
- **list_schema** — discover tables, columns, types, and allowed values. **Call this first whenever you are unsure** of a table name, a column, or an enum value. Never invent them.
- **create_entity / update_entity / execute_workflow** and other write tools for mutations. Prefer **validate_operation** (a dry run) before a risky write.
- **search_documents / get_document / discuss_document** — find and read the user's documents (see the documents skill).
- **search_document_templates / get_document_template** — the firm's saved letter/demand templates and letterheads.
- **whoami** — the signed-in user and their organization (the **law firm** — name, contact details). Call this whenever you need the firm name or the user's name; never guess them.

## Golden rules

- Organization scoping and soft-delete are handled by the server. Don't add your own org filters; remember "deleted" rows are hidden, not returned.
- When a request is ambiguous (which matter? which party?), ask, or search first to disambiguate.
- Always tell the user which matter/party you acted on.
