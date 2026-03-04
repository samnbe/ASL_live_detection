# Personal ASL Webcam Recognition Project

A real-time American Sign Language (ASL) hand landmark detection system built with Python, OpenCV, and MediaPipe. This project uses your webcam to detect and visualize hand landmarks as a foundation for ASL gesture recognition.

---

## Features

- Real-time webcam feed with hand landmark overlay
- Detects up to 2 hands simultaneously
- Draws 21 landmarks and skeletal connections per hand
- Mirrored display for natural interaction
- Graceful exit via `q` key or closing the window

---

## Requirements

- Python 3.8+
- JupyterLab or Jupyter Notebook
- Webcam

### Python Dependencies

```
opencv-python
mediapipe
```

Install them with:

```bash
pip install opencv-python mediapipe --user
```

---

## Project Structure

```
asl-webcam/
├── README.md
├── hand_landmarker.task       # MediaPipe pre-trained hand landmark model
└── asl_webcam.ipynb           # Main Jupyter notebook
```

---

## Setup

### 1. Clone or download the project

```bash
git clone https://github.com/your-username/asl-webcam.git
cd asl-webcam
```

### 2. Install dependencies

```bash
pip install opencv-python mediapipe --user
```

### 3. Download the MediaPipe hand landmark model

Download `hand_landmarker.task` and place it in the project root folder:

```
https://storage.googleapis.com/mediapipe-models/hand_landmarker/hand_landmarker/float16/latest/hand_landmarker.task
```

### 4. Launch Jupyter

```bash
python -m jupyter lab
```

---

## Usage

Open `asl_webcam.ipynb` and run the cells in order:

| Cell | Description |
|------|-------------|
| Cell 1 | Imports and opens the webcam |
| Cell 2 | `draw_landmarks_on_image` drawing function |
| Cell 3 | `landmarker_and_result` class — loads the MediaPipe model |
| Cell 4 | Main webcam loop — runs detection and displays the feed |
| Cell 5 | Cleanup — releases webcam and closes windows |

> **Tip:** If the loop crashes or you interrupt it, manually run Cell 5 to release the webcam.

To quit the webcam feed:
- Press `q` with the OpenCV window in focus, or
- Click the `X` button on the webcam window

---

## How It Works

1. Each video frame is captured from the webcam and mirrored
2. The frame is converted from BGR (OpenCV format) to RGB (MediaPipe format)
3. The frame is passed asynchronously to the MediaPipe hand landmarker
4. When landmarks are detected, 21 key points are mapped onto the hand
5. Connections between landmarks are drawn to form a skeletal overlay
6. The annotated frame is converted back to BGR and displayed

### Landmark Map

MediaPipe tracks 21 landmarks per hand:

```
Wrist: 0
Thumb:        1-4
Index finger: 5-8
Middle finger: 9-12
Ring finger:  13-16
Pinky:        17-20
```

---

## Roadmap

- [ ] ASL alphabet classification model
- [ ] Real-time letter prediction overlay
- [ ] Word/phrase prediction from letter sequences
- [ ] Dataset collection tool for custom training

---

## Known Issues

- On some systems, closing the OpenCV window with the `X` button may behave inconsistently — use `q` as a reliable fallback
- The first few frames may not show landmarks due to the async nature of MediaPipe's live stream mode
- `mediapipe.solutions` is deprecated in newer versions of MediaPipe (0.10+) — this project uses the newer `mediapipe.tasks` API

---

## References

- [MediaPipe Hand Landmarker Documentation](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)
- [MediaPipe Python Tasks API](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker/python)
- [OpenCV Documentation](https://docs.opencv.org/)
- [NVIDIA Deep Learning Institute — Fundamentals of Deep Learning](https://www.nvidia.com/en-us/training/instructor-led-workshops/fundamentals-of-deep-learning/)
- [Finger Counting in Real-Time Video with OpenCV and MediaPipe — Medium](https://medium.com/@oetalmage16/a-tutorial-on-finger-counting-in-real-time-video-in-python-with-opencv-and-mediapipe-114a988df46a)
