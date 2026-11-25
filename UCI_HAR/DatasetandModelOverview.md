Report for testing different model on UCI_HAR (Human Activity Recognition) Dataset

Dataset Preview
===========================
Dataset was collected from 30 volunteers wearing waist-mounted smartphone with embedded accelerometer and gyroscope. 
The dataset contain 10,299 total sample and separated 70% (7,352 samples) as training set and 30% (2,947 samples) as testing set. 
The data are recorded at 50Hz and segmented into fixed 2.56 second producing 128 timesteps per window. 
There are 9 signal channel body acceleration (body_acc x,y,z), angular velocity (body_gyro x,y,z) and total acceleration (total_acc x,y,z).
There are 6 activities, 3 dynamic activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS) and 3 static postures (SITTING, STANDING, LAYING) mapped from labels 1-6 but tensorflow/ keras use 0-based indexing, we had to convert into 0-5.

Preprocessing & Training
==============================

The training set (7,352 samples) also seperated into 80% training (5,881 samples) and 20% validation (1,471 samples) to detect overfitting during training because if we use 100% of the training data, the model will memorize every training examples but real-world performance on unseen data would perform poorly. 
Signals were reshaped to (samples * 128, 9) and scaler.fit() on 80% newly splited training dataset so the standard scaler can computes the mean and standard deviation using only training data. Then apply those training statistics on other data X_train, X_val and X_test using scaler.transform().
Another reason is EarlyStopping feature when the validation loss does not improve for 15 times, the best model will be chosen.
SEED = 42 to result same result whenever we test the model.
Models were trained for up to 100 epochs using the Adam optimizer and sparse_categorical_crossentropy because every data single integer indices (0-5) belongs to one of six exclusive activities.
