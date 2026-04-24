---
created: 2025-11-26
last_edited: 2026-04-24
tags:
- transformer
- attention
- multi-head-attention
- positional-encoding
- encoder-decoder
- machine-translation
- large-language-models
- nlp
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[2025 Nested Learning - Behrouz et al]]'
ai_generated: false
human_approved: false
category:
- AI
- Papers
---
The 2017 paper **“Attention Is All You Need”** by Vaswani et al. marks a turning point in machine learning. It introduces the **Transformer**, a new neural network architecture for transductive sequence modeling such as language translation. Before this paper, the dominant models were **Recurrent Neural Networks (RNNs)** and their improved version, **Long Short-Term Memory networks (LSTMs)**. These models process text sequentially, word by word, keeping track of what came before through an internal state. While effective, this sequential nature limits parallel computation and makes training slow, especially on long sentences.

The Transformer eliminates recurrence entirely and relies solely on **attention mechanisms** to model dependencies between words. Attention allows the model to look at all parts of a sentence simultaneously and decide how strongly each word should influence another. Formally, this is achieved using three components derived from each input vector: the **query (Q)**, the **key (K)**, and the **value (V)**. These are obtained through **learned linear projections**—that is, multiplying the input representation by learned weight matrices W_Q, W_K, W_V. The attention mechanism computes a similarity score between each query and key using their **dot product**, scales it by \sqrt{d_k} (the square root of the key’s dimension), and applies a **softmax** function to convert these scores into probabilities. These probabilities, called **attention weights**, are then used to take a weighted sum of the value vectors. The result is a new representation for each token that encodes its relationships to every other token in the sequence.

To strengthen expressivity, the Transformer uses **Multi-Head Attention**: several attention mechanisms run in parallel, each learning to focus on different kinds of relationships (e.g., syntactic or semantic). The outputs of these heads are concatenated and projected again. Every encoder and decoder layer also includes a **position-wise feed-forward network**—a small neural network applied independently to each token—and **residual connections**, which add the input of a layer back to its output to preserve information and stabilize training. **Layer normalization** further helps maintain consistent activation scales.

Because the Transformer lacks recurrence, it must encode word order explicitly. It does this through **positional encodings**—mathematical patterns (sine and cosine waves of varying frequencies) added to input embeddings so that the model can infer both absolute and relative positions of tokens. This design enables the Transformer to process all tokens in a sequence at once, dramatically increasing **parallelization** and reducing training time from weeks to hours on GPU hardware.

The architecture follows an **encoder–decoder** structure common in machine translation. The **encoder** reads the input sentence and produces contextualized representations. The **decoder** generates the output sentence one word at a time in an **auto-regressive** fashion, meaning each new word depends on the ones already generated. Within the decoder, **masked self-attention** prevents the model from “seeing the future” — it can only attend to previous output positions. Another attention layer, the **encoder–decoder attention**, lets the decoder attend to the encoder’s outputs, effectively aligning translated words with their sources.

The authors trained their model on large datasets, such as the **WMT 2014 English–German** and **English–French** corpora, and used the **Adam optimizer** (an adaptive version of stochastic gradient descent) with a custom learning-rate schedule that warms up and then decays. They also applied **dropout** (randomly deactivating neurons during training) and **label smoothing** (slightly softening target distributions) to improve generalization.

The Transformer achieved record-breaking **BLEU scores** (a standard metric for translation quality) while using a fraction of the computational cost of earlier models. Its combination of simplicity, scalability, and effectiveness made it the blueprint for virtually all modern **large language models (LLMs)**, including BERT, GPT, and T5. The paper’s central claim — that _attention alone is sufficient_ to capture global dependencies in sequences — reshaped the field of artificial intelligence.

