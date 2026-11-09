# Preferly for Hermes Agent

Check how AI tools see a website — crawler access, llms.txt, structured data, readable business facts, robots.txt lines, and a quick AI visibility preview — from inside Hermes Agent.

Preferly is the AI visibility audit that also checks whether AI gets your facts right: 10 leading AI models, every claim checked against your website, and the exact fixes. Free check; full report $99, no subscriptions.

This package is a portable [Agent Plugins v1](https://agent-plugins.org) package: configuration and one skill, no plugin code. It connects Hermes to Preferly's hosted MCP server.

## What you get

- **MCP server `preferly`** (`https://www.preferly.ai/mcp?client=hermes`, Streamable HTTP, no sign-in) with **six** read-only tools (verified live on Preferly's MCP `tools/list` and server card):

  | Tool | Inputs | What it does |
  |---|---|---|
  | `check_ai_crawler_access` | `website` (required) | robots.txt allow/block for 13 AI crawlers (including GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, PerplexityBot, Google-Extended) with the deciding rule; whether the homepage loads or shows a bot check; whether `/llms.txt` exists; schema.org types on the homepage |
  | `generate_ai_robots_txt` | `allow_search` (default true), `allow_training` (default false), optional `disallow_paths[]` | robots.txt lines that let AI search/assistant crawlers in and keep training crawlers out (or any mix). Fetches nothing; needs no website |
  | `check_llms_txt` | `website` (required) | Whether the site publishes `/llms.txt`, size and title, and format problems vs llmstxt.org |
  | `check_business_schema` | `website` (required) | Business type and fields in homepage JSON-LD (present, missing, or off) |
  | `check_business_info` | `website` (required) | Whether name, phone, email, address, hours and prices are readable as text (homepage + up to 2 contact/booking/pricing pages) |
  | `quick_ai_visibility_check` | `website` (required), optional `business_name`, `city` | Asks 3 AI models 3 typical customer questions from the public site (no web search); reports whether each named the business, up to 2 contradictions vs the website, and a one-line crawler summary |

- **Skill `preferly:ai-visibility-check`**: when to call each tool, how to explain the result, and when to offer Preferly's free browser check / $99 report.

## Install

```bash
hermes plugins install preferly-ai/preferly-hermes --no-enable
hermes plugins enable preferly
```

Then ask, for example:

- "Can AI crawlers read example.com?"
- "Does my robots.txt block GPTBot or ClaudeBot?"
- "Write robots.txt lines that let AI search crawlers in, keep training crawlers out, and keep /admin private"
- "Does example.com have a valid llms.txt?"
- "Is the business schema on example.com complete?"
- "Can AI read the phone number and hours from example.com without running scripts?"
- "What do AI assistants say about the business at example.com?"

### Example tool calls

```json
{"website": "example.com"}
```

```json
{"allow_search": true, "allow_training": false, "disallow_paths": ["/admin", "/cart"]}
```

```json
{"website": "example.com", "business_name": "Acme Plumbing", "city": "Austin"}
```

## What it sends, and to whom (disclosure)

- **Network:** each tool call that needs a website sends that address to `www.preferly.ai`. Preferly's crawler (`PreferlyBot`, signed with Web Bot Auth) then fetches that site's robots.txt, homepage, `/llms.txt`, and (for `check_business_info` / `quick_ai_visibility_check`) up to 2 more public pages robots.txt allows. `generate_ai_robots_txt` fetches nothing. Nothing on the site is changed.
- **Model calls:** only `quick_ai_visibility_check` calls AI models, on Preferly's servers. The other five tools do not.
- **No credentials:** no API key, account or sign-in. The package stores nothing.
- **Limits:** calls are rate-limited per IP address (10 an hour, 30 website checks a day) and per domain (about 20 fresh checks a day). A crawler-style result for the same domain may come from a check run in the last 10 minutes; a quick visibility result may come from a check in the last 24 hours.
- **Data:** Preferly stores the domain checked and a keyed hash (HMAC) of the rate-limit key. It doesn't log your IP address or the result. See https://www.preferly.ai/privacy.
- **Links:** tools and the skill may give links to preferly.ai (free check, tools pages, example report). Links from this package carry `utm_source=hermes` so we can count visits from Hermes. No telemetry is sent from your machine.
- **No shell commands, background processes or file reads** outside the package.

## Use the MCP server without the plugin

```yaml
# ~/.hermes/config.yaml
mcp_servers:
  preferly:
    url: https://www.preferly.ai/mcp?client=hermes
```

Server card (every tool): https://www.preferly.ai/.well-known/mcp/server-card.json

## Links

- Website: https://www.preferly.ai
- Free AI crawler check in the browser: https://www.preferly.ai/ai-crawler-check
- Free tools: https://www.preferly.ai/tools
- Privacy: https://www.preferly.ai/privacy · Terms: https://www.preferly.ai/terms
- Contact: hello@preferly.ai

## License

MIT
