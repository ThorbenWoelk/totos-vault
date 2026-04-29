---
created: 2025-12-27
last_edited: 2026-04-29
tags:
- alexnet
- cnn
- deep-learning
- image-classification
- relu
- dropout
- data-augmentation
- gpu
- lrn
- max-pooling
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[Attention Optimization for LLMs]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
- Papers
---
#### Design choices that were _new_ at the time

| Innovation                                                 | Why it mattered                                                                                                                                                                                                                                                                                                                           |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ReLU as activation function (`max(0,·)`)                   | Faster convergence than sigmoids/tanh; reduced vanishing‑gradient risk.                                                                                                                                                                                                                                                                   |
| GPU‑accelerated training                                   | First vision network to harness two GPUs in parallel, making the huge model feasible.                                                                                                                                                                                                                                                     |
| Dropout (prob 0.5) on fully connected (FC, *dense*) layers | Powerful regularizer against over‑fitting, especially with many parameters in the fully‑connected part of the NN. Works by dropping/masking some neurons of a previous layer in the forward-pass. This mask is drawn randomly for each mini-batch of inputs passing through the net, with a specific probability for each neuron to drop. |
| Data augmentation (random crops, flips, colour jitter)     | Generated virtually unlimited training examples → better generalisation on ImageNet.                                                                                                                                                                                                                                                      |
| Local Response Normalization (LRN)                         | At the time thought to help generalisation; later replaced by BatchNorm.                                                                                                                                                                                                                                                                  |
| Overlapping max‑pooling (3×3 stride 2)                     | Slightly smoother down‑sampling, improved accuracy.                                                                                                                                                                                                                                                                                       |

## References

- Krizhevsky, A., Sutskever, I., & Hinton, G. E. (2012). [ImageNet classification with deep convolutional neural networks](https://proceedings.neurips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf). Advances in Neural Information Processing Systems.
