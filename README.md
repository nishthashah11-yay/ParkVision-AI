# ParkVision AI – Smart Parking Occupancy Detection System

**Student Name:** Nishtha Priyesh Shah  
**Candidate Registration Number:** 1000436  
**Programme:** International Baccalaureate Career-related Programme (IBCP) – Grade 12  
**School:** Aspee Nutan Academy  
**Career-related Study (CRS):** Artificial Intelligence  
**Project Title:** ParkVision AI – Smart Parking Occupancy Detection System  
**Live Application:** [Insert your Streamlit app link here]

---

## 1. Project Overview & Problem Definition

### 1.1 Introduction

Parking management is an important challenge in urban areas, where increasing numbers of vehicles create difficulties in finding available parking spaces. Drivers often spend unnecessary time searching for vacant slots, contributing to traffic congestion, fuel consumption and environmental pollution.

Traditional parking management systems often depend on physical sensors installed in individual parking spaces. These systems can involve additional installation and maintenance costs.

**ParkVision AI** is an AI-powered smart parking solution that uses Computer Vision and Deep Learning to identify empty and occupied parking spaces from uploaded parking-lot images.

The system uses the YOLOv8 object detection model to analyse images, identify parking spaces and generate useful occupancy statistics through an interactive web dashboard.

### 1.2 Project Objectives

The main objectives of ParkVision AI are:

1. Automate the identification of empty and occupied parking spaces.
2. Use artificial intelligence to analyse parking-lot images.
3. Display detected parking spaces using colour-coded bounding boxes.
4. Calculate total parking spaces and occupancy percentages.
5. Identify parking congestion levels.
6. Generate recommendations based on parking availability.
7. Provide an interactive and user-friendly dashboard for visualising results.

### 1.3 Input and Output Mapping

**System Input:**
- Uploaded parking-lot images.
- Supported image formats: JPG, JPEG, PNG and WEBP.
- Adjustable detection confidence threshold.
- Optional image contrast enhancement.

**System Output:**
1. Detection and classification of parking spaces.
2. Green bounding boxes representing empty spaces.
3. Red bounding boxes representing occupied spaces.
4. Total number of detected parking spaces.
5. Number of empty and occupied spaces.
6. Overall parking occupancy percentage.
7. Congestion classification and recommendations.
8. Downloadable detection report in CSV format.

---

## 2. System Architecture & Workflow

ParkVision AI follows a computer vision-based workflow in which an uploaded image is processed by a trained YOLO model and converted into meaningful parking occupancy information.

### 2.1 System Workflow

```text
       PARKING LOT IMAGE
               |
               ▼
       IMAGE UPLOAD
               |
               ▼
       IMAGE PREPROCESSING
     (RGB Conversion & Optional
       Contrast Enhancement)
               |
               ▼
        YOLOv8 DETECTION
               |
               ▼
       CLASSIFICATION OF
       PARKING SPACES
               |
        ┌──────┴──────┐
        ▼             ▼
      EMPTY        OCCUPIED
     (Green)         (Red)
        └──────┬──────┘
               |
               ▼
       OCCUPANCY CALCULATION
               |
               ▼
       CONGESTION ANALYSIS
               |
               ▼
       PARKVISION AI DASHBOARD
               |
        ┌──────┴──────────┐
        ▼                 ▼
   VISUAL RESULTS    CSV REPORT
```

### 2.2 Technologies Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| YOLOv8 | Object detection and classification |
| Ultralytics | YOLO model implementation |
| OpenCV | Image processing and bounding-box annotations |
| NumPy | Image array processing |
| Pillow (PIL) | Image loading and conversion |
| Pandas | Detection data management and CSV reports |
| Streamlit | Interactive web application |
| PyTorch | Deep learning framework |
| PKLot | Referenced parking dataset |

---

## 3. Dataset & Image Processing

### 3.1 Dataset

ParkVision AI references the **PKLot dataset**, which contains parking-lot images used for research and development in automated parking occupancy detection.

The dataset provides examples of parking spaces under different environmental and lighting conditions.

The project uses a YOLO model stored as `best.pt` for detecting and classifying parking spaces.

### 3.2 Image Preprocessing

Before the image is passed to the detection model, the application performs the following operations:

1. **Image Loading:** The uploaded image is opened using Pillow.
2. **Orientation Correction:** EXIF orientation is corrected to ensure the image is displayed properly.
3. **RGB Conversion:** The image is converted into RGB format.
4. **Optional Contrast Enhancement:** OpenCV's CLAHE (Contrast Limited Adaptive Histogram Equalization) is used to improve image contrast when enabled.

### 3.3 CLAHE – Image Enhancement

CLAHE is an image-processing technique used to improve local contrast.

In ParkVision AI, users can enable or disable contrast enhancement through the sidebar.

This feature can help improve image visibility in situations involving uneven lighting or shadows. However, enhanced images may not always improve detection accuracy, so results should be compared carefully.

---

## 4. AI Model & Detection Process

### 4.1 YOLOv8 Object Detection

ParkVision AI uses **YOLOv8 Nano**, a lightweight object detection model from the Ultralytics YOLO family.

YOLO (You Only Look Once) is an object detection approach that identifies objects and their locations within an image.

Unlike image classification, which assigns a label to an entire image, object detection identifies multiple objects and provides bounding boxes around them.

The model used in this application is stored in:

`models/best.pt`

### 4.2 Detection and Classification

The model analyses the uploaded image and generates predictions containing:

- Object class.
- Confidence score.
- Bounding-box coordinates.

The application maps the model's output into three categories:

| Class | Description | Bounding Box |
|---|---|---|
| Empty | Available parking space | Green |
| Occupied | Parking space containing a vehicle | Red |
| Unknown | Unrecognised class | Yellow |

The classification is based on the predicted class name, with class IDs 0 and 1 used as fallback mappings for empty and occupied spaces respectively.

### 4.3 Confidence Threshold

The application includes an adjustable confidence threshold ranging from 0.10 to 0.90.

The default confidence threshold is **0.25**.

- Lower confidence values allow more predictions to be included.
- Higher confidence values retain only predictions with stronger model confidence.

This allows users to experiment with detection sensitivity.

### 4.4 Detection Parameters

| Parameter | Value |
|---|---|
| Model | YOLOv8 Nano |
| Image Size | 640 pixels |
| Default Confidence | 0.25 |
| IoU Threshold | 0.7 |
| Inference Mode | Image-based prediction |
| Model File | `models/best.pt` |

---

## 5. Parking Occupancy & Mathematical Analysis

After detecting the parking spaces, ParkVision AI calculates the total number of classified spaces and determines the overall parking occupancy.

### 5.1 Occupancy Calculation

The occupancy percentage is calculated using:

\[
\text{Occupancy Rate} =
\frac{\text{Occupied Spaces}}{\text{Total Classified Spaces}}
\times 100
\]

Where:

\[
\text{Total Classified Spaces}
=
\text{Empty Spaces}+\text{Occupied Spaces}
\]

For example, if a parking lot has 60 detected spaces, of which 45 are occupied:

\[
\text{Occupancy Rate} =
\frac{45}{60}\times100
\]

\[
=75\%
\]

Therefore, the parking lot has an occupancy rate of 75%.

### 5.2 Congestion Classification

ParkVision AI uses three congestion levels based on the calculated occupancy percentage.

| Occupancy Rate | Congestion Level | System Recommendation |
|---|---|---|
| Below 40% | LOW | Availability is relatively good. Direct drivers towards empty spaces. |
| 40%–75% | MODERATE | Some spaces remain. Guide incoming drivers towards available spaces. |
| Above 75% | HIGH | Limited availability. Consider directing drivers to another parking area. |

These thresholds are implemented directly in the application's congestion analysis function.

---

## 6. Streamlit Web Application

ParkVision AI is developed using Streamlit to provide an interactive dashboard where users can upload parking-lot images and view the results of AI-based detection.

### 6.1 User Interface & Design

The application features a modern dark-themed interface designed around a smart-city and computer-vision concept.

The interface includes:

- Dark navy background with teal and green accents.
- Custom typography using DM Sans and Space Grotesk.
- Interactive metric cards.
- Organised sections for image input, model settings and detection results.
- Colour-coded detection annotations.

### 6.2 Main Application Features

**1. Image Upload**

Users can upload parking-lot photographs in JPG, JPEG, PNG and WEBP formats.

**2. Adjustable Detection Settings**

Users can modify the confidence threshold and enable or disable image contrast enhancement.

**3. Automated Parking Detection**

The YOLOv8 model analyses the uploaded image and identifies empty and occupied parking spaces.

**4. Parking Overview Dashboard**

The dashboard displays four important metrics:

- Total Detected Spaces
- Empty Spaces
- Occupied Spaces
- Occupancy Percentage

**5. Occupancy Visualisation**

A progress bar displays the percentage of occupied parking spaces.

**6. Congestion Status**

The application automatically classifies parking congestion as LOW, MODERATE or HIGH and displays a corresponding recommendation.

**7. Visual Detection Comparison**

Users can compare the original uploaded image with the annotated image generated by ParkVision AI.

**8. Detection Report & Export**

The application generates a detailed detection table containing:

- Predicted class.
- Original model class.
- Confidence score.
- Bounding-box coordinates.
- Bounding-box width and height.

Users can download this information as a CSV file for further analysis.

---

## 7. Project Folder Structure

The GitHub repository is organised as follows:

```text
parkvision-ai/
│
├── app.py
│
├── requirements.txt
│
├── runtime.txt
│
├── README.md
│
└── models/
    └── best.pt
```

**File Description:**

- `app.py` – Main Streamlit application containing the interface, image processing, YOLO detection and occupancy calculations.
- `requirements.txt` – Contains the Python libraries and dependencies required to run the application.
- `runtime.txt` – Specifies the Python runtime configuration.
- `models/best.pt` – Trained YOLO model used for parking-space detection.
- `README.md` – Project documentation.

---

## 8. Installation & Running the Application

### 8.1 Prerequisites

- Python 3.11 (recommended).
- pip package manager.
- Git.
- Required project dependencies.

### 8.2 Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

Navigate into the project folder:

```bash
cd parkvision-ai
```

### 8.3 Install Dependencies

```bash
pip install -r requirements.txt
```

### 8.4 Launch the Application

```bash
streamlit run app.py
```

The application will open in the browser, allowing users to upload parking-lot images and analyse parking occupancy.

---

## 9. Deployment on Streamlit Community Cloud

ParkVision AI is designed to run as a web application through Streamlit Community Cloud.

### Deployment Steps

1. Upload the project files to a GitHub repository.
2. Sign in to Streamlit Community Cloud using GitHub.
3. Select the project repository.
4. Choose the `main` branch.
5. Set the main file path to `app.py`.
6. Deploy the application.
7. Streamlit Cloud installs the dependencies listed in `requirements.txt` and launches the application.

**Deployment Configuration:**

| Setting | Value |
|---|---|
| Repository | parkvision-ai |
| Branch | main |
| Main File Path | app.py |
| Framework | Streamlit |
| Model Location | models/best.pt |

---

## 10. Limitations & Future Improvements

### 10.1 Current Limitations

1. **Image-Based Analysis:** The current application analyses uploaded images rather than continuously processing live CCTV footage.
2. **Camera Perspective:** Detection performance may vary depending on the camera angle and visibility of parking spaces.
3. **Lighting Conditions:** Shadows, reflections and poor lighting can affect detection results.
4. **Model Dependency:** The quality of predictions depends on the trained YOLO model and the images provided.
5. **Detection Accuracy:** The output represents model predictions and does not guarantee actual real-time parking availability.

### 10.2 Future Scope

The project can be further developed through:

- Integration with live CCTV camera feeds.
- Real-time parking occupancy monitoring.
- Interactive parking-space maps.
- Automatic identification of individual parking slots.
- Integration with mobile applications.
- Historical occupancy tracking and analysis.
- Smart navigation to available parking spaces.

---

## 11. Conclusion

ParkVision AI demonstrates how Artificial Intelligence and Computer Vision can be applied to solve a practical urban problem.

By combining YOLOv8 object detection, image processing and an interactive Streamlit dashboard, the application transforms ordinary parking-lot images into meaningful occupancy information.

The system identifies available and occupied parking spaces, calculates occupancy percentages, determines congestion levels and provides useful recommendations.

The project highlights the potential of AI-powered smart parking systems in improving parking management, reducing unnecessary searching and supporting the development of smarter urban environments.

---

**Project Developed By:**  
**Nishtha Priyesh Shah**  
Candidate Registration Number: 1000436  
IBCP Grade 12 | Aspee Nutan Academy  
Career-related Study: Artificial Intelligence

**Project:** ParkVision AI – Smart Parking Occupancy Detection System
