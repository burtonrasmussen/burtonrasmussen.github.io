---
layout: post
title: Inverted Pendulum Self-Balancing Robot
description: Modeling, embedded firmware development, and custom motor driver hardware design for a two-wheeled self-balancing inverted pendulum robot. Features state-space dynamic modeling, Linear Quadratic Regulator (LQR) feedback control, real-time sensor fusion with an IMU and optical encoders, and a custom KiCAD dual H-bridge motor driver PCB.
skills:
  - Controls Engineering (LQR / State-Space)
  - Embedded Systems & C Programming
  - Microcontroller Integration (STM32)
  - PCB Design & Layout (KiCAD)
  - Sensor Fusion (IMU / Encoders)
  - Dynamic System Simulation (MATLAB)
main-image: /lqr_response.png
hidden: true
---

# Project Goal

The inverted pendulum on a mobile cart is a classic, highly non-linear benchmark problem in control theory and robotics. The goal of this project was to design, model, simulate, and build a self-balancing two-wheeled robot from scratch. The system integrates full-state feedback control (LQR) with real-time embedded sensor fusion to maintain upright stability and reject external disturbances.

---

# Phase 1 – Dynamic Modeling & LQR Control Design
{% include image-gallery.html images="lqr_response.png" height="500" %}

**Goal:** Derive equations of motion and synthesize optimal feedback controllers for stability and disturbance rejection.

## System Modeling & Formulation
- **Nonlinear Dynamics:** Derived governing nonlinear equations of motion using Lagrangian mechanics, coupling cart translational acceleration with pendulum tilt dynamics and wheel rotational inertia.
- **State-Space Linearization:** Linearized the continuous-time dynamics around the upright unstable equilibrium point $\mathbf{x} = [x, \dot{x}, \theta, \dot{\theta}]^T = \mathbf{0}$.
- **Linear Quadratic Regulator (LQR) Tuning:** Formulated quadratic cost weighting matrices ($\mathbf{Q}$ and $\mathbf{R}$) to penalize angular tilt deviation and cart position displacement while optimizing control effort and actuator saturation limits.
- **Simulation & Disturbance Rejection:** Validated closed-loop stability and recovery time against step force impulses and initial angle deflections in simulation.

---

# Phase 2 – Robot Design and hardware selection

**Goal:** Select appropriate hardware for the robot around the STM32 microcontroller and a 24V brushed DC motor, ensuring sensors and drivers will be able to provide adequate control input and sensing of the robot's state at a high enough frequency and resolution for the LQR control law to work

## Hardware & Schematic Architecture
- **Power Train:** Selected a 24V brushed DC motor I had on hand,and used a 3:1 pulley gear ratio. The motor had a built-in 400 CPR quadrature encoder. We went with a POLULU TB8041FTG motor driver due to its high efficiency and ability to drive high current DC motors.
- **KiCAD wiring schematic:** Due to time constraints, we went with a perfboard and point-to-point wiring strategy for the robot, using KiCAD to create a wiring schematic to help keep track of the wiring.
- **Pendulum encoder:** selected a magnetic rotary encoder to provide high resolution feedback on the angle of the pendulum.
---

# Phase 3 – Embedded Firmware & Real-Time Sensor Fusion

**Goal:** Implement real-time control algorithms and sensor acquisition on an embedded microcontroller platform.

## Firmware Implementation Highlights
- **Deterministic Control Loop:** Configured hardware timer interrupts running at 100 Hz to guarantee deterministic sampling and state updates.
- **Sensor Fusion:** Integrated a 6-DOF IMU (accelerometer + gyroscope) using a complementary filter to obtain drift-free, low-latency pitch angle ($\theta$) and angular velocity ($\dot{\theta}$) estimates.
- **Encoder Quadrature Decoding:** Utilized hardware timer encoder interfaces to track wheel positions ($x$) and velocities ($\dot{x}$) without processor overhead.
- **Full-State Feedback Computation:** Calculated real-time motor PWM command voltages via $u = -\mathbf{K}\mathbf{x}$, incorporating anti-windup deadband compensation for motor static friction.

---

# Key Learnings & Future Enhancements

- **Practical Friction & Deadband:** Overcoming real-world motor deadband and gearbox backlash required fine-tuning feedforward friction compensation alongside theoretical LQR gains.
- **Future Direction:** Implementing trajectory tracking for autonomous waypoint navigation and integrating remote Bluetooth / RC telemetry for live parameter tuning.
