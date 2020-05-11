### Project Write-up


#### 1. **Code Description**

*Include details so someone unfamiliar with your project will know how to run your code and use your algorithm.*

I have built an algorithm to predict heart rate measured in beats per minute (BPM). To estimate the BPM there are two kinds of signals the PPG signal (photoplethysmographic) measured by the two wrists (two signals) and IMU signals (x,y,z dimensions) from the Inertial Measurement unit (for better accuracy). Also, there's the "ECG" signal which is not yet used in this part of the project. In my code I used only the second PPG signal which is more noisy.

The code is available in the jupyter notebook and returns the mean absolute error and the confidence (of the final estimation) as a 2-tuple of numpy arrays.

It can be easily changed to also return the heart rate estimation for use in projects.

#### 2. **Data Description**

*Describe the dataset that was used to train and test the algorithm. Include its short-comings and what data would be required to build a more complete dataset.*

The Troika data set is used to build the algorithm. The training data is placed in the folder `nd320-c4-wearable-data-project-starter/part_1/datasets/troika/training_data` (https://arxiv.org/pdf/1409.5181.pdf)

Regarding the signals:

* ECG signal with one channel
* PPG two signals from each wrist (Only one is used)
* Three channels fro the accelerometer each one corresponding to (x,y and z).
* The data is 125Hz sampled.

#### 2. **Algorithhm Description**

##### How the algorithm works

The algorithm uses regression (`RandomForestRegressor`) to fit the heart rate training data.

##### The specific aspects of the physiology that it takes advantage of

We use the PPG signals from the two wrists. The capillaries in the wrist fill with blood when the heart's ventricles contract. When the blood returns to the heart there are fewer blood cells in the wrist. The PPG sensor emits a green light which can be absorbed by the red blood cells and the photodetector will see the various levels of blood flow in the reflected light. When the blood returns to the heart  fewer blood cells can absorb the green light and the photo detector can see an increase in the reflected light.

##### A description of the algorithm outputs

The algorithm returns the predicted heart rate (BPM) and the confidence rate of this prediction. More confidence rate means that we can trust the prediction more.

##### Caveats on algorithm outputs

The confidence rate is calculated based on the magnitude of a small area that contains the estimated spectral frequency relative to the sum magnitude of the entire spectrum.

##### Common failure modes

Sometimes the PPG picks a higher frequency that is not from the heart rate. This could be from the hand movements To deal with this problem, we take into consideration the accelerometer values.


#### 3. **Algorithm Performance**

*Detail how performance was computed (eg. using cross-validation or train-test split) and what metrics were optimized for. Include error metrics that would be relevant to users of your algorithm. Caveat your performance numbers by acknowledging how generalizable they may or may not be on different datasets.*

We calculated the mean absolute error of the heart rate estimation and the ground truth reference signal from the ECG sensors.

For cross validation I used the `LeaveOneGroupOut()` or `KFold` cross vilidation.

The error rate was around 10 BPM on the test set.  Since the data is using only limited subjects the algorithm may not be able to generalize well.




#### 4. **Clinical Conclusion**

1. For women, we see higher heart rate for 35 to 65
2. For men, we see a steady rates for 35 to 75
3. In comparison to men, women's heart rate has more variance
4. What are some possible reasons for what we see in our data?
  * The women sample is 4 times lower than of men (277 whereas men 1260). Also, we may have gotten the sample for a specific demographic (for example, office clerks).
5. What else can we do or go and find to figure out what is really happening? How would that improve the results?
  * Repeat the experiments with more data.
6. Did we validate the trend that average resting heart rate increases up until middle age and then decreases into old age? How?
  * No because the sample is small.

#### 5. **Test**

I was able to achieve lower than 10BMP heart error rate:

![pass](pass)
