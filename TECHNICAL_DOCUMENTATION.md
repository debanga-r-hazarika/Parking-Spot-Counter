# Car Park Counter - Technical Documentation

## Overview
This application is a computer vision-based parking space counter system that processes video feeds to detect and count available parking spaces in real-time. The system uses OpenCV for image processing and provides an interactive interface for space marking and monitoring.

## System Architecture

### Core Components
1. **Space Marking System** (`ParkingSpacePicker.py`)
   - Handles parking space selection and position storage
   - Uses mouse events for interactive space marking
   - Saves parking space coordinates in a binary file

2. **Main Detection System** (`main.py`)
   - Processes video feed for space occupancy detection
   - Implements real-time space counting
   - Provides dynamic parameter adjustment through trackbars

## Technical Implementation Details

### 1. Space Marking System (ParkingSpacePicker.py)

#### Key Functions:
- `mouseClick(events, x, y, flags, params)`
  - Handles left-click for adding spaces (coordinates stored as tuples)
  - Handles right-click for removing spaces
  - Automatically saves updates to 'CarParkPos' file

#### Data Storage:
- Parking space positions stored in binary format using pickle
- Each position stored as (x, y) coordinate tuple
- Default space dimensions: 107x48 pixels

### 2. Main Detection System (main.py)

#### Image Processing Pipeline:
1. **Video Frame Capture**
   ```python
   cap = cv2.VideoCapture('carPark.mp4')
   ```

2. **Pre-processing Steps**:
   - Grayscale conversion
   - Gaussian blur (3x3 kernel)
   - Adaptive thresholding
   - Median blur
   - Dilation

#### Parameter Configuration:
- Three adjustable parameters via trackbars:
  1. Val1: Adaptive threshold block size (odd values 1-50)
  2. Val2: Threshold constant (1-50)
  3. Val3: Median blur kernel size (odd values 1-50)

#### Space Detection Algorithm:
1. **Region of Interest (ROI) Extraction**
   - Each parking space cropped based on stored coordinates
   - Fixed dimensions: 103x43 pixels

2. **Occupancy Detection**
   - Uses non-zero pixel count in thresholded image
   - Threshold value: 900 pixels
   - Below threshold: Space marked as available (green)
   - Above threshold: Space marked as occupied (red)

3. **Visual Feedback**
   - Rectangle drawing around spaces
   - Color coding (Green: Available, Red: Occupied)
   - Non-zero pixel count display
   - Total available spaces counter

## Integration Guide

### Prerequisites
```
opencv-python==4.8.1.78
cvzone==1.5.6
numpy==1.26.2
```

### Setup Steps
1. Install required dependencies
2. Prepare video input source
3. Run ParkingSpacePicker.py to mark parking spaces
4. Execute main.py for real-time detection

### Customization Points
1. **Space Dimensions**
   - Modify `width` and `height` variables in both files
   - Adjust based on camera angle and parking space size

2. **Detection Sensitivity**
   - Modify the pixel count threshold (default: 900)
   - Adjust trackbar parameters for different lighting conditions

3. **Visual Output**
   - Customize colors and text display
   - Modify rectangle thickness and text formatting

## Performance Considerations

1. **Image Processing**
   - Adaptive thresholding used for varying lighting conditions
   - Median blur helps reduce noise
   - Dilation helps connect broken contours

2. **Memory Management**
   - Video frames processed one at a time
   - Automatic video loop implementation
   - Efficient space coordinate storage using pickle

## Troubleshooting

1. **Poor Detection Accuracy**
   - Adjust trackbar values for better thresholding
   - Check lighting conditions
   - Verify parking space dimensions

2. **Performance Issues**
   - Reduce video resolution if needed
   - Optimize ROI size
   - Adjust processing parameters

## Future Enhancements
1. Multiple camera support
2. Database integration for logging
3. Network streaming capabilities
4. Machine learning-based detection
5. Weather condition handling

This documentation provides a comprehensive overview of the technical implementation and serves as a guide for future development and integration.