---
layout: post
title: Mechatronics Competition Robot
description: As part of a 4-person team for my university's mechatronics course, I led the electrical and drivetrain systems design for a competitive, Minecraft-themed robot. Our robot, which secured 2nd place, was designed to autonomously 'mine' and 'craft' blocks. My key contributions included designing a robust and modular wiring harness, implementing a mecanum wheel drivetrain, significant contributions to code structure and debugging, and overseeing the CAD design in Onshape and preparing files for 3D printing and laser cutting. The project highlights my skills in system integration, control systems, and practical electronics.
skills: 
- System Integration & Modular Design
- Drivetrain Design (Mecanum Wheels)
- Wiring Harness & Electronics Design
- Control Systems (State Machines, P-Control)
- CAD & Digital Fabrication (3D Printing, Laser Cutting)
- Arduino & Embedded Systems
- GitHub & Team Collaboration
main-image: /robot_render.png
---

# Project Overview

For my final mechatronics class project, my team of four was tasked with building a robot for a Minecraft-themed competition. The goal was to "mine" blocks, transport them to a "crafting table," and score points to "upgrade" tools for mining more valuable materials. Our robot successfully accomplished these tasks and earned **2nd place** in the competition.

My primary responsibilities included the design and implementation of the entire electrical system and wiring harness, the in-depth design of the mecanum drivetrain, and overseeing the team's CAD work, including processing all files for laser cutting and 3D printing.

---

# Electrical System Design

{% include image-gallery.html images="all_protoboards_unassembled.jpg" height="400" %}
{% include image-gallery.html images="MEGA_shield_colorcoded_topside.jpg, MEGA_shield_wiring_underside.jpg" height="200" %}
{% include image-gallery.html images="robot_wiring_first_iteration.jpg, robot_wiring_reorganized_iteration.jpg" height="200" %}


**Goal:** Create a reliable, modular, and easy-to-troubleshoot electrical system capable of handling a complex array of motors and sensors.

## Design Highlights
- **Modular Architecture:** The system was divided into logical subsystems (Arduino Mega Shield, Drivetrain, Conveyor, Sensors), each on its own protoboard. This kept wiring clean and simplified assembly.
- **Robust Connectors:** I used 10-pin IDC ribbon cables for logic signals between boards and JST connectors for all external components (motors, sensors, switches). This created a robust system that made assembly and debugging efficient.
- **Error-Proofing:** All connectors were keyed and color-coded to prevent incorrect connections, a critical feature that prevented catastrophic failures during testing and competition.

## Lessons Learned
Well-organized wiring was our biggest competitive advantage. While other teams struggled with electrical faults, our modular and robust design allowed for rapid troubleshooting and iteration, ensuring we had more time to optimize our design.

---

# Drivetrain & Mechanical Systems
{% include image-gallery.html images="robot_render.png, robot_mining_example.jpg, robot_mining_loading_example.jpg, robot_crafting_example.jpg" height="800" %}

**Goal:** Minimize the number of movements needed to position our robot on the playing field, and maintain accuracy, utilizing line sensors for error correction.

## Design Highlights
- **Mecanum Drive:** A 4-wheel mecanum drive provided holonomic movement (the ability to move in any direction), giving us superior maneuverability compared to other teams.
- **Digital Fabrication:** I managed the CAD workflow, using our team's models to laser cut the chassis and 3D print custom mounts and components on my own 3D printer.
- **Closed-Loop Control:** Quadrature encoders were mounted to each drivetrain motor, providing the feedback necessary for precise position control.
- **Error Correction:** Two reflectance arrays are used to detect lines on the playing field, which allows the robot to correct for slippage errors typical to mecanum drivetrains.

## Challenges
- The mecanum system significantly increased complexity, roughly doubling the wiring and motor control outputs required compared to a standard 2WD setup. This placed heavy demands on the Arduino Mega's available I/O pins and required careful planning.

---

# Control Systems & Software Architecture
{% include image-gallery.html images="trapezoidal_trajectory_example.jpg" height="400" %}

**Goal:** Develop a non-blocking, multi-tasking control system for smooth, efficient, and autonomous operation.

## Design Highlights
- **State Machine Logic:** The code was built on a non-blocking state machine architecture. Each subsystem (drivetrain, conveyor, sensors) ran its own independent state machine, allowing for effective multi-tasking. This was a major advantage, enabling our robot to perform multiple actions simultaneously.
- **Trapezoidal Motion Profiling:** Rather than a more complex PID controller, we used trapezoidal trajectories with a simple Proportional (P) controller. This provided smooth acceleration and deceleration, and reliably positioned our robot within **+/- 10 encoder counts**—a high degree of precision.
- **Advanced Sensing:** The robot integrated a suite of sensors for navigating the field and interacting with game elements, including a reflectance array for line centering, distance sensors, color sensors, limit switches, and a hall effect sensor.
- **Collaborative Workflow:** Our team used **GitHub** for version control, which was essential for organizing code contributions and maintaining a stable codebase.

## Lessons Learned
- We learned that the simplest effective solution is often the best. After testing, we found that advanced signal filtering (with op-amps or digital filters) was unnecessary for our sensors. Likewise, P-control with motion profiling was far easier to tune and more reliable than a full PID loop for our needs. This focus on practical, validated solutions was key to our success.
