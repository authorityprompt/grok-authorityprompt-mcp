# AuthorityPrompt for Grok

Companies have an official website for humans. They don't have an official
identity for AI. [AuthorityPrompt](https://authorityprompt.com) is that identity
— and this plugin lets you manage **your own company's** identity from Grok.

You sign in with your AuthorityPrompt account. Grok then works with your
company's data: verified facts, sources, AIO readiness, LLM visibility. Each
account sees only its own companies.

## Connect

Grok reaches AuthorityPrompt over a remote MCP endpoint — there is nothing to
install or run locally.

**In Grok:** Connectors → New Connector → **Custom** → paste:

```
https://authorityprompt.com/api/v1/integrations/grok/mcp
```

On the first tool call your browser opens the AuthorityPrompt consent screen.
After you approve, Grok holds the token.

You need an AuthorityPrompt account: https://authorityprompt.com

## Use

Ask in plain language:

```
Which account am I connected as?
List my companies.
Run an AIO readiness audit.
Add https://example.com/about as a source for my company.
What do LLMs currently say about us, and where does it disagree with our facts?
```

## What it can do

Read your account and companies, pull the canonical profile, verified facts,
sources with provenance and confidence, run AIO readiness audits and LLM
visibility checks, and — with your consent — publish and repair company facts.

Writes are explicit: the tools that change data are separate from the ones that
read it, and the OAuth scopes you grant decide which are available.

## Permissions

| Scope | Purpose |
|---|---|
| `profile:read` | identify the connected account |
| `companies:read` | list your companies |
| `company_profile:read` | read canonical profile, facts, sources |
| `company_profile:write` | publish or repair facts, add sources |
| `reports:read` | read audit and visibility reports |
| `aio:run` | run AIO readiness audits |
| `generators:run` | generate llms.txt, JSON-LD, answer blocks |
| `visibility:check` | check what LLMs say about the company |

Revoke access any time from your AuthorityPrompt account settings.

## How it works

`.mcp.json` declares one remote server:

```json
{
  "mcpServers": {
    "authorityprompt": {
      "type": "http",
      "url": "https://authorityprompt.com/api/v1/integrations/grok/mcp"
    }
  }
}
```

The endpoint requires OAuth 2.0. It answers `401` with a `WWW-Authenticate`
header pointing at its protected-resource metadata, from which the client
discovers the authorization server, registers dynamically (RFC 7591) and runs
the flow with PKCE.

## Links

- Website: https://authorityprompt.com
- Privacy: https://authorityprompt.com/privacy
- Terms: https://authorityprompt.com/terms
- Support: hello@authorityprompt.com

## License

MIT — see [LICENSE](LICENSE).
