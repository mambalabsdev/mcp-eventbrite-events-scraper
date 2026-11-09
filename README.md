# Eventbrite Events Scraper MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-eventbrite-events-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-eventbrite-events-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-eventbrite-events-scraper)](https://github.com/mambalabsdev/mcp-eventbrite-events-scraper/blob/main/LICENSE)

An MCP server for the Mamba Labs Eventbrite Events Scraper actor on Apify. Search Eventbrite events by location and keyword, with organizer and ticket prices.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give a location and keywords, or Eventbrite event URLs. You get one flat row per event with dates, venue, category, organizer, and the ticket price range shown on the event page.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-eventbrite-events-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-eventbrite-events-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_eventbrite_events` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "List tech events in San Francisco next month with their organizers and ticket prices."
- "Which business events are coming up in Austin, TX, and who runs them?"
- "Read these Eventbrite event pages and give me the price range of each."

## Tool

`search_eventbrite_events`. One tool. Example arguments:

```json
{
  "locations": [
    "Austin, TX"
  ],
  "keywords": [
    "startup"
  ],
  "max_events": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `locations` | array | no | Places to search, one per line: "City, Region" such as "Austin, TX" or "London, United Kingdom", an Eventbrite location slug such as ca--san-francisco, or online. A location Eventbrite does not know returns a free row. |
| `keywords` | array | no | Search words, such as tech, startup, or yoga. Each keyword is searched in each location. Leave empty to list all events in a location. |
| `event_urls` | array | no | Eventbrite event URLs or event IDs, on any Eventbrite domain. Each event page is read for its organizer and price. |
| `start_date_from` | string | no | Optional, YYYY-MM-DD. Search results that start earlier are skipped and not charged. |
| `start_date_to` | string | no | Optional, YYYY-MM-DD. Search results that start later are skipped and not charged. |
| `read_details` | boolean | no | true (default) or false. With true, each event page is read for the organizer name and the ticket price range, at the event row with details price. With false, rows come from search pages only, at the lower event row price, with no organizer name and no price. |
| `max_events` | number | no | The most events to return in this run, across every search and URL, from 1 to 2,000. Default 50. Text or a number. |
| `max_pages_per_search` | number | no | Search pages read for each location and keyword pair, from 1 to 50. A page holds about 20 events. Default 5. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `event-detail-row`, one event with its event page read (organizer and price) | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `event-row`, one event from a search page only (read_details false) | $0.0015 | $0.001425 | $0.00135 | $0.001275 |
| `search`, one location and keyword search that Eventbrite answered, with or without events | $0.007 | $0.00665 | $0.0063 | $0.00595 |
| `apify-actor-start`, per run start | $0.026 | $0.026 | $0.026 | $0.026 |

Refused pages, missing events, and refused input are free. A search Eventbrite answers is charged once, with or without events. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/eventbrite-events-scraper](https://apify.com/mambalabs/eventbrite-events-scraper)

## License

MIT. See [LICENSE](LICENSE).
