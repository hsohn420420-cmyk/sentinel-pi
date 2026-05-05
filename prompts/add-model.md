---
description: Add a new model or provider to pi
---
Add the following model/provider to pi: $ARGUMENTS

Steps:
1. Determine provider type (openai-compatible, anthropic, gemini, custom)
2. Find the correct baseUrl and model ID
3. Read current settings.json:
   bash: cat ~/.pi/agent/settings.json
4. Add provider block with correct schema (zero cost if free tier)
5. Write updated settings.json
6. Test with /reload then /model to verify it appears
7. Report: provider ID, model ID, context window, cost

Important:
- Provider ID must be unique (check existing IDs first)
- reasoning: true ONLY if model has genuine chain-of-thought
- Set cost to 0 for free providers
