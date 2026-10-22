---
tags: [ai-work-fit]
max_turns: 10
timeout_seconds: 300
allowed_tools: [Read, Glob, Grep, Skill]
---

My LangGraph agent keeps looping forever whenever the search tool throws a timeout. The traceback ends with 'httpx.ReadTimeout' and then the node retries immediately and it just spins. How do I fix it?
