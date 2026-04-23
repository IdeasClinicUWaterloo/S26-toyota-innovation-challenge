# Use Raspberry Pi to Control Your Robot
You have the option to run the client code on your local computer or on the Raspberry Pi. Running on the Raspberry Pi allows the robot to be operated wirelessly. It also enables additional functions like local computer vision process. But this requires a bit more setup. This tutorial will walk you through the steps to set up your Raspberry Pi for running the source code.

## Usage Description: 
The raspberry pi essentially replaces your laptop to plug into the PRIZM controller. It will run the same client.py code that you would run on your laptop. 

## Note:
- Please **speak to a co-op student or a TA** if you are interested in controlling one or more of your robots with Raspberry Pi. They need to provide you with the raspberry pi, credentials to use the pi, and hardware to mount the pi on the robot. Note **inventory is limited**, so please reach out early if you are interested in using the raspberry pi.

- you will also be on an alternate network to allow direct IP pinging. Ensure you obtain credential for the network as well. 

- After obtaining the raspberry pi and its kits, the steps below are for setting up the "sync" between your local computer and the Raspberry Pi. This allows you to edit code on your local computer and have it automatically update on the Raspberry Pi, which is much more convenient than editing code directly on the Pi.


## Setting Up the "Sync"

1. Install: Search for the SFTP extension in VS Code (the one by Natizyskane is popular) and install it.
<img src="sftp.png" width="50%" alt="Description">

2. Initialize: Press Ctrl + Shift + P and type SFTP: Config.
3. Configure: Your .vscode/sftp.json. Paste the following code into that file.
```json
{
    "name": "Robot-Pi-Sync", // Name of the connection
    "host": "10.37.x.x", // Your Pi's IP
    "protocol": "sftp",
    "port": 22,
    "username": "pi",
    "remotePath": "/home/pi/YourProjectFolder",// The folder on the Pi where you want to sync your code
    "uploadOnSave": true,
    "ignore": [
        ".vscode",
        ".git",
        ".env" 
    ]
}
```

4. SSH into your Pi to complete your sftp setup: 
    1. The Pi Provided to you should have a piece of paper with the following information: 
        - IP address 
        - username 
        - password 
    2. replace the "host", "username" fields in the sftp.json file with the actual information provided to you.
    3. Open a terminal (either through VS code or your computer's own terminal) and type `ssh (username)@(IP address)` (replace with the actual username and IP address provided to you). For example, if the username is "pi" and the IP address is "12.34.56.78", you would type `ssh pi@12.34.56.78`.
    4. You will be prompted to enter the password. Enter the password provided to you. You should now be logged into the Pi through the terminal.
    5. now you can setup a folder that your code will live in.
    ```bash
    # 1. Navigate to the home directory (the safest place for code)
        cd ~
    # 2. Create a new folder (e.g., 'robot_project')
        mkdir robot_project
    # 3. Enter that folder
        cd robot_project
    ```
    6. Now, update the "remotePath" field in your sftp.json file to match the path of the folder you just created. simply copy the path shown in your terminal (after you cd into the folder) and paste it into the "remotePath" field. For example, if your terminal shows that you are in `"/home/pi/robot_project"`, then your "remotePath" should be `"/home/pi/robot_project"`. Ensure the quotes around the path are preserved when you paste it in, and forward slash.

5. ctrl+s in the json file to save the sftp information 

## Prepare for syncing
Be sure you are SSHed into the Pi through the terminal before proceeding with the steps below. 
`ssh (username)@(IP address)`

1. clone the entire repository from github to your local computer if you have not already, and open the folder in VS code.

2. right click on the **Autonomous_Fleets** folder. Scroll down on the list of commands and click sync local-remote. This folder will be synce to the pi at the location specified in the "remotePath" field of your sftp.json file.
    - alternatively, you can also press Ctrl + Shift + P and type SFTP: Sync Local -> Remote to achieve the same result.This will sync the entire challenge folder

3. navigate to the folder on the Pi through the terminal to ensure the files are there. For example, if your remotePath is "/home/pi/robot_project", you can type `cd /home/pi/robot_project` and then `ls` to list the files in that folder. You should see the same files that are in your local folder.

## Workflow for SFTP Sync
1. Edit code on your local computer in VS code.
2. Press Ctrl + S to save the file. This will automatically upload the file to the Pi.
3. in the bottom of VScode, select the Output tab, and select SFTP from the dropdown menu. Look for the "SFTP:uploaded" message to confirm that your file has been uploaded to the Pi. If you see an error message instead, please check your sftp.json configuration and ensure you are SSHed into the Pi through the terminal.

3. Run the code on the Pi through the terminal. For example, if you have a Python script called "main.py", you can run it by typing `python main.py` in the terminal while you are in the correct directory.


