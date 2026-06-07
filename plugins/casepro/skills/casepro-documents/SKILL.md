---
description: Search and read CasePro / LitBox documents. Use whenever the user wants to find a document, read or summarize a PDF, pull a specific record (medical records, agreements, demands, affidavits), or ask questions about the contents of their case files.
---

# CasePro Documents

The connector can search the user's document store and return documents for you to read **natively** — you read the PDF/image directly (no copy-paste, no OCR step needed). Access respects the user's own document permissions.

**First, make sure the connector is available.** If the `search_documents` / `get_document` tools aren't present in this chat, the CasePro connector isn't enabled here — ask the user to enable the CasePro connector for this conversation and sign in. **Do NOT download or open documents through a web browser**; only use the connector tools below.

## Tools

- **search_documents** — deep search across the user's documents.
  - `query`: keywords.
  - `filter`: `all` (default), `files`, `folders`, `content`, `tags`. **Use `content` to search the OCR'd text *inside* PDFs** (e.g. find every document that mentions a diagnosis or a party name).
  - Optional: `folderId`, `mimeTypes` (e.g. `application/pdf`), `page`, `pageSize`.
  - Returns matches with a **`file_id`** for each hit — pass that to `get_document`.
- **get_document** — fetch a document by `file_id`. It returns the document itself; **read it** to answer the user's question.
- **discuss_document** — convenience: give it a `file_id` OR a `query` (it picks the most relevant match), and it returns the document ready for Q&A.

## How to use it

1. To answer a question about a document the user names: call **discuss_document** with a `query`, then read and answer.
2. To find something across many files: **search_documents** with `filter: content`, review the snippets, then **get_document** on the right `file_id`.
3. To read a known document: **get_document** with its `file_id`.

## Notes

- Always cite the document name you read.
- If the user lacks access, you'll get a clear "account not linked / no access" message — relay it plainly; don't retry blindly.
- Large files may come back as a link to open rather than inline — tell the user to open it if so.
