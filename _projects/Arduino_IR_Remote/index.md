---
layout: post
title: Arduino IR Universal Remote
description: This project showcases my electronics skills and design approach in making a universal IR remote. It showcases my process from initial concept sketching and circuit prototyping to PCB design. The project is currently in the PCB design phase, demonstrating my commitment to seeing a complex project through to a polished final product.

skills: 
- Circuit Design & Prototyping
- PCB Design (KiCAD)
- Microcontroller Programming
- Component Selection
- Soldering & Electronics Assembly
main-image: /pcb_top_poured.png
---

# Project Goal

To design and build a programmable, Microcontroller-powered IR remote capable of replacing multiple household remotes into a single, custom device. The project focuses on learning new skills in order to prototype the devices' functionality and feasability. The end goal is to develop a compact and ergonomic final product using a manufactured pcb and 3D printed enclosure.

---

# Phase 1 – Concept and Ergonomics
{% include image-gallery.html images="button_layout_brainstorm.jpg" height="600" %}

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

## Lessons Learned
- Gained practical experience with circuit design principles, including the application of shift registers and diode matrices for input expansion.
- Honed skills in soldering and physical prototyping.

---

# Phase 3 – PCB Design & Miniaturization (V2 SMD Prototype)
{% include image-gallery.html images="pcb_top_poured.png, pcb_bottom_poured.png" height="500" %}
{% include image-gallery.html images="kicad_schematic.png" height="600" %}

**Goal:** Transition the validated prototype circuit into a compact, professional PCB using KiCAD.

## Design Highlights
- Transitioned from bulky through-hole components to surface-mount technology (SMD) to dramatically reduce board area and maintain an ergonomic handheld form factor.
- Routed a multi-layer PCB in **KiCAD** incorporating ground and power copper pours for signal integrity and low noise.
- Integrated dedicated power regulation circuitry (boost regulator and MOSFET power switching), OLED header, and tactile button matrix.
- Used a 16-channel button matrix multiplexer IC to reduce the number of pins required to read the button array from 27 to 7 GPIO pins.

## Challenges
- Managing high trace density while preserving ground planes
- Could not decide on a microcontroller yet, so ended up using pin headers to allow for testing the compact PCB design while allowing for the flexibility to stick with using development boards like the STM32Nucleo and NRF52840.

## Lessons Learned
- Gained experience with Design for Manufacture (DFM) and SMD component selection.

---

# Future Direction

- **Fabrication & Assembly:** Send the SMD PCB layout for fabrication and solder the board for testing.
- **Firmware Development:** Finalize the embedded firmware using the TCA8418 for button matrix scanning, deep sleep power management, IR code learning/transmission, and OLED menu navigation.
- **Enclosure Design:** Design and 3D print an ergonomic handheld enclosure with the manufactured PCB dimensions.
