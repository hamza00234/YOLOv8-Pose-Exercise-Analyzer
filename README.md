

# YOLOv8 Exercise Analysis System

A computer vision project that analyzes exercise performance from video using YOLOv8 pose estimation, temporal keypoint smoothing, rule-based motion analysis, and structured JSON reporting.

## Overview

This project implements an exercise-analysis pipeline capable of detecting and evaluating:

* Squats
* Push-ups

The system processes video input, estimates human body keypoints using YOLOv8 Pose, smooths noisy detections using Exponential Moving Average (EMA), counts exercise repetitions, assesses movement quality using joint-angle analysis, and generates structured JSON summaries.

## Features

### Pose Estimation

* Uses **YOLOv8 Small Pose (`yolov8s-pose.pt`)**
* Detects human body keypoints using the COCO 17-keypoint format
* Selects the primary person when multiple people are detected

### Temporal Smoothing

* Implements **Exponential Moving Average (EMA)** smoothing
* Reduces keypoint jitter and frame-to-frame noise
* Handles short periods of missing detections

### Repetition Counting

* Rule-based state machine for robust counting
* Detects complete movement cycles:

  * Top → Bottom → Top
* Prevents false counts caused by noisy keypoint estimates

### Form Assessment

#### Squats

* Knee-angle analysis
* Squat-depth validation
* Good-form repetition detection

#### Push-Ups

* Elbow-angle analysis
* Body-line validation
* Good-form repetition detection

### JSON Report Generation

Produces structured exercise summaries containing:

* Total repetitions
* Good-form repetitions
* Video metadata

Example output:

```json
{
  "video_id": "uuid",
  "summary": {
    "squats": {
      "total_reps": 12,
      "good_form_reps": 10
    },
    "pushups": {
      "total_reps": 8,
      "good_form_reps": 7
    }
  }
}
```

## Project Pipeline

```text
Input Video
      │
      ▼
YOLOv8 Pose Estimation
      │
      ▼
Keypoint Extraction
      │
      ▼
EMA Temporal Smoothing
      │
      ▼
Joint-Angle Computation
      │
      ▼
Exercise-Specific Analysis
      │
      ├── Repetition Counting
      ├── Form Evaluation
      └── Movement Validation
      │
      ▼
Structured JSON Output
```

## Technologies Used

* Python
* YOLOv8 (Ultralytics)
* OpenCV
* NumPy
* Matplotlib

## Repository Structure

```text
.
├── CVproject.ipynb
├── README.md
├── input_videos/
├── outputs/
│   └── exercise_analysis_output.json
└── assets/
```

## Installation

```bash
pip install ultralytics opencv-python matplotlib numpy
```

## Usage

1. Place exercise videos in the desired location.
2. Update the video paths in the notebook configuration section.
3. Run all notebook cells.
4. Review the generated JSON report.

## Methodology

### Pose Detection

Human body keypoints are extracted using the YOLOv8 pose estimation model.

### Keypoint Smoothing

An Exponential Moving Average filter is applied to stabilize keypoint trajectories across frames.

### Joint-Angle Analysis

Angles are calculated from detected body joints to evaluate exercise motion.

### Rule-Based Evaluation

Exercise-specific rules determine:

* Repetition completion
* Movement quality
* Form correctness


