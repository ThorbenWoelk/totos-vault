---
created: 2026-04-01
last_edited: 2026-04-24
tags:
- ai-security
- agent-risk
- data-leakage
- llm-vulnerability
- system
- prompt
connections:
- '[[Common AI Development Prompts]]'
- '[[001 Knowledge Base/AI/_index|AI Index]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
- Security
tagging_processed_count: 1
---
AI Agenten sind `eval()` auf Steroiden:

- **Access to your private data**—one of the most common purposes of tools in the first place!
- **Exposure to untrusted content**—any mechanism by which text (or images) controlled by a malicious attacker could become available to your LLM
- **The ability to externally communicate** in a way that could be used to steal your data (I often call this “exfiltration” but I’m not confident that term is widely understood.)

![[Pasted image 20260401122222.png]]

