# Canopy by Copp Oak — MCP Server

**Canopy lets homeowners book HVAC and plumbing shops directly from AI assistants — live availability, booked into the system the shop already runs.**

This repository is the public home of Canopy's hosted [Model Context Protocol](https://modelcontextprotocol.io) (MCP) server. The server runs hosted — there is no local binary to install or run. Any MCP client can connect to the public endpoint and check bookability, see real availability, and book, reschedule, or cancel appointments with Canopy-connected home-service businesses.

- **Product:** [coppoak.com/canopy](https://coppoak.com/canopy/)
- **Endpoint:** `https://mcp.coppoak.com/mcp/public`
- **Registry name:** `com.coppoak/canopy` (published in the official MCP Registry — see [`server.json`](server.json))
- **Contact:** support@coppoak.com

## Connect

Add Canopy as a remote (streamable HTTP) MCP server. No API key is required for the public endpoint.

Generic MCP client configuration:

```json
{
  "mcpServers": {
    "canopy": {
      "url": "https://mcp.coppoak.com/mcp/public"
    }
  }
}
```

Claude Code:

```bash
claude mcp add --transport http canopy https://mcp.coppoak.com/mcp/public
```

## Tools

The live endpoint currently exposes 10 tools (verified against the endpoint on 2026-11-14):

| Tool | What it does |
|---|---|
| `canopy_check_bookable` | Check which businesses are bookable: given businesses you already found (name plus phone, website, or address), reports which ones can be booked through Canopy and how. |
| `resolve_business` | Look up one business on Canopy: matches a business the user named to its Canopy listing by phone, website, or name. |
| `get_availability` | Get open appointment times for a Canopy business, optionally for one service and one location, between two dates. |
| `get_business_terms` | Get a business's prices and terms: the prices of its bookable services, its fees, and its cancellation terms, exactly as the business has them on file. |
| `request_booking` | Start a booking with a Canopy-connected business. |
| `propose_times` | Propose preferred times to a business that reviews booking requests. |
| `request_callback` | Request a callback from a business. |
| `get_request_status` | Check a booking request — looks up a booking request, and the booking an approved request became. |
| `reschedule_booking` | Ask to move a booking the customer already confirmed to another open slot of the same service and location. |
| `cancel_booking` | Cancel a booking made through Canopy. |

A typical flow: `canopy_check_bookable` or `resolve_business`, then `get_availability` (and `get_business_terms` for prices and cancellation terms), then `request_booking`. Every response states plainly whether anything is booked.

## Research data

The [`data/`](data/) folder carries the aggregate results of Copp Oak's October 2026 audit of how ChatGPT and Gemini handle homeowner requests for local service companies — see [`data/README.md`](data/README.md) for provenance and terms.

## License

MIT — see [`LICENSE`](LICENSE). Copyright © 2026 Copp Oak LLC. (The research dataset in `data/` is published separately under CC BY 4.0; see `data/README.md`.)
