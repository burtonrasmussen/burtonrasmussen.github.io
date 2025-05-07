---
layout: post
title: Dobsonian Telescope Project
description: This project showcases the iterative design and development of a computerized Dobsonian telescope system. Over several years, I have refined the mechanical structure, explored computer-guided tracking using the OnStep open-source controller, and tested approaches to improve portability, rigidity, and ease of assembly. Each version builds on lessons from the last, integrating CAD modeling, physical prototyping, CNC fabrication, and electronics assembly to create a functional, transportable telescope mount.

skills: 
- Mechanical Design
- CAD (SolidWorks, Fusion 360, Onshape)
- 3D Printing & Design for Manufacture
- Soldering & Electronics Assembly
- CNC Routing
main-image: /20240720_063313-cropped.jpg
---

# Iteration 1 – 2021  
{% include image-gallery.html images="1_Iteration_thumbnail.jpg" height="400" %}

**Goal:** Implement a motorized tracking system for stargazing under a $200 budget using the OnStep open-source controller.

## Design Highlights
- Low-cost materials and 3D-printed parts
- Custom motor mounts and belt-driven alt-az axes
- Prototyping and iteration via Onshape CAD and hand-built components

## Electronics & Control
- Soldered and assembled OnStep controller board
- Tuned configuration parameters for NEMA17 motors
- Basic slewing and tracking functions tested

## Lessons Learned
- Electronics added complexity during assembly/disassembly
- Mechanical rigidity and belt tensioning issues impacted performance
- Learned soldering and practical troubleshooting for embedded systems

---

# Iteration 2 – 2023  
{% include image-gallery.html images="2_Iteration_thumbnail.jpg" height="400" %}

**Goal:** Improve portability with a truss design and reuse original telescope parts for accessibility and modularity.

## Design Highlights
- Designed a collapsible truss structure for easier transport
- Integrated reused optics and hardware to keep the project approachable
- Fabricated components using manual woodworking and 3D printing

## Challenges
- Manual fabrication lacked precision despite use of jigs
- Structural rigidity was insufficient for reliable motorized tracking

## Lessons Learned
- Precision limitations of manual fabrication tools constrained performance
- Deferred re-integrating electronics due to structural instability

---

# Iteration 3 – 2024  
{% include image-gallery.html images="20240720_063313-cropped.jpg" height="400" %}

**Goal:** Increase rigidity and dimensional accuracy through CNC-routed parts and refine CoG adjustment features for improved balance.

## Design Highlights
- Fabricated base and truss connectors using a CNC router for higher accuracy
- Used Python to model spring counterbalance and determine optimal spring constant and mounting point
- Added CoG-adjustment slots for flexibility with upper tube weights

## Challenges
- Upper tube assembly was too heavy for design intent
- Truss connections were difficult to assemble, reducing portability
- Required excessive CoG shift that negated space-saving benefits

## Lessons Learned
- Improved early prototyping using small-scale mockups
- Identified limitations in current structural and connection approach

---

# Future Direction

The next revision will focus on:

- Weight reduction in the upper tube assembly by designing a custom lightweight focuser and secondary mirror spider
- Quick-release truss connectors to reduce assembly time
- Lower tube brackets for simplified truss mounting
- More extensive use of scaled prototypes to validate design concepts early

---
