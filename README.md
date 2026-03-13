# opencode_kong_sakura


## Setup

1. Copy `.env.example` to `.env`
2. Set `KONG_LICENSE_DATA`
3. Set `SAKURA_AI_AUTH_HEADER`
4. Replace `REPLACE_WITH_A_LONG_RANDOM_API_KEY` in `kong.yaml` with your own API key
5. Run `docker compose up -d`

## OpenCode config

Copy `opencode.json.example` to your project as `opencode.json`.

Set an environment variable for the key-auth header.

```bash
export OPENCODE_KONG_API_KEY=replace-with-the-same-key-as-kong-yaml
```

Then use the provider in OpenCode.

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "sakura-kong": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Sakura via Kong",
      "options": {
        "baseURL": "http://127.0.0.1:8000/v1",
        "headers": {
          "x-api-key": "{env:OPENCODE_KONG_API_KEY}"
        }
      },
      "models": {
        "your-sakura-model": {
          "name": "Your Sakura Model"
        }
      }
    }
  }
}
```

`baseURL` is `/v1` because OpenCode's OpenAI-compatible provider calls `/v1/chat/completions`.

## Test

After starting Kong, send a request through the proxy:

```bash
curl -X POST http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "x-api-key: replace-with-the-same-key-as-kong-yaml" \
  -d '{
    "model": "your-sakura-model",
    "messages": [
      { "role": "user", "content": "Hello" }
    ]
  }'
```
