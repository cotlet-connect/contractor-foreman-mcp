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

## What you can ask your AI to do in Contractor Foreman

These are real jobs Contractor Foreman users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in Contractor Foreman account.

### QuickBooks and accounting

#### How do I connect Contractor Foreman to QuickBooks Online so invoices, bills and payments sync without double entry?

Partly. Contractor Foreman syncs contacts, invoices, payments, bills, estimates and purchase orders with QuickBooks Online on its own, but change orders and daily logs do not sync. With Cotlet, your agent finds what is missing on either side and enters it. Nothing is entered until you say yes.

Ask your agent: "Compare this month's Contractor Foreman invoices, bills and payments with QuickBooks Online and list what is missing on either side. Enter nothing yet."

As an API endpoint: Yes. Save the comparison as an endpoint in Cotlet and call it every Friday from n8n, Make or Zapier to catch gaps early.

#### Does Contractor Foreman work with QuickBooks Desktop, Xero or Sage, and which accounting file should I pick?

Partly. Contractor Foreman's own sync is with QuickBooks Online. With Cotlet, your agent can read your Contractor Foreman invoices and bills and enter them in Xero, Sage or another web accounting app signed in in the same Cloud Browser. QuickBooks Desktop is not a web app. Nothing is entered until you say yes.

Ask your agent: "Read this month's Contractor Foreman invoices and bills and enter each one in Xero with the same number, customer and amount. Show me the list first."

As an API endpoint: Yes. Save the invoice read as an endpoint in Cotlet that returns JSON, and pass it to your accounting app from n8n or Make.

#### Can AI check my Contractor Foreman invoices and bills against QuickBooks and flag what did not sync?

Yes. With Cotlet, your agent compares the month's invoices and bills in Contractor Foreman with QuickBooks, both signed in in your Cloud Browser, and lists what did not sync. Change orders and daily logs never sync, so it lists those too. Nothing changes in either account.

Ask your agent: "Compare this month's Contractor Foreman invoices and bills with QuickBooks and list every one that did not sync, with number, project and amount."

As an API endpoint: Yes. Save the check as an endpoint in Cotlet and call it weekly from n8n, Make or Zapier to get the list of unsynced items by email.

#### How do I stop QuickBooks mapping errors by cleaning up cost codes and products before syncing with Contractor Foreman?

Yes. Contractor Foreman maps cost codes to QuickBooks products and services in its settings. With Cotlet, your agent lists the cost codes with no matching product or service, and the duplicates, so you fix them before the next sync. Nothing changes until you say yes.

Ask your agent: "List my Contractor Foreman cost codes that have no matching QuickBooks product or service, and any duplicates. Change nothing yet."

### Payroll and time cards

#### How do I move approved Contractor Foreman time cards into Gusto or ADP payroll without retyping hours?

Yes. Contractor Foreman is not linked to Gusto or ADP: hours go over as a file from Reports, Time Card Report. With Cotlet, your agent totals approved hours per employee and enters them in Gusto or ADP, signed in in the same Cloud Browser. Nothing is submitted until you say yes.

Ask your agent: "Total last week's approved Contractor Foreman time cards per employee and fill the hours into Gusto. Stop before submitting."

As an API endpoint: Yes. Save the hours total as an endpoint in Cotlet that returns JSON per employee, and call it every Monday from n8n or Make.

#### Can AI flag late starts, long lunches and odd hours from Contractor Foreman GPS time cards?

Yes. Contractor Foreman time cards can carry a GPS location from the mobile app. With Cotlet, your agent reads the week's time cards in your signed-in account and lists late starts, long breaks and shifts that look wrong, so you decide what to approve. Nothing changes in Contractor Foreman.

Ask your agent: "Read last week's Contractor Foreman time cards and list late starts, lunches over 45 minutes and shifts over 10 hours, with the GPS location."

As an API endpoint: Yes. Save the check as an endpoint in Cotlet and call it every Friday from n8n, Make or Zapier before you approve hours.

#### How do I keep time cards and daily logs working on a Contractor Foreman job site with no cell signal?

Not on its own. Contractor Foreman's help centre has no page on offline use of the mobile app, and Cotlet works online in a Cloud Browser. Once the crew has signal again, your agent can check that the time cards and daily logs arrived and list any that are missing.

Ask your agent: "List the Contractor Foreman time cards and daily logs from yesterday that are missing for the crews scheduled to work."

### Leads and calendar

#### How do I send Google Form or website leads into Contractor Foreman automatically?

Partly. Contractor Foreman has a Zapier app with a Create Lead action and a lead form you can add to your website. With Cotlet, your agent reads a form response or lead email and adds it as a lead in Contractor Foreman. Nothing is created until you say yes.

Ask your agent: "Read today's new lead emails and add each person to Contractor Foreman as a lead with name, phone, address and the work they want."

As an API endpoint: Yes. Save the lead entry as an endpoint in Cotlet and call it from n8n, Make or Zapier whenever a form is submitted.

#### How do I get Angi or HomeAdvisor leads into Contractor Foreman without typing them in?

Partly. Contractor Foreman lists Angi Leads as a partner and has a help article on syncing leads from an Angi or HomeAdvisor account. With Cotlet, your agent reads the new leads in your signed-in Angi account and adds the ones missing in Contractor Foreman. Nothing is created until you say yes.

Ask your agent: "Read my new Angi leads from the last 7 days and add each one missing in Contractor Foreman as a lead with phone, address and the job."

As an API endpoint: Yes. Save the lead read as an endpoint in Cotlet and call it hourly from n8n or Make to push new Angi leads into Contractor Foreman.

#### How do I get my Contractor Foreman schedule onto Google or Outlook calendar and send invites to customers?

Partly. Contractor Foreman syncs its calendar with Google or Outlook on its own. With Cotlet, your agent reads the schedule for a job and drafts an invite for each customer with the date, time and address. Nothing is sent until you say yes.

Ask your agent: "Read next week's Contractor Foreman schedule for the Oak St job and draft a calendar invite for the customer for each visit."

As an API endpoint: Yes. Save the schedule read as an endpoint in Cotlet and call it every evening from n8n, Make or Zapier to update your calendar.

### Estimates and bids

#### Can AI build a Contractor Foreman estimate from a job description or plans, or is the built-in AI Estimator the only way?

Partly. Contractor Foreman has its own AI Estimator that drafts an estimate from project details. With Cotlet, ChatGPT or Claude can also build the estimate from your job description and measurements, using the items in your Cost Items Database. Nothing is saved until you say yes.

Ask your agent: "Build a draft Contractor Foreman estimate for a 12 by 16 deck from these measurements, using items from my cost database. Do not send it."

As an API endpoint: Yes. Save the estimate draft as an endpoint in Cotlet that takes the description as input, and call it from n8n or Make.

#### Can AI draft a change order or schedule of values invoice in Contractor Foreman from a client email?

Yes. With Cotlet, your agent reads the client's email, drafts the change order in the Change Orders module and builds the invoice from the Schedule of Values. Nothing is saved or sent until you say yes. Change orders do not sync to QuickBooks on their own.

Ask your agent: "Read this client email, draft a change order for the Oak St job and prepare the next Schedule of Values invoice. Do not send either."

#### Can AI compare three subcontractor quotes and set up the bid package in Contractor Foreman?

Partly. Contractor Foreman's Bid Management builds a bid package from an estimate and shows bids side by side. With Cotlet, your agent reads the three quotes you give it, compares them line by line and sets up the bid package and request to bid. Nothing is sent until you say yes.

Ask your agent: "Compare these three subcontractor quotes line by line and set up a bid package for the Oak St job in Contractor Foreman. Send nothing."

### Jobs and daily logs

#### Can ChatGPT or Claude write my daily logs in Contractor Foreman from my site notes and photos?

Yes. Contractor Foreman's Daily Logs module holds photos inside each log. With Cotlet, ChatGPT or Claude writes the log from your site notes and photos for the job you name and fills it in. Nothing is saved until you say yes.

Ask your agent: "Write today's Contractor Foreman daily log for Oak St from these site notes and photos. Show it to me before saving."

As an API endpoint: Yes. Save the log entry as an endpoint in Cotlet and call it from n8n or Make when a voice note arrives.

#### Can AI show me which Contractor Foreman jobs are over budget and why?

Yes. With Cotlet, your agent reads the job costing and work in progress reports in your signed-in Contractor Foreman account, lists the jobs over budget and shows the cost codes that pushed each one over. Nothing changes in Contractor Foreman.

Ask your agent: "List my Contractor Foreman jobs that are over budget and show the cost codes that caused it for each."

As an API endpoint: Yes. Save the budget check as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier to email you the jobs over budget.

### Imports, exports and refunds

#### How do I export all my data (projects, estimates, invoices, contacts) from Contractor Foreman to move to another system?

Yes. Contractor Foreman's help centre shows reports to Excel or PDF per module and a Download Zip for files, but no one-click full export. With Cotlet, your agent runs each report in your signed-in account and saves every list as a file you keep.

Ask your agent: "Save my Contractor Foreman projects, estimates, invoices and contacts as separate Excel or CSV files, one per list."

#### How do I export a Contractor Foreman report to Excel or CSV on a schedule?

Yes. Contractor Foreman's custom reports download as PDF or Excel by hand, and its help centre describes no scheduled delivery. With Cotlet, your agent runs your saved report and gives you the file or table, and you can run it on a schedule as an automation.

Ask your agent: "Run my saved Contractor Foreman report for time cards this week and give me the rows as a CSV."

As an API endpoint: Yes. Save the report run as an endpoint in Cotlet and call it on a schedule from n8n, Make or Zapier to send the file to your inbox or a sheet.

#### How do I bulk import estimate items or cost codes from Excel into Contractor Foreman?

Partly. Contractor Foreman can import a CSV of items into an estimate and cost codes from Excel on its own. With Cotlet, your agent cleans your sheet first, runs the import in the right screen and checks the result. Nothing is imported until you say yes.

Ask your agent: "Check this spreadsheet of cost codes for duplicates and blanks, then import it into Contractor Foreman. Show me what will be added first."

#### How do I import my customer and contact list into Contractor Foreman?

Partly. Contractor Foreman has an Importing Contacts feature on its own. With Cotlet, your agent tidies your list, skips people already in Contractor Foreman and adds the rest. Nothing is created until you say yes to the first batch.

Ask your agent: "Take the customers in this spreadsheet and add each one to Contractor Foreman as a contact. Show me the first five first."

#### How do I cancel Contractor Foreman and get the 100-day money-back refund?

Not on its own. The 100-day guarantee applies only to annual Plus, Pro and Unlimited plans and needs a completed demo or kickoff call, per the refund policy. With Cotlet, your agent can check your plan against the policy and draft the request. Cancelling stays your own step.

Ask your agent: "Check my Contractor Foreman plan against the refund policy and draft a cancellation and refund request. Do not cancel anything."

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
