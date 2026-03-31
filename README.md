# ardupilot-waypoint-mission

> **Simulated autonomous multi-waypoint UAV mission using ArduPilot SITL (ArduCopter) and QGroundControl. No hardware required.**

This project demonstrates the design, validation, and execution of a fully autonomous drone mission. By leveraging **Software-In-The-Loop (SITL)**, I simulated an ArduCopter navigating complex GPS paths, validating MAVLink commands and autopilot behavior before deployment to physical flight controllers like Pixhawk.

---

## 📺 Demo & Visualization

![Mission demo](media/demo.gif)
![29AF50B2-29D8-458A-A4EC-EF9979043CDD_1_102_o](https://github.com/user-attachments/assets/c8367853-6c25-4068-ab4a-26919e0c21ee)


| Mission Plan | Flight Execution |
| :--- | :--- |
|<img width="1440" height="900" alt="Screenshot 2026-03-31 at 8 30 47 PM" src="https://github.com/user-attachments/assets/ade41ab8-8eb0-4dfc-9bf0-f9375c09d6ed" />
  |<img width="1440" height="900" alt="Screenshot 2026-03-31 at 8 31 00 PM" src="https://github.com/user-attachments/assets/90c84a94-d26d-496e-be22-08ec4e651e0f" />
 |

---

## 🛠️ Tech Stack & Architecture

| Tool | Role |
| :--- | :--- |
| **ArduPilot SITL** | High-fidelity flight controller & physics simulation |
| **MAVProxy** | Ground station software & MAVLink packet routing |
| **QGroundControl** | GCS for mission planning, GUI monitoring, and telemetry |
| **MAVLink Protocol** | Communication pipeline for mission items and commands |
| **Python + pymavlink** | (Optional) Programmatic control and automation |

### System Data Flow
```text
ArduPilot SITL (ArduCopter)
        |
    MAVProxy
   /        \
QGroundControl    [pymavlink scripts — optional]

### 1. Start SITL

```bash
cd ardupilot
sim_vehicle.py -v ArduCopter --console --map
```

### 2. Connect QGroundControl

Open QGroundControl. It auto-connects to SITL on UDP port 14550.

### 3. Load the mission

- Open **Plan** view in QGroundControl
- Click **Open** → select `mission/square_mission.plan`
- Click **Upload** to send to the simulated vehicle

### 4. Fly

- Switch to **Auto** mode
- Click **Start Mission**
- Watch the drone follow the waypoints on the map

---

## Mission parameters

| Parameter | Value |
|-----------|-------|
| Takeoff altitude | 20 m |
| Cruise speed | 5 m/s |
| Number of waypoints | 13 |
| Total path distance | ~2740 m |
| Flight mode | AUTO |
| RTL altitude | 10 m |

---

## What I observed

- Path tracking was accurate within ~2–3 m of planned waypoints
- Altitude held steady throughout (no significant oscillation)
- Turns at waypoints showed slight overshoot at higher speeds — reducing cruise speed to 3 m/s improved turn precision
- RTL and landing completed successfully

---

## Key concepts demonstrated

- MAVLink mission item protocol (MAV_CMD_NAV_WAYPOINT, TAKEOFF, RETURN_TO_LAUNCH)
- Autopilot flight mode management (GUIDED → AUTO)
- GCS–autopilot communication pipeline
- SITL as a hardware-free validation environment

---

## Setup

```bash
# Clone ArduPilot
git clone https://github.com/ArduPilot/ardupilot
cd ardupilot
git submodule update --init --recursive

# Install dependencies (macOS)
Tools/environment_install/install-prereqs-mac.sh

# Install Python libs
pip install pymavlink MAVProxy
```

---

## Next steps

- [ ] Script mission upload via pymavlink (no GUI)
- [ ] Add telemetry logging and plot altitude/speed profile
- [ ] Implement geofence boundary
- [ ] Test coverage path planning algorithm

---

## Author

**Neha Priya** — ECE Student, Mahindra University   
[LinkedIn] www.linkedin.com/in/v-neha-priya-j
