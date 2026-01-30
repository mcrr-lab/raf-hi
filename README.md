# RAF-HI (Robot-Assisted Feeding in a Hospital Inpatient Setting)

This repository contains the software for a Robot-Assisted Feeding system utilizing a **Kinova Gen3 7-DOF robot**, an **Intel RealSense** camera, and custom grippers. The system features real-time visual servoing for both food acquisition and delivery to the user's mouth.

## System Overview
The system utilizes a state-machine orchestrator to manage the feeding cycle:
1.  **Detection**: Identification and live segmentation of food items using GPT-4o --> DINO-X.
2.  **Acquisition**: Real-time visual servoing to the food item using SAM2 for tracking, using custom grasp detection and sequencing to pick up the food.
3.  **Transfer**: Movement to an intermediate face-scan position.
4.  **Delivery**: Visual servoing to the user's mouth using MediaPipe BlazeFace.

## Prerequisites

### Hardware
* **Robot**: Kinova Gen3 7-DOF.
* **Camera**: Intel RealSense (configured for 848x480 resolution).
* **Gripper**: Robotiq 2F-140 with custom feeding attachments.

### Software
* **OS**: Ubuntu 24.04.
* **ROS2**: Jazzy Jalisco.
* **Python**: 3.12.3.
* **Environment**: Python Venv.

## Installation

1.  **Workspace Setup**:
    ```bash
    mkdir -p ~/raf-live/src
    cd ~/raf-live
    # Clone this repository and its submodules
    ```

2.  **SAM2 Real-Time Integration**:
    Place the `sam2_streaming` repository inside the `raf-hi` folder. Follow the installation steps provided in the [SAM2 Real-Time repository](https://github.com/Gy920/segment-anything-2-real-time/tree/main) to set up the segmentation engine.

3.  **Python Dependencies**:
    Activate your virtual environment and install the required packages:
    ```bash
    source ~/deploy-env/bin/activate
    pip install pygame numpy pyyaml mediapipe
    ```

4.  **API Configuration**:
    Create a `.env` file in the `raf-hi` folder with the following keys:
    * `DINOX_API_KEY`
    * `OPENAI_API_KEY`
    * `GOOGLE_API_KEY`
    
    *Note: To reduce the number of required APIs, you can configure the system to use Gemini exclusively for food identification.*

## Configuration

Before running, update `config.yaml` with your environment-specific values:

* **Positions**: Define the `overlook` and `intermediate` (face scan) joint angles. Move the robot to the desired positions and record the joint values.
* **Table Height**: Set `dist_from_table` (in meters) by checking the depth values from the RealSense camera.
* **Calibration**: The `overlook` position should be directly above the plate with the camera plane parallel to the table. Check the RealSense pixel values to find the coordinate corresponding to the midpoint between your grippers and update `check_pixel_points`.
* **Audio**: Ensure the `.mp3` files for system feedback (power on, confirmation, snap, etc.) are located in the path specified in `config.yaml`.

## Running the System

In every terminal, source the environment:
```bash
cd ~/raf-live
source ~/workspace/ros2_kortex_ws/install/setup.bash
source install/setup.bash
source ~/deploy-env/bin/activate
export PYTHONPATH=/home/mcrr-lab/raf-live/SAM2_streaming:$PYTHONPATH
```
1.  **Terminal 1 - Hardware Drivers** Launch the core robot and camera drivers:
    ```bash
    ros2 launch raf_bringup raf_system.launch.py
    ```
2.  **Terminal 2 - Perception & Support Services** Launch the food detection, face detection, voice, and servoing nodes:
    ```bash
    ros2 launch raf_bringup raf_services.launch.py
    ```
1.  **Terminal 3 - Orchestrator** Start the main feeding state machine:
    ```bash
    python3 src/scripts/orchestrator2.py
    ```

