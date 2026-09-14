# Holiday Atlas

Public documentation for [Holiday Atlas](https://holiday-atlas.rafikadiche.com), a free public API and MCP server for public-holiday data.

Holiday Atlas covers 210 countries, curated from a hand-maintained dataset rather than a third-party feed. Every holiday is stored as a recurring rule (a fixed calendar date, an Easter-relative rule, an nth-weekday-of-month rule, or a Hijri/Hebrew/Chinese calendar date) and resolved fresh for whichever year is requested, so the data never goes stale year to year.

This repository has no code. It exists to give visitors and integrators one place to read about the API, the MCP server, and what the web app itself does.

## Public API

Base URL: `https://p4c6e4mu4k5sg4fwnertd2zgwi0hordn.lambda-url.eu-north-1.on.aws`

Free, unauthenticated, read-only, rate-limited to 20 requests per minute per IP.

| Endpoint | Description |
|---|---|
| `GET /api/countries` | Every country with data, plus each country's holiday count for the current year. |
| `GET /api/holidays?countryCode={ISO}&year={year}` | That country's public holidays for the given year (defaults to the current year). |

Example:

```bash
curl "https://p4c6e4mu4k5sg4fwnertd2zgwi0hordn.lambda-url.eu-north-1.on.aws/api/holidays?countryCode=FR&year=2026"
```

Each holiday includes its date, its name in English and in the country's own language, a short description in English and French, whether it's observed nationwide or only in specific regions (with those regions listed when it's regional), and whether the date is `estimated` (Hijri/Islamic-calendar dates depend on physical moon sighting and can shift by a day from what's officially announced).

Full interactive reference, with a "try it out" console: **[holiday-atlas.rafikadiche.com/docs.html](https://holiday-atlas.rafikadiche.com/docs.html)**
Raw OpenAPI spec: **[holiday-atlas.rafikadiche.com/openapi.json](https://holiday-atlas.rafikadiche.com/openapi.json)**

## MCP server

The same data is exposed as a Model Context Protocol server, so any MCP-compatible AI assistant can query it directly instead of going through the REST API.

Connect it:

```bash
claude mcp add --transport http holiday-atlas https://p4c6e4mu4k5sg4fwnertd2zgwi0hordn.lambda-url.eu-north-1.on.aws/mcp
```

Tools it exposes:

| Tool | What it does |
|---|---|
| `list_countries` | Lists every country this server has public-holiday data for. |
| `get_public_holidays` | Gets public holidays for a country (ISO 3166-1 alpha-2 code) in a given year, defaulting to the current year. |
| `get_next_public_holidays` | Gets the next upcoming public holidays for a country. |

## Frontend

The web app at [holiday-atlas.rafikadiche.com](https://holiday-atlas.rafikadiche.com) is a Next.js static site, in English and French, with light and dark themes.

- **World map** - click any of the 210 covered countries to load its holidays. Unsupported countries are greyed out and excluded from the search selector.
- **Country selector** - search by name; each result shows its flag and how many public holidays it has this year.
- **Calendar** - one row per month. Each month's holidays sit in a horizontally-scrolling strip of cards (name, formatted date, description, and region if the holiday is regional rather than nationwide) next to a small calendar for that month with the holiday days highlighted.
- **API and MCP** - a dedicated section links to the interactive API docs and lets you copy the MCP connect command directly, no digging through this documentation required.

## Questions

[adicherafik@gmail.com](mailto:adicherafik@gmail.com)
