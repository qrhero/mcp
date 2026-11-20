![QR Hero](assets/logo.svg)

# QR Hero for AI assistants

Manage your [QR Hero](https://qrhero.com) dynamic QR codes from an AI assistant. Ask for a set of codes for a campaign, point a printed code at a new page, get a scan report for last month, or bring over the codes you made with another generator.

This repository holds two things:

- The address of the hosted QR Hero MCP server, `https://mcp.qrhero.com/mcp`, with 25 tools for codes, brands, domains, styles and scan statistics.
- Four skills that show the assistant how to use those tools well.

## Skills

| Skill | Use it when you want to |
|---|---|
| `launch-qr-campaign` | Create one code or a few hundred for a flyer, menu, poster, packaging or event, with the right brand, style and domain |
| `change-qr-destination` | Send a code that is already printed somewhere new, or find out why a code stopped working |
| `qr-scan-report` | See how your codes are doing over the last 7, 30 or 90 days, as a summary or a CSV |
| `move-qr-codes-to-qrhero` | Bring codes or a list of links over from another QR code generator |

## Requirements

- A QR Hero account on a plan with API access (Business and up). New accounts start with a 14-day free trial.
- An assistant that supports remote MCP servers.

## Connect

**Claude and ChatGPT:** add `https://mcp.qrhero.com/mcp` as a connector and sign in with your QR Hero account. On the sign-in page you choose which organization the assistant may use and whether it gets full or read-only access. You can disconnect it at any time under Settings, API tokens, Connected apps.

**Cursor, VS Code, Windsurf and other clients:** create an API token in QR Hero with the `mcp.access` ability and send it as `Authorization: Bearer <token>`. Setup for each client is in the [MCP docs](https://docs.qrhero.com/mcp).

## What this plugin runs, sends and fetches

- It runs nothing on your computer. The skills are Markdown instructions and the server is hosted by QR Hero.
- Your assistant sends tool calls to `https://mcp.qrhero.com/mcp`, signed in as you. The server reads and changes codes, brands, domains, styles and scan statistics in the organization you picked, within what your role allows. A read-only connection cannot change anything.
- Bulk creates above 10 codes, deletes and bulk status changes show a preview first and only run after a second, confirmed call.
- Nothing else is fetched. The assistant only visits a destination URL if you ask it to check one.

Data handling is described in the [privacy policy](https://qrhero.com/legal/privacy/).

## Support

Questions and bug reports: open an issue in this repository or use the contact form at [qrhero.com](https://qrhero.com/contact/).

## License

MIT
