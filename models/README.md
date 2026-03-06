# Models

This folder contains the trained ASL classification models created throughout the development of this project. Each model represents a different approach to improving real-time ASL letter prediction accuracy.

---

## asl_model1.pth

**Approach:** Grayscale CNN trained on Sign MNIST

**Training Data:** Sign MNIST train and validation CSV datasets (28x28 grayscale images, 1 channel)

**Architecture:** CNN using `MyConvBlock` layers with grayscale input (1 channel)

**Results:**
- Correctly predicted approximately 5 out of 24 letters the majority of the time
- No noticeable difference in performance between a plain background and a random/busy background
- Prediction was unstable — the letter would change rapidly even when the hand was held still

**Limitations:**
- Model was trained on clean, tightly cropped, plain background grayscale images which look very different from a real webcam feed
- The grayscale format caused a mismatch with the color webcam input, requiring conversion to grayscale before prediction which introduced additional error

---

## asl_model2.pth

**Approach:** Color CNN trained on Sign MNIST with duplicated RGB channels

**Training Data:** Sign MNIST train and validation CSV datasets, converted from 1 grayscale channel to 3 duplicate channels to simulate RGB input

**Architecture:** CNN using `MyConvBlock` layers with color input (3 channels)

**Results:**
- In progress

**Motivation:**
- By duplicating the grayscale channel 3 times and training the model to expect 3 channel input, the webcam feed can be passed directly to the model without converting to grayscale first
- This removes one source of error from the prediction pipeline

**Limitations:**
- The 3 channels are identical duplicates of the original grayscale data — no real color information is added
- Sign MNIST images are still very different from real webcam conditions regardless of channel count
- Significant accuracy improvement is unlikely without switching to a more realistic dataset or a landmark-based approach

**Results:**

Testing was performed at two distances and two background conditions.
Letters marked as **majority** were correctly predicted most of the time.
Letters marked as **close** were nearly correct but inconsistent.

| Condition | Majority Correct | Close |
|-----------|-----------------|-------|
| No background, 1 foot away | A, C, F, I, L, O, V, W | D, M, Q, Y |
| With background, 1 foot away | A, C, F, I, L, V, Y | D, O, Q |
| No background, 2 feet away | F, I, L, O, V, W, Y | A, Q |
| With background, 2 feet away | A, C, F, L, V, W, Y | O, Q |

**Observations:**
- Performance was fairly consistent between background and no background conditions, suggesting the color input did not significantly improve background robustness over asl_model1
- F, L, V and Y appeared consistently across almost all test conditions
- Some letters like D, M and Q were never majority correct but showed up as close in some conditions
- Distance had a relatively small effect on overall performance
- Total majority correct improved from approximately 5/24 (asl_model1) to approximately 8/24 letters (asl_model2)

---

## Notes

- J and Z are excluded from all models as they require motion to perform and cannot be captured in a single static frame
- All models predict from 24 possible ASL letters
- For a significant accuracy improvement, future models should consider training on landmark coordinate data collected directly from the webcam rather than pixel-based datasets