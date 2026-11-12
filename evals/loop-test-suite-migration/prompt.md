---
tags: [ai-work-fit]
max_turns: 10
timeout_seconds: 300
allowed_tools: [Read, Glob, Grep, Skill]
---

We need to move about 200 internal Python scripts off an old in-house date library and onto the standard library. Every script has unit tests that pass today. Each one uses the old library a bit differently, so find-and-replace didn't work when we tried it. Is this a job for an agent or am I overcomplicating it?
