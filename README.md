# Plancraft MCP server

Connect Plancraft to ChatGPT, Claude and any AI agent through Cotlet.dev. Work with contacts, projects, offers, invoices and working hours in your own signed-in account. Sending documents, recording payments and booking hours ask for approval by default.

## What you can ask

> List overdue invoices with the customer and open amount.

> Draft an offer from my site notes in the selected project.

> Prepare payment reminders and show me the list before sending.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Plancraft tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 plancraft_agent.py serve
```

Or with Docker:

```bash
docker build -t plancraft-mcp .
docker run -i --rm -e COTLET_API_KEY plancraft-mcp
```

Tools: `plancraft_guide`, `plancraft_status`, `plancraft_open`, `plancraft_read`, `plancraft_click`, `plancraft_type`, `plancraft_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 plancraft_agent.py connect   # ready config for ChatGPT, Claude, Claude Code and Cursor
python3 plancraft_agent.py status    # is Plancraft ready in your Cotlet.dev account
python3 plancraft_agent.py guide     # Plancraft screens and sign-in link
python3 plancraft_agent.py read      # open Plancraft, read the page, close the browser
python3 plancraft_agent.py tools     # list Cotlet.dev tools
python3 plancraft_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Plancraft users

**Which account should I connect?**

Open plancraft.com/de-de/login in your Cotlet.dev Cloud Browser and sign in with your own office user.

**Which actions ask first?**

Sending documents, recording payments and booking working hours ask for approval by default. You can change this per tool.

**Is Cotlet affiliated with Plancraft?**

No. Cotlet.dev is independent and not affiliated with Plancraft.

## Learn more

- [Plancraft integration and hosted tools](https://cotlet.dev/integrations/plancraft-api-mcp-server)

---

More: https://cotlet.dev/integrations/plancraft-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=plancraft

Request access: https://cotlet.dev/signup?app=plancraft&utm_source=github&utm_medium=readme&utm_campaign=plancraft

## Workflows on the hosted integration

The live integration page documents these office operations, selected from the live page:

- `list_contacts`
- `create_contact`
- `list_projects`
- `get_project`
- `create_project`
- `list_documents`
- `create_offer`
- `create_invoice_from_document`
- `send_document`
- `list_invoices`
- `record_payment`
- `create_payment_reminder`
- `list_time_entries`
- `create_time_entry`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
