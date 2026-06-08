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
- **get_document** — fetch a document by `file_id`. Returns the document and/or a secure, time-limited **download link**.
- **discuss_document** — give it a `file_id` OR a `query` (picks the best match) and it returns the document to discuss.

**Always get the `file_id` from `search_documents` (or `discuss_document`).** Do NOT pass a document id you got from somewhere else (a matter's document records, a CRM query, a URL the user pasted) — those are different ids and `get_document` will reject them. If you only have a file's name, `search_documents` for it first and use the `file_id` it returns.

## How to actually read a document

`get_document` returns the document in the form that fits its type:

- **Images** (PNG/JPG/…) — returned inline so you can view them directly, **plus** a download link.
- **PDFs** — returned as the document's extracted/OCR **text** so you can read it (scales to long medical-record packets), **plus** a download link to the original.
- **Everything else** (DOCX, XLSX, …) — returned as a **download link** (a direct, time-limited URL to the original file). Open the link to read or save it.

So for anything that isn't an image, you'll get a **download link**. To read or save it:

1. Call `get_document` (or `discuss_document`).
2. If it returns content directly (image, or PDF text), just read it.
3. If you got a **download link**, **download the file and open it** — in **Claude Code** open it directly (any size); in **Claude Cowork** use the code tool to fetch the link to a local file (`curl`/`requests`) and then read it (e.g. a DOCX/PDF library) — or save it to a folder. The link is a real download URL for the original, so you can write the file to disk.

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
