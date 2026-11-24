Stochastic Gradient Descent (SGD) is an iterative optimization algorithm used in machine learning to minimize a loss function by updating model parameters using one randomly selected training sample at each step, rather than computing gradients across the entire dataset.
How SGD Works
In standard (Batch) Gradient Descent, the model evaluates all training examples before making a single parameter update. On large datasets, this is slow and memory-intensive.
SGD solves this by shuffling the dataset each epoch and updating the parameters immediately after evaluating the loss gradient on a single sample.
| Variant | Samples per Update | Convergence Behavior | Primary Trade-off |
| :--- | :--- | :--- | :--- |
| **Batch GD** | All $N$ samples | Smooth, direct path to minimum | Too slow and memory-heavy for large datasets |
| **Stochastic GD (SGD)** | $1$ random sample | Noisy, zig-zag path toward minimum | Very fast updates, but high variance near the minimum |
| **Mini-Batch SGD** | $32$–$512$ samples | Stable path with mild noise | Best balance of GPU efficiency and convergence speed |
