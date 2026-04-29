---
created: 2025-12-16
last_edited: 2026-04-24
tags:
- deeplearning
- neural-networks
- deep-learning-ai
- activation-functions
- backpropagation
- gradient-descent
- supervised-learning
- logistic-regression
- loss-functions
connections:
- '[[Yann Le Cun]]'
- '[[Open Source Models with Hugging Face]]'
- '[[Connectionism]]'
ai_generated: false
human_approved: false
category:
- Notes
- DeepLearning.ai
tagging_processed_count: 1
---
# Deep Learning Specialization

### What are we even doing?

Neural networks are parameterized functions designed to predict an outcome $y$ (e.g. a category like "cat" or "dog") given some input $x$ (e.g. some image). For this, they linearly transform the input ($z(x)$) and then nonlinearly activate ($g(z$)) the result in each hidden and output layer. That is, the architecture of the neural net specifies a family of functions: repeated compositions of affine maps $z = wx + b$ and nonlinear activation functions $g(z)$. The linear part mixes and re-weights information; the nonlinearity prevents the whole construction from collapsing into a single linear map. Learning consists of gradually adjusting the parameters so that the selected function minimizes a loss on the training data, producing predictions that match the data well while still generalizing beyond it.

### Logistic Regression as a neuron in a neural network

A neuron in a neural network computes a weighted sum of its inputs and then applies an activation function. When the activation is a sigmoid (and the loss is binary cross‑entropy), the unit is exactly logistic regression—it outputs a probability that can be thresholded to 0 or 1 - the neuron fires or not. The softmax is the multi‑class generalisation of the sigmoid. 

A single neuron with a **sigmoid** activation function and **Binary Cross-Entropy (BCE)** loss is functionally identical to a logistic regression model. A neural network, in contrast to the logistic regression, adds additional layers that repeat the linear transformation and activation function ($z = \mathbf{w} \cdot \mathbf{x} + b$ and $a$ calculations)

The math:
- **Weighted Sum:** It computes $z = \mathbf{w} \cdot \mathbf{x} + b$ where $x$ is the input
- **Activation:** It passes $z$ through the sigmoid function: $a=\sigma(z) = \frac{1}{1 + e^{-z}}$.
- **Interpretation:** The output represents the probability $P(y=1 | x)$. Thresholding this at 0.5 effectively creates a linear decision boundary, just like in classic statistics.

In supervised learning we need a cost (loss) function that tells us how far the model’s predictions $\hat{y}$​ are from the true targets $y$.  
Two of the most common choices for regression problems are the L1 loss (also called _Mean Absolute Error_) and the L2 loss (also called _Mean Squared Error_).

### The neural network 

Neural network representation denotes a network with 1 hidden (activation) layer and 1 output layer a 2-layer network. 

![[Pasted image 20251219095823.png]]

### Activation functions

Different functions for activation $g(z)$ are actually working better than sigmoid. E.g. $\tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$, which is a shifted version of the sigmoid that goes through $(0, 0)$, is strictly better than $\sigma(x) = \frac{1}{1 + e^{-x}}$ for hidden layers. For output layer, $\sigma(x)$ works well as we want $\hat{y}$ between 0 and 1. Most people, however use faster ReLU with derivatives 0 up until $(0, 0)$, then 1. There's also leaky ReLU. 

### Forward propagation and backward propagation (gradient descent)

Think of a neural network as two coupled procedures running on the same object—the function $f_\theta(x)$.

**Forward propagation** is calculating status quo.
**Backward propagation** is learning/adapting.

Forward propagation means: given an input x and a current set of parameters $\theta = \{W_\ell, b_\ell\}$, compute the output step by step.

At layer $\ell$, you take the previous activations $a_{\ell-1}$, apply an affine map

$z_\ell = W_\ell a_{\ell-1} + b_\ell$, then apply a nonlinearity $a_\ell = g_\ell(z_\ell)$.

You do this repeatedly until you reach the output layer. The final activation is the prediction $\hat y$. That entire cascade is forward propagation. No learning happens here. You’re just asking, “What does the current function do to this input?”

Once you have $\hat y$, you compare it to the true target $y$ using a loss function $L(\hat y, y)$. Now comes the only part that matters for learning.

Backward propagation answers a single question:
**How should each parameter change to reduce the loss?**

Formally, it computes the gradient $\frac{\partial L}{\partial \theta}$, but doing this naively would be absurdly expensive. Backprop is an efficient application of the chain rule from calculus, exploiting the layered structure of the network. The key insight is that the loss depends on later layers, which depend on earlier layers. So you start at the end and work backward. At the output layer, you compute how sensitive the loss is to the output activations. Then you propagate that sensitivity backward through each layer, step by step, computing two things at each layer: 
- how the loss changes with respect to that layer’s pre-activation $z_\ell$, 
- how the loss changes with respect to that layer’s parameters $W_\ell, b_\ell$.

Each layer reuses gradients from the layer above it. Nothing is recomputed from scratch. That reuse is what makes backprop fast.

Once the gradients are known, an optimizer (like gradient descent) updates the parameters:

$\theta \leftarrow \theta - \eta \nabla_\theta L$,

where $\eta$ is the learning rate. That update step is not backprop itself—it’s what _uses_ the results of backprop.

(**General gradient descent rule**: 𝜃=𝜃−𝛼∂𝐽∂𝜃 where 𝛼 is the learning rate and 𝜃 represents a parameter.)

Forward propagation:
“Given these parameters, what output does the network produce?”

Backward propagation:
“Given this error, how much did each parameter contribute to it?”

# Glossary

ReLU = rectified linear unit = a function resting at 0 and then taking off linearly

