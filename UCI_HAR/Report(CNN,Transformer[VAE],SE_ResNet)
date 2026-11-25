Architecture & Model Footprint

⚬ Parameter Count: 320,414 total parameters (1.22 MB) — 319,262 trainable and 1,152 non-trainable (from BatchNormalization).
⚬ Dominant Components: The Transformer Encoder block contains 149,120 parameters (46.5% of the entire model)—led by MultiHeadAttention (66,048 parameters) and its convolutional positional encoding (49,280 parameters)—followed closely by SE-ResNet Block 2 with 136,848 parameters (42.7%).

Training & Convergence

⚬ Best Checkpoint: Reached its lowest validation loss of 0.02491 at Epoch 72 (with validation accuracy peaking around 98.71%).
⚬ Stopping Point: EarlyStopping halted training at Epoch 87 (after ReduceLROnPlateau decayed the learning rate down to 3.13e-5) and restored the weights from Epoch 72.

Overall Test Performance (2,947 Samples / 9 Unseen Subjects)

⚬ Test Accuracy: 93.69% (2,761 correct predictions, 186 errors)
⚬ Test Loss: 0.4845
⚬ Macro & Weighted Metrics: Macro Precision 0.9372, Macro Recall 0.9378, Macro F1 0.9375, Weighted F1 0.9369.

Near-Perfect Dynamic & Horizontal Recognition

⚬ LAYING: 100% precision and 100% recall (537/537 correct, 1.0000 F1).
⚬ WALKING: 98.99% precision and 98.59% recall (489/496 correct, 0.9879 F1; 6 confused with WALKING_DOWNSTAIRS and 1 with WALKING_UPSTAIRS).
⚬ WALKING_DOWNSTAIRS: 94.47% precision and 97.62% recall (410/420 correct, 0.9602 F1; 9 confused with WALKING_UPSTAIRS and 1 with WALKING).
⚬ WALKING_UPSTAIRS: 96.96% precision and 94.90% recall (447/471 correct, 0.9592 F1; 18 confused with WALKING_DOWNSTAIRS, 4 with WALKING, and 2 with SITTING).

Primary Bottleneck (SITTING vs. STANDING)

⚬ Direct confusion between SITTING and STANDING caused 141 of the 186 total errors (75.8%).
⚬ SITTING had the lowest F1-score (0.8498, 416/491 correct), with 71 samples (14.46%) misclassified as STANDING and 4 as WALKING_UPSTAIRS.
⚬ STANDING scored 0.8676 F1 (462/532 correct), with 70 samples (13.16%) misclassified as SITTING.

The Root Cause

SITTING and STANDING data had near-zero linear acceleration and identical upright torso orientation, producing nearly identical sensor readings. Meanwhile, the gap between validation accuracy (98.71%, best validation loss 0.02491) and test accuracy (93.69%, 0.4845 test loss) reflects generalization across 9 completely unseen subjects. Combining unsupervised 1D Conv-VAE pre-training with SE-ResNet blocks (64 and 128 filters) and a Transformer Encoder (4 attention heads, 128 dimensions) proved to be an effective, lightweight architecture for this dataset—improving overall accuracy and SITTING/STANDING separation while using fewer parameters than BiLSTM-based models.
