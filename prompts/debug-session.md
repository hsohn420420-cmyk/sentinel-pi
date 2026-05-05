---
description: Debug a broken or stuck pi session
---
Debug the following issue: $ARGUMENTS

Debug protocol:
1. Reproduce: what exact command/action triggers the issue?
2. Isolate layer:
   - Provider error (HTTP status)?
   - Extension error (TypeScript/ESM)?
   - MCP error (server crashed)?
   - Config error (JSON syntax)?
   - Path/binary error?
3. Gather evidence (run relevant checks)
4. Identify root cause
5. Propose minimal fix with:
   - WHAT changes
   - WHY this fixes it
   - RISK of the change
   - VALIDATION command
6. Apply fix only after explaining it
7. Verify fix resolves the original issue
