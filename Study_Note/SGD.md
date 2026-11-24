# Stochastic Gradient Descent (SGD)

**Stochastic Gradient Descent (SGD)** is a fast learning method that helps a machine learning model fix its mistakes step by step. Instead of studying the entire dataset before making one small improvement, SGD looks at just one random example at a time and updates the model right away.

---

### Everyday Analogy

Imagine you are standing on top of a mountain in thick fog and want to reach the lowest valley:

- **Standard Gradient Descent:** You measure the slope of the entire mountain before taking a single step. It is accurate, but way too slow.
- **SGD:** You quickly check the ground right under your feet and immediately take one step downhill. Your path wobbles a bit, but you start moving down the mountain right away.

---

### How SGD Works

1. **Shuffle the data:** Mix up all training examples so the model does not memorize the order.
2. **Pick one example:** Grab a single data point and let the model make a guess.
3. **Measure the mistake:** Compare the model's guess to the real answer (this mistake is called the loss).
4. **Adjust and repeat:** Slightly tweak the model's internal settings (weights) to make a better guess next time, then move to the next example.

---

### The Update Formula

$$
\theta = \theta - \eta \nabla_\theta J(\theta; x^{(i)}, y^{(i)})
$$

- **$\theta$ (Parameters):** The model's current weights and biases that we want to update.
- **$\eta$ (Learning Rate):** How big of a step we take. Too big and we overshoot the valley; too small and learning takes forever.
- **$\nabla_\theta J(\theta; x^{(i)}, y^{(i)})$ (Gradient):** The slope of the loss function $J$ calculated using just one training example $(x^{(i)}, y^{(i)})$.

---

### Comparing the 3 Types of Gradient Descent

| Method | Examples Checked per Step | Path Downhill | Best For |
| :--- | :--- | :--- | :--- |
| **Batch GD** | Every example in the dataset | Very smooth and straight | Small datasets only (too slow for big data) |
| **Stochastic GD (SGD)** | 1 random example | Bouncy and zig-zag | Very fast updates on huge datasets |
| **Mini-Batch SGD** | A small group (32 to 512) | Mostly smooth with tiny bounces | Most real-world deep learning (best balance) |

---

### Good Points & Weak Points

- **Starts learning immediately:** Does not wait to read millions of examples before improving.
- **Uses very little memory:** Only needs to hold one example (or a small batch) in memory at a time.
- **Can escape bad spots:** Because the path is slightly random and bouncy, it does not easily get stuck in shallow dead-ends.
- **Wobbles near the finish line:** Even when close to the best answer, it keeps bouncing around unless you slowly shrink the learning rate ($\eta$).

- Link to Study : Statquest SGD (https://www.youtube.com/watch?v=vMh0zPT0tLI&t=400s)
