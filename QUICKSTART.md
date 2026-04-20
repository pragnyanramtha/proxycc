# Quick Start Guide

## Localhost setup for Claude Code

This repo is preconfigured to proxy Claude Code to:

- `OPENAI_BASE_URL=https://ai.hackclub.com/proxy/v1`
- `BIG_MODEL=z-ai/glm-5`
- `MIDDLE_MODEL=z-ai/glm-5`
- `SMALL_MODEL=morph/morph-v3-large`

### 1. Create the virtualenv and install dependencies

```bash
UV_CACHE_DIR=/tmp/uv-cache uv venv .venv
UV_CACHE_DIR=/tmp/uv-cache uv sync
```

### 2. Start the proxy on localhost

```bash
./run-local.sh
```

The proxy listens on `http://127.0.0.1:8082`.

### 3. Point Claude Code at the proxy

In another terminal:

```bash
ANTHROPIC_BASE_URL=http://127.0.0.1:8082 ANTHROPIC_API_KEY=dummy claude
```

### 4. Optional health checks

```bash
curl http://127.0.0.1:8082/health
curl http://127.0.0.1:8082/test-connection
```

## Model mapping

| Claude request | Backend model |
| --- | --- |
| haiku | `morph/morph-v3-large` |
| sonnet | `z-ai/glm-5` |
| opus | `z-ai/glm-5` |

## Compatibility note

The configured `sonnet` and `opus` backend (`z-ai/glm-5`) supports OpenAI-style tool calls and is suitable for Claude Code workflows.

The configured `haiku` backend (`morph/morph-v3-large`) is accepted by the provider for normal chat completions, but the provider metadata and a live tool-call test both indicate that tool use is not available for that model. If Claude Code routes a tool-heavy workflow through haiku, it may fail until you switch `SMALL_MODEL` to a tool-capable model.
