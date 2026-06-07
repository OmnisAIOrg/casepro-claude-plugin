# CasePro for Claude

Bring your CasePro legal-CRM into Claude. Once installed, Claude can:

- **Search and read your matter documents** — find files (including text *inside* PDFs) and read them directly to answer your questions.
- **Run case workflows** — client intake, create matters, add parties and injuries, record providers and bills.
- **Query your CRM in plain English** — "show me the open matters for Jane Doe", "what's the total billed by Dr. Smith on matter CP-2026-014?".

Your permissions are exactly your CasePro permissions — Claude only sees what you can see. The connection uses secure sign-in (OAuth); it stays active while you use Claude and you can reconnect any time.

## Install (Claude Code)

```
/plugin marketplace add https://casepro-app.omnisai.io/claude/marketplace.json
/plugin install casepro@casepro-tools
```

Then Claude will prompt you to sign in to CasePro in your browser. Approve it, and you're done.

## Use it

Just ask. For example:

- "Find the medical records for matter CP-2026-014 and summarize the diagnosis."
- "Create a new matter for client Jane Doe."
- "Which documents mention a herniated disc?" (searches inside your PDFs)
- "Read the Legal Services Agreement for this client and tell me the fee percentage."

## What's included

Five skills give Claude the CasePro know-how it needs — terminology, how to query, common workflows, how to search/read documents, and the data-safety rules — plus the secure CasePro connector. Nothing to configure.
