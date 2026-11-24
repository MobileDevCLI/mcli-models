# mcli-models

Public catalog of current Claude / Gemini / Codex / Grok / (future) model IDs,
consumed by [`mcli-model`](https://github.com/MobileDevCLI/MobileCLI-v9.0.0)
across every MobileCLI variant.

## Why this exists

MobileCLI runs on Termux ARM64 Android. Vendors (Anthropic, Google, OpenAI,
xAI) don't publish Android-native builds of their CLI tools, so each CLI is
pinned at the last-JS-only version. The pinned CLI's internal model list
goes stale as vendors ship new models.

`mcli-model` solves this by keeping the live model catalog **out of the APK**:
users refresh from this repo at any time, no app update required.

## Schema

```json
{
  "schema": 1,
  "updated_utc": "ISO-8601 timestamp",
  "providers": {
    "<provider>": {
      "default": "<model-id>",
      "env_var": "ENV_VAR_THE_CLI_READS",
      "models": [
        {"id": "<vendor-model-id>", "name": "<display>", "backend": "<wrapper>"}
      ]
    }
  }
}
```

`backend` values:
- `claude-run` — pinned 2.1.112 Claude Code CLI (works only for IDs in its allowlist)
- `mcli-fable` — direct Anthropic API via the user's OAuth token (any model ID)
- `gemini` / `codex-run` / `grok-run` — pinned vendor wrappers

## Updating

When a vendor ships a new model, append it to `models.json` and PR. Users then:

```
mcli-model refresh
mcli-model set claude <new-model-id>
mcli-model run claude
```

No APK ship, no Play review wait.
