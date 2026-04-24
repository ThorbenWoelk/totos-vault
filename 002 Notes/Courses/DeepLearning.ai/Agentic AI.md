---
created: 2025-11-26
last_edited: 2026-04-24
tags:
- agentic-ai
- multi-agent
- code-execution
- design-patterns
- evaluation
- prompting
- llm
connections:
- '[[Common AI Development Prompts]]'
- '[[How Transformer LLMs Work]]'
- '[[Deep Learning]]'
- '[[Open Source Models with Hugging Face]]'
ai_generated: false
human_approved: false
category:
- Notes
- DeepLearning.ai
---
[Agentic AI course](https://learn.deeplearning.ai/courses/agentic-ai/lesson/bvuwkr/agentic-ai-applications)

## General Takeaways

Andrew: "Let's use the term agentic for all LLM-based workflows and distinguish thm by autonomy". OK. 

The best way to learn prompting is to read good prompts in other people's software. 

## Design Patterns

**1. Reflection** 

*Example:*
ask_ai() -> regex extract code -> **run** (external feedback) -> evaluate_ai() -> update_code_ai() -> run ...


**2. Tool Use**
**3. Planning**
**4. Multi-agent Workflow** (*Du et al., 2023: ChatDev*)


## Evaluation
### Objective Evaluation
Code-based evaluation, e.g. create ground truth db to evaluate performance against.  


### Subjective Evaluation

LLMs have issues being used **as a judge**. The answer is often not very good and it experiences position bias (tendency for option A rather than option B, for instance). 

More consistent results can be achieved when giving it a **rubric** of binary criteria to score the results on (0/1 per category). 

![[Pasted image 20251126212006.png]]

## Design Decisions in AI systems

- planning with code execution: when to use hardcoded tools and when to let the AI generate code at runtime. Generally, the latter works great https://arxiv.org/abs/2402.01030

