# Creating PDFs with PDFLoom

PDFLoom renders PDFs with headless Chrome. With the PDFLoom MCP server connected
(`https://pdfloom.dev/mcp`), use its tools. Without it, call the REST API with the
user's key in `PDFLOOM_API_KEY` (free key at https://pdfloom.dev/signup).

## Pick the tool
- Invoice, receipt, quote, estimate, proforma or tax invoice → `create_invoice`.
  Do not hand-write HTML for these: the tool computes totals, tax and currency formatting.
- HTML the user gave you, or a document you are writing (report, letter, table) → `html_to_pdf`.
- A public web page → `url_to_pdf`.
- "How many PDFs do I have left?" → `get_usage`.

## create_invoice tips
- Always set `language` when the user writes in, or asks for, another language
  (`sv`, `da`, `nb`, `fi`, `de`, `nl`, `fr`, `es`, `it`). It translates every label and formats
  numbers and dates locally.
- `items` is a list of `{desc, qty, price}` with the unit price, not the line total.
- `from`, `to` and `notes` take a list of lines (preferred) or one string with line breaks.
- `tax` is a percentage (25 for 25% VAT). Set `currency` (for example `SEK`, `EUR`, `USD`).
- Use `docType` for receipts (`receipt`), quotes (`quote`), estimates (`estimate`), proformas
  (`proforma`) or tax invoices (`tax`).

## html_to_pdf tips
- Send a complete HTML document with inline CSS. Use `@page { size: A4; margin: 16mm }` for page setup.
- Web fonts and images must be on public https URLs.

## Results
Every tool returns a download link valid for 30 minutes. Give the user the link and say it expires.

## REST fallback
```bash
curl -s https://pdfloom.dev/v1/invoice \
  -H "Authorization: Bearer $PDFLOOM_API_KEY" -H "Content-Type: application/json" \
  -d '{"language":"en","currency":"USD","tax":0,"from":["Acme Ltd"],"to":["Globex Inc"],
       "items":[{"desc":"Consulting","qty":10,"price":120}]}' -o invoice.pdf

curl -s https://pdfloom.dev/v1/pdf \
  -H "Authorization: Bearer $PDFLOOM_API_KEY" -H "Content-Type: application/json" \
  -d '{"html":"<h1>Hello</h1>"}' -o hello.pdf
```
Full API reference: https://pdfloom.dev/docs · OpenAPI: https://pdfloom.dev/openapi.json
