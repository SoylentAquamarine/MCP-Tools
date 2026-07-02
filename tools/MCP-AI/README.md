# MCP-AI

`MCP-AI` is the planned AI provider broker tool for the MCP-Tools suite.

It should route requests to configured AI providers while keeping API keys in the shared secrets store.

## Purpose

```text
Claude or another caller -> MCP-AI -> configured AI provider
```

Supported provider direction:

```text
OpenAI
Anthropic
Gemini
Groq
Mistral
Ollama/local models
OpenRouter
```

## Uses Secrets Store

Provider configs should store `secretRef` values only.

Example:

```json
{
  "providers": {
    "openai": {
      "type": "openai",
      "defaultModel": "gpt-4.1-mini",
      "secretRef": "provider/openai/api-key"
    }
  }
}
```

The plaintext API key should never be returned to MCP callers.

## Planned MCP Tools

```text
ai_list_providers
ai_discover_models
ai_list_models
ai_route
ai_ask
ai_compare
ai_health_check
```

## Status

Skeleton only. Secrets tool will be built first.
