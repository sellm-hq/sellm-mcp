---
name: ai-visibility
description: Track brand mentions, share of voice, citations, and competitor rankings across ChatGPT, Perplexity, Gemini, Grok, Copilot, and Google AI with the Sellm MCP. Use when the user asks about AI search visibility, GEO, LLM monitoring, ChatGPT ranking, or share of voice.
---

# Sellm AI visibility

Use the Sellm MCP tools. Do not call the REST API unless the MCP tools are unavailable.

## Tools

1. `list_providers` — confirm supported provider IDs and locations.
2. `submit_analysis` — queue a job. Returns `analysisId` immediately.
3. `get_analysis` — poll until `succeeded` or `failed`. Typical jobs finish in 1–2 minutes. Poll every 5–10 seconds. Do not submit a second job while waiting.

## Auth rules

- Project API keys analyze the project's configured brand. Do not pass `brandName`.
- Organization API keys require `brandName` per job and spend prepaid credits.
- Credits reserved equal `prompts × providers × locations × replicates`.
- `submit_analysis` is not a dry run.

## How to run a useful job

Keep the first job small: 1 prompt, 1 provider, 1 location, 1 replicate.

Example prompt: "best CRM for startups"

Supported provider IDs include `chatgpt`, `perplexity`, `gemini`, `grok`, `google_aio`, `google_ai_mode`, and `copilot`. Locations are ISO 3166-1 alpha-2 codes such as `US` or `DE`.

## How to report results

Summarize compact metrics from `get_analysis`:

- Share of voice
- Coverage
- Average position
- Competitors
- Per-provider breakdown
- Citations when present

Do not ask for raw provider transcripts. The MCP omits them on purpose.

If the job fails, report the error and stop. If the key is invalid or credits are exhausted, tell the user how to fix it in [Sellm API settings](https://sellm.io/docs/api/getting-started/authentication).
