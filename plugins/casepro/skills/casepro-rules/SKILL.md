---
description: Guardrails and data-safety rules for working in CasePro. Use whenever you create, update, or delete CasePro data, or when a write fails or behaves unexpectedly.
---

# CasePro Rules & Guardrails

Follow these to avoid data mistakes. They reflect how the CasePro backend actually behaves.

## Always

- **Discover before you guess.** If you're unsure of a table name, a column, or an allowed value (enum / status / `record_type`), call **list_schema** for that entity first. Never invent values.
- **Dry-run risky writes.** Use **validate_operation** before a large or destructive change to confirm the plan.
- **Confirm the target.** Before a write, be sure which matter/party you're acting on. Search to resolve ids from names/emails/phones.
- **State what you did.** After a write, report the entity and id you changed.

## Never

- **Don't add organization filters.** Org-scoping is automatic from the signed-in user's connection. Adding your own org id can cause wrong or empty results.
- **Don't treat soft-deleted rows as gone.** "Deleted" records are hidden from reads, not erased. A "missing" record may be soft-deleted, not absent — say so rather than recreating it.
- **Don't change immutable fields.** A matter's `intake_questionnaire` is fixed after creation.
- **Don't fabricate enum values or statuses.** Use only values returned by `list_schema`. Wrong status strings can mis-route a record.

## When something fails

- A permission error on documents means the user lacks access to that file — relay it; don't loop.
- A schema/validation error usually means a wrong field or enum — re-check with `list_schema` and retry with corrected values.
- If a record "already exists" unexpectedly, search for it (it may be soft-deleted or already linked) before creating a duplicate.
