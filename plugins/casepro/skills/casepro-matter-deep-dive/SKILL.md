---
description: How to find a matter and pull its COMPLETE picture for deep work — analyzing a case, building a demand letter, a liability narrative, a settlement summary, or any task that needs the full matter. Use whenever the user names a matter (by name, number, or id) and asks you to analyze it, summarize it, or draft anything from it.
---

# CasePro Matter Deep-Dive

When the user asks about a specific matter — "analyze this case", "build a demand letter", "write the liability narrative", "summarize where this matter stands" — do NOT work from the matter record alone. Pull the full picture first.

## 1. Find the matter

Matters can be referenced by **name**, **matter number** (e.g. `MAT-20251118_20` or `CP-2026-014`), or **id**. Resolve it first:

```
Find matter with matter_number: {number} and return id, matter_name, status, client_id, plaintiff, defendant, incident_date, incident_time
Find matter with matter_name containing {name} and return id, matter_name, matter_number, status, client_id, plaintiff, defendant, incident_date
Get matter {matter_id} and return all fields
```

Keep the matter's **id** — almost everything else is fetched by `matter_id`.

## 2. The data model (so your filters actually work)

- Most case entities carry a **`matter_id`** foreign key, so you filter them directly by the matter: **medical_providers, insurances, injuries, litigations, depositions, mediations, witnesses, negotiations, resolutions, liens, expenses, damages, tasks, notes**.
- **parties do NOT have a matter_id.** A matter's people are referenced by the matter's **`plaintiff`** and **`defendant`** arrays (lists of party ids) plus the `party_id` / `adjuster_id` / `defendant_id` foreign keys on related records (medical_providers.party_id, insurances.party_id / bi_adjuster_id / pd_adjuster_id, etc.). **Never filter the parties table by matter_id — it has no such column, and you'll get the whole org back.** Instead, collect the party ids from the matter arrays and the related records, then fetch those parties by id.
- **bills** are children of **medical_providers** (filter bills by `medical_provider_id`, not by matter).
- Contact details (name, phone, email, address) for a provider/insurer/adjuster live on the linked **party**, not on the provider/insurance record.

## 3. Pull everything for the matter

For a deep task, gather the relevant set (skip what's irrelevant to the request):

```
Get all injuries for matter {matter_id} and return all fields
Get all medical_providers for matter {matter_id} and return id, party_id, status, reductions, no_reduction_needed
  → then for each provider: Get all bills for medical_provider {provider_id} and return total_amount, amount_due, adjusted_amount, bill_date, icd_codes, cpt_codes
Get all insurances for matter {matter_id} and return insurance_name, insurance_type, claim_number, policy_limit, liability_percentage, party_id, bi_adjuster_id, defendant_id
Get all liens for matter {matter_id} and return all fields
Get all negotiations for matter {matter_id} and return all fields
Get all litigations for matter {matter_id} and return cause_number, venue, judge, trial_date, positive_facts, negative_facts, trial_story, settlement_story
Get all expenses for matter {matter_id} and return all fields
Get all notes for matter {matter_id} and return all fields
Get all tasks for matter {matter_id} and return all fields
```

Then resolve the people: take the `plaintiff`/`defendant` party ids from the matter and the `party_id`/adjuster ids from providers and insurances, and `Get party {id} and return full_name, party_name, record_type, email, mobile_number, address` for each.

## 4. Task playbooks

- **Demand letter:** matter facts + injuries + every provider with itemized bills (totals, reductions) + insurances (policy limits, claim numbers, adjusters) + liens + the relevant documents (crash report, medical records, existing demand draft — see the documents skill). Build the medical-specials total from the bills.
- **Liability narrative:** matter incident facts (description, geometry, date/time/location) + the crash/police report and any liability evidence from documents + litigation `positive_facts`/`negative_facts`/`trial_story` if present + witness records + the at-fault insurance's `liability_percentage`. Read the actual crash report (documents skill) — don't infer the officer's findings.
- **Settlement / case status:** injuries + bills total + liens + negotiations history + insurance limits + litigation deadlines.

## Be resilient — don't give up on the whole task

If one piece fails — a document won't open, a query errors, a file is too large — **do not abandon the task.** Keep going: pull the other entities, read the other documents, and build the best answer you can from everything that DID come back. A demand letter or liability narrative can be largely built from the structured matter data even if one exhibit won't open. At the end, briefly note what you couldn't include (e.g. "I couldn't open the crash report exhibit — open it in Claude Cowork and I'll fold in the officer's findings") so the user can fill the gap. Never refuse the whole task because one file or one query failed.

Always tell the user which matter you pulled and call out anything missing (in plain terms, no technical reasons) so they can fill the gap.
