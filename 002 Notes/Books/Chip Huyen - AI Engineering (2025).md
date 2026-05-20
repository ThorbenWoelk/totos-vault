---
created: 2026-05-12
last_edited: 2026-05-12
tags:
- book
- chip-huyen
- ai-engineering
- deep-learning
- transformers
- language-models
- generative-ai
- training
- history
connections: []
ai_generated: false
human_approved: false
category:
- 002 Notes
- Books
tagging_processed_count: 10
tagging_last_processed: 2026-05-15
---
## Basics

The basic unit of a language model is a *token*.

The set of all tokens the model can work with is the model's *vocabulary*. 

Language models are *self-supervised*: The text input brings its own training samples for "predict the next word". Labels can thus be inferred from the input data. 

### Types of Language Models
1. masked language model
	- uses before and after to predict missing token
	- e.g. BERT
	- used for non-generative tasks such as sentiment analysis
2. autoregressive language model
	- using preceding tokens to predict the next
	- e.g. chatgpt
	- used for text generation

## Training

### Pre-training
- Training for text completion
- Starts at randomly initialized model weights
- Most resource-intensitive

### Post-training and finetuning
- post-training when done by model developers
- tweak the model to the use case

## Timeline

### **The Deep Learning Boom (2012–2016)**

- **2012 | AlexNet**
    
    - **The Breakthrough:** Krizhevsky, Sutskever, and Hinton crush the ImageNet competition using deep convolutional neural networks (CNNs) trained on GPUs, officially kicking off the modern deep learning revolution.
        
- **2013 | Word2Vec & Deep Q-Networks (DQN)**
    
    - **The Breakthrough:** Google introduces Word2Vec, showing that neural networks can capture dense semantic relationships between words. DeepMind introduces DQN, mastering Atari 2600 games using raw pixels and reinforcement learning.
        
- **2014 | Generative Adversarial Networks (GANs)**
    
    - **The Breakthrough:** Ian Goodfellow pits two neural networks against each other (a generator and a discriminator), enabling AI to create highly realistic synthetic images.
        
- **2015 | ResNet**
    
    - **The Breakthrough:** Microsoft Research introduces Residual Networks (ResNet). By utilizing "skip connections," researchers successfully train networks over 100 layers deep without vanishing gradients, setting new standards in computer vision.
        
- **2016 | AlphaGo**
    
    - **The Breakthrough:** DeepMind’s AlphaGo defeats 18-time world champion Lee Sedol in the ancient game of Go—a feat experts thought was still decades away.
        

---

### **The Transformer Revolution (2017–2021)**

- **2017 | The Transformer Architecture**
    
    - **The Breakthrough:** Google researchers publish _"Attention Is All You Need"_, introducing the Transformer. By replacing recurrent layers with self-attention mechanisms, it allows massively parallel training on sequential data.
        
- **2018 | BERT & GPT-1**
    
    - **The Breakthrough:** OpenAI releases GPT-1 (proving the viability of generative pre-training), while Google releases BERT (bidirectional context understanding). NLP benchmarks are shattered across the board.
        
- **2020 | GPT-3 & AlphaFold 2**
    
    - **The Breakthrough:** OpenAI scales up to 175 billion parameters with GPT-3, demonstrating strong few-shot learning capabilities without task-specific fine-tuning. DeepMind's AlphaFold 2 solves the 50-year-old grand challenge of protein folding prediction.
        
- **2021 | CLIP & DALL-E**
    
    - **The Breakthrough:** OpenAI bridges the gap between text and vision. CLIP learns visual concepts from natural language supervision, laying the critical foundation for high-quality text-to-image generation.
        

---

### **The Generative AI Explosion (2022–Present)**

- **2022 | ChatGPT & Stable Diffusion**
    
    - **The Breakthrough:** Generative AI hits the mainstream. Midjourney and Stable Diffusion make high-fidelity image generation accessible to the masses. In November, OpenAI launches ChatGPT, driving the fastest consumer adoption of a tech product in history.
        
- **2023 | GPT-4 & The Open-Weight Movement**
    
    - **The Breakthrough:** Multimodal, highly advanced reasoning arrives with GPT-4 and Google's Gemini 1.0. Simultaneously, Meta releases Llama 2, triggering an explosion of powerful open-weight models and custom, local fine-tuning.
        
- **2024 | Massive Context Windows & Generative Video**
    
    - **The Breakthrough:** Context limits expand dramatically (e.g., Gemini 1.5 Pro processing over a million tokens, including full-length movies and massive codebases). High-fidelity AI video generation takes a massive leap forward with models like Sora and Veo.
        
- **2025–2026 | Agentic AI & Advanced Reasoning**
    
    - **The Breakthrough:** The focus shifts from pure pattern matching to native inference and reasoning (models that "think" and verify steps before responding) and autonomous AI agents capable of executing complex, multi-step workflows across web browsers, coding environments, and enterprise tools.