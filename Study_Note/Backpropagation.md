# Backpropagation

**Backpropagation** (backward propagation of errors) is the core algorithm used to train artificial neural networks. It efficiently computes the gradient of the loss function with respect to every weight and bias in the network by applying the **chain rule of calculus** from the output layer back to the input layer.

---

### How Backpropagation Works

Training a neural network repeats four steps on every batch of data:

1. **Forward Pass:** Input data passes layer by layer through the network to compute intermediate activations and a final prediction $\hat{y}$.
2. **Loss Calculation:** A loss function $L(\hat{y}, y)$ measures how far the prediction $\hat{y}$ is from the true label $y$.
3. **Backward Pass:** Using the chain rule, the algorithm propagates the error backward from the output layer to compute partial derivatives ($\frac{\partial L}{\partial w}$) for every weight $w$.
4. **Parameter Update:** An optimization algorithm (such as **SGD** or **Adam**) uses those gradients to update the weights and reduce the loss.

---

### The Chain Rule Formula

For a weight $w$ connected to a neuron with pre-activation $z$ and output $\hat{y}$, backpropagation breaks the gradient into a product of local derivatives:

$$
\frac{\partial L}{\partial w} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial z} \cdot \frac{\partial z}{\partial w}
$$

- **$\frac{\partial L}{\partial \hat{y}}$:** How fast the loss changes as the prediction changes.
- **$\frac{\partial \hat{y}}{\partial z}$:** The derivative of the activation function (such as ReLU or Sigmoid).
- **$\frac{\partial z}{\partial w}$:** How the weighted sum changes with respect to the weight $w$ (equal to the layer's input).

---

### Forward Pass vs. Backward Pass

| Aspect | Forward Pass | Backward Pass (Backpropagation) |
| :--- | :--- | :--- |
| **Direction** | Input layer $\rightarrow$ Output layer | Output layer $\rightarrow$ Input layer |
| **Inputs** | Raw features and current weights | Loss value and cached forward activations |
| **Outputs** | Predicted values and loss score | Gradients ($\nabla_w L$) for every parameter |
| **Purpose** | Make predictions and evaluate error | Determine how to adjust weights to lower error |

---

### Advantages and Limitations

- **High computational efficiency:** By caching intermediate values during the forward pass (dynamic programming), backpropagation computes all gradients in a single backward sweep rather than recalculating derivatives from scratch for each weight.
- **Scales to deep architectures:** Works uniformly across arbitrarily deep networks, CNNs, RNNs, and Transformers.
- **Vanishing gradients:** In very deep networks using Sigmoid or Tanh activations, multiplying many small derivatives shrinks gradients toward zero, stalling early layers.
- **Exploding gradients:** Large weights can cause gradients to grow exponentially during backward passes, leading to unstable updates unless mitigated with gradient clipping or normalization.
