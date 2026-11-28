# Paper Summary: Human Activity Recognition via Temporal Fusion Contrastive Learning

**Paper:** *Human Activity Recognition via Temporal Fusion Contrastive Learning* (IEEE Access, 2024)

---

### 1. Research Gap
Supervised deep learning for sensor-based HAR requires large amounts of labeled time-series data, which is expensive, time-consuming for experts to annotate, and restricted by privacy concerns. While self-supervised learning (SSL) and contrastive learning methods have been introduced for HAR to leverage unlabeled sensor data, existing approaches rely solely on movement signals and ignore the wearer's **biometric information** (e.g., age, gender, height, weight)—preventing models from learning personalized, user-specific movement patterns.

### 2. Proposed Solution / Main Research Narrative
Instead of training contrastive models exclusively on raw IMU time-series signals, the paper proposes **Temporal Fusion Contrastive Learning (TFCL)** to fuse static biometric metadata with temporal sensor representations during self-supervised pretraining. Specifically, a **Static Encoder** (using variable selection and Gated Residual Networks, GRNs) extracts static context and enrichment vectors from biometric traits and injects them before and after a 3-block 1D-CNN **Observed Encoder** across two augmented views. A Transformer-based autoregressive model then generates context vectors trained jointly with **cross-view Predictive Contrastive Loss (InfoNCE)** and **Contextual Contrastive Loss (NT-Xent)**.

### 3. Experimental Validation
Across two public wearable datasets containing wearer biometric attributes (*DLR* and *MobiAct*), the experiments validate the framework across three downstream paradigms (fine-tuning, linear evaluation, and supervised learning from scratch):
* **Impact of Biometric Information:** Including wearer biometrics consistently outperforms excluding them across metrics and augmentation combinations (e.g., boosting DLR fine-tuning macro-$F_1$ from $0.9678$ to **$0.9791$**, and MobiAct linear evaluation $F_1$ from $0.5499$ to **$0.6575$**), with ablation studies confirming that combining all available biometric variables yields the strongest results.
* **Comparison with SSL Baselines:** TFCL substantially outperforms existing HAR contrastive baselines (*SimCLR on HAR*, *CSSHAR*, and *CPC on HAR*), achieving top macro-$F_1$ scores of **$0.9791$** on *DLR* ($+15.2\text{ p.p.}$ over baseline average) and **$0.7433$** on *MobiAct* ($+55.46\text{ p.p.}$ over baseline average).
* **Data Efficiency & Transfer Learning:** Under low-label regimes ($1\%\text{--}75\%$ labeled data) and cross-dataset transfer (*MobiAct* $\leftrightarrow$ *DLR*), TFCL pretraining matches or surpasses purely supervised training from scratch, proving effective representation learning with minimal labels.

---

### In Short
* **Gap:** Labeling HAR sensor data is costly/privacy-sensitive, and existing HAR SSL methods ignore wearer biometric traits needed for personalized motion modeling.
* **Idea:** Fuse static biometric features into a dual-view 1D-CNN + Transformer contrastive learning architecture (**TFCL**) via Gated Residual Networks and joint predictive/contextual contrastive losses.
* **Evidence:** Achieve state-of-the-art $F_1$ scores ($0.9791$ on DLR, $0.7433$ on MobiAct) over prior HAR SSL baselines and outperform supervised learning in low-label and transfer settings.