# PipeSight AI — AI-Based Sewer Pipe Defect Detection

PipeSight AI is a computer vision project focused on detecting defects in sewage pipes using a trained **YOLO26s model** and **OpenCV 5**. The project aims to analyze sewer inspection images and videos, identify potential defects, and present the results in an understandable format.

The current version focuses on the trained detection model and backend development. Video-based multi-object tracking, a complete user interface, and cloud deployment are planned extensions and should not be considered completed features unless implemented and tested.

---

## 1. Project Overview

Sewer pipes can develop defects such as cracks, debris accumulation, joint faults, root intrusion, and surface damage. Manually inspecting long CCTV recordings can be time-consuming.

PipeSight AI explores how computer vision can assist this process by detecting potential pipe defects from images and, as the project develops, from video footage.

### Technologies

- **Python** — Core programming language.
- **YOLO26s (Ultralytics)** — Trained object detection model.
- **OpenCV 5** — Image and video processing.
- **PyTorch** — Deep learning framework used by the model.
- **FastAPI** — Planned or in-progress backend API for receiving images and videos.
- **AWS** — Intended cloud deployment environment.

## 2. Current Project Status

- **Model training:** Completed for the current experiment.
- **Defect detection:** Implemented in the trained model; actual performance depends on the input image and inspection conditions.
- **Video processing:** Intended next step for processing video frames with OpenCV 5.
- **ByteTrack:** Not yet implemented.
- **Frontend dashboard:** Not yet confirmed as implemented.
- **AWS deployment:** Not currently deployed; the previous EC2 instance has been terminated.
- **Automated testing and PDF reports:** Not claimed as completed in this version.

## 3. Model and Dataset

The project uses a custom-trained YOLO26s model for sewer pipe defect detection.

The target defect categories are:

- Crack
- Debris
- Joint Fault
- Root Intrusion
- Surface Damage

The training dataset contains 978 images. Data augmentation was applied to training images, while validation and test images were kept unaugmented.

The trained model checkpoint is named `best.pt`.

**Model limitation:** The current model has limited detection performance, with approximately 50 mAP reported during development. This figure should be accompanied by the exact metric, evaluation split, and evaluation results when those details are available. Further dataset improvements and model evaluation are needed before practical deployment.

## 4. Planned Development

The next development stages are:

1. Build a FastAPI endpoint for image uploads and defect detection.
2. Add video upload and sequential frame processing using OpenCV 5.
3. Integrate ByteTrack to maintain defect track IDs across video frames.
4. Save annotated images and video evidence.
5. Present detection results and inspection summaries through a simple interface.
6. Evaluate deployment on AWS when the backend is ready.

These are development goals, not claims that every feature is already operational.

## 5. Model Setup

Place the trained model checkpoint at the following location if your backend is configured to load it from this path:

```text
backend/
└── models/
    └── best.pt
```

The model weights are not included in the repository by default. Keep large model files outside Git when necessary, and follow the repository's model setup instructions.

## 6. Development Environment

The project was trained and tested in a Google Colab environment using a Tesla T4 GPU.

Reported development environment:

- Python 3.13.16
- OpenCV 5.0.0
- Ultralytics 8.4.187
- PyTorch 2.11.0+cu130
- CUDA-enabled Tesla T4 GPU

These are the recorded training environment versions. They do not guarantee that every deployment environment will support the same configuration.

## 7. Future Scope

- Improve detection accuracy using additional labeled training data.
- Implement ByteTrack-based defect tracking across video frames.
- Develop video-level defect summaries and evidence export.
- Build a user-friendly inspection dashboard.
- Explore AWS deployment for remote access and demonstration.

## 8. Conclusion

PipeSight AI is a developing computer vision project for sewer pipe defect detection. It currently provides a foundation through a custom-trained YOLO26s model and an OpenCV 5 development environment. Future work will focus on reliable video processing, object tracking, improved model performance, and deployment.

The project is intended as a prototype to explore AI-assisted sewer inspection, not as a validated replacement for professional engineering inspection.
