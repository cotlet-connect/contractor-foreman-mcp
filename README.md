# Contractor Foreman API integration and MCP server: connect Contractor Foreman to ChatGPT, Claude and any AI agent

Contractor Foreman API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in Contractor Foreman account: projects, estimates, change orders, invoices, daily logs, time cards and schedules, on the same screens your login opens. No developer and no Contractor Foreman API key needed. Cotlet is independent and is not affiliated with Contractor Foreman.

**Does Contractor Foreman have an API or an MCP server?** Contractor Foreman has no public API (checked 23 Nov 2026); its API keys only connect Zapier and a website lead form. Its Zapier app has seven triggers and two actions, Create Customer and Create Lead, and Zapier's Contractor Foreman MCP has the same two actions. It has no Make or n8n app. Its QuickBooks Online sync does not carry change orders or daily logs, and time cards reach Gusto or ADP only as a file you download and import. Contractor Foreman has no MCP server and no ChatGPT or Claude app of its own; its AI Estimator works inside the app.

## What contractors ask their AI to do in Contractor Foreman

| Job | Ask your AI | Make it repeat |
|---|---|---|
| QuickBooks check | "Compare this month's Contractor Foreman invoices and bills with QuickBooks and list what did not sync." | Monthly |
| Jobs over budget | "Show which jobs are over budget, by cost code, and what pushed each one over." | Weekly |
| Time cards to payroll | "Total last week's approved time cards per employee and job, ready for Gusto." | Weekly |
| Time card check | "Flag late starts, long lunches and odd hours in this week's GPS time cards." | Weekly |
| Daily logs from site notes | "Write today's daily log for the Maple St job from my notes and photos. Show me before you save." | Daily |
| Estimate from a job description | "Build an estimate for a 12x16 deck from this description, using my cost codes. Show me before you save." | Per bid |
| Change order from a client email | "Draft a change order from this client email about the extra window. Send nothing yet." | When the client asks |
| Leads from Angi or your website | "Add today's Angi leads as leads with phone, address and the work asked for." | Your own endpoint, called from your form tool or Zapier |
| Reports in a spreadsheet | "Put each job's budget, costs and billed amount in my Google Sheet." | Your own endpoint, called from Google Sheets |
| Estimate items from Excel | "Import these estimate items and cost codes from my spreadsheet. Show me before you save." | Once |
| Leaving Contractor Foreman | "Export all projects, estimates, invoices and contacts before I move to another system." | Once |

Your AI asks you before it sends, books or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own Contractor Foreman API, built with Cotlet

Contractor Foreman gives you no public API; its Zapier app only creates customers and leads. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, QuickBooks, Gusto, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `job-costs` | budget, actual cost and variance per cost code for a job |
| `invoices-and-bills` | invoices and bills for a period, with job and amount |
| `time-cards` | approved hours per employee and job for a period |
| `daily-logs` | daily logs of a date across jobs |
| `open-change-orders` | change orders waiting for the client, with amounts |
| `new-lead` | adds a lead with phone, address and the work asked for (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/job-costs \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=contractor-foreman

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to Contractor Foreman once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Contractor Foreman tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"contractor-foreman":{"command":"uvx","args":["contractor-foreman-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t contractor-foreman-mcp .
docker run -i --rm -e COTLET_API_KEY contractor-foreman-mcp
```

Tools: `contractor_foreman_guide`, `contractor_foreman_status`, `contractor_foreman_open`, `contractor_foreman_read`, `contractor_foreman_click`, `contractor_foreman_type`, `contractor_foreman_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install contractor-foreman-mcp        # or run it once: uvx contractor-foreman-mcp connect
contractor-foreman-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 contractor_foreman_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 contractor_foreman_agent.py status    # is Contractor Foreman ready in your Cotlet.dev account
python3 contractor_foreman_agent.py guide     # Contractor Foreman screens and sign-in link
python3 contractor_foreman_agent.py read      # open Contractor Foreman, read the page, close the browser
python3 contractor_foreman_agent.py tools     # list Cotlet.dev tools
python3 contractor_foreman_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Contractor Foreman contractors
**Does Contractor Foreman have an API?**

No public one (checked 23 Nov 2026). Its API keys only connect Zapier and a website lead form, and the Zapier app's actions only create customers and leads. With Cotlet you build your own endpoints in your signed-in Contractor Foreman account.

**Can AI check my Contractor Foreman invoices and bills against QuickBooks?**

Yes. Your AI compares the month's invoices and bills with QuickBooks and lists what did not sync. Change orders and daily logs do not sync on their own, so it lists those too.

**How do I move Contractor Foreman time cards into Gusto or ADP?**

Contractor Foreman hands hours over as a file you download and import. Your AI totals approved hours per employee and job and prepares that file for Gusto or ADP.

**Can ChatGPT or Claude write my Contractor Foreman daily logs?**

Yes. It writes the daily log from your notes and photos for the job you name, and saves it after you check it.

**Can AI show which Contractor Foreman jobs are over budget?**

Yes. It reads budgets and costs per cost code and tells you which jobs are over and what pushed them over.

**Is Zapier's Contractor Foreman MCP enough for ChatGPT or Claude?**

It has two actions, Create Customer and Create Lead. Cotlet works in your whole Contractor Foreman account: estimates, change orders, invoices, daily logs and time cards, after your OK for anything that changes.

**How do I export everything from Contractor Foreman?**

Your AI exports projects, estimates, invoices and contacts from the screens your login opens, so you keep everything when you move.

## Find it on

- [Contractor Foreman MCP server on Cotlet.dev](https://cotlet.dev/integrations/contractor-foreman-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=contractor-foreman&utm_content=find)
- [Contractor Foreman API with Cotlet](https://cotlet.dev/integrations/api/contractor-foreman?utm_source=github&utm_medium=readme&utm_campaign=contractor-foreman&utm_content=find)
- [Does Contractor Foreman have an API or MCP server?](https://cotlet.dev/api-mcp-status/contractor-foreman?utm_source=github&utm_medium=readme&utm_campaign=contractor-foreman&utm_content=find)
- [PyPI package contractor-foreman-mcp](https://pypi.org/project/contractor-foreman-mcp/)

## More construction apps on Cotlet

- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Tradify MCP](https://github.com/cotlet-connect/tradify-mcp)
- [Joist MCP](https://github.com/cotlet-connect/joist-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Buildxact MCP](https://github.com/cotlet-connect/buildxact-mcp)
