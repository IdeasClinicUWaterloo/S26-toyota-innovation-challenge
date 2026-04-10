# Autonomous Fleets

## Challenge Description

Modern factories and warehouses increasingly rely on fleets of mobile robots to move material, support assembly, and reduce wasted motion. The challenge in this track is not just making one robot move, but coordinating multiple robots safely and predictably in a shared workspace.

Students are invited to build autonomy, coordination, and fleet-management features for a small robotics testbed. Solutions can focus on robot-to-robot coordination, task allocation, path planning, operator tooling, safety behaviors, or telemetry-driven monitoring.

## Potential Solutions

- Multi-robot path planning and collision avoidance
- Fleet dispatching or centralized task arbitration
- Telemetry dashboards for operators and judges
- Safety logic using ultrasonic or other onboard sensors
- Smarter pickup, drop-off, or gripper control workflows
- Reliability improvements for command handling and robot recovery

## Starter Package Overview

This folder contains starter material for the student hackathon. It is intended as a base package that teams can extend during the event, not as a polished production system.

### Included Components

- `starter-code/python-scripts/central-arbiter.py`
  Central laptop/server process that accepts robot connections, displays telemetry in a GUI, sends commands, and supports a two-robot coordinated traverse mode.
- `starter-code/python-scripts/client.py`
  Laptop-side bridge that connects one robot to the central arbiter over TCP and forwards commands to the robot over serial.
- `starter-code/python-scripts/gui.py`
  Telemetry dashboard with robot status, arena visualization, manual controls, test-path buttons, and grid-based coordination controls.
- `starter-code/python-scripts/messages.py`
  Shared message definitions for telemetry, path assignment, acknowledgements, pause/resume/stop, gripper toggle, and heartbeat messages.
- `starter-code/testing-bot-controls/testing-bot-controls.ino`
  Simple Arduino/PRIZM sketch for basic manual movement and gripper testing over serial commands.
- `starter-code/testing-bot-controls/telemetry_and_communicate_to_aribiter/telemetry_and_communicate_to_aribiter.ino`
  More complete robot sketch with odometry, waypoint execution, telemetry publishing, ultrasonic sensor readings, acknowledgements, and command handling.

## Development Setup

### Python Environment

The Python starter code expects a local environment with the packages listed in `starter-code/python-scripts/requirements.txt`.

Example workflow:

1. Create a virtual environment
2. Install the requirements
3. Copy `starter-code/.env.example` to `.env`
4. Update the values for your local machine and robot

### Environment Variables

The provided example environment file includes:

- `SERVER_HOST_IP_ADDRESS`
- `SERVER_PORT`
- `LOCAL_SERIAL_PORT`
- `SERIAL_BAUD`
- `ROBOT_ID`
- `CLIENT_NAME`

These values control how each robot laptop connects to the central arbiter and which serial port is used to talk to the robot controller.

## Suggested Workflow

### 1. Load Robot Firmware

Choose the Arduino sketch that matches your use case:

- Use `testing-bot-controls.ino` for simple manual drive and gripper testing
- Use `telemetry_and_communicate_to_aribiter.ino` for telemetry, waypoint following, and arbiter communication

### 2. Start the Central Arbiter

Run `starter-code/python-scripts/central-arbiter.py` on the machine acting as the server/operator station.

The arbiter:

- Listens for robot laptop connections on port `9000` by default
- Displays robot telemetry in a Tkinter + Matplotlib dashboard
- Tracks robot state, heartbeat, status, and path progress
- Plans grid-based coordinated traverses for two robots in a `4 m x 4 m` arena represented as `40 x 40` cells at `10 cm` resolution

### 3. Start a Robot Client

Run `starter-code/python-scripts/client.py` on the laptop connected to a robot controller.

The client:

- Opens the configured serial port
- Connects to the arbiter over TCP
- Registers the robot with a `hello` message
- Forwards robot telemetry upstream
- Relays path, stop, pause, resume, and gripper commands back to the robot

### 4. Test and Extend

From the GUI you can already:

- Pause, resume, or stop a robot
- Toggle the gripper
- Send straight-line, turn-around, and L-shaped test paths
- Launch a coordinated two-robot traverse using goal cells in the arena grid

Teams can build on this foundation by improving localization, obstacle handling, scheduling logic, fleet behaviors, operator interfaces, or robot hardware integration.

## Notes for Teams

- The included code assumes a shared communication protocol based on newline-delimited JSON messages
- The more advanced robot sketch publishes telemetry including pose and ultrasonic distances
- The starter package currently provides basic coordination logic and operator tooling, but leaves substantial room for student innovation
- If you change robot geometry, motor behavior, or sensor layout, you should review the constants in the Arduino sketch before testing
