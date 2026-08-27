# Sellm MCP for Cursor

Track how brands appear across ChatGPT, Perplexity, Gemini, Grok, Copilot, Google AI Overviews, and AI Mode directly from Cursor.

- Measure AI share of voice
- Track brand mentions and ranking position
- Compare competitors
- Inspect citations and AI search coverage
- Analyze results across providers and countries

This repository is the public Cursor plugin / MCP listing package. The server itself is hosted at `https://sellm.io/mcp` (Streamable HTTP). No local process is required.

## Install in Cursor

### Marketplace (after listing)

Search for **Sellm** in Cursor **Customize → MCP → Browse Marketplace**, then set `SELLM_API_KEY` when prompted.

### Manual config

Add this to `.cursor/mcp.json` or `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "sellm": {
      "url": "https://sellm.io/mcp",
      "headers": {
        "Authorization": "Bearer ${env:SELLM_API_KEY}"
      }
    }
  }
}
```

Create a key in [Project Settings](https://sellm.io) or Organization API Integrations, then restart Cursor or toggle the server under **Customize**. The tools `submit_analysis`, `get_analysis`, and `list_providers` should appear.

Full setup: [Sellm MCP docs](https://sellm.io/docs/api/getting-started/mcp)

## Authentication

Send a Sellm API key as a Bearer token. Never commit a real key.

- **Project API keys** analyze the project's configured brand. Do not pass `brandName`.
- **Organization API keys** require `brandName` per job and spend prepaid API credits.

The Cursor plugin declares `SELLM_API_KEY` as a secret variable. Cursor substitutes `${SELLM_API_KEY}` into `mcp.json`.

## Example prompts

After the tools appear, ask Cursor:

- Track ChatGPT visibility for Acme in the US for "best CRM for startups".
- Compare share of voice for Acme vs HubSpot on Perplexity and Gemini.
- List the AI providers Sellm can query from this MCP.

Keep the first job small: 1 prompt × 1 provider × 1 location × 1 replicate. Typical jobs finish in 1–2 minutes. Poll `get_analysis` every 5–10 seconds. Do not submit a second job while waiting.

`submit_analysis` spends analysis credits. Credits reserved equal `prompts × providers × locations × replicates`.

## Tools

| Tool | What it does |
| --- | --- |
| `submit_analysis` | Queue a brand-visibility job and return `analysisId` |
| `get_analysis` | Poll until the job finishes; return compact share-of-voice metrics |
| `list_providers` | List provider IDs accepted by `submit_analysis` |

Supported provider IDs: `chatgpt`, `perplexity`, `gemini`, `grok`, `google_aio`, `google_ai_mode`, `copilot`. Locations are ISO 3166-1 alpha-2 codes such as `US` or `DE`.

Raw provider transcripts are omitted from MCP results so agent context stays compact.

## Other MCP clients

Any client that supports remote Streamable HTTP can use the same URL and `Authorization` header.

Official MCP Registry name: `com.sellm/mcp` (publishing is a separate DNS-verified step).

## Security

- Do not put API keys in this repository.
- Rotate a key immediately if it was pasted into a shared config file.
- Rate limit: 60 requests per minute per API key.

## Docs and support

- Product: [sellm.io](https://sellm.io)
- MCP guide: [sellm.io/docs/api/getting-started/mcp](https://sellm.io/docs/api/getting-started/mcp)
- API overview: [sellm.io/docs/api/getting-started/overview](https://sellm.io/docs/api/getting-started/overview)
- Authentication: [sellm.io/docs/api/getting-started/authentication](https://sellm.io/docs/api/getting-started/authentication)
- Support: [info@sellm.io](mailto:info@sellm.io)

## License

MIT
