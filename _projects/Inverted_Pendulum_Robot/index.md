---
layout: post
title: Inverted Pendulum Self-Balancing Robot
description: Modeling, dynamic simulation, embedded firmware development, and hardware design for a two-wheeled self-balancing inverted pendulum robot. Features Euler-Lagrange dynamic modeling, motor voltage-input state-space formulation, Linear Quadratic Regulator (LQR) feedback control, real-time 1 kHz sensor feedback, and custom KiCAD wiring.
skills:
  - Controls Engineering (LQR / State-Space)
  - Dynamic System Simulation (Python / MATLAB)
  - Embedded Systems & C Programming (STM32)
  - Electrical Schematics (KiCAD)
  - Actuator Modeling & Motor Drives
main-image: /lqr_response.png
---

# Project Goal

The inverted pendulum on a mobile cart is a classic non-linear benchmark problem in control theory and robotics. The goal of this project was to design, model, simulate, and build a self-balancing robot from scratch. The system integrates full-state feedback control (LQR) derived from first-principles Lagrangian dynamics to maintain upright stability and reject external disturbances in real time.

<div style="margin: 15px 0; display: flex; gap: 12px; flex-wrap: wrap;">
  <a href="https://github.com/burtonrasmussen/Pendulum-Robot" target="_blank" rel="noopener noreferrer" style="background-color: var(--link-color, #4a76ee); color: white; padding: 8px 16px; border-radius: 6px; text-decoration: none; font-weight: 500; display: inline-flex; align-items: center; gap: 6px;">
    <i class="fa-brands fa-github"></i> View Firmware Code on GitHub
  </a>
  <a href="Inverted_Pendulum_Control_Derivation.pdf" target="_blank" rel="noopener noreferrer" style="background-color: var(--light-background-color, #f3f5fb); color: var(--text-color, #1a1c20); border: 1px solid var(--border-color, #ddd); padding: 8px 16px; border-radius: 6px; text-decoration: none; font-weight: 500; display: inline-flex; align-items: center; gap: 6px;">
    <i class="fa-solid fa-file-pdf"></i> Download Handwritten Derivation (PDF)
  </a>
</div>

---

# Phase 1 – Dynamic Modeling, Voltage Input Formulation & LQR Simulation

**Goal:** Derive the governing equations of motion from first principles, model the DC motor voltage-to-force actuator dynamics, and synthesize an optimal LQR controller in simulation before building physical hardware.

<div style="margin: 20px 0; text-align: center;">
  <video width="100%" style="max-width: 650px; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" controls autoplay loop muted playsinline>
    <source src="pendulum_sim.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <p style="font-size: 0.85em; color: #666; margin-top: 6px;"><em>Simulation of the inverted pendulum cart stabilizing from an initial tilt disturbance under LQR feedback control.</em></p>
</div>

{% include image-gallery.html images="lqr_response.png" height="400" %}

## 1. Lagrangian System Dynamics
Using the Euler-Lagrange formulation $\frac{d}{dt}\left(\frac{\partial L}{\partial \dot{q}_i}\right) - \frac{\partial L}{\partial q_i} = Q_i$ with generalized coordinates $\mathbf{q} = [x, \theta]^T$, the nonlinear equations coupling the cart position ($x$) and pendulum tilt angle ($\theta$) are:

$$ (M + m)\ddot{x} + m l \ddot{\theta}\cos\theta - m l \dot{\theta}^2\sin\theta = F_x $$

$$ (I + ml^2)\ddot{\theta} + m l \ddot{x}\cos\theta - m g l \sin\theta = 0 $$

Linearizing around the upright equilibrium point ($\theta \approx 0$, $\cos\theta \approx 1$, $\sin\theta \approx \theta$, $\dot{\theta}^2 \approx 0$):

$$ (M + m)\ddot{x} + m l \ddot{\theta} = F_x $$

$$ (I + ml^2)\ddot{\theta} + m l \ddot{x} = m g l \theta $$

## 2. Motor Actuator & Voltage Input Model
To directly control the robot via PWM duty cycle on the microcontroller, the force $F_x$ was derived as a function of the motor armature terminal voltage $V_{in}$:

$$ F_x = \frac{k_t G}{R r} V_{in} - \frac{k_t k_b G^2}{R r^2} \dot{x} $$

Where:
- $k_t$: Motor torque constant
- $k_b$: Back-EMF constant
- $R$: Armature winding resistance
- $G$: Gear reduction ratio ($3:1$)
- $r$: Wheel radius

Substituting $F_x$ yields the linear continuous-time state-space representation $\mathbf{\dot{x}} = \mathbf{A}\mathbf{x} + \mathbf{B}V_{in}$ with state vector $\mathbf{x} = [x, \dot{x}, \theta, \dot{\theta}]^T$.

## 3. LQR Optimal Control Law
The Linear Quadratic Regulator (LQR) was designed to minimize the quadratic performance index:

$$ J = \int_0^\infty \left( \mathbf{x}^T \mathbf{Q} \mathbf{x} + R u^2 \right) dt $$

Solving the Continuous Algebraic Riccati Equation (CARE) yields the optimal state-feedback gain vector $\mathbf{K}$:

$$ u(t) = -\mathbf{K} \mathbf{x}(t) = - (k_1 x + k_2 \dot{x} + k_3 \theta + k_4 \dot{\theta}) $$

{% include image-gallery.html images="derivation_preview-1.png" height="350" %}
*Snippet of the handwritten Lagrangian mechanics and voltage-input state-space derivation. [View the complete 6-page derivation document (PDF)](Inverted_Pendulum_Control_Derivation.pdf).*

---

# Phase 2 – Robot Design & Hardware Selection

**Goal:** Select and integrate hardware centered around the STM32 microcontroller and a 24V brushed DC motor, ensuring sensor bandwidth and driver resolution were sufficient for a 1 kHz control loop.

## Hardware & Schematic Highlights
- **Powertrain:** Selected a 24V brushed DC motor with a 3:1 pulley reduction and an integrated 400 CPR quadrature encoder for cart positioning.
- **Motor Driver:** Integrated a **POLULU TB8041FTG** dual motor driver capable of high-frequency PWM switching and continuous current delivery.
- **Pendulum Angle Sensing:** Selected an ultra-low-noise magnetic rotary encoder mounted directly to the pendulum pivot axle to provide high-resolution angular feedback without mechanical friction.
- **Wiring & Circuit Layout:** Designed a complete wiring schematic in **KiCAD** and built a modular perfboard layout with point-to-point wiring and keyed headers for debugging.

---

# Phase 3 – Embedded Firmware & Real-Time Control

**Goal:** Implement real-time control algorithms and sensor acquisition routines to execute the LQR control law deterministically at 1 kHz on the STM32 microcontroller.

## Firmware Implementation Highlights
- **1 kHz Deterministic Control Loop:** Configured hardware timer interrupts running at exactly 1 kHz (1 ms period) to guarantee deterministic state sampling, integration, and PWM actuation.
- **High-Speed I2C Communication:** Optimized I2C peripheral transfers to query the magnetic encoder in under $150\,\mu\text{s}$, well within the 1 ms interrupt budget. Verified timing and signal integrity using a hardware logic analyzer.
- **Hardware Encoder Decoding:** Utilized STM32 timer encoder interface mode to decode quadrature pulses from the cart wheel encoders in hardware with zero CPU overhead.
- **State Estimation & Actuation:** Computed real-time voltage commands $u = -\mathbf{K}\mathbf{x}$ and mapped them to bidirectional timer PWM outputs with deadband friction compensation.

---

# Key Learnings

- **Sensor Phase Lag & Stability:** Digital filtering of the encoder derivative signals was initially required to suppress high-frequency noise; however, the introduced phase lag destabilized the closed-loop system. We implemented lead-lag feedforward compensation to preserve stability margins.
- **Real-World Non-linearities:** Practical friction deadbands and drivetrain belt backlash differed from the idealized linear model. Fine-tuning the $\mathbf{Q}$ state weighting matrix (placing higher penalty on pendulum angle $\theta$ and angular velocity $\dot{\theta}$) provided robust balance recovery in physical testing.
