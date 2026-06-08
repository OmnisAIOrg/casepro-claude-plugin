---
description: How to read and query CasePro data — natural-language patterns for execute_operation, plus query_entities and aggregate_data. Use when the user wants to look up, find, list, count, or report on matters, parties, providers, bills, injuries, insurances, negotiations, tasks, or notes.
---

# CasePro Query Patterns

Drive **execute_operation** with plain-English instructions. It runs in three phases (entity discovery → schema context → plan + execute), so you write English and the system handles the rest. Always say which fields you want returned.

## Read

```
Get matter {matter_id} and return all fields
Get matter {matter_id} and return client_id, incident_date, matter_name, plaintiff
Get party {party_id} and return full_name, email, mobile_number, address, date_of_birth
Get all medical providers for matter {matter_id} and return id, party_id, status, reductions
Get all bills for medical provider {medical_provider_id} and return id, total_amount, amount_due, bill_date
Get all injuries for matter {matter_id} and return all fields
Get all insurances / negotiations / notes / tasks for matter {matter_id} and return all fields
Get intake questionnaire {intake_id} and return all fields
```

## Search / find

```
Find party with email: {email} and return id, full_name, record_type
Find party with mobile_number: {phone} and return id, full_name, record_type
Find matter with matter_number: {number} and return id, matter_name, status
Find case type with name containing {name} and return id, name
Find matter stage with name containing Pre-Lit and return id, name
```

## Aggregate / report

Use **aggregate_data** for COUNT / SUM / GROUP BY ("How many open matters?", "Total billed by provider for matter X").

## Matter stage vs sub-stage (important)

A matter has **two** stage fields, and they are different things:

- **`stage_id`** — the broad stage (e.g. "Intake", "Litigation", "Demand"). References `matter_stages` (name column `matter_stage_name`).
- **`sub_stage`** — the finer step within work (e.g. "Demand Writing", "Active Treating", "Demanded - Confirm Coverage"). References `matter_sub_stages` (name column `sub_stage_name`).

Both are UUID foreign keys, but **you can filter by the name directly** — e.g. `sub_stage = "Demand Writing"` or `stage_id = "Demand"`. The server resolves the name to its id for you (exact match first, then a contains match), so you never need to look up the UUID yourself, and a name query won't fail with a UUID error.

**When the user just says "stage"** (e.g. "matters in the demand writing stage", "cases at the demand stage"), it could be the broad stage OR the sub-stage. **Check both**: run the filter on `stage_id` by that name and on `sub_stage` by that name, then combine the results. If you're not sure which stage/sub-stage names exist, query `matter_stages` and `matter_sub_stages` (they're org-scoped) to list the available names first, then filter matters.

Example — "list all matters in the demand writing stage":
1. Query `matters` where `sub_stage = "Demand Writing"` (and also try `stage_id = "Demand Writing"` in case the firm models it as a broad stage).
2. Return the matter name, number, client, and current stage/sub-stage.

## Tips

- Unsure of a column or an allowed value? Call **list_schema** for that entity first — don't guess field names or enum values.
- IDs are UUIDs. When you only have a name/email/phone, search first to resolve the id, then read. (For stage/sub-stage FKs the server auto-resolves names — you can filter by name directly.)
- Prefer returning a focused field list over "all fields" when the user asked something specific.
