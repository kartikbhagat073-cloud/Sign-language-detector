# 🤟 Sign Language Detector

A real-time sign language recognition system built with Python, OpenCV, and a custom-trained Keras model. The project consists of two parts: a **data collection script** and a **live detection script** — enabling you to train and deploy your own hand gesture classifier.

---

## 🎯 Supported Gestures

| Label  | Gesture  |
|--------|----------|
| `hello` | 👋 Hello |
| `yes`   | ✊ Yes   |

> You can extend this by collecting more gesture data and retraining the model.

---

## 🚀 Features

- 📷 Real-time hand detection via webcam
- ✂️ Aspect-ratio-preserving hand crop with white background padding
- 🧠 Keras-based image classification for gesture recognition
- 🖼️ Live bounding box + label overlay on webcam feed
- 💾 Automated image saving for dataset creation
- Supports up to **2 hands** during data collection and **1 hand** during detection

---

## 🧰 Tech Stack

| Component        | Library / Tool                                   |
|------------------|--------------------------------------------------|
| Computer Vision  | [OpenCV](https://opencv.org/)                    |
| Hand Tracking    | [cvzone HandTrackingModule](https://github.com/cvzone/cvzone) |
| Classification   | [cvzone ClassificationModule](https://github.com/cvzone/cvzone) |
| Model Framework  | [Keras / TensorFlow](https://keras.io/)          |
| Math Utilities   | NumPy, Python `math`                             |
| Language         | Python 3.8+                                      |

---

## 📦 Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/sign-language-detector.git
cd sign-language-detector
```

### 2. Install dependencies

```bash
pip install opencv-python cvzone mediapipe tensorflow numpy
```

---

## 🗂️ Project Structure

```
sign-language-detector/
│
├── Data_collector.py          # Script to collect training images
├── Signlanguage_detector.py   # Script for real-time detection
│
├── Data/
│   ├── hello/                 # Collected images for "hello" gesture
│   └── yes/                   # Collected images for "yes" gesture
│
└── Model/
    ├── keras_model.h5         # Trained Keras classification model
    └── labels.txt             # Label names (must match order in labels list)
```

---

## 🔧 Usage

### Step 1 — Collect Training Data

Run `Data_collector.py` to capture hand gesture images for each label.

```bash
python Data_collector.py
```

- Point your hand at the webcam.
- Press **`S`** to save the current frame to the target folder.
- Press **`ESC`** to exit.

> ⚠️ Update the `folder` variable in the script to point to your desired save directory:
> ```python
> folder = "C:/path/to/your/data/yes"
> ```

Repeat for each gesture class (e.g., `hello`, `yes`).

---

### Step 2 — Train the Model

Use **[Teachable Machine by Google](https://teachablemachine.withgoogle.com/)** (recommended) or any Keras image classifier to train on your collected data. Export as:

- `keras_model.h5`
- `labels.txt`

---

### Step 3 — Run the Detector

```bash
python Signlanguage_detector.py
```

- The webcam feed opens with live hand tracking.
- Detected gesture label is displayed above the bounding box.
- Press **`ESC`** to exit.

> ⚠️ Update the model and label paths in the script:
> ```python
> classifier = Classifier(
>     "C:/path/to/keras_model.h5",
>     "C:/path/to/labels.txt"
> )
> ```

---

## 🧠 How It Works

```
Webcam Feed
    │
    ▼
Hand Detection (cvzone + MediaPipe)
    │
    ▼
Bounding Box Crop → Aspect-Ratio Resize → White Canvas (300×300)
    │
    ▼
Keras Model Prediction
    │
    ▼
Label Overlay on Live Feed
```

1. **Hand Detection** — MediaPipe-powered hand tracking locates the hand and returns a bounding box.
2. **Preprocessing** — The hand region is cropped, resized while preserving aspect ratio, and centered on a blank 300×300 white canvas.
3. **Classification** — The preprocessed image is fed into the trained Keras model via cvzone's `Classifier`.
4. **Display** — The predicted label is overlaid on the live webcam output.

---

## ⚙️ Configuration

| Parameter  | Default   | Description                            |
|------------|-----------|----------------------------------------|
| `imgSize`  | `300`     | Size of the white canvas (px)          |
| `offset`   | `20`      | Padding around the hand bounding box   |
| `maxHands` | `2` / `1` | Max hands tracked (collect / detect)   |

---

## ⚠️ Limitations

- Works best under **good lighting conditions**.
- Model accuracy depends on the **quantity and diversity** of collected training data.
- Currently supports only **static gestures** (no motion-based signs).
- Paths to model and data folders are **hardcoded** — consider using config files or CLI arguments for portability.

---

## 🛠️ Potential Improvements

- Add support for more gesture classes (letters A–Z, digits 0–9)
- Use a config file (`config.yaml`) for paths and parameters
- Build a Streamlit or Flask UI for non-technical users
- Add confidence score display on the live feed
- Support dynamic/motion gestures using LSTM or sequence models

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙌 Acknowledgements

- [cvzone by Murtaza Hassan](https://github.com/cvzone/cvzone)
- [MediaPipe by Google](https://mediapipe.dev/)
- [Teachable Machine by Google](https://teachablemachine.withgoogle.com/)
- [OpenCV](https://opencv.org/)
