# Drone-Based Vehicle Detection and Tracking

A computer vision project that uses **YOLO11n and ByteTrack** to detect and track vehicles in drone footage.

The project explores how object detection models can be adapted to aerial imagery through fine-tuning, how inference settings affect detection results, and how multiple objects can be tracked across video frames.
<img width="743" height="417" alt="image" src="https://github.com/user-attachments/assets/4e072893-8bbf-442f-b70c-b369e13051d2" />


## Overview

Detecting vehicles in drone footage presents different challenges compared to ordinary street-level object detection. Vehicles may appear small, crowded, or from different viewing angles because the camera is positioned above the scene.

In this project, I built an experimental vehicle detection and tracking pipeline using the Ultralytics YOLO framework.

The workflow includes:

1. Fine-tuning a pretrained YOLO11n model on the VisDrone dataset.
2. Running vehicle detection on drone footage.
3. Experimenting with confidence thresholds and input image resolutions.
4. Integrating ByteTrack to track detected vehicles across video frames.
5. Analysing tracking IDs based on how frequently they appear in the video.

## Technologies Used

- **Python** – Primary programming language.
- **YOLO11n (Ultralytics)** – Object detection model.
- **VisDrone** – Dataset used for fine-tuning.
- **ByteTrack** – Multi-object tracking algorithm.
- **OpenCV** – Image and video processing.
- **PyTorch** – Deep learning framework.
- **Google Colab** – Model training and GPU-based inference.

## Project Workflow

### 1. Model Fine-Tuning

Started with the pretrained YOLO11n model and fine-tuned it using the VisDrone dataset.

The model was adapted to detect vehicles in aerial images.

The vehicle categories selected for inference were:

| Class | Vehicle |
|---|---|
| 3 | Car |
| 4 | Van |
| 5 | Truck |
| 8 | Bus |
| 9 | Motor |

### 2. Vehicle Detection

Loaded the fine-tuned model and used it to perform inference on a drone video.

For every processed frame, YOLO generates detection results containing information such as:

- Bounding box coordinates.
- Predicted object class.
- Confidence score.

The confidence threshold determines the minimum confidence required for a detection to be retained.

### 3. Inference Experiments

Experimented with different confidence thresholds and image resolutions to understand how they affect detection output.

The configurations tested were:

| Confidence | Image Size |
|---|---:|
| 0.3 | 640 |
| 0.1 | 640 |
| 0.1 | 1280 |
| 0.1 | 1920 |

The number of detected bounding boxes increased as the confidence threshold was lowered and the image resolution was increased.

This highlighted an important distinction: **a higher number of detections does not necessarily mean better detection accuracy.** Detection quality also needs to be evaluated using appropriate metrics and visual inspection.

### 4. Multi-Object Tracking with ByteTrack

Integrated ByteTrack with YOLO to associate detected objects across video frames.

Unlike object detection, which identifies objects independently in each frame, tracking attempts to maintain an identity for an object as it moves through the video.

Each tracked object is assigned a tracking ID.

The tracking configuration used:

- ByteTrack tracker configuration (`bytetrack.yaml`).
- Persistent tracking IDs across frames.
- GPU-based inference.
- Video-based processing.

### 5. Tracking Analysis

After tracking, analysed how frequently each tracking ID appeared across the processed frames.

Used Python dictionaries and counters to:

- Count the number of frames in which each tracking ID was detected.
- Associate tracking IDs with predicted vehicle classes.
- Identify IDs appearing in at least 15 frames.
- Analyse the distribution of tracked vehicle classes.

This provides a way to examine tracking persistence throughout the video.

**Note:** A tracking ID is not guaranteed to represent one unique physical vehicle throughout the entire video. Tracking can be fragmented, and the same vehicle may receive different IDs.

## Experimental Results

The tracking experiment produced the following observations:

| Measurement | Result |
|---|---:|
| Total tracking IDs generated | 686 |
| IDs detected in at least 15 frames | 201 |

Vehicle class distribution among IDs appearing in at least 15 frames:

| Vehicle Class | Count |
|---|---:|
| Car | 110 |
| Motor | 61 |
| Van | 19 |
| Bus | 8 |
| Truck | 3 |

These values represent tracking IDs and their predicted classes, not verified counts of unique physical vehicles.

The experiments were conducted as an initial exploration of the pipeline and should not be interpreted as a comprehensive evaluation of model accuracy.

## Repository Structure

```text
drone-vehicle-detection/
│
├── notebooks/
│   └── drone_vehicle_detection.ipynb
│
├── outputs/
│   ├── detection_samples/
│   └── tracking_samples/
│
├── README.md
├── requirements.txt
└── .gitignore
```

*The structure above is a suggested layout. Adjust the filenames and folders to match the actual repository.*

## Running the Project

### 1. Clone the repository

```bash
git clone https://github.com/Pragatisource08/drone-vehicle-detection.git
cd drone-vehicle-detection
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

Alternatively:

```bash
pip install ultralytics opencv-python
```

### 3. Prepare the required files

Make sure you have:

- The fine-tuned YOLO model weights.
- The drone video you want to process.
- The required dataset configuration if you intend to reproduce training.

Update the file paths in the notebook to match your environment.

### 4. Run the notebook

Open the notebook in Google Colab or Jupyter Notebook and execute the cells in sequence.

For GPU-based execution, select a compatible GPU runtime.

## Limitations

- The training run was an initial fine-tuning experiment, not a fully optimised training process.
- Detection counts alone do not establish model accuracy.
- Tracking IDs may change when an object is temporarily lost or incorrectly associated.
- Results can vary depending on video quality, object size, confidence threshold, and image resolution.
- The current experiments do not establish production-level performance.

## Future Scope

- Train for more epochs and evaluate the model on a separate validation or test set.
- Evaluate detection performance using precision, recall, and mAP.
- Improve tracking consistency in crowded aerial scenes.
- Explore real-time drone video processing.
- Investigate deployment and optimisation for GPU-based inference.

## Learning Outcomes

Through this project, I gained practical experience with:

- Fine-tuning pretrained object detection models.
- Understanding YOLO inference and detection outputs.
- Experimenting with confidence thresholds and image resolution.
- Processing video frames using computer vision.
- Integrating object detection with multi-object tracking.
- Analysing tracking results using Python.

---

**Author:** Pragati Srivastava

**GitHub:** [Pragatisource08](https://github.com/Pragatisource08)
