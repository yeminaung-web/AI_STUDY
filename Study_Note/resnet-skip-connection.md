# ResNet / Skip Connection

**A Skip Connection (Residual Block)** is a plug-in shortcut path that carries the original input directly around one or more layers and adds it to the output. It lets deep networks grow dozens or hundreds of layers deep without losing the training signal.

---

### Everyday Analogy

Imagine passing a message down a long line of people who each whisper it to the next person. By the end, the message fades away. If you also hand an exact photocopy straight ahead every few people, the original message arrives intact even if middle steps make small errors.

---

### How It Works

1. **Main Path:** Input $x$ goes through stacked layers (such as convolutions) to learn a small adjustment $F(x)$.
2. **Skip Connection:** At the same time, an unchanged copy of $x$ skips directly over those layers.
3. **Addition:** The network adds the adjustment $F(x)$ and the original input $x$ together before moving forward.

---

### Formula

$$
y = F(x, \{W_i\}) + x
$$

If $x$ and $F(x)$ have different shapes or channel counts, multiply $x$ by a $1 \times 1$ projection matrix $W_s$ first so both tensors match in size:

$$
y = F(x, \{W_i\}) + W_s x
$$

- **$y$:** Final output leaving the residual block.
- **$x$:** Original input entering the block.
- **$F(x, \{W_i\})$:** The residual change learned by the stacked layers with weights $W_i$.
- **$W_s$:** Optional $1 \times 1$ layer used only when input and output dimensions do not match.

---

### Standard Layer Stack vs. Skip Connection

| Feature | Standard Layer Stack | With Skip Connection |
| :--- | :--- | :--- |
| **What Layers Learn** | Full output mapping from scratch | Only the small difference from input $x$ |
| **Gradient Flow** | Shrinks in deep networks (vanishing gradient) | Flows straight backward through the shortcut |
| **Extra Weights** | Standard layer weights | Zero extra weights when shapes match |

---

### Good Points & Weak Points

- **Prevents vanishing gradients:** Gives error signals a direct path back to early layers during backpropagation.
- **Easy to plug in:** Can be wrapped around almost any convolutional or linear block with minimal cost.
- **Still uses compute:** Adding skip connections solves training depth issues, but very deep stacks still need significant memory and time.
