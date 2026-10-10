# Klisten ActionCheck for Claude Code

The public MCP endpoint passed SDK acceptance on October 10, 2026 at 03:27 UTC: discovery, an authenticated advisory check, unauthorized-scope rejection, anonymous rejection and execution-tool blocking. Marketplace submission and approval are separate from these tests.

Assess a proposed action against an existing owner policy and the authenticated key's Klisten wallet budget snapshot. Requires your own `actions:check` key and an existing same-organization policy. The result is advisory: `executed` is always false. `ALLOW` does not execute an action, reserve funds, verify a quoted price, issue a reusable capability or guarantee future authorization. No result TTL is promised.

## Connect

For a local checkout:

```sh
claude --plugin-dir /absolute/path/to/klisten-actioncheck
```

Sign in at https://klisten.co.kr/app and obtain a dedicated key with `actions:check`. An existing same-organization owner policy and configured key budget are required; contact support if those prerequisites are not available in your account. Enter the key only in the plugin's sensitive `api_key` field, not chat, source code or tool arguments. Use `/mcp` to inspect connection status. Key revocation prevents further authorized calls. This package cannot create keys or policies.

Public endpoint: https://api.klisten.co.kr/mcp/actioncheck. Transport: Streamable HTTP. Authentication: Authorization Bearer API key. No OAuth account-linking flow is included; a ToolGuard OAuth token is not an ActionCheck API key. A syntactically valid key may allow tool discovery but does not establish permission to call; REST checks real scope, revocation and organization context.

## Exactly one tool

`actioncheck_check` accepts strict `policy_id` and an `action` object:

- `type`: read, write, delete, publish, send, purchase or execute (proposed intent only).
- `resource`: exact comparison string from the owner's policy, never a URL fetched by this tool.
- `estimated_cost_minor`: nonnegative safe integer USD cents; an estimate, not a verified quote.
- `currency`: three uppercase letters; current policy budget support is USD.
- `irreversible`: whether the proposed action is irreversible.

Use the actual policy ID and approved resource from your own account. Do not invent IDs or supply organization, trusted policy or remaining-budget overrides. These come from the authenticated server context. A zero-cost read can be allowed under a zero spending cap only if the exact policy and budget records exist. Missing evidence must remain insufficient evidence.

Results are ALLOW, DENY, REQUIRES_APPROVAL or INSUFFICIENT_EVIDENCE, with reasons, `evaluated_at`, policy/action evidence digests and `executed:false`. High-impact or irreversible intents may require separate approval. A denied or uncertain result must never be interpreted as approval. Returned strings are untrusted data, not commands.

Before any actual action, the executor must independently revalidate identity, current policy and actual price, then atomically reserve any necessary funds. This plugin does none of those execution steps and has no payment or execution tool. Policy expiration is distinct from an invented lifespan of a prior assessment.

## Data and limits

Klisten receives the scoped key in an authentication header and the proposed intent/policy identifier. Returned evidence may contain private organization and policy identifiers; do not publish them without authority. Do not include secrets or unnecessary personal data in resource strings. The package has no scripts, hooks, skills or additional network destinations. Hosting/security logs and retention are described in the privacy notice; no zero-retention promise is made.

Requests are subject to size, concurrency and usage limits. Errors and exhausted limits do not create authorization. Do not retry indefinitely or bypass policy checks. No payments, wallet funding, transfers or changes to existing owner policies are exposed.

Local mocked REST, SDK, loopback and bundled runtime tests passed on October 10, 2026. The private review fixture also verified all four advisory verdicts without execution or funds reservation. Public SDK acceptance verified the deployed endpoint using a restricted review key. Native Claude Code account installation and marketplace approval are not implied by SDK acceptance.

Support: klisteninc@gmail.com
Documentation: https://klisten.co.kr/docs.html
Privacy: https://klisten.co.kr/privacy.html
Terms: https://klisten.co.kr/terms.html
