# Vehicle Detection and Traffic Analysis using YOLOv8

## Overview

This project implements an adaptive traffic signal timing system based on real-time vehicle density and type. It uses the **Density-Based Weighted Signal Allocation (DBWSA) Algorithm** to dynamically allocate green, yellow, and red signal durations based on traffic data.  

### **Key Features**  
✔ Dynamically adjusts signal timing based on traffic density.  
✔ Weighs different vehicle types for fair allocation.  
✔ Ensures efficient traffic flow by preventing unnecessary delays.  
✔ Works with real-time traffic data in JSON format.  

## Project Structure

```
project_root/
│── models/
│   └── VehicleDetectionYolov8Model.pt  # Pretrained YOLOv8 model
│   └── VehicleDetectionYolov11LModel.pt  # Pretrained YOLOv8 model
│── traffic-videos/
│   └── test-video.mp4                   # Place input videos here
│── masks/
│   └── test-video_mask.jpg               # Place mask images here
│── output/
│   └── Results will be stored here
│── traffic_data/
│   └── Intersection Data
│── generate_vehicle_detection.py           # Main vehicle detection script
│── run_detector.py                         # CMD Script for Vehicle Detection
│── generate_mask.py                        # Script to generate mask images
│── get_predictions.py                      # Script to generate detection(without mask)
│── DBWSA.py                                # Traffic Signal Optimization Algorithm
│── requirements.txt                        # Required Python packages
│── README.md                               # This file
```

<p align="center">
    <img src="preview.gif">
</p>

## Setup Instructions

### 1. Clone the Repository

Ensure you have Python(preferably 3.8+) & git installed. Then, install the required dependencies:

First, clone the repository using Git:

```bash
git clone https://github.com/saketjha34/Traffic-Management-System.git
cd Traffic-Management-System
```

### 2. Install Dependencies

Ensure you have Python installed (preferably 3.8+). Then, install the required dependencies:

```bash
pip install -r requirements.txt
```

### 3. Place Input Video

Place your traffic video inside the `traffic-videos/` folder. Example:

```
traffic-videos/test-video.mp4
```

### 4. Generate a Mask Image

A mask is used to filter the region of interest in the video. To generate a mask automatically, run:

```bash
python generate_mask.py 
```

This will create a grayscale mask image where white (255) represents the area to analyze, and black (0) is ignored.

### 5. Run the Vehicle Detector

Execute the script with the following command:

```bash
python run_detector.py --input_video traffic-videos/test-video1.mp4 --output_folder output --mask_image masks/test-video1_mask.jpg --confirmation_frame 15 --confidence_threshold 0.35
```

### 6. Run the Traffic Signal Optimization Algorithm

#### **Parameters & Customization**  

The algorithm uses the following default parameters, which can be modified:  

| Parameter         | Description                              | Default |
|------------------|--------------------------------------|---------|
| `BASE_GREEN_TIME`  | Minimum green signal duration       | 5s      |
| `MAX_GREEN_TIME`   | Maximum green signal duration       | 60s     |
| `YELLOW_TIME`      | Fixed yellow signal duration        | 3s      |
| `TOTAL_CYCLE_TIME` | Total signal cycle duration        | 60s     |

You can modify these values in `DBWSA.py`.  

```bash
python DBWSA.py 
```

### 7. Output

- **Processed Video**: The annotated video is saved in `output/test-video/`.
- **JSON Report**: Vehicle tracking data is stored in `output/test-video/test-video_traffic_data.json`.

## Example Output

```
Processed video saved in output/test-video/
Traffic data saved to output/test-video/test-video_traffic_data.json
Total confirmed vehicles detected: 50
Car: 30
Bus: 10
Truck: 5
Motorcycle: 5
Optimized Traffic Signal Timings:
Road 1 - Green: 20s, Yellow: 3s, Red: 37s
Road 2 - Green: 15s, Yellow: 3s, Red: 42s
Road 3 - Green: 25s, Yellow: 3s, Red: 32s
Road 4 - Green: 18s, Yellow: 3s, Red: 39s
```

## Notes

- You can adjust detection parameters like confidence threshold using `--confidence_threshold`.
- Change the confirmation frame count using `--confirmation_frame` to tweak detection stability.

## Credits

Developed using Ultralytics and Supervision Library.