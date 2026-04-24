---
created: 2025-11-26
last_edited: 2026-04-24
tags:
- nested-learning
- catastrophic-forgetting
- continual-learning
- multi-timescale-optimization
- continuum-memory
- deep-momentum-gradient-descent
- large-language-models
- hierarchical-optimization
- self-modifying-architectures
- memory-modules
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[2017 Attention Is All You Need - Vaswani et al]]'
ai_generated: false
human_approved: false
category:
- AI
- Papers
---
Google introduces nested learning in 2025 to tackle catastrophic forgetting and anterograde amnesia (limitedness to immediate context) in LLMs. 

## The illusion

> We have been treating deep learning models as if their power came solely from stacking more layers, more parameters, and more data.

Because we constantly hear headlines like “Bigger models = Better intelligence,” the community has learned to equate **architectural depth** with **cognitive depth**. The paper calls this an  illusion since it obscures the **multi‑timescale, nested optimization** that actually drives performance.

NL understands deep learning models as a hierarchy of interconnected optimization problems. The consisting parts update in various intervals of speed (multi-timescale). This challenges the rigid dichotomy of “short-term memory” (like attention) and “long-term memory” (like weights of feed forward networks). Instead, it proposes a **Continuum Memory System (CMS)**, a chain of MLP blocks, each operating at a different frequency.

![[Pasted image 20251112095623.png]]

For instance, the paper shows that gradient descent with momentum is a two-level nested optimization process. The authors propose **Deep Momentum Gradient Descent (DMGD)**, where the linear memory of momentum is replaced by a multi-layer perceptron (MLP).

## Titans
A neural network that contains a **learnable long‑term memory (LLM) module** whose **update rule is itself a neural network**. During inference the model can **write into that memory** on the fly, rather than being frozen after pre‑training.

## Hope

The culmination of these ideas is **HOPE**, a new sequence architecture that combines a self-modifying mechanism with the Continuum Memory System.

