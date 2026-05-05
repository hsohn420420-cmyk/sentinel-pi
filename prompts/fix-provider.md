---
description: Diagnose and fix provider connection issues
---
Diagnose and fix the provider issue: $ARGUMENTS

Steps:
1. Check which provider is failing
2. Verify the API endpoint is reachable:
   bash: curl -v {endpoint}/models 2>&1 | head -30
3. Check the API key is set:
   bash: cat ~/.pi/agent/auth.json | jq '.{provider}'
4. Check settings.json has the correct provider ID
5. If iflow2api: verify proxy is running on port 28000
6. Report root cause + exact fix command
