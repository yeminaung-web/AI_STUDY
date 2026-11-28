# Study Note: Mamba (Selective State Space Models)

## 1. The Big Picture (Sticky-Note Analogy)

Imagine listening to a long lecture while summarizing everything on **one single sticky note** (the **hidden state**):

* **Transformers (Self-Attention):** Keep a transcript of every single word ever spoken and re-read the entire pile every time a new word is said. Very accurate, but slow and exhausting when the lecture gets long.
* **Conventional / Structured SSMs (e.g., S4):** Use one neat sticky note with a **strict timer rule** (e.g., *"Every 5 seconds, erase 10% of the note and write down 10% of what was just said"*). It is fast and organized, but blind to meaning—it writes down coughs and filler words just as much as exam hints.
* **Mamba (Selective SSM):** Uses the **same neat sticky note**, so it stays fast and lightweight, but now **listens to the meaning of the input**. When something important happens, it clears space and writes it down; when noise or filler happens, it ignores it and protects what is already written.

---

## 2. How Mamba Works (Step-by-Step Flow)

1. **Expand and Split:** The incoming data is widened into a larger internal workspace and split into two paths—a **main path** (which tracks sequence memory) and a **gating path** (which acts like a volume knob on the final output).
2. **Short Local Look:** Before updating memory, the main path runs through a tiny local filter so immediate neighboring steps are smoothed together.
3. **Selective Update (The Core Upgrade):** Mamba inspects the current input and dynamically sets its update controls:
   * **Important input:** Increases the step size to absorb the new signal strongly and overwrite stale information.
   * **Noisy or irrelevant input:** Shrinks the step size near zero so the new input passes by without ruining the existing memory.
4. **Output Gating:** The updated memory readout is multiplied by the gating path and compressed back to the original feature size.
5. **Fast Training vs. Fast Inference:**
   * **During training:** Even though the rules change at every step, Mamba uses a hardware-aware **parallel scan** inside fast on-chip GPU memory (SRAM) so it does not have to crawl step-by-step like an old RNN.
   * **During inference:** It runs one step at a time using only the fixed-size sticky note (hidden state).

---

## 3. Quick Comparison

| Model Family | How It Remembers | Speed on Long Sequences | Content-Aware Filtering |
| :--- | :--- | :--- | :--- |
| **Transformer** | Stores every past token (growing KV cache) | Slow (scales quadratically) | Very high (direct token-to-token lookup) |
| **RNN (LSTM / GRU)** | Fixed-size hidden state with nonlinear loops | Linear, but slow to train step-by-step | Moderate (via gates) |
| **Structured SSM (S4)** | Fixed-size state with a rigid, unchanging rule | Very fast (linear / FFT convolution) | Low (treats every step with the same rule) |
| **Mamba (Selective SSM)** | Fixed-size state with input-driven updates | Very fast (linear + parallel scan) | High (adapts updates to what the input says) |

---

## 4. Strengths and Limitations

### Strengths
* **Linear Scaling:** Handles very long sequences (long text, audio, dense sensor streams) without quadratic slowdown.
* **Constant Memory Per Step:** Inference memory stays flat regardless of how long the sequence gets.
* **Noise Filtering:** Selectivity naturally suppresses irrelevant filler or sensor jitter while retaining key events.

### Limitations
* **Fixed Sticky-Note Size:** Because all history must fit inside a fixed-size state, recalling exact tiny details from thousands of steps ago can still be harder than direct attention lookup.
* **Hardware Kernel Reliance:** Full training speed depends on custom parallel-scan GPU kernels rather than basic matrix multiplications alone.

Link to study : https://www.youtube.com/watch?v=N6Piou4oYx8&t=1088s
https://www.youtube.com/watch?v=qOyoHCyO6v8
