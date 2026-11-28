# Paper Summary: Contrastive Accelerometer-Gyroscope Embedding Model for HAR

**Paper:** *Contrastive Accelerometer-Gyroscope Embedding Model for Human Activity Recognition* (*IEEE Sensors Journal*, 2023)

---

### 1. Research Gap
Most multimodal deep learning models for sensor-based HAR rely on **data-level fusion**, concatenating raw signals from accelerometers and gyroscopes into a single multichannel input. However, accelerometers measure linear acceleration ($\text{m/s}^2$) while gyroscopes measure angular velocity ($\text{rad/s}$)—capturing distinct physical properties and signal patterns. Processing them together in a single shared feature extractor overlooks these modality-specific characteristics and prevents the encoders from effectively leveraging complementary cross-modal information.

### 2. Proposed Solution / Main Research Narrative
Inspired by CLIP, the authors propose **CAGE (Contrastive Accelerometer-Gyroscope Embedding)**, an end-to-end multimodal HAR system built on two core components:
* **Two-Stream Modality-Specific CNN (Feature-Level Fusion):** Separate 3-layer 1D CNN encoders independently extract $64\text{-D}$ feature vectors ($f^A$ and $f^G$) from triaxial accelerometer and gyroscope windows, which are then concatenated into a $128\text{-D}$ joint representation for the MLP classifier.
* **Auxiliary Cross-Modal Contrastive SSL Objective:** During training, $f^A$ and $f^G$ are projected onto a unit-sphere shared embedding space ($A_i, G_i$). A symmetric, temperature-scaled cross-entropy contrastive loss ($L_{\text{ssl}}$) maximizes cosine similarity for synchronized $(A_i, G_i)$ pairs from the same activity instance while minimizing similarity across mismatched pairs ($L_{\text{total}} = L_{\text{cls}} + \lambda L_{\text{ssl}}$). Because the projection heads are discarded at test time, CAGE improves representation quality with **zero added inference latency**.

### 3. Experimental Validation
The authors evaluate CAGE over 10 random initialization trials across four public benchmarks (*UCI-HAR*, *mHealth*, *PAMAP2*, and *MobiAct*) against supervised baselines (*CNN*, *DeepConvLSTM*, *LSTM-CNN*), self-supervised baselines (*CAE*, *Masked Reconstruction*, *CPC*), and two internal ablations (*One-stream* data-level fusion and *Two-stream* without SSL):
* **Benchmark Superiority:** CAGE outperforms prior supervised and SSL methods across datasets—achieving **$92.50\%$** accuracy ($0.9249$ weighted $F_1$) on *UCI-HAR*, **$75.48\%$** ($0.6842$ $F_1$) on *mHealth*, **$70.47\%$** ($0.6947$ $F_1$) on *PAMAP2*, and **$89.70\%$** ($0.8984$ $F_1$) on *MobiAct*, as well as consistent gains on additional cross-subject folds of *mHealth* and *PAMAP2*.
* **Ablation & Representation Analysis:** Two-stream feature-level fusion beats one-stream data-level fusion on multi-class datasets, and adding $L_{\text{ssl}}$ consistently improves two-stream performance across all four datasets and across varying batch sizes ($64$–$512$) and embedding dimensions ($32$–$256$). Training purely with $L_{\text{ssl}}$ (without labels) produces well-separated t-SNE clusters on *UCI-HAR* and boosts downstream MLP accuracy from **$75.26\%$** (raw signals) to **$81.43\%$**.

---

### In Short
* **Gap:** Early data-level fusion treats heterogeneous accelerometer and gyroscope signals identically, ignoring their distinct physical dynamics and cross-modal relationships.
* **Idea:** Extract modality-specific features via a **two-stream CNN** and align synchronized accelerometer-gyroscope pairs during training using an **auxiliary CLIP-style contrastive SSL loss** (discarded at inference).
* **Evidence:** Outperform supervised and SSL HAR baselines across *UCI-HAR*, *mHealth*, *PAMAP2*, and *MobiAct*, with ablations confirming gains from both feature-level fusion and cross-modal contrastive learning at zero inference cost.