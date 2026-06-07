---
description: Step-by-step CasePro workflows — client intake, creating a matter, linking parties and injuries, recording providers and bills, and uploading documents. Use when the user wants to set up a new case, add people or injuries to a matter, or perform a multi-step CasePro task.
---

# CasePro Workflows

Use these as playbooks. Resolve ids by searching first (see the query skill), confirm ambiguous choices, and prefer `validate_operation` before large writes.

## New client + matter intake

1. **Find or create the client party** (an `Individual`):
   ```
   Find or create party with first_name: {first}, last_name: {last}, email: {email}, mobile_number: {phone}, record_type: Individual
   ```
2. **Create the intake questionnaire** for the client (matters require one; it is immutable after creation).
3. **Create the matter** — required: `matter_name`, `intake_questionnaire`. Set `client_id` to the client party. `matter_number` auto-generates.
4. Add `plaintiff` / `defendant` party ids as needed.

## Add parties to a matter

- Create/find the party with the right `record_type` (`Individual`, `Business`, `Medical Provider`, `Insurance Company`, `Vendor`). Business/Provider/Insurance types need `party_name` and a contact person; Medical Provider needs `provider_type`.
- Link it to the matter (plaintiff/defendant array, or the relevant relation).

## Record injuries

```
Create injury for matter {matter_id} with {body_part}, {description}, {severity}
Get all injuries for matter {matter_id} and return all fields
```

## Medical providers, bills, reductions

- Create the provider party (`record_type: Medical Provider`, `provider_type`).
- Add bills under the provider (`total_amount`, `amount_due`, `bill_date`); record reductions / `no_reduction_needed` as applicable.

## Documents

- Upload a document to a matter with **upload_document** (from a URL).
- To **find and read** existing documents, use the documents skill (`search_documents` → `get_document`).

## Rules that prevent mistakes

- A matter's `intake_questionnaire` is **immutable** — set it correctly at creation.
- Never invent enum values (`record_type`, statuses). Call `list_schema` when unsure.
- Confirm the target matter/party before writing.
