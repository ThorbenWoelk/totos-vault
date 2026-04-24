---
created: 2025-11-26
last_edited: 2026-04-24
tags:
- optimization
- gradient-descent
- learning-rate
- momentum
- adam
- loss-function
- deep-learning
- machine-learning
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[2025 Nested Learning - Behrouz et al]]'
- '[[Attention Optimization for LLMs]]'
ai_generated: false
human_approved: false
category:
- AI
---
In machine learning, we have a **loss function** that measures how wrong our model's predictions are. We want to minimize the loss function. 

## Gradient descend
It repeatedly takes steps in the direction of steepest descent until it reaches a minimum. 

## Learning rate
Controls step size.

## Momentum
Momentum adds "inertia" to gradient descent by accumulating a velocity vector in directions of persistent gradient.

## Adam
Combines momentum and adaptive learning rates. It's the most popular optimizer in deep learning.

**The intuition:** Use momentum to build velocity in consistent directions while simultaneously adapting learning rates per parameter based on gradient history. This creates a robust optimizer that works well across diverse problems with minimal tuning.

