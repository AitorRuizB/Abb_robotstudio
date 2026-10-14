# 🏭 ABB RobotStudio Simulation Projects

<p align="center">
  <img src="https://img.shields.io/badge/ABB-RobotStudio-ff0000?style=for-the-badge" alt="ABB RobotStudio">
  <img src="https://img.shields.io/badge/Language-RAPID-0055a5?style=for-the-badge" alt="RAPID">
  <img src="https://img.shields.io/badge/Modeling-Fusion360-ff7900?style=for-the-badge" alt="Fusion360">
</p>

## 📌 Overview
This repository contains advanced industrial robotic simulations developed in **ABB RobotStudio**. The projects demonstrate proficiency in offline programming, **RAPID** coding, multi-robot coordination, and the integration of **Smart Components** to create realistic industrial automation cells.

---

## 🤖 1. Dual IRB 120 Multi-Robot System
This project focuses on the coordinated operation of two ABB IRB 120 robots sharing the same workspace, executing complex collaborative tasks.

### ⚙️ Key Features:
* **Synchronized Trajectories:** Implementation of I/O handshaking (`WaitDI`, `SetDO`) between virtual controllers to ensure perfect synchronization during movements.
* **Cooperative Manipulation:** Both robots locate a shared payload using linear search routines (`SearchL` with simulated laser sensors) and lift it simultaneously without collisions.
* **Jenga Tower Assembly:** Alternating turn-based logic to build a Jenga tower. The RAPID code dynamically calculates placement offsets (height and 90-degree rotations) based on the number of pieces already placed.

---

## 📦 2. Smart Sorting Cell (Multi-Conveyor)
An automated pick-and-place industrial station designed to classify and route boxes of three different sizes arriving from random input conveyors.

### ⚙️ Key Features:
* **Custom Tooling:** Design and configuration of a dual-TCP vacuum gripper (modeled in Fusion360) adapted for different box dimensions.
* **Smart Components Integration:** Extensive use of simulated logic to mimic a real factory environment:
  * `Source` & `Queue`: Random generation of boxes across 3 input conveyors.
  * `LinearMover` & `Sink`: Conveyor belt physics and object disposal.
  * `LineSensor`: Detection of box height and conveyor origin.
  * `Attacher` / `Detacher`: Logic to simulate the vacuum gripping of objects.
* **Dynamic Routing:** RAPID logic that reads sensor arrays, selects the appropriate TCP, picks the box, and routes it to one of the 3 corresponding output conveyors.

---

## 💻 Technologies & Skills
* **Environment:** ABB RobotStudio (Virtual Controllers IRC5).
* **Programming:** RAPID (Motion control, offset logic, I/O handling, conditional routing).
* **Simulation:** Smart Components, I/O Cross Connections, Signal configuration.
* **CAD:** Autodesk Fusion360 for end-effector modeling.

---
