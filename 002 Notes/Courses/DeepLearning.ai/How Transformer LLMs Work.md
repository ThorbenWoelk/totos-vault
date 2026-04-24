---
created: 2025-12-27
last_edited: 2026-04-24
tags:
- transformer
- llm
- attention
- tokenization
- sampling
- decoding
connections:
- '[[Neural Network Embeddings Process]]'
- '[[Agentic AI]]'
- '[[GPT-OSS]]'
ai_generated: false
human_approved: false
category:
- Notes
- DeepLearning.ai
tagging_processed_count: 1
tagging_last_processed: 2026-02-19
---
[DeepL.ai course](https://learn.deeplearning.ai/courses/how-transformer-llms-work/lesson/ch1aa/understanding-language-models%3A-(word)-embeddings)

## From Neural Networks to Transformers

In contrast to the simple "bag-of-words" (counting of words that are in/ are not in a text), neural networks (NNs) can be used to encode text instead. 

**Word2Vec** 
- capture meaning of words in the form of vector embeddings through NNs.
- predict words that are close neighbors
- learn properties of tokens on `N`dimensions
- static word embeddings -> regardless of context

![[Pasted image 20251227211352.png|200x200]]

**RNNs**
- encoding (representing) and decoding (generating)
- autoregressive (generated one at a time)

![[Pasted image 20251228121438.png|400x200]]

**Attention**
- **Attention** in decoder block allows to focus on part of the input that's relevant for the specific part (instead of entire input)

**Transformers**

![[Pasted image 20251228200041.png]]

- Encoder-decoder architecture
- no RNN, instead stacked transformer encoder and decoder blocks
- parallel training possible
- encoder
	- instead of Word2Vec start with random values for embeddings
	- then use self-attention (attention only focusing on input) to update embeddings
	- then FFN
- decoder
	- masked self-attention (masks future positions)
	- encoder attention
	- FFN


![[Pasted image 20251228122433.png|500x300]]

**BERT**
- Bidirectional Encoder Representations
- encoder-only model
- randomly masking words, then fill gaps
- pre-training + fine-tuning

**Generative models**
- randomly initialized embeddings
- decoder-only (masked self-attention + FFN) stacks

## Tokenization

- Use pretrained bert tokenizers to create tokens from words (or words from tokens). Tokenizers have a preset vocabulary that determines how to split text. 
- CLS token and SEP token represent start and end of input (e.g. in BERT/encoder tokenizers)

## Self-Attention

1. relevance scoring
	1. Attention heads with projection matrices (query, key, value). Queries x Keys (relevance scores) for each previous token (not the one currently processed).
	2. relevance scores x values = weighted values
2. combining information from attention heads

**Efficiency for Self-Attention**
- Multi-Query attention -> Attention heads share keys and values projection matrices. 
- Grouped Query attention -> n_groups of attention heads share same key value matrices respectively
- Sparse (strided or fixed) attention let's later layers only attend to most recent tokens
- Ring-attention
- **KV caching**: The transformer doesn't actually recompute everything from scratch for each new token. When processing the input and generating tokens, it caches the intermediate calculations (specifically the "keys" and "values" in the attention mechanism) for all previous tokens. When generating token N+1, it only needs to compute the new attention calculations for that single token against the cached values. This dramatically reduces redundant computation.

## Sampling 

The sampling happens **outside the transformer architecture itself**.

The transformer produces a vector of numbers called **logits**—one number for each possible word (or token) in the model's vocabulary. These logits represent unnormalized scores for how likely each token is to come next.

These logits then pass through a **softmax function**, which converts them into a probability distribution—numbers between 0 and 1 that sum to 1 across all possible tokens.

The **sampling step** happens after this, using those probabilities. This is typically handled by:

- The **inference engine** or **decoding algorithm** that sits on top of the model
- Different sampling strategies can be applied: greedy decoding (always pick the highest probability), temperature sampling (adjust the probability distribution), top-k sampling (sample only from the k most likely tokens), or nucleus sampling (sample from the smallest set of tokens whose cumulative probability exceeds a threshold)

So the transformer's job ends at producing logits. The conversion to probabilities and the selection of which token to actually output is handled by external logic—it's a post-processing step rather than part of the neural network itself.

## 2017 vs 2024

![[Pasted image 20251228203846.png]]
# Intuition

A large language model is a machine that stores information about language and the world encoded in numerical patterns. When you provide text as input, the model predicts how that text would most naturally continue.

The process works through several steps:

**Encoding the input**: The model converts your text into vectors—sequences of numbers where each word or word fragment gets its own numerical representation. These numbers capture semantic properties, so words with similar meanings have similar numerical patterns.

**Building context**: The model processes the input through multiple transformer blocks. Each block applies two transformations in sequence:
- An attention mechanism that calculates relationships between all positions in the input. Each word's representation gets updated based on which other words appear nearby and how they typically relate to each other. This lets the model understand that "bank" means something different in "river bank" versus "savings bank."
- A feedforward network (FFN) that takes the attention-updated representation at each position and transforms it further through learned non-linear patterns

**Predicting what comes next**: Using these context-aware representations, the model's neural network (LM Head) computes probabilities (logits) for what word should follow. It examines the patterns it learned during training—billions of examples of how humans actually write—and identifies which continuation would be most statistically consistent with both the immediate context and broader patterns of language use.

**Generating output**: The model samples from these probabilities to select the next word, adds it to the sequence, and repeats the process. Each new word becomes part of the context for predicting subsequent words.

The model's knowledge comes from statistical patterns absorbed during training, where it processed vast amounts of text and adjusted its internal numbers to better predict how sentences continue. What appears as understanding or reasoning emerges from the model finding and applying these learned patterns to new inputs.

