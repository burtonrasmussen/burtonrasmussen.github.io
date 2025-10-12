---
layout: post
title: Arduino IR Universal Remote
description: This project showcases my electronics skills and design approach in making an ESP-32 universal IR remote. It showcases my process from initial concept sketching and circuit prototyping to PCB design. The project is currently in the PCB design phase, demonstrating my commitment to seeing a complex project through to a polished final product.

skills: 
- Circuit Design & Prototyping
- PCB Design (KiCAD)
- ESP32/Arduino
- Soldering & Electronics Assembly
- Component Selection
main-image: /kicad_big_pcb.png
---

# Project Goal

To design and build a programmable, ESP-32 powered IR remote capable of replacing multiple household remotes into a single, custom device. The project focuses on learning new skills in order to prototype the devices' functionality and feasability. The end goal is to develop a compact and ergonomic final product using a manufactured pcb and 3D printed enclosure.

---

# Phase 1 – Concept and Ergonomics
{% include image-gallery.html images="button_layout_brainstorm.jpg" height="400" %}

**Goal:** Define the core functionality and create an intuitive and comfortable button layout.

## Design Highlights
- Researched existing remote control layouts to identify best practices for button placement and grouping.
- Sketched design on paper to finalize the user interface before committing to an electronic design. This allowed for rapid, low-cost exploration of different ergonomic concepts.

## Lessons Learned
- This phase emphasized the importance of referencing existing work, and getting user feedback. Finalizing the physical layout first helped define the technical requirements for the electronics, rather than letting the electronics dictate the user experience.

---

# Phase 2 – Electronics Prototyping

**Goal:** Design and build a functional proof-of-concept circuit to validate the electronic design before creating a permanent and compact PCB.
{% include image-gallery.html images="diylayoutcreator_schematic.png, prototype_circuit.jpg" height="800" %}

## Design Highlights
- Developed a circuit to read a 9x3 button array using a minimal number of pins on the ESP-32 microcontroller.
- Implemented a diode matrix and shift registers to efficiently manage the 33 button inputs.
- Designed a stripboard layout using **diylayoutcreator** to plan the physical prototype.
- Assembled and tested the circuit on protoboard and breadboard to confirm the design's functionality.
- Wrote Arduino code to test the OLED display and reading and sending IR signals.

## Challenges
- The large number of buttons exceeded the available GPIO pins on the ESP32, which required me to research and learn more advanced input handling techniques to solve the problem.
- The IR emmitting LED required amplification in order for other devices to pick up it's signal from larger distances.
- 


## Lessons Learned
- Gained practical experience with circuit design principles, including the application of shift registers and diode matrices for input expansion.
- Honed skills in soldering and physical prototyping

---

# Phase 3 – PCB Design & Miniaturization (Current Stage)
{% include image-gallery.html images="kicad_schematic.png" height="600" %}
{% include image-gallery.html images="kicad_big_pcb.png" height="400" %}
{% include image-gallery.html images="kicad_pcbdesigner.png" height="400" %}


**Goal:** Transition the validated prototype circuit into a compact and professional Printed Circuit Board (PCB) using KiCAD.

## Design Highlights
- Created a complete electrical schematic and a 2-layer PCB layout in **KiCAD**.

## Challenges
- The initial PCB layout using through-hole components was too large for an ergonomic remote. This created the need to learn SMD-based design, which is what the current stage of this project is.

## Lessons Learned
- This stage is providing valuable experience in Design for Manufacture (DFM) and the trade-offs between different component packages (Through-Hole vs. Surface Mount) in terms of space, cost, and ease of assembly.

---

# Future Direction

- **Finalize PCB Design:** Complete the SMD-based PCB layout and send it for fabrication.
- **Firmware Development:** Write the Arduino code to manage button scanning, IR code learning, and transmission, utilizing the OLED display to provide useful information to the user.
- **Enclosure Design:** Once the final PCB dimensions are confirmed, I will design and 3D print a custom, ergonomic enclosure to house the electronics.
