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

`get_document` returns a **secure download link**. To read the document's content, you need to download it and open it — which works best in **Claude Cowork** (or any environment where you can save and open files):

1. Call `get_document` (or `discuss_document`) to get the document's download link.
2. **Download the file to your workspace** (e.g. fetch the link and save it locally), then open and read it — including large PDFs like exhibit packets and medical records.
3. Read the content and answer; for a demand or narrative, pull the specific facts you need (the officer's narrative and citation from a crash report, dates and amounts from bills, the liability section from an existing demand).

If you can't open a document in the current environment (for example, plain chat without file access), say so plainly and suggest the user open it in **Claude Cowork**, where you can download and read it. Then continue with whatever you *can* do from the structured matter data.

## Notes

- Download links are time-limited — if one stops working, just call `get_document` again to get a fresh one.
- Always tell the user which document you read, by its name.
- If the user doesn't have access to a document, say so simply and move on — don't keep retrying.
