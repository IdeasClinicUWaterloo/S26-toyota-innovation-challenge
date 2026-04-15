# Autonomous Fleets Starter Code Guide

This starter code is a small fleet-management stack for PRIZM-based mobile robots. It is built around one central computer running the arbiter GUI, one laptop per robot running the client bridge, and robot firmware that receives serial commands and reports telemetry.

## System Architecture

```text
Operator computer
  central-arbiter.py
  gui.py
        |
        | newline-delimited JSON over TCP
        v
Robot laptop
  client.py
        |
        | newline-delimited JSON or compact control opcodes over serial
        v
PRIZM robot controller
  telemetry_and_communicate_to_aribiter.ino
```

The system intentionally keeps the protocol simple. Every normal message is a single JSON object followed by a newline. The laptop bridge reads complete lines from one side, validates or compresses them when needed, and forwards them to the other side.

## Main Pieces

- `python-scripts/central-arbiter.py` is the server process. It accepts robot laptop connections, tracks connected robots, updates the GUI, and sends commands back to robots.
- `python-scripts/gui.py` is the Tkinter and Matplotlib operator interface. It displays robot telemetry and sends manual or test-path commands through the arbiter.
- `python-scripts/client.py` runs on the robot laptop. It connects TCP to the arbiter and serial to the PRIZM controller.
- `python-scripts/messages.py` defines Python dataclasses for the shared message shapes used by the GUI and arbiter.
- `testing-bot-controls/testing-bot-controls.ino` is a simple manual-control sketch for checking motors and the gripper.
- `testing-bot-controls/telemetry_and_communicate_to_aribiter/telemetry_and_communicate_to_aribiter.ino` is the fuller robot sketch for telemetry, waypoint execution, and arbiter communication.

## Typical Run Order

1. Copy `.env.example` to `.env` on the robot laptop.
2. Set `SERVER_HOST_IP_ADDRESS` to the arbiter computer's IP address.
3. Set `LOCAL_SERIAL_PORT` to the PRIZM serial port, such as `COM5`.
4. Set a unique `ROBOT_ID` for each robot, such as `robot_A` or `robot_B`.
5. Install the Python requirements in `python-scripts/requirements.txt`.
6. Upload the telemetry Arduino sketch to the robot controller.
7. Start `python-scripts/central-arbiter.py` on the operator computer.
8. Start `python-scripts/client.py` on each robot laptop.
9. Use the GUI to inspect telemetry, send test paths, pause, resume, stop, or toggle the gripper.

## Runtime Data Flow

Startup:

1. The arbiter opens a TCP server on port `9000`.
2. A robot laptop opens its serial port and connects to the arbiter.
3. The client sends a `hello` message with its `robot_id`.
4. The arbiter binds that TCP session to the robot ID and starts displaying future telemetry for that robot.

Telemetry:

1. The Arduino sketch periodically prints a `telemetry` JSON line.
2. The client reads it from serial.
3. The client forwards it to the arbiter over TCP.
4. The arbiter stores the latest telemetry and calls `TelemetryGUI.update_robot`.

Commands:

1. The GUI builds a command message, usually using classes from `messages.py`.
2. The arbiter sends the message to the selected robot session.
3. The client forwards the message to serial.
4. The Arduino sketch handles the command and prints acknowledgements, status updates, and path events.

## Important Assumptions

- Coordinates are in centimeters.
- The arena used by the GUI and coordinated traversal is `400 cm x 400 cm`.
- The grid planner uses `10 cm` cells, so the arena becomes a `40 x 40` grid.
- Each robot ID must be unique while connected to the arbiter.
- The advanced robot sketch starts with an initial pose of `x=200 cm`, `y=100 cm`, and `theta=90 degrees`. Change those values before testing if the physical robot starts somewhere else.

## Where To Extend

- Add smarter path planning in `central-arbiter.py`.
- Add more operator controls or richer visualizations in `gui.py`.
- Add new message types in `messages.py`, then teach the arbiter, client, and firmware how to handle them.
- Improve odometry or add sensor-based safety stops in the telemetry Arduino sketch.
- Replace the simple one-waypoint-at-a-time sequencing with a richer reservation or traffic-management system.

