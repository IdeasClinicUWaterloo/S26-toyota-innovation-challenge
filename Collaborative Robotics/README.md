# Collaborative Robotic Arm Folders

## Must Read: Robot Arm Safety Training
Please read through the safety operation procedue in here [insert link] before operating the provide robot arm. Failure to do so may result in personal property damange or even injuries.

## Usage

Refer to `testDobot.py` for basic Dobot control

Notes

- Do not modify the lib folder (unless you're sure of what you're doing) -> it includes the DLLs to interface with DobotLink

## Calibration Steps

You need to run `getTransformationMatrix.py` every single time the camera is moved (Even a 1cm movement can lead to errors). Further instructions are in the .py file.

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

## Roadmap

1. Better H_Matrix Generation Technique (not very accurate)
2. Better move_to_xyz() command (joint limit avoidance)
3. Other CV applications
