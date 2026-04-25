---
created: 2026-04-15
last_edited: 2026-04-24
tags:
  - ai-agents
  - ai-safety
  - multi-agent-systems
  - security
  - adversarial-attacks
  - prompt-injection
  - agent-vulnerabilities
  - google-deepmind
connections:
  - "[[AI Weaknesses]]"
  - "[[AI Agents - Engineering Guidelines]]"
  - "[[Agentic Memory]]"
  - "[[Levels of AI Autonomy]]"
ai_generated: true
human_approved: false
category:
  - AI
  - Papers
source: https://ssrn.com/abstract=6372438
doi: 10.2139/ssrn.6372438
---
# AI Agent Traps

**Authors:** Matija Franklin, Nenad Tomašev, Julian Jacobs, Joel Z. Leibo, Simon Osindero — Google DeepMind  
**Date:** March 8, 2026 | 25 pages  
**Keywords:** AI Agents, AI Agent Safety, Multi-Agent Systems, Security

---

As autonomous AI agents increasingly navigate the web, they face a novel challenge: the **information environment itself**. This paper introduces the first known systematic framework for understanding **AI Agent Traps** — adversarial content designed to manipulate, deceive, or exploit visiting agents. The framework is model- and agent-agnostic.

## The Core Threat Model

Agents browsing the web are uniquely vulnerable because they:
- Parse content programmatically (not visually like humans)
- Maintain long-term memory and learned policies that can be poisoned
- Take real-world actions (clicks, form submissions, API calls)
- Often operate with a human-in-the-loop who can themselves be manipulated

## 6 Trap Categories

### 1. Content Injection Traps
Exploit the **gap between human perception, machine parsing, and dynamic rendering**. What a human sees on a page and what an agent "reads" can differ — hidden text, steganographic instructions, rendering-dependent content.

### 2. Semantic Manipulation Traps
Corrupt an agent's **reasoning and internal verification processes**. Crafted content that leads the agent to draw false conclusions, validate incorrect assumptions, or reach adversary-desired outputs through seemingly coherent logic.

### 3. Cognitive State Traps
Target an agent's **long-term memory, knowledge bases, and learned behavioural policies**. Persistent poisoning attacks that degrade agent reliability across sessions, not just within a single interaction.

### 4. Behavioural Control Traps
**Hijack an agent's capabilities** to force unauthorised actions — e.g. making an agent exfiltrate data, submit forms, or take actions outside its sanctioned scope.

### 5. Systemic Traps
Use **agent-to-agent interaction** to create cascading systemic failure. In multi-agent systems, one compromised agent can propagate malicious instructions or corrupt shared state.

### 6. Human-in-the-Loop Traps
Exploit **cognitive biases of human overseers** rather than the agent itself — e.g. presenting outputs that manipulate the human into approving adversarial actions.

## Key Contributions

- First systematic taxonomy of adversarial threats targeting AI agents (not just models)
- Maps attack surface across the full agent ecosystem: perception, reasoning, memory, action, oversight
- Identifies **critical gaps in current defences** — most existing safety work focuses on model-level, not agent-level, threats
- Proposes a **research agenda** to secure the agent ecosystem end-to-end

## Why It Matters

The threat surface for agents is qualitatively different from the threat surface for chatbots. Agents act, remember, and coordinate — which means attacks can have **persistent, real-world consequences** beyond a single conversation. This paper is a foundational framing document for agent security research.

## Connections & Relatedness

- Closely related to prompt injection but significantly broader — covers memory, multi-agent dynamics, and human oversight
- Relevant to anyone building agentic systems that browse untrusted content (web agents, research agents, RPA)
- Pairs well with work on agent memory architectures ([[Agentic Memory]]) and autonomy levels ([[Levels of AI Autonomy]])

