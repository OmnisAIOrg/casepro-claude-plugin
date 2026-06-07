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

## Tips

- Unsure of a column or an allowed value? Call **list_schema** for that entity first — don't guess field names or enum values.
- IDs are UUIDs. When you only have a name/email/phone, search first to resolve the id, then read.
- Prefer returning a focused field list over "all fields" when the user asked something specific.
