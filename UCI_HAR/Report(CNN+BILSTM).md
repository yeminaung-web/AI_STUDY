Architecture & Model Footprint
=================================

Parameter Count: 324,806 total parameters (1.24 MB) 324,422 trainable and 384 non-trainable (from BatchNormalization).
Dominant Layer: The Bidirectional(LSTM) layer contains 263,168 parameters (81.0% of the entire model), double the 131,584 parameters of a unidirectional LSTM(128).

Training & Convergence
=================================

Best Checkpoint: Reached its lowest validation loss of 0.03982 (97.89% validation accuracy) at Epoch 64.
Stopping Point: EarlyStopping halted training at Epoch 79 (after ReduceLROnPlateau decayed the learning rate down to 7.81e-6) and restored the weights from Epoch 64.

Overall Test Performance (2,947 Samples/9 Unseen Subjects)
================================================================

Test Accuracy: 92.67% (2,731 correct predictions, 216 errors)
Test Loss: 0.4537
Macro & Weighted Metrics: Macro Precision 0.9271, Macro Recall 0.9283, Macro F1 0.9268, Weighted F1 0.9257.

Near-Perfect Dynamic & Horizontal Recognition
=====================================================

LAYING: 100% recall (537/537 correct, 0.9963 F1).
WALKING_DOWNSTAIRS: 100% recall (420/420 correct, 0.9633 F1).
WALKING_UPSTAIRS: 98.30% recall (463/471 correct, 0.9706 F1; 8 confused with WALKING_DOWNSTAIRS).
WALKING: 100% precision and 95.36% recall (473/496 correct, 0.9763 F1; 23 confused with WALKING_DOWNSTAIRS).

Primary Bottleneck (SITTING vs. STANDING)
==============================================

Direct confusion between SITTING and STANDING caused 160 of the 216 total errors (74.1%).
SITTING had the lowest F1-score (0.8000, 370/491 correct), with 96 samples (19.55%) misclassified as STANDING.
STANDING scored 0.8540 F1 (468/532 correct), with 64 samples (12.03%) misclassified as SITTING.

The Root Cause
====================
SITTING and STANDING data had near-zero linear acceleration which produce nearly identical sensor readings. But the validation accuracy (97,89%) and test accuracy (92.67%) with 0.4537 test loss calculate from testing 9 completely unseen data. So, combining CNN with BILSTM layer 128 unit (forward and backward passes into 256 features) proved to be an effective model for this dataset.
