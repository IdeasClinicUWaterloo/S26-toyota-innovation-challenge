# Robot Arm Setup and Safety Training

This document outlines the safety procedures that must be followed while operating the robotic arm.

The arms provides does not come with any native sensing capabilities. Its movement entirely depends on the robot's motor encoder data. Hence, human monitoring is needed to ensure the robot does not reach its movement limits, and does not crash into items its unintended to contact.

## Before Operation:
Dobot Magician arm is capable in movin in the following highlighted areas. Before powering on the robot, ensure this area is free of any obstacles, such as personal items. At the same time, ensure the electrical wiring and the pheumatic pump hose is secured so it does not bloc the robot's movement.
![FOV picture](<./25392_dobot range of motion.jpg>)

## Software Setup:
1. Download the Dobot Link and Dobot Lab from the (here)[https://www.dobot.us/download-center/]
2. Follow the instruction in the download files. However, for each "install" button you must click install and let the installation run 3 times for the software to fully install.
3. A successful installation of Dobot Link should look like a tiny icon in your tray, and Dobot Lab should be a launchable app
4. to launch dobotlink: pull up the application's icon in the tray, click and select "launch developer interface" then select "device test" In the connect row, select your robot arm and connect
5. to launch dobotlab: launch the application, then select the tab with the python logo. Click the conenct button on top of that interface

## Common Protocols for Robot Malfunction (Please be prepared to perform any of the steps below as soon as troubek starts)
### Locate the Following Emergency Switch Locations
1. Main Power Switch: located at the back fo the robot base as a switch
2. Booting Switch: metallic press button located at the top of the robots base
3. Robot Joint Unlocking Switch: Located at the link that connects to the gripper. A plastic press button with a lock symbol on it. Press and Hold the button to move the robot's arm without motors working against it.
4. Robot Status Light: located at top of the robot's base. Green light = normal operation, red light = trouble flagged

### Common Failure Scenario
| Scenario | Symptoms | Resolution |
| -------- | -------- | -------- |
| Robot gets caught in a limit position or a physical obstacle    | Robot makes a clicking movement sound     |  Hold down the join unlocking swtich and hold the arm, and power off the main switch. Restart robot connect and power up steps |
| Robot's Status Light turns red    | Robot does not respond to any programs   |  1. Disconnect robot from dobotlink 2. connect robot through dobot lab 3. check error flags displayed and act according to the flags  |
 



## Robot Connect & Power Up:
1. press the button with a "lock" icon on the robot arm and move it slightly out of its shutdown position. ensure that you are not at a limit point for the robot's movement. To check, ensure your robot can still move to the spaces around it.
2. connect the robot to your compuer by plugging in its USB cable
3. press the power button on the robot's base. The robot should power up, beep after waiting for a moment. Robot should being its initialization process bu conducting a sweep over its entire operation region.
4. Launch Dobot Link and connect 
5. Now you should be prepared to run your script! Try running testDobot or pickCVBlock and see if you run into any issues. 


   
