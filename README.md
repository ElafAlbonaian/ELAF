# RayBot 2030 TEAM

## Project Overview

The RayBot 2030 TEAM robot is developed for the WRO24 competition, focusing on agility and intelligent navigation. This robot leverages ultrasonic sensors for obstacle detection, a Raspberry Pi camera for visual processing, and a compact display for user feedback. Its design aims to navigate complex terrains autonomously while providing real-time data to the operator.

## Components

### Hardware

- **Chassis:** Lightweight and robust chassis built from aluminum and plastic, optimizing for both strength and weight.
- **Motors:** High-torque DC motors equipped with encoders for precise movement control.
- **Microcontroller:** Raspberry Pi 5, serving as the main processing unit.
- **Sensors:**
  - **Ultrasonic Sensors:** Two HC-SR04 sensors placed at the front for detecting obstacles within 30 cm.
  - **Camera:** Raspberry Pi camera module for capturing images and video for processing.
- **Display:** 5-inch LCD touchscreen for real-time monitoring and control interface.
- **Power Supply:** 11.1V lithium-polymer battery providing extended operational time.

### Software

- **Operating System:** Raspbian OS for full compatibility with the Raspberry Pi.
- **Programming Language:** Python for ease of implementation and library support.

## Key Features

- **Autonomous Navigation:** Capable of detecting and avoiding obstacles while navigating predefined paths.
- **Real-Time Image Processing:** Uses computer vision to identify objects and make navigation decisions based on visual input.
- **User Interaction:** The touchscreen allows operators to control the robot and view live data.
- **Data Logging:** Records sensor data and images for post-competition analysis.

## Code Structure

### Main Program

```python
import RPi.GPIO as GPIO
import time
import picamera
import cv2
import numpy as np

# GPIO Setup
TRIG = 23
ECHO = 24
GPIO.setmode(GPIO.BCM)
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)

# Initialize Camera
camera = picamera.PiCamera()
camera.resolution = (640, 480)

# Function to measure distance
def get_distance():
    GPIO.output(TRIG, True)
    time.sleep(0.01)
    GPIO.output(TRIG, False)

    start_time = time.time()
    stop_time = time.time()

    while GPIO.input(ECHO) == 0:
        start_time = time.time()

    while GPIO.input(ECHO) == 1:
        stop_time = time.time()

    distance = (stop_time - start_time) * 17150
    return distance

# Function to process image
def process_image():
    camera.capture('/home/pi/image.jpg')
    img = cv2.imread('/home/pi/image.jpg')
    gray_img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    return gray_img

# Main control loop
try:
    while True:
        distance = get_distance()
        print(f"Distance: {distance:.2f} cm")
        
        if distance < 30:
            print("Obstacle detected! Reversing...")
        
        processed_img = process_image()
        # Update display with processed image (pseudo code)
        display_on_screen(processed_img)

        time.sleep(1)

except KeyboardInterrupt:
    GPIO.cleanup()

 logo, and our journey



