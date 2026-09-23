---
layout: post
title: Automated Manufacturing SPC & Reject Analysis Tool
description: An interactive desktop application built in Python for Statistical Process Control (SPC) and automated manufacturing reject analysis. Ingests raw MES/Illuminate production logs to generate Analysis of Means (ANOM) charts with binomial decision limits, time-ordered p-charts, rolling reject-rate heatmaps, and zero-configuration executables for plant-floor engineering.
skills:
  - Python (Pandas / NumPy / Matplotlib)
  - Statistical Process Control (SPC / ANOM / p-Charts)
  - GUI Development (Tkinter)
  - Data Visualization & Heatmaps
  - Software Packaging (PyInstaller)
  - Manufacturing Execution Systems (MES)
main-image: /carrier_p_chart.png
hidden: true
---

# Project Goal

In automated manufacturing lines with hundreds of pallets (carriers) and multi-nest tooling, identifying the root cause of intermittent reject spikes can be like finding a needle in a haystack. Standard line dashboards only report aggregate scrap numbers, masking individual carrier defects or nest-specific misalignments. This project aimed to build an intuitive, zero-dependency statistical analysis tool that empowers manufacturing engineers and technicians to ingest raw MES reject history logs and pinpoint exact failure contributors in seconds.

---

# Phase 1 – Statistical Methodology & Analysis Architecture
{% include image-gallery.html images="carrier_p_chart.png" height="500" %}

**Goal:** Establish rigorous statistical methods to distinguish true process shifts and fixture anomalies from random statistical variation.

## Core Statistical Explorers
- **Carrier & Nest ANOM (Analysis of Means):** Computes individual carrier/nest reject fractions against the grand line average using binomial $\pm 3\sigma$ decision limits. Points exceeding Upper Decision Limits (UDL) instantly isolate statistically anomalous tooling or warped pallets.
- **Time-Ordered Line & Per-Carrier p-Charts:** Calculates subgroup reject proportions over 30-minute production intervals with Western Electric / Nelson runs rules to detect out-of-control line excursions and temporal drifts.
- **Rolling Rejection Rate Heatmaps:** Renders high-density matrix heatmaps (Carriers vs. Time) computing rolling 30-minute reject rates per hour. Provides an immediate visual signature of transient jams versus continuous fixture wear across 200+ carriers simultaneously.
- **Interactive Filtering & Window Bracketing:** Implemented dynamic time-window sliders and reject-type filters that dynamically recompute binomial limits and redraw charts in real time.

---

# Phase 2 – Data Pipeline & GUI Engineering

**Goal:** Build a robust, responsive desktop application capable of processing hundreds of thousands of timestamped records without latency.

## Architecture Highlights
- **High-Performance Data Parsing (Pandas):** Automated ingestion of Illuminate / MES CSV exports, cleaning station prefixes (`Z4.C10.S80`), standardizing timezone formats, and parsing nest numbers from hierarchical string descriptions.
- **Modular GUI (Tkinter):** Designed an ergonomic desktop UI with one-click batch analysis, custom carrier filtering, and dedicated live explorer windows for interactive data slicing.
- **Automated Plot Export:** Integrated programmatic high-resolution PNG batch export for direct inclusion in Engineering Change Orders (ECO), validation protocols, and shift handover reports.

---

# Phase 3 – Zero-Friction Deployment & Packaging

**Goal:** Ensure seamless adoption by manufacturing engineers and shop-floor technicians without requiring Python installation or environment troubleshooting.

## Deployment Strategy
- **One-Click Launcher (`run.bat`):** Implemented an automated batch launcher that checks the host system, creates an isolated virtual environment (`.venv`), installs runtime dependencies, and launches the UI seamlessly.
- **Standalone Executable (`PyInstaller`):** Configured automated build pipelines to compile single-file, zero-install executables (`reject_tool.exe`) for restricted enterprise IT environments.
- **Comprehensive User & IT Documentation:** Authored user guides, troubleshooting SOPs, and IT security documentation to facilitate frictionless plant-wide rollout.

---

# Results & Impact

- **Rapid Excursion Triage:** Reduced reject investigation and root-cause isolation time from hours of manual spreadsheet filtering down to under 60 seconds.
- **Tooling Anomaly Detection:** Identified several damaged pallet carriers and worn nest bushings that were causing intermittent station reject spikes undetectable on aggregate line charts.
- **Plant-Floor Adoption:** Deployed as a standard diagnostic tool utilized by process engineers and line technicians.
