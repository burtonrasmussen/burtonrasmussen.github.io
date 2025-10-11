---
layout: post
title: Arduino IR Universal Remote
description: This project documents the design and development of a custom, ESP32-based universal IR remote. It showcases a journey from initial concept sketching and circuit prototyping to advanced PCB design, highlighting skills in electronics, embedded systems, and design for manufacturability. The project is currently in the PCB design phase, demonstrating a commitment to seeing a complex project through to a polished final product.

skills: 
- Circuit Design & Prototyping
- PCB Design (KiCAD)
- Schematic Capture
- Embedded Systems (ESP32/Arduino)
- Soldering & Electronics Assembly
- Component Selection (THT vs. SMD)
main-image: /path/to/your/main-image.jpg
---

# Project Goal

To design and build a programmable, ESP32-powered infrared (IR) remote capable of consolidating multiple household remotes into a single, custom device. The project focuses on creating a compact and ergonomic final product by progressing from paper sketches to a custom-designed PCB and enclosure.

---

# Phase 1 – Concept and Ergonomics

**Goal:** Define the core functionality and create an intuitive and comfortable button layout.

## Design Highlights
- Researched existing remote control layouts to identify best practices for button placement and grouping.
- Sketched multiple design iterations on paper to finalize the user interface before committing to an electronic design. This allowed for rapid, low-cost exploration of different ergonomic concepts.

## Lessons Learned
- This phase emphasized the importance of user-centric design. Finalizing the physical layout first helped define the technical requirements for the electronics, rather than letting the electronics dictate the user experience.

---

# Phase 2 – Electronics Prototyping

**Goal:** Design and build a functional proof-of-concept circuit to validate the electronic design before creating a permanent PCB.

## Design Highlights
- Developed a circuit to read an 11x3 button array using a minimal number of pins on a compact ESP32 Super Mini microcontroller.
- Implemented a diode matrix and shift registers to efficiently manage the 33 button inputs.
- Designed a stripboard layout using **diylayoutcreator** to plan the physical prototype.
- Assembled and tested the circuit on protoboard and breadboard to confirm the design's functionality.

## Challenges
- The large number of buttons exceeded the available GPIO pins on the compact ESP32, which required me to research and learn more advanced input handling techniques to solve the problem.

## Lessons Learned
- Gained practical experience with circuit design principles, including the application of shift registers and diode matrices for input expansion.
- Honed skills in soldering and physical prototyping to create a reliable testbed for software development.

---

# Phase 3 – PCB Design & Miniaturization (Current Stage)

**Goal:** Transition the validated prototype circuit into a compact and professional Printed Circuit Board (PCB) using KiCAD.

## Design Highlights
- Created a complete electrical schematic and a 2-layer PCB layout in **KiCAD**.
- Currently redesigning the board to use **Surface Mount Devices (SMD)** for buttons, resistors, and diodes. This transition is critical for achieving the miniaturization required for a handheld device.

## Challenges
- The initial PCB layout using through-hole components was too large for an ergonomic remote. This created the need to learn SMD-based design, which introduces new constraints for component selection and routing.

## Lessons Learned
- This stage is providing valuable experience in Design for Manufacture (DFM) and the trade-offs between different component packages (Through-Hole vs. Surface Mount) in terms of space, cost, and assembly.

---

# Future Direction

The next steps will focus on finalizing the hardware and moving into enclosure design:

- **Finalize PCB Design:** Complete the SMD-based PCB layout and send it for fabrication.
- **Firmware Development:** Write the Arduino code to manage button scanning, IR code learning, and transmission.
- **Enclosure Design:** Once the final PCB dimensions are confirmed, I will design and 3D print a custom, ergonomic enclosure to house the electronics.
