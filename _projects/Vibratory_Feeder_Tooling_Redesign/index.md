---
layout: post
title: Vibratory Feeder Tooling & V-Clip Escapement Redesign
description: Redesign and optimization of automated vibratory bowl tooling and inline escapement track for high-speed component feeding. Eliminated chronic part piggybacking and line stoppages through iterative 3D-printed prototyping, tolerance stack-up analysis, air-jet optimization, and precision CNC-machined hardened 440 stainless steel tooling.
skills:
  - CAD (SolidWorks)
  - Design for Manufacturing (DFM)
  - Additive Prototyping (FDM / 3D Printing)
  - Precision Tooling & Escapements
  - Tolerance Stack-up Analysis
  - Change Control & Technical Documentation
main-image: /vclip_redesign_assembly.png
hidden: true
---

# Project Goal

In high-speed medical device automated assembly, reliable parts feeding is critical to maintaining overall equipment effectiveness (OEE). This project focused on troubleshooting, redesigning, and validating the vibratory feeder bowl tooling and track escapement mechanism for V-clip components (Zone 5, Station 70/75). The objective was to eliminate part jams caused by piggybacking and tolerance variations while adapting the solution across multiple high-volume production lines.

---

# Phase 1 – Problem Definition & Root Cause Analysis
{% include image-gallery.html images="vclip_old_escapement_design.png" height="500" %}

**Goal:** Identify root failure modes causing feeder track jams, sensor misfires, and line downtime.

## Failure Mechanisms Identified
- **Part Piggybacking in Feeder Bowls:** Due to their geometry, V-clip components frequently shingled or piggybacked on top of one another along the vibratory track before reaching the escapement nest.
- **Tolerance Stack-Up & Jamming:** Variations in incoming stamped part dimensions caused tight downstream tracks to pinch good parts, leading to intermittent jams.
- **Escapement Transition Gaps:** The legacy top guide and roof tooling left an unconstrained gap near the escapement entrance, allowing piggybacked parts to wedge against the holdback fingers and nest bridges.

## Key Insights
- Rather than solely tightening tolerances, the geometry needed controlled relief channels combined with aerodynamic part separation (boost air jets) to actively de-nest piggybacked clips prior to track entry.

---

# Phase 2 – Iterative Prototyping & Line Testing
{% include image-gallery.html images="vclip_redesign_assembly.png" height="500" %}

**Goal:** Rapidly iterate and test redesigned roof guides, part nests, and air nozzles directly on production tooling.

## Design Highlights
- **Extended Full-Constraint Roof:** Designed a continuous top guide extending fully to the escapement nest to maintain part orientation and prevent shingling.
- **Integrated Boost Air Jet Nozzle:** Modeled custom air-assist geometry engineered to propel the lead V-clip forward, separating it from following clips before entering the single-file track.
- **Dowel-Locating Precision Alignment:** Replaced slotted friction mounts with precision locating dowel pin holes on the top guide mounting plates to ensure repeatable, drop-in alignment during line maintenance.
- **Rapid 3D Print Validation:** 3D printed multiple geometric iterations in tough resin and polymer to evaluate track clearance, clearing accessibility, and operator ergonomics before committing to metal fabrication.

---

# Phase 3 – Tooling Hardening & Multi-Line Adaptation

**Goal:** Transition validated prototypes into robust production tooling and standardize across multiple assembly lines.

## Engineering & DFM Considerations
- **Material Selection:** Selected **440C hardened stainless steel** and **A2 tool steel** for high wear resistance against repetitive friction and vibration from stamped metal clips.
- **Fastener & Hardware Sizing:** Upgraded mounting fasteners from M3 to heavy-duty hardware to withstand continuous high-frequency vibratory loads without loosening.
- **Cross-Line Adaptation (Lines 1, 2, & 3):** Customized mounting brackets and mirrored geometry for station tracks (Tracks 1–4) and integrated fiber-optic / vacuum cube part-presence sensor mounts.
- **Change Control & Quality Documentation:** Authored comprehensive Engineering Change Orders (ECO / CC), updated assembly drawings, and wrote Standard Operating Procedures (SOP) for maintenance calibration.

---

# Results & Impact

- **Zero Piggyback Jams:** Fully eliminated feeder jams caused by part piggybacking in Station 70/75 escapements.
- **Enhanced OEE & Line Uptime:** Dramatically reduced operator intervention and nuisance faults.
- **Standardized Line Tooling:** Successfully deployed robust, precision-doweled tooling across multiple high-volume production lines.
