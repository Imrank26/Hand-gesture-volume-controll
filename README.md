
# Hand Gesture Volume Control (Computer Vision)

This project uses Python, OpenCV, and MediaPipe to control your system's visual volume bar using hand gestures. By measuring the distance between your thumb and index finger, the script calculates a volume percentage and displays a real-time visual feedback bar.

## 🚀 Features

* **Real-time Hand Tracking:** Uses `cvzone` and `MediaPipe` for high-accuracy landmark detection.
* **Dynamic Volume Scaling:** Maps physical finger distance to a 0–100% volume scale.
* **Visual Feedback:** Overlays a dynamic volume bar and percentage text directly onto the video feed.
* **Google Colab Compatible:** Includes custom JavaScript wrappers to access local webcams within a browser-based environment.

---

## 🛠️ Tools & Libraries

The project relies on the following key libraries:

| Library | Purpose |
| --- | --- |
| **OpenCV (`cv2`)** | Image processing and real-time video rendering. |
| **MediaPipe** | Google’s framework for high-fidelity hand and finger tracking. |
| **cvzone** | A high-level wrapper for MediaPipe that simplifies hand detection logic. |
| **NumPy** | Used for mathematical operations and linear interpolation (`np.interp`). |

---

## 💻 How to Run

### 1. Environment Setup

Since this notebook is designed for **Google Colab**, you do not need to install local drivers for your webcam. However, the libraries must be installed in a specific order to avoid version conflicts.

### 2. Installation Sequence

Run the following commands to ensure a clean and compatible environment:

```bash
# 1. Uninstall existing versions to avoid conflicts
!pip uninstall -y cvzone mediapipe

# 2. Install specific compatible version of Mediapipe
!pip install mediapipe==0.10.13

# 3. Install cvzone
!pip install cvzone

```

### 3. Execution

1. Open the `.ipynb` file in Google Colab.
2. Run the initialization cells to import the `HandDetector`.
3. Execute the main loop. A prompt will appear asking for webcam access.
4. **Gesture:** Bring your thumb and index finger together to lower the volume; move them apart to increase it.

---

## ⚠️ Expected Errors & Troubleshooting

### The `AttributeError` Conflict

The most common issue encountered in this project is:

> `AttributeError: module 'mediapipe' has no attribute 'solutions'`

**Why it happens:**
This error typically occurs when there is a version mismatch between `cvzone` and the latest `mediapipe` release (e.g., v0.10.31). The `cvzone` module expects certain attributes in `mediapipe.solutions` that may have been moved or renamed in newer builds.

**The Solution:**
As identified in the project logs, the error is resolved by **downgrading MediaPipe** to a stable, compatible version:

1. Completely uninstall `cvzone` and `mediapipe`.
2. Force install `mediapipe==0.10.13`.
3. Reinstall `cvzone`.

---

## 📝 Code Explanation (Logic Flow)

1. **Frame Capture:** The `capture_frame()` function uses JavaScript to bridge the browser's webcam feed to the Python environment.
2. **Hand Detection:** `detector.findHands(img)` identifies 21 unique landmarks on the hand.
3. **Distance Calculation:** * **Thumb Tip:** Landmark 4
* **Index Tip:** Landmark 8
* The distance is calculated using the Hypotenuse formula: .


4. **Interpolation:** * The raw pixel distance (ranging roughly from 30 to 250) is "mapped" to a volume range of 0–100 using `np.interp`.
5. **Rendering:** `cv2.rectangle` draws the volume bar, and `cv2.putText` displays the current FPS and volume percentage.

---

## 📈 Next Steps

* **System Integration:** Connect the `volPer` variable to the `pycaw` library to control actual PC system volume.
* **Smoothing:** Implement a moving average filter to reduce "flickering" in the volume bar levels.
* **Gesture Locks:** Add a "fist" gesture to lock the volume at a specific level.

