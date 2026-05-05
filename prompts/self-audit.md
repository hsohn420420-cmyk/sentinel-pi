---
description: Run a full self-healing audit of the pi installation
---
You are running a full self-healing audit of this pi installation.

Follow this exact sequence:

1. Check config directory exists and list contents:
   bash: ls -la ~/.pi/agent/

2. Check auth.json for configured providers:
   bash: cat ~/.pi/agent/auth.json 2>/dev/null | jq 'keys' || echo "No auth.json"

3. Check settings.json for active model:
   bash: cat ~/.pi/agent/settings.json 2>/dev/null || echo "No settings.json"

4. Check MCP servers:
   bash: cat ~/.pi/agent/mcp.json 2>/dev/null | jq '.mcpServers | keys' || echo "No mcp.json"

5. Check extensions loaded:
   bash: ls ~/.pi/agent/extensions/ 2>/dev/null || echo "No extensions dir"

6. Check Node.js version:
   bash: node --version

7. Check pi binary exists:
   bash: ls -la ~/pi-coding-agent-modified/dist/pi 2>/dev/null || echo "Not built"

8. Check iflow2api is running:
   bash: curl -s http://localhost:28000/v1/models | jq '.data[].id' 2>/dev/null || echo "iflow2api not running"

9. Check for stale lockfiles:
   bash: ls ~/.pi/agent/*.lock 2>/dev/null || echo "No stale locks"

10. Check thoughts directory size:
    bash: ls ~/.pi/agent/thoughts/ 2>/dev/null | wc -l || echo "No thoughts dir"

After each check, report the finding concisely.
At the end, produce a summary table: Component | Status | Action Required
Only list items that need attention in the action column.
