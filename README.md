# Preferly for Hermes Agent

Check whether AI crawlers can reach a website, from inside Hermes Agent.

Preferly is the AI visibility audit that also checks whether AI gets your facts right: 10 leading AI models, every claim checked against your website, and the exact fixes. Free check; full report $99, no subscriptions.

This package is a portable [Agent Plugins v1](https://agent-plugins.org) package: configuration and one skill, no plugin code.

## What you get

- **MCP server `preferly`** (`https://www.preferly.ai/mcp`, Streamable HTTP, no sign-in) with one read-only tool, `check_ai_crawler_access`. Give it a website and it reports:
  - which of 13 AI crawlers (including GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, PerplexityBot and Google-Extended) the site's robots.txt allows or blocks, and the rule that decides each one;
  - whether the homepage loads for an automated visitor or shows a bot check;
  - whether the site publishes `/llms.txt`, and which schema.org types the homepage declares.
- **Skill `preferly:ai-visibility-check`**: tells the agent when to use the tool, how to explain the result, and, when you ask what AI says about a business, where to run Preferly's free check.

## Planned (not available yet)

These are planned for a later version and aren't in this release. The server lists only the tools that are live.

- More read-only site tools: an AI robots.txt generator, an llms.txt check, a business schema check, and a check of whether a site's business details (name, address, phone, hours, services) are readable by AI.
- A quick AI visibility check with no sign-up. It would ask a few customer questions to smaller AI models, through Preferly's servers at no cost to you, and show whether they name the business and where they contradict its website. Before that ships, this README will list exactly what it sends and to whom.

## Install

```bash
hermes plugins install preferly-ai/preferly-hermes --no-enable
hermes plugins enable preferly
```

Then ask, for example:

- "Can AI crawlers read example.com?"
- "Does my robots.txt block GPTBot or ClaudeBot?"
- "Is Cloudflare stopping AI tools from reading my site?"

## What it sends, and to whom (disclosure)

- **Network:** each tool call sends the website you name to `www.preferly.ai`. Preferly's crawler (`PreferlyBot`, signed with Web Bot Auth) then fetches that site's robots.txt, homepage and `/llms.txt`. Nothing else is fetched, and nothing on the site is changed.
- **No credentials:** no API key, account or sign-in. The package stores nothing.
- **No model calls by Preferly:** the crawler check doesn't call any AI model.
- **Limits:** calls are rate-limited per IP address (10 an hour, 30 a day) and per domain. A result for the same domain may come from a check run in the last 10 minutes.
- **Data:** Preferly stores the domain checked and a keyed hash (HMAC) of the rate-limit key. It doesn't log your IP address or the result. See https://www.preferly.ai/privacy.
- **Links:** the skill may give you one link to preferly.ai when you ask what AI says about a business. Links carry `utm_source=hermes` so we can count visits from Hermes. No telemetry is sent from your machine.
- **No shell commands, background processes or file reads** outside the package.

## Use the MCP server without the plugin

```yaml
# ~/.hermes/config.yaml
mcp_servers:
  preferly:
    url: https://www.preferly.ai/mcp?client=hermes
```

## Links

- Website: https://www.preferly.ai
- Free AI crawler check in the browser: https://www.preferly.ai/ai-crawler-check
- Free tools: https://www.preferly.ai/tools
- Privacy: https://www.preferly.ai/privacy · Terms: https://www.preferly.ai/terms
- Contact: hello@preferly.ai

## License

MIT
