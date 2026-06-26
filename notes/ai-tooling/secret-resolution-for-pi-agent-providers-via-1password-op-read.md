---
title: "Secret resolution for pi agent providers via 1Password op read"
date: 2026-06-26
captured: 2026-06-26T00:00:00.000Z
tags: ["pi", "1password", "secrets", "agent-config", "security"]
source: "pi-coding-agent docs + ops-toolkit session"
status: refined
---

The pi coding agent (`@earendil-works/pi-coding-agent`) supports `!op read` command execution, `$ENV_VAR` interpolation, and bare literals in any field that accepts a secret. This means provider API keys never need to live as plaintext in `~/.pi/agent/auth.json` or `~/.pi/agent/models.json`. Both files support the same three value forms for `apiKey` / `key`.

## The three resolution forms

Documented in `docs/providers.md` (auth-file `key` field) and `docs/models.md` (provider/model `apiKey`):

- `!op read 'op://vault/item/field'` - run a shell command, use its stdout. 1Password CLI resolves the secret at request time, never at rest.
- `"$ENV_VAR"` or `"${ENV_VAR}"` - environment interpolation. Works inside larger literals too.
- `sk-...` bare literal - the plaintext form. Avoid this for anything that could leak via a transcript, a screenshot, or a repo push.

## Wiring it for the neuralwatt and opencode-go providers

`models.json` (custom providers):

```json
{
  "neuralwatt": {
    "baseUrl": "https://api.neuralwatt.com/v1",
    "api": "openai-completions",
    "apiKey": "!op read 'op://Toolkit/neuralwatt-api-key/credential'",
    "models": [ ... ]
  }
}
```

`auth.json` (built-in API-key providers):

```json
{
  "opencode-go": {
    "type": "api_key",
    "key": "!op read 'op://Toolkit/opencode-api-key-coding/credential'"
  }
}
```

Once wired this way, deleting the plaintext entry from `auth.json` is safe: pi resolves the value on each request via the `!op read` hook, so nothing sensitive sits on disk.

## Prerequisites for the service-account path

1. Install the 1Password CLI (`op`) and authenticate. For headless / CI / agent contexts, a **service account** is the right shape: `OP_SERVICE_ACCOUNT_TOKEN` in the environment, then `op whoami` reports `User Type: SERVICE_ACCOUNT` against the vault's 1Password tenant. No unlock prompt, no desktop app dependency.
2. The referenced item must exist in a vault the service account can read. `op item list --vault Toolkit` confirms visibility; `op read 'op://Toolkit/<item>/<field>'` confirms the field path resolves.
3. Tighten file perms after editing: `chmod 600 ~/.pi/agent/auth.json ~/.pi/agent/models.json`. Defense in depth, not a substitute for the resolution indirection.

## The failure mode this prevents

Two ways a plaintext key in `auth.json` / `models.json` leaks in normal agent usage, neither of which the `!op read` indirection is vulnerable to:

- **Read the file into a transcript.** Any `cat ~/.pi/agent/auth.json` or `read` of the file during a session prints the key into the session log. With `!op read`, the file contains only a reference string; the actual key is fetched at request time and never appears in a file read.
- **Paste the key inline into a probe.** Copying a key out of the auth file to run a `curl -H "Authorization: Bearer ..."` against a new endpoint hardcodes it into the command history and the transcript. The disciplined alternative is to resolve the key into an env var first, then reference the env var: `KEY=$(op read 'op://Toolkit/neuralwatt-api-key/credential'); curl -H "Authorization: Bearer $KEY" ...`. The key still touches the shell history briefly, but it is not committed to a file or a transcript.

Rotation is the actual fix once a key has touched a transcript; the indirection only prevents future exposure.

## Discovering supported models on a custom OpenAI-compatible endpoint

The same session surfaced a useful pattern for populating `models.json` from a provider's `/v1/models` list rather than hand-maintaining it:

```bash
op read 'op://Toolkit/neuralwatt-api-key/credential' \
  | xargs -I{} curl -sS -H "Authorization: Bearer {}" \
    "https://api.neuralwatt.com/v1/models" > /tmp/nw_models.json

python3 -c "import json; d=json.load(open('/tmp/nw_models.json')); [print(m['id'], m['metadata']['display_name']) for m in d['data']]"
```

Then map the response into the pi schema fields: `id`, `name`, `reasoning` (caps.supportsThinking), `contextWindow` (limits.max_context_length), `maxTokens` (limits.max_output_tokens), `input` (`["text","image"]` when caps.vision), `cost` (pricing.input/output/cache), and `compat` (`supportsDeveloperRole` / `supportsReasoningEffort` set false when the endpoint reports them unsupported). The capability metadata is nested under `metadata.capabilities` and `metadata.limits` in this provider's response shape, not at the model root.

## Related

- [[age-and-1password-complementary-encryption-tiers] - the two-tier secret model this pattern lives inside (age for file-level, 1Password for credential-level)
- [[local-llm-hybrid-stack-ollama-ollama-cloud-openrouter-for-hermes-agent]] - same family of provider config for a different agent
- [[ollama-cloud-cloud-suffix-hosted-inference-via-local-endpoint]] - companion note on endpoint discovery for custom providers