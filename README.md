# sentinel-pi

Operational runbook, prompt library, and infrastructure documentation for the
`pi-coding-agent-modified` system running on Raspberry Pi.

> **Status:** Active — last audit 2026-05-05

---

## Repos in this system

| Repo | Purpose | Status |
|---|---|---|
| [pi-coding-agent-modified](https://github.com/hsohn420420-cmyk/pi-coding-agent-modified) | Core agent fork | Active |
| [iflow2api-pi-extension](https://github.com/hsohn420420-cmyk/iflow2api-pi-extension) | Free model provider extension | Active |
| [pi-memory-md](https://github.com/hsohn420420-cmyk/pi-memory-md) | Markdown memory store | Active |
| [sentinel-pi](https://github.com/hsohn420420-cmyk/sentinel-pi) | This repo — docs + runbook | Active |

---

## Quick Setup (Raspberry Pi 4B)

```bash
# 1. Clone and build pi
git clone https://github.com/hsohn420420-cmyk/pi-coding-agent-modified.git
cd pi-coding-agent-modified
npm ci
npm run build

# 2. Install extension
mkdir -p ~/.pi/agent/extensions
cp path/to/iflow2api-extension.ts ~/.pi/agent/extensions/

# 3. Activate meta system prompt
mkdir -p ~/.pi/agent
cp agent/system-prompt.md ~/.pi/agent/system-prompt.md

# 4. Start iflow2api proxy
pip install iflow2api
python -m iflow2api &

# 5. Configure auth (add API keys)
./dist/pi  # opens interactive setup

# 6. Performance tweak
echo 'export PI_SKIP_VERSION_CHECK=1' >> ~/.bashrc
source ~/.bashrc

# 7. Start pi
./dist/pi
```

---

## Provider Priority

```
1. iflow2api  (localhost:28000)  — Free, no auth, local proxy
2. iflow-cloud (apis.iflow.cn)   — Free, needs IFLOW_API_KEY
3. nvidia-nim  (NVIDIA API)      — Free tier, needs NIM_API_KEY
4. anthropic / openai / gemini   — Paid, last resort
```

---

## Config Files

| File | Purpose |
|---|---|
| `~/.pi/agent/auth.json` | API keys (chmod 600) |
| `~/.pi/agent/settings.json` | Model + provider selection |
| `~/.pi/agent/mcp.json` | MCP server definitions |
| `~/.pi/agent/smithery.json` | Smithery connections |
| `~/.pi/agent/system-prompt.md` | Meta system prompt |
| `~/.pi/agent/extensions/` | Loaded TypeScript extensions |
| `~/.pi/agent/prompts/` | Slash-command templates |
| `~/.pi/agent/skills/` | Skill docs |
| `~/.pi/agent/thoughts/` | SequentialThinking JSONL logs |
| `~/.pi/agent/sessions/` | Session JSONL history |

---

## Known Issues & Fixes (Audit 2026-05-05)

| ID | Severity | Description | Fix |
|---|---|---|---|
| P0-1 | FIXED | ESM `require()` in iflow2api extension | ESM imports |
| P0-2 | FIXED | `node_modules` committed to Git | `.gitignore` |
| P0-3 | FIXED | Provider ID conflict `iflow` vs `iflow2api` | Renamed to `iflow-cloud` |
| P1-1 | FIXED | MCP no reconnect on crash | Exponential backoff + health guard |
| P1-2 | FIXED | SequentialThinking not persisted | JSONL to `~/.pi/agent/thoughts/` |
| P1-3 | FIXED | Smithery wrong API endpoint | Correct `/connect/{ns}/{id}/.tools` |
| P2-1 | FIXED | Token estimation Unicode-blind | `Intl.Segmenter` grapheme counting |
| P3-1 | FIXED | `reasoning:true` on non-CoT models | Only `kimi-k2-thinking` has it |

---

## Troubleshooting

### iflow2api not responding
```bash
curl http://localhost:28000/v1/models
# If fails:
python -m iflow2api
```

### Stale lockfile on startup
```bash
rm -f ~/.pi/agent/auth.json.lock
```

### pi binary not found
```bash
cd ~/pi-coding-agent-modified
npm run build
# Binary at: dist/pi
```

### MCP tools missing after restart
```bash
# Check mcp.json syntax
cat ~/.pi/agent/mcp.json | python3 -m json.tool
# Restart pi (MCP connects only at startup)
```

### Smithery tools missing
```bash
# Ensure smithery.json has 'namespace' field
cat ~/.pi/agent/smithery.json
# Must contain: { "apiKey": "...", "namespace": "...", "connections": [...] }
```

### High memory on Pi 3B
```bash
# Use smaller models via NIM
# In settings.json: nvidia-nim/meta/llama-3.1-8b-instruct
# Or run with Bun binary instead of Node:
dist/pi  # Bun static binary, no Node.js needed
```

---

## Raspberry Pi Hardware Notes

| Device | Node.js | Recommended | Notes |
|---|---|---|---|
| Pi 4B (4GB) | 20.x LTS | ✅ Primary | Full stack |
| Pi 3B (1GB) | 20.x marginal | Bun binary | Watch OOM |
| Pi 2B (512MB) | Not supported | Bun binary only | ARMv7 |
| Windows 11 | Any | ✅ Dev machine | RTX 3050 for Ollama |

---

## Slash Commands Reference

| Command | Description |
|---|---|
| `/settings` | Open settings (providers, MCP, keys) |
| `/model` | Switch model |
| `/reload` | Hot-reload extensions, prompts, skills |
| `/session` | Show session info + token stats |
| `/compact` | Manually compact context |
| `/export` | Export session to HTML/JSONL |
| `/fork` | Fork from a previous message |
| `/unrestricted` | Toggle unrestricted mode |

---

## Useful pi Tools (in-session)

```
iflow_check_health       — Check iflow2api is running
iflow_analyze_chunking   — Check if text needs chunking before sending
iflow_chunk_text         — Split large documents
sequential_thinking      — Step-by-step reasoning (persisted to disk)
search_docs              — ctx7 documentation search
```

---

## Environment Variables

```bash
# Add to ~/.bashrc or ~/.zshrc
export PI_SKIP_VERSION_CHECK=1    # Save ~500ms on slow Pi SD card
export IFLOW_API_KEY=your_key     # iFlow cloud provider
export NIM_API_KEY=your_key       # NVIDIA NIM free tier
```

---

*Maintained by sentinel-pi — auto-updated by pi-coding-agent audit system.*
