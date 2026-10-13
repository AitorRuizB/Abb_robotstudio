# Physical ABB IRB 120 Dual-Robot Coordination via TCP/IP Sockets 🤖🔌🤖

<p align="center">
  <img src="https://img.shields.io/badge/Hardware-ABB_IRB_120-FF0000?style=for-the-badge" alt="ABB IRB 120">
  <img src="https://img.shields.io/badge/Python-3.12-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54" alt="Python 3.12">
  <img src="https://img.shields.io/badge/RAPID-IRC5_Controller-orange?style=for-the-badge" alt="RAPID">
  <img src="https://img.shields.io/badge/Comms-TCP%2FIP_Sockets-008080?style=for-the-badge" alt="TCP/IP Sockets">
</p>

<p align="center">
  <!-- Optional: Replace demo_real.gif with your actual GIF filename if you generate one -->
  <img src="demo_real.gif" alt="Real ABB IRB 120 Cooperative Box Lifting" width="650">
</p>

## 📌 Overview

This project implements a real-world cooperative manipulation task using **two physical ABB IRB 120 industrial manipulators**. A central **Python 3.12 orchestrator** communicates with both IRC5 robot controllers over **TCP/IP Sockets** to coordinate the progressive, turn-based lifting and lowering of a shared payload (box) within a shared physical workspace.

* 🎥 **Video Demonstration:** [Watch the physical execution on YouTube](https://youtu.be/epKAPwj1lvU)

## ⚙️ System Architecture & Communication Protocol

### 1. Client-Server Socket Architecture
* **Central Orchestrator (Python 3.12):** Acts as the coordinator managing the IP addresses and ports of both IRC5 controllers. It establishes the socket connections, verifies link integrity, and dispatches `UTF-8` encoded state commands.
* **Synchronous Handshake (`Command` -> `DONE`):** To guarantee collision-free execution in the shared workspace, the system implements a strict blocking acknowledgment protocol:
  1. The Python node sends a phase command to Robot A.
  2. Robot A parses the string, executes the physical trajectory, and replies with a `'DONE'` message upon reaching the target pose.
  3. Only after receiving `'DONE'` does the Python script issue the next step to Robot B.

### 2. Task Sequence & State Machine
The collaborative lifting operation is divided into **6 sequential phases** dispatched to the robots:

    HOME -> PRECOGER (Pre-grasp) -> COGER (Grasp) -> SUBIDA (Lift) -> BAJADA (Lower) -> HOME

* **Dynamic Z-Offsets:** Instead of teaching excessive static points via the FlexPendant, the `SUBIDA` (Lift) and `BAJADA` (Lower) phases utilize parameterized RAPID functions that dynamically apply vertical Cartesian offsets (`Offs` along the Z-axis) relative to the taught grasp pose.

### 3. RAPID Controller Logic & Kinematics
* **Online Calibration:** Base target waypoints were manually taught on the physical workcell using the **ABB FlexPendant** and synchronized with the RAPID `switch` (`TEST / CASE`) command parser via RobotStudio.
* **Motion Profiling:** Velocity and interpolation types (`MoveJ` vs. `MoveL`) are tailored to the physical constraints of each task phase. Cartesian linear interpolation is strictly enforced during payload contact to maintain constant TCP orientation and prevent box twisting or slippage.

## 📊 Trajectory & Velocity Profile

| Task Phase | Command Tag | TCP Velocity (mm/s) | Interpolation Type | Engineering Justification |
| :--- | :--- | :--- | :--- | :--- |
| **Pre-Grasp** | `PRECOGER` | `v100` | Joint (`MoveJ`) | Unconstrained approach to lateral safety offset |
| **Grasp** | `COGER` | `v50` | Cartesian (`MoveL`) | Linear contact approach preserving tool orientation |
| **Progressive Lift** | `SUBIDA` | `v30` | Cartesian (`MoveL`) | High-precision vertical Z-offset steps under load |
| **Progressive Lower**| `BAJADA` | `v30` | Cartesian (`MoveL`) | Controlled descent maintaining lateral pressure |
| **Return Home** | `HOME` | `v200` | Joint (`MoveJ`) | Fast clearance back to home configuration |

## 👥 Authors
* **Aitor Ruiz Bautista**
* **Juan Pérez Fernández**
