# Backpropagation

How a neural network actually learns from its mistakes. When the model makes a bad guess, backprop goes **backward** from the output layer to the input layer to figure out which weights messed up and by how much.

### The flow
1. **Forward pass:** Push input data through the layers to get a prediction.
2. **Loss:** Check how far off the prediction is from the real answer.
3. **Backward pass (Backprop):** Work backward using the chain rule from calculus to calculate the gradient (blame) for every weight.
4. **Update:** Pass those gradients to an optimizer like SGD to tweak the weights.

Basically just the chain rule over and over:
`total gradient = (how wrong the output was) * (activation derivative) * (layer input)`

### Good to know
- Saves intermediate values during the forward pass so it doesn't have to recalculate everything from scratch (makes training way faster).
- **Vanishing gradient:** If the network is super deep and uses Sigmoid/Tanh, multiplying tiny decimals together shrinks the gradient to almost 0, so early layers stop learning. (Using ReLU mostly fixes this).
- **Exploding gradient:** Opposite problem—big numbers multiply into massive gradients and break training. Fix with gradient clipping.
