---
description: Find and read CasePro documents — crash/police reports, medical records, demand letters, agreements, affidavits, exhibits. Use whenever the user wants to find a document, read or summarize a PDF, pull a specific record, or ask about the contents of their case files.
---

# CasePro Documents

The connector finds the user's documents and gives you a link to read each one. Reading respects the user's own document permissions.

**First, make sure the connector is available.** If the `search_documents` / `get_document` tools aren't present, the CasePro connector isn't enabled for this chat — ask the user to enable the CasePro connector and sign in. Use only these tools to reach documents; do not try to open the CasePro website in a browser.

## Tools

- **search_documents** — search the user's documents.
  - `query`: keywords. `filter`: `all` (default), `files`, `folders`, `content`, `tags`. Use `content` to search text *inside* documents.
  - Optional: `folderId`, `mimeTypes` (e.g. `application/pdf`), `page`, `pageSize`.
  - Returns each match with a **`file_id`**.
- **get_document** — fetch a document by `file_id`. Returns the document's metadata and a secure, time-limited download link.
- **discuss_document** — give it a `file_id` OR a `query` (picks the best match) and it returns the document to discuss.

## How to actually read a document

`get_document` returns the document. Small documents come back ready to read. Larger ones come with a **secure download link** — to read those you download the file and open it, which works best in **Claude Cowork** or **Claude Code** (anywhere you can run code and save files).

1. Call `get_document` (or `discuss_document`) to get the document (or its download link).
2. If the document is returned directly, just read it.
3. If you got a download link, **download the file into your workspace and open it** — for example, in Cowork or Claude Code, use the code tool to fetch the link to a local file (`curl`/`requests`) and then read it with a PDF/text library.

### Reading large documents (bigger than ~30 MB)

In **Claude Cowork** a single file can't be loaded straight into the conversation if it's over ~30 MB, but the code environment can still handle it — **download it to a local file and process it there, without pulling the whole thing into the conversation:**

- Download the file to disk first (don't try to read 50 MB into context at once).
- Then read it with code: open the PDF and pull **page ranges or specific sections** (e.g. the police-report pages, the billing summary), or **split it into smaller parts** and read the part you need. This keeps each step small while still letting you read the whole document across steps.
- Search within the downloaded file for the keywords that matter (party names, "citation", "diagnosis", dollar amounts) and read around the hits.

In **Claude Code** there's no size limit — download and read the whole file directly.

If you genuinely can't open a document in the current environment (for example, a plain chat without file access), say so simply and suggest the user open it in **Claude Cowork**, where you can download and read it — then keep going with whatever you can do from the structured case data.

## Notes

- Download links are time-limited — if one stops working, just call `get_document` again to get a fresh one.
- Always tell the user which document you read, by its name.
- If the user doesn't have access to a document, say so simply and move on — don't keep retrying.
