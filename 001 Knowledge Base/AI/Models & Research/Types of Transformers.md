---
created: 2025-12-16
last_edited: 2026-04-29
tags:
- transformer
- encoder-only
- decoder-only
- bert
- gpt
- masked-language-modeling
- autoregressive
- text-diffusion
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[GPT-OSS]]'
- '[[2017 Attention Is All You Need - Vaswani et al]]'
ai_generated: false
human_approved: false
category:
- AI
---
## A Short History of Transformers

The original Transformer architecture, introduced in [2017](https://arxiv.org/abs/1706.03762), was an encoder-decoder model. In 2018, researchers realized that the encoder and decoder components of the model could be separated (with the advent of [BERT](https://arxiv.org/abs/1810.04805) and [GPT](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf)), and two distinct families of models were created:

1. **Encoder-only models (BERT-style, bidirectional)**

Encoder models used masked language modeling (MLM) as a training objective: randomly mask out a subset of tokens of each input and train the encoder to reconstruct the missing tokens (fill in the blanks). The model sees the entire (partially masked) context at once and learns bidirectional representations. This architecture excelled at tasks requiring a full‐sentence (or paragraph) representation (e.g., classification and retrieval).

2. **Decoder-only models (GPT-style, autoregressive)**

Decoder models used next‐token prediction as a training objective: at each position tt, predict the token at position t+1t+1 given all tokens up to tt as context. Only the left context is used to predict future values (unidirectional). This architecture excelled at generative tasks where you produce text one token at a time, such as open‐ended generation, summarization, and translation.

Originally, BERT saw immediate use in tasks such as classification, whereas GPT-style models didn’t become popular until later (due to initial limited capabilities). Eventually, the generation capabilities of autoregressive (decoder) transformers vastly improved. The general training objective of “next token prediction” means a much larger space of use cases when compared to encoder models.

# BERT is just a Single Text Diffusion Step
We’ve seen that masked language models like RoBERTa, originally designed for fill-in-the-blank tasks, can be repurposed into fully generative engines by interpreting variable-rate masking as a discrete diffusion process. By gradually corrupting text with `<MASK>` tokens and training the model to iteratively denoise at increasing mask intensities, we effectively turn the standard MLM objective into a step-by-step generation procedure.

Even without architectural changes, a fine-tuned RoBERTa can generate coherent looking text after slightly modifying the training objective, validating the idea that BERT-style models are essentially just text diffusion models trained on one masking rate.

## References

- Nathan. (n.d.). [BERT is just a single text diffusion step](https://nathan.rs/posts/roberta-diffusion/). nathan.rs.
