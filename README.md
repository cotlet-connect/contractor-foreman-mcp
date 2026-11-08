# How to connect Contractor Foreman to Claude Code, ChatGPT and any AI agent

**Yes, Claude, ChatGPT and other AI agents can work in your Contractor Foreman account.** Contractor Foreman's published integrations list no official ChatGPT app or Claude connector. Cotlet.dev connects your own signed-in Contractor Foreman account to any AI agent, so you can ask in plain words and the AI builds estimates, updates projects and sends invoices for you.

## What you can ask

> Create an estimate for the Miller kitchen remodel: cabinets $14,500, countertops $6,200 and 80 hours of labor at $65, then send it to Dave Miller.

> Turn the approved Johnson deck estimate into a project and schedule framing to start next Monday.

> Which invoices are more than 30 days past due? List the client, project and amount.

> Invoice the Oak Street bathroom project for the 50% progress payment and email it to Sarah Chen.

> Show me all active projects with budget vs. actual costs and list any that are more than 10% over.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

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

## Questions from Contractor Foreman users

**Does Contractor Foreman have an API?**

Contractor Foreman connects to other apps through integrations such as Zapier. Cotlet.dev works in your own Contractor Foreman account directly, so your AI agent can create estimates, update projects and send invoices without a Contractor Foreman API key.

**How do I connect Contractor Foreman to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Contractor Foreman once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Contractor Foreman API key.

**Is there a way to use Claude with Contractor Foreman?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own Contractor Foreman account.

**Is there a Contractor Foreman MCP server?**

Contractor Foreman's published integrations list no official ChatGPT app or Claude connector. Cotlet.dev is a hosted MCP server that works in Contractor Foreman for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

## Find it on

- [Contractor Foreman on Cotlet.dev](https://cotlet.dev/integrations/contractor-foreman-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=contractor-foreman&utm_content=find)
- [PyPI package contractor-foreman-mcp](https://pypi.org/project/contractor-foreman-mcp/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-contractor-foreman-to-ai-agents-guide)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/contractor-foreman-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=contractor-foreman

Start free: https://cotlet.dev/signup?app=contractor-foreman&utm_source=github&utm_medium=readme&utm_campaign=contractor-foreman
