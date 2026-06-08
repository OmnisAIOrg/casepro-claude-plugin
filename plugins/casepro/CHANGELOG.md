# Changelog — CasePro for Claude

This plugin uses **commit-based versioning**: every push to `main` is the latest version, so installed users receive updates automatically (when marketplace auto-update is on, or via `/plugin marketplace update` + `/plugin update`).

## Latest
- **whoami** — Claude gets the real law-firm name, firm contact details, and the signed-in user (no more guessing the firm name).
- **Document templates** — Claude can find the firm's letterheads and saved letter/demand templates and use them when drafting.
- **Deep matter work** — a dedicated skill for pulling a matter's full picture (providers, bills, insurances, liens, injuries, negotiations, litigation facts, documents) for demand letters and liability narratives, with the correct data model so queries return the right records.
- **Reading documents** — guidance for reading large case files in Claude Cowork (download and read locally; page-range/split for very large files) and in Claude Code (no size limit).
- **Tone** — Claude talks like it's helping an attorney; no technical jargon, and it keeps going if one document can't be opened.
- **Connection & sign-in** — clearer guidance to enable the connector and sign in if the CasePro tools aren't available in a chat.

## Skills
- `casepro-basics` — core concepts and how to drive the tools
- `casepro-query` — looking up and reporting on case data
- `casepro-workflows` — intake, creating matters, linking parties/injuries
- `casepro-matter-deep-dive` — full-matter analysis, demand letters, liability narratives
- `casepro-documents` — finding and reading documents
- `casepro-rules` — guardrails and attorney-appropriate tone
