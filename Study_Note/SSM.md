# Study Note: State Space Models (SSMs)

## 1. Core Idea and Intuition

A **State Space Model (SSM)** is a sequence modeling framework that processes incoming data step by step while maintaining a compact internal summary called the **hidden state**. You can think of the hidden state as a running notepad that records relevant context from earlier steps and updates as new inputs arrive.

Originally rooted in control theory and signal processing, SSMs describe how a continuous or discrete signal evolves over time through an internal state and produces an output. In deep learning, structured and selective SSMs (such as S4 and Mamba) serve as alternatives or complements to Recurrent Neural Networks (RNNs) and Transformer self-attention for handling long sequences, audio, time series, and sensor data.

---

## 2. How SSMs Work (Conceptual Flow)

1. **Input Representation:** At each step in a sequence, an input item enters the model.
2. **State Update:** The model blends information from its previous hidden state with the new input to form an updated hidden state.
3. **Output Projection:** The updated hidden state is read out to produce the output for that step.
4. **Step Size / Selection (in Selective SSMs):** Models like Mamba adjust how strongly new inputs are absorbed or how much past state is retained based on the input content itself, rather than keeping all dynamics completely fixed across time.

---

## 3. Comparing SSMs, RNNs, and Self-Attention

| Aspect | RNNs (LSTM / GRU) | Standard / Structured SSMs (e.g., S4) | Selective SSMs (e.g., Mamba) | Self-Attention (Transformers) |
| :--- | :--- | :--- | :--- | :--- |
| How context is stored | Fixed-size hidden state | Fixed-size latent state | Fixed-size latent state | Growing key-value (KV) cache |
| Parameter behavior over time | Recurrent and input-driven | Time-invariant (fixed dynamics) | Input-dependent (content-aware) | Dynamic token-to-token weights |
| Training style | Sequential step-by-step | Parallel via convolution / FFT or scan | Parallel via hardware-aware scan | Parallel across all token pairs |
| Inference memory per step | Constant size | Constant size | Constant size | Grows with sequence length |
| Long-context behavior | Can struggle with long-range decay | Strong on smooth/long patterns, less selective | Strong on long sequences with selectivity | Strong direct token lookup, costly on long inputs |

### Key Differences from RNNs
* **Internal Memory Dynamics:** RNNs apply nonlinear activation functions (such as squashing curves) and gating inside the recurrent step, which requires stepping through time sequentially during training. SSMs keep the state transition linear with respect to the previous state, enabling parallel computation across sequence positions.
* **Training Efficiency:** Because RNN training is inherently sequential along the time axis, long sequences can be slow to train. SSMs can compute sequence outputs in parallel during training while still operating step by step during inference.
* **Shared Step-by-Step Inference:** Like RNNs, SSMs maintain a fixed-size state during autoregressive or streaming inference, keeping per-step memory usage steady regardless of sequence length.

---

## 4. Advantages of SSMs

* **Linear Scaling with Sequence Length:** Training and processing scale linearly with sequence length rather than quadratically like full self-attention, making very long sequences much more practical.
* **Constant Per-Step Inference Memory:** Because the model summarizes history into a fixed-size state instead of storing every past token, memory usage does not keep expanding as sequences grow longer.
* **Strong Fit for Continuous and Time-Series Signals:** Their continuous-time background makes them naturally suited for sensor streams, physiological signals, audio, and time-series data.
* **Content-Aware Selectivity (in Selective Variants):** Input-dependent parameterization allows the model to focus on relevant signals and ignore noise or irrelevant steps.

---

## 5. Limitations and Practical Trade-Offs

* **Fixed State Capacity:** Compressing an entire history into a fixed-size state means fine-grained details from very early steps can be diluted or overwritten if a task requires exact recall of many distant items.
* **Less Direct Token-to-Token Lookup Than Attention:** Transformers can directly attend to any earlier position, whereas SSMs must route information through their latent state.
* **Domain and Task Sensitivity:** Performance depends on state size, discretization choices, and whether a task favors state-space compression, local convolution, or explicit attention.

---

## 6. When SSMs Are Used in Practice

* **Long-Context Sequence Modeling:** Processing long documents, logs, or multi-step trajectories where quadratic attention memory or compute becomes a bottleneck.
* **Time-Series and Wearable Sensor Data:** Modeling continuous or sampled streams where temporal trends and noise filtering matter.
* **Hybrid Architectures:** Combining SSM layers with attention or feedforward/convolutional blocks to get both linear sequence efficiency and strong local or exact retrieval capabilities.

* Link to study: https://www.youtube.com/watch?v=HbZD0XoN5fc
