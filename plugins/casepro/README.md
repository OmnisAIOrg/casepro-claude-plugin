# CasePro for Claude

Bring your CasePro legal-CRM into Claude. Once installed, Claude can:

- **Search and read your matter documents** — find files (including text *inside* PDFs) and read them directly to answer your questions.
- **Run case workflows** — client intake, create matters, add parties and injuries, record providers and bills.
- **Query your CRM in plain English** — "show me the open matters for Jane Doe", "what's the total billed by Dr. Smith on matter CP-2026-014?".

Your permissions are exactly your CasePro permissions — Claude only sees what you can see. The connection uses secure sign-in (OAuth); it stays active while you use Claude and you can reconnect any time.

## Install (Claude Code)

```
/plugin marketplace add OmnisAIOrg/casepro-claude-plugin
/plugin install casepro@casepro-tools
```

Then Claude will prompt you to sign in to CasePro in your browser. Approve it, and you're done. (You need access to the OmnisAIOrg GitHub — the same access you use for the rest of CasePro's tooling.)

## Updates

This plugin auto-updates: every new release is published to `main`, and Claude picks it up when it refreshes the marketplace (turn on **auto-update** for the marketplace in the Plugins screen so it happens automatically).

If you're stuck on an old version and the **Update** button is greyed out, your app cached the old marketplace. To force a clean refresh:

- **Claude Code:** `/plugin marketplace update casepro-tools` then `/plugin update casepro@casepro-tools`. If still stuck: `/plugin marketplace remove casepro-tools` then re-add and reinstall.
- **Claude desktop / web:** in **Customize → Plugins → Marketplaces**, **remove** the casepro marketplace entirely, then re-add it (`OmnisAIOrg/casepro-claude-plugin`) and install again. (Removing the *marketplace* clears the cache — uninstalling just the plugin does not.)

## Use it

Just ask. For example:

- "Find the medical records for matter CP-2026-014 and summarize the diagnosis."
- "Create a new matter for client Jane Doe."
- "Which documents mention a herniated disc?" (searches inside your PDFs)
- "Read the Legal Services Agreement for this client and tell me the fee percentage."

## What's included

Five skills give Claude the CasePro know-how it needs — terminology, how to query, common workflows, how to search/read documents, and the data-safety rules — plus the secure CasePro connector. Nothing to configure.
