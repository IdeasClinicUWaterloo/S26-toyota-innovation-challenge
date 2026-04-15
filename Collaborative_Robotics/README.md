# Collaborative Robotic Arm 
## Challenge Overview
Robotics are growing increasingly common in manufacturing/warehouse settings to assist humans with various tasks. Traditionally, robots are in cages where human workers must stay out of during their operation. Toyota would like to take an innovative approach, where production workers and robots collaborate to achieve productivity boosts on their factory floor. This means that the robot and the human worker will be working on the same task but handling different parts of the task. 

You are tasked with designing the control of a desktop robotic arm that resembles a scaled-down version of a high torque arm found in TMMC, to work along with humans to achieve automative assembly tasks. Engineers at TMMC have already enabled the robot arm to complete a pick-and-place motion according to human actions. Your role is to continue developing this function to achieve more robot collaboration capabilities. The robot should consider efficiency, the safety of users around it, as well as the comfort of the users who interact with the robots.  

## Potential Solutions: 

- Robot that places parts delivered by human worker into the correct assembly position  
- Robot arm that allows teleoperation or hand mimicry 
- Identify the correct object and hand over to a human worker 
- Safety feature or human collision avoidance mechanism  
- Friendly robot: social robot-human interaction


## Recommend Area of Development: 

The following directions can be a starting direction for your development. You are not just limited to these focuses; feel free to create something that has not been mentioned in the following list.
- Vision/Perception: improve the current computer vision to achieve the detection of the car parts
- Human-machine interface: add in functions to detect worker's hand motions and steer robot away
- tbc

## Starter Material Introduction:
Each team will be provided with the following equipment: 
1. A Dobot Magician arm with a gripper at the tip
2. A Orbbec camera
3. Camera Stand to mount camera


## Must Read: Robot Arm Set Up & Safety Training
Please read through the safety operation procedure in [here](https://github.com/IdeasClinicUWaterloo/S26-toyota-innovation-challenge/tree/docs/Collaborative_Robotics/Safety%20Instructions) before operating the provided robot arm. Failure to do so may result in damage to personal property or even injuries.

## Usage

Refer to `testDobot.py` for basic Dobot control

Notes

- Do not modify the lib folder (unless you're sure of what you're doing) -> it includes the DLLs to interface with DobotLink

## Calibration Steps

You need to run `getTransformationMatrix.py` every single time the camera is moved (Even a 1cm movement can lead to errors). Further instructions are in the .py file.

### To use the calibration file: 
1. set up and connect to the robot as described in the set up procedure
2. run the calibration script
3. the robot should put the gripper down to the table
4. place a red target right in between claws of the gripper
5. press space and let the robot lift its gripper
6. check camera feed: if the target is obstructed by the robot , use the unlock joint button to move the arm away until the camera can fully capture the target
7. repeat this process. There are about 12 calibration pointsi n the script and the calibration is done after all 12 points are collected

If you're using a Webcam other than the Astra Orbecc, you should run the `calibrateCamera.py` to get appropriate camera parameters. This requires you to have a 4x4 ArUco board -> or any other similar calibration tool.

### Using Intrinsic Calibration Data

You need to first import all the data

```python
data          = np.load("camera_params.npz")
camera_matrix = data["camera_matrix"]   # 3x3 intrinsic matrix  (K)
dist_coeffs   = data["dist_coeffs"]     # distortion vector
```

- Option A - undistort a single frame (simple):
  `undistorted = cv2.undistort(frame, camera_matrix, dist_coeffs)`

- Option B - fast per-frame using pre-computed maps (recommended for live video):

```python
h, w = frame.shape[:2]
new_K, roi = cv2.getOptimalNewCameraMatrix(camera_matrix, dist_coeffs, (w,h), alpha=1)
map1, map2 = cv2.initUndistortRectifyMap(camera_matrix, dist_coeffs,
                                              None, new_K, (w,h), cv2.CV_16SC2)
undistorted = cv2.remap(frame, map1, map2, cv2.INTER_LINEAR)
```

- Option C - pose estimation with solvePnP:
  `cv2.solvePnP(obj_pts, img_pts, camera_matrix, dist_coeffs)`

## pickCVBlock Instructions
a basic script implementing a detect part/pick place unfction has been provided to you as a starter code. Feel free to base your solution off of this script, or feel free to created something new!
