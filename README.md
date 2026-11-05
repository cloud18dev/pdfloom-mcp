# PDFLoom MCP server

<img src="assets/logo.png" width="64" alt="PDFLoom logo" align="right">

Create PDFs from your AI assistant: invoices, receipts and quotes from plain details (in 10 languages),
HTML documents, or any public web page. Rendered with headless Chrome by [PDFLoom](https://pdfloom.dev).

- **Server URL:** `https://pdfloom.dev/mcp` (Streamable HTTP, remote, nothing to install)
- **Sign-in:** OAuth. Your client opens a PDFLoom login and asks you to approve access. API keys also work.
- **Free plan:** 100 PDFs per month, no card. Paid plans from $9/month.

## Tools

| Tool | What it does |
|---|---|
| `create_invoice` | Invoice, receipt, quote, estimate, proforma or tax invoice from structured fields. Totals, tax and currency formatting are computed. Labels and number/date formats in en, sv, da, nb, fi, de, nl, fr, es, it. |
| `html_to_pdf` | Renders an HTML document (modern CSS, web fonts, `@page`, headers/footers with page numbers) to PDF. |
| `url_to_pdf` | Loads a public web page in headless Chrome and saves it as PDF. |
| `get_usage` | PDFs used this month and the plan limit. |

Every tool returns a download link that expires after 30 minutes.

## Connect

**Claude Code**
```bash
claude mcp add --transport http pdfloom https://pdfloom.dev/mcp
```
Or install the plugin (MCP server + skill): `/plugin marketplace add cloud18dev/pdfloom-mcp`, then `/plugin install pdfloom@pdfloom`.

**Claude (web/desktop):** Settings → Connectors → Add custom connector → `https://pdfloom.dev/mcp`.

**Cursor:** add to `~/.cursor/mcp.json`:
```json
{ "mcpServers": { "pdfloom": { "url": "https://pdfloom.dev/mcp" } } }
```

**VS Code (Copilot):** `code --add-mcp '{"name":"pdfloom","type":"http","url":"https://pdfloom.dev/mcp"}'`

**Gemini CLI**
```bash
gemini extensions install https://github.com/cloud18dev/pdfloom-mcp
```

**Other clients:** use `https://pdfloom.dev/mcp` with Streamable HTTP. If your client can't do OAuth, create an API key
at https://pdfloom.dev/dashboard and send it as `Authorization: Bearer plk_...`.

## Example prompts
- "Create an invoice from Acme AB to Globex Ltd for 10 hours of consulting at 950 SEK per hour, with 25% VAT, in Swedish."
- "Make a receipt for 2 coffee bags at €14 each, paid today."
- "Turn https://example.com into a PDF."
- "Write a one-page project summary and give it to me as a PDF."

## What's in this repo
Manifests that let MCP and plugin directories find the hosted server: `server.json` (Official MCP Registry),
`mcp.json` / `.mcp.json`, `.cursor-plugin/`, `.claude-plugin/`, `gemini-extension.json` + `GEMINI.md`, and an agent
skill in `skills/pdfloom/SKILL.md`. The server itself is hosted at pdfloom.dev; there is no code to run.

## Privacy and support
Input is used only to render the file; generated PDFs are deleted after 30 minutes. You can disconnect assistants
from your dashboard at any time. [Privacy policy](https://pdfloom.dev/privacy) · [Terms](https://pdfloom.dev/terms) ·
[Docs](https://pdfloom.dev/docs#mcp) · [Contact](https://pdfloom.dev/contact)
