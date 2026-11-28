# Numerical Walkthrough: RNN vs. SSM / Mamba

## Setup and Inputs

Assume a 1D memory state $h_t$ initialized at $h_0 = 0$, with the following input sequence:

* Step 1: $x_1 = 2.0$
* Step 2: $x_2 = 0.5$ (less relevant)
* Step 3: $x_3 = 3.0$ (important)

---

## 1. RNN-Style Update (Nonlinear Squishing Inside the Loop)

Suppose a simplified nonlinear recurrence updates as:

$$h_t = \tanh(0.8 \cdot h_{t-1} + 0.6 \cdot x_t)$$

* **Step 1 ($x_1 = 2.0$):**

$$h_1 = \tanh(0.8(0) + 0.6(2.0)) = \tanh(1.2) \approx 0.8337$$

* **Step 2 ($x_2 = 0.5$):**

$$h_2 = \tanh(0.8(0.8337) + 0.6(0.5)) = \tanh(0.6670 + 0.3000) = \tanh(0.9670) \approx 0.7471$$

* **Step 3 ($x_3 = 3.0$):**

$$h_3 = \tanh(0.8(0.7471) + 0.6(3.0)) = \tanh(0.5977 + 1.8000) = \tanh(2.3977) \approx 0.9836$$

**What happens here:** Every step passes through $\tanh$, which squishes values toward $1.0$. Even though $x_3 = 3.0$ is six times larger than $x_2 = 0.5$, the state only reaches $\approx 0.9836$, and $h_3$ depends directly on the squished state $h_2$.

---

## 2. SSM / Mamba-Style Update (Linear Scaling Inside the Loop)

Suppose a discrete linear state update is defined as:

$$h_t = \overline{a}_t h_{t-1} + \overline{b}_t x_t$$

where:

* $\overline{a}_t = 0.8$ (retains **80%** of the previous state, decays **20%**)
* $\overline{b}_t$ is input-dependent (selective):
  * Less relevant input (Step 2, $x_2 = 0.5$): $\overline{b}_2 = 0.2$ (small write)
  * Important inputs (Step 1 and Step 3): $\overline{b}_1 = 0.6$ and $\overline{b}_3 = 0.8$ (larger writes)

* **Step 1 ($x_1 = 2.0$, $\overline{b}_1 = 0.6$):**

$$h_1 = 0.8(0) + 0.6(2.0) = 1.200$$

* **Step 2 ($x_2 = 0.5$, $\overline{b}_2 = 0.2$):**

$$h_2 = 0.8(1.200) + 0.2(0.5) = 0.960 + 0.100 = 1.060$$

* **Step 3 ($x_3 = 3.0$, $\overline{b}_3 = 0.8$):**

$$h_3 = 0.8(1.060) + 0.8(3.0) = 0.848 + 2.400 = 3.248$$

**What happens here:**

1. **No squishing curve:** $h_3$ grows cleanly to $3.248$ without saturating near $1.0$.
2. **Selectivity via input-dependent $\overline{b}_t$ (and $\Delta_t$):** At Step 2, a smaller $\overline{b}_2 = 0.2$ limits how much $x_2$ writes into state, whereas at Step 3, a larger $\overline{b}_3 = 0.8$ lets $x_3$ update the state strongly.
3. **Parallelizability:** Because there is no nonlinear activation wrapping $h_{t-1}$, $h_3$ can be written directly as a linear combination of inputs:

$$h_3 = \overline{a}_3 \overline{a}_2 \overline{b}_1 x_1 + \overline{a}_3 \overline{b}_2 x_2 + \overline{b}_3 x_3$$

Associative prefix-scan algorithms compute these cumulative linear products in parallel across the sequence during training.
