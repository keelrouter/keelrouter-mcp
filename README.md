# KeelRouter MCP server and agent skill

[KeelRouter](https://keelrouter.com) gives you one API key and one prepaid USD balance for language, image and video AI models (Claude, GPT, Gemini, DeepSeek, Seedream, GPT Image, Nano Banana, Seedance, Kling, Veo and more). This repository holds the connection details for its remote MCP server and an [Agent Skill](skills/keelrouter/SKILL.md) that teaches an agent to use both the MCP tools and the HTTP API. The service itself is hosted; there is nothing to install or run.

- **MCP endpoint:** `https://api.keelrouter.com/mcp` (Streamable HTTP)
- **Auth:** `Authorization: Bearer <key>`. Create a key at [keelrouter.com/console/keys](https://keelrouter.com/console/keys).
- **Docs:** [MCP](https://keelrouter.com/docs#mcp) · [HTTP API](https://keelrouter.com/docs) · [Pricing](https://keelrouter.com/pricing)

## Connect

Put your key in an environment variable first:

```bash
export KEELROUTER_API_KEY=sk_...
```

**Claude Code**

```bash
claude mcp add --transport http keelrouter https://api.keelrouter.com/mcp --header "Authorization: Bearer $KEELROUTER_API_KEY"
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "keelrouter": {
      "url": "https://api.keelrouter.com/mcp",
      "headers": { "Authorization": "Bearer sk_..." }
    }
  }
}
```

**Codex** (`~/.codex/config.toml`)

```toml
[mcp_servers.keelrouter]
url = "https://api.keelrouter.com/mcp"
bearer_token_env_var = "KEELROUTER_API_KEY"
```

## Tools

| Tool | What it does | Cost |
| --- | --- | --- |
| `list_models` | List image and video models on KeelRouter with USD prices per tier and accepted request parameters (`kind`: `image` or `video`) | Free |
| `generate_image` | Generate an image with a chosen model (`model`, `prompt`, optional `params`, optional `idempotency_key`). Waits up to 50 seconds; returns result links or a `task_id` | Per-model USD price from `list_models`; failed calls cost nothing |
| `generate_video` | Start an async video generation job and return `task_id` immediately; poll with `get_result` | Per-model USD price from `list_models`; failed calls cost nothing |
| `get_result` | Check the status of an image or video task; when completed, returns temporary download URLs and the final USD cost | Free |
| `get_balance` | Check available and held USD balance on the account | Free |

Listing tools works without a key; calling one requires your `Bearer` key. Language models are served over the OpenAI-compatible (`/v1/chat/completions`, `/v1/responses`) and Anthropic-compatible (`/v1/messages`) HTTP endpoints documented at [keelrouter.com/docs](https://keelrouter.com/docs).

## Pricing

Pay as you go from a prepaid USD balance: top-ups from $10, no subscription, credits do not expire, and failed or refused calls are never charged. Current per-model prices are at [keelrouter.com/pricing](https://keelrouter.com/pricing) and `GET https://api.keelrouter.com/v1/models`.

## Agent skill

[`skills/keelrouter/SKILL.md`](skills/keelrouter/SKILL.md) follows the [Agent Skills](https://agentskills.io) format. Install it with

```bash
npx skills add keelrouter/keelrouter-mcp
```

or copy the folder into your agent's skills directory (for Claude Code, `~/.claude/skills/keelrouter/`).

## Support

support@keelrouter.com

## License

The files in this repository are MIT licensed. The KeelRouter service has its own [terms](https://keelrouter.com/terms).
