# ACME IoT Docker Setup

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
  - [Step 1: Build and Start Services](#step-1-build-and-start-services)
    - [Option A: Attached Mode](#option-a-attached-mode)
    - [Option B: Detached Mode](#option-b-detached-mode)
    - [Rebuild and Start](#rebuild-and-start)
  - [Step 2: Open the UI](#step-2-open-the-ui)
    - [Monitor WebSocket Events and API Network Calls](#monitor-websocket-events-and-api-network-calls)
  - [Step 3: Use the CLI to Register Devices, Simulate Readings, Delete Devices, Update Sensors](#step-3-use-the-cli-to-register-devices-simulate-readings-delete-devices-update-sensors)
    - [Step 3.1: Access acme-simulator](#step-31-access-acme-simulator)
    - [Step 3.2: Simulate Device Readings](#step-32-simulate-device-readings)
    - [Step 3.3: Stop Simulation](#step-33-stop-simulation)
    - [Step 3.4: Delete Devices](#step-34-delete-devices)
    - [Step 3.5: Modify Sensors](#step-35-modify-sensors)
- [Shutdown and Delete Containers](#shutdown-and-delete-containers)
- [License](#license)

## Overview

This project consists of an IoT device and sensor simulator, a REST API backend, and a UI for monitoring real-time updates. The system is containerized using **Docker Compose** and provides WebSocket-driven updates to the UI.

## Getting Started

### Step 1: Build and Start Services
To bring up `ACME-REST`, `acme-ui`, and `ACME-simulator`:

#### Option A: Attached Mode
- Logs are observable in the current shell.

    <br>Initial build:
    ```sh
    docker compose up --build
    ```

#### Option B: Detached Mode
- Runs in the background, no logs.
    
    <br>Initial build:
    ```sh
    docker compose up --build -d
    ```

#### Rebuild and Start
To reset the DB, without going into `ACME-REST` and undoing-redoing migrations:

Stop all running containers, remove volumes, rebuild, and start the services:
```sh
docker compose down -v && docker compose up --build
```

### Step 2: Open the UI
Open the following URL in your browser:
<br>http://localhost:5173/

#### Monitor WebSocket Events and API Network Calls

To observe UI<->REST communications:

1. **Open DevTools**: Right-click → Inspect → Console tab.
2. **View WebSocket logs** in the **Console**.
3. **Check API calls** under the **Network tab** (filter by XHR).

### Step 3: Use the CLI to Register Devices, Simulate Readings, Delete Devices, Update Sensors

#### Step 3.1: Access acme-simulator 
For Step 1. Option A: Open a new shell and enter the following  
For Step 1. Option B: Enter the following in the current shell

```sh
docker exec -it acme-simulator sh
```

Register devices:

```sh
node cli.js register-devices "car=2,truck=2"
```

📌 **Expected Result**: The UI updates and displays 4 devices in the **Registered Devices** table.

Click on the **first device** in the table and notice that it currently has **no readings**.

#### Step 3.2: Simulate Device Readings

Start sensor data simulation for specific devices:

```sh
node cli.js simulate-readings-for-devices 1,3
```

📌 **Expected Result**:

- The selected device now shows readings under the **Sensor Data** table.
- Clicking on another device (e.g., the 3rd device) also shows real-time sensor updates.

#### Step 3.3: Stop Simulation

To stop the running simulation process, press:

- **Ctrl + C**

#### Step 3.4: Delete Devices

Delete a registered device:

```sh
node cli.js delete-device 4
```

📌 **Expected Result**: The last device is removed from the UI.

#### Step 3.5: Modify Sensors

Update a sensor unit:
- With a fresh db and by following the above commands as they are, with the first row (car with ID 1) selected for which data is being simulated:

```sh
node cli.js update-sensor -i 1 -u "px"
```

📌 **Expected Result**: This will update the camera sensor's unit from pixels to px.

### Shutdown and Delete Containers
```sh
docker compose down -v
```

## License

This project is licensed under a private license for educational purposes and the author's skill-set evaluation for job or contract applications only. You may use, modify, and learn from the code provided here solely for your personal educational use or the evaluation of the author's skill-set during a job or contract application process. Redistribution, commercial use, or privately sharing of this code for any other purpose than identified or sharing it publicly in any form for any purpose is strictly prohibited without explicit permission from the author.

By using this software, you acknowledge that it is provided "as is" without any warranties, express or implied, including but not limited to fitness for a particular purpose. The author shall not be held liable for any damages, losses, or other consequences arising from the use or misuse of this software. You agree to indemnify and hold the author harmless from any claims, liabilities, or expenses related to your use of this software.

[Back to top](#acme-iot-docker-setup)
