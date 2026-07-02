# VTX-MCP-AI Design

## Goal

Provide a local MCP AI broker that can use multiple provider APIs without exposing API keys to Claude or other MCP callers.

## Main Files

```text
providers.json       provider config, no secrets
models-cache.json    discovered model metadata
routing-rules.json   local routing preferences
```

## Provider Config Rule

Provider config stores this:

```text
secretRef
```

It does not store this:

```text
API key plaintext
```

## First Routing Modes

```text
best
cheap
fast
private
code
long_context
vision
```

## First Integration Test with Secrets

1. Add `provider/openai/api-key` to the Console-managed `secrets.json` (see `docs/CONSOLE-SECRETS.md`).
2. Add OpenAI provider config with `secretRef`.
3. Run `ai_health_check` from the Console, which resolves the ref and passes the value at runtime.
4. Confirm the key is used internally and never printed, logged, or returned.
