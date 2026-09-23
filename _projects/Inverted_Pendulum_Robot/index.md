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
  <a href="https://github.com/uofu-emb-26/Pendulum-Robot" target="_blank" rel="noopener noreferrer" style="background-color: var(--link-color, #4a76ee); color: white; padding: 8px 16px; border-radius: 6px; text-decoration: none; font-weight: 500; display: inline-flex; align-items: center; gap: 6px;">
    <i class="fa-brands fa-github"></i> View Firmware Code on GitHub
  </a>
  <a href="/assets/projects/Inverted_Pendulum_Robot/Inverted_Pendulum_Control_Derivation.pdf" target="_blank" rel="noopener noreferrer" style="background-color: var(--light-background-color, #f3f5fb); color: var(--text-color, #1a1c20); border: 1px solid var(--border-color, #ddd); padding: 8px 16px; border-radius: 6px; text-decoration: none; font-weight: 500; display: inline-flex; align-items: center; gap: 6px;">
    <i class="fa-solid fa-file-pdf"></i> Download Handwritten Derivation (PDF)
  </a>
</div>

---

# Phase 1 – Dynamic Modeling, Voltage Input Formulation & LQR Simulation

**Goal:** Derive the governing equations of motion from first principles, model the DC motor voltage-to-force actuator dynamics with back-EMF damping, and synthesize an optimal LQR controller in simulation before building physical hardware.

<div style="display: flex; gap: 20px; align-items: flex-start; justify-content: center; flex-wrap: wrap; margin: 25px 0;">
  <div style="flex: 1 1 340px; max-width: 480px; text-align: center;">
    <video style="width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" controls autoplay loop muted playsinline>
      <source src="/assets/projects/Inverted_Pendulum_Robot/pendulum_sim.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <p style="font-size: 0.85em; color: #666; margin-top: 8px;"><em>Simulation of the inverted pendulum cart stabilizing under LQR control.</em></p>
  </div>
  <div style="flex: 1 1 340px; max-width: 480px; text-align: center;">
    <img src="/_projects/Inverted_Pendulum_Robot/lqr_response.png" alt="LQR Step Response Plot" style="width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);">
    <p style="font-size: 0.85em; color: #666; margin-top: 8px;"><em>LQR closed-loop impulse & step disturbance recovery response.</em></p>
  </div>
</div>

## 1. Linearized System Dynamics
Using the small-angle approximation ($\sin\theta \approx \theta$, $\cos\theta \approx 1$, $\dot{\theta}^2 \approx 0$) and modeling friction with viscous damping coefficient $b$:

$$ (M + m)\ddot{x} + b\dot{x} + ml\ddot{\theta} = F \quad (1) $$

$$ (I + ml^2)\ddot{\theta} + mgl\theta + ml\ddot{x} = 0 \quad (2) $$

Defining the determinant factor $q = (M+m)(I+ml^2) - (ml)^2$, the state-space form with horizontal input force $F$ is:

$$ \begin{bmatrix} \dot{x} \\ \ddot{x} \\ \dot{\theta} \\ \ddot{\theta} \end{bmatrix} = \begin{bmatrix} 0 & 1 & 0 & 0 \\ 0 & -\frac{(I+ml^2)b}{q} & -\frac{(ml)^2 g}{q} & 0 \\ 0 & 0 & 0 & 1 \\ 0 & -\frac{ml b}{q} & \frac{mgl(M+m)}{q} & 0 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \\ \theta \\ \dot{\theta} \end{bmatrix} + \begin{bmatrix} 0 \\ \frac{I+ml^2}{q} \\ 0 \\ -\frac{ml}{q} \end{bmatrix} F $$

## 2. Motor Actuator & Voltage-Input Formulation
When using motor armature voltage $V$ as the direct control input (via PWM), back-EMF creates an electrical damping effect on cart motion ($\omega_m = \frac{G\dot{x}}{r}$ and $i = \frac{V - K_e \omega_m}{R}$):

$$ F = \frac{G K_t}{r}\left( \frac{V - K_e \frac{G\dot{x}}{r}}{R} \right) = \underbrace{\left(\frac{G K_t}{r R}\right)}_{\text{Input Gain}} V - \underbrace{\left(\frac{G^2 K_t K_e}{r^2 R}\right)\dot{x}}_{\text{Back-EMF Damping}} $$

The effective damping coefficient in matrix $\mathbf{A}$ becomes:

$$ b_{\text{eff}} = b + \frac{G^2 K_t K_e}{r^2 R} $$

## 3. Physical Parameters & Empirical System Identification
The system parameters were determined through physical measurements and bench experiments:
- **Cart Mass ($M$):** $2.525\text{ kg}$ (with effective drivetrain inertia $M_{\text{eff}} = M + \frac{J G^2}{r^2}$)
- **Pendulum Mass ($m$):** $0.152\text{ kg}$
- **Center of Mass Distance ($l$):** $0.105\text{ m}$
- **Pendulum Rotational Inertia ($I$):** $3.770 \times 10^{-3}\text{ kg}\cdot\text{m}^2$ (derived experimentally from free-oscillation period $T = 0.975\text{ s}$ via $I = \frac{mgl T^2}{4\pi^2}$)
- **Wheel Radius ($r$):** $0.038\text{ m}$, **Gear Ratio ($G$):** $3:1$
- **Motor Parameters:** Winding resistance $R = 6.5\ \Omega$ (measured at stall), $K_t = K_e = 0.1394\text{ N}\cdot\text{m/A}$ (measured via lever-arm torque vs. current test)

Substituting these values yields $q = 14.32 \times 10^{-3}$ and the final numerical state-space system:

$$ \begin{bmatrix} \dot{x} \\ \ddot{x} \\ \dot{\theta} \\ \ddot{\theta} \end{bmatrix} = \begin{bmatrix} 0 & 1 & 0 & 0 \\ 0 & -7.084 & -0.1745 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 20.76 & 29.26 & 0 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \\ \theta \\ \dot{\theta} \end{bmatrix} + \begin{bmatrix} 0 \\ 0.6437 \\ 0 \\ -1.887 \end{bmatrix} V $$

## 4. LQR Optimal Control Law
The Linear Quadratic Regulator (LQR) minimizes the quadratic performance index:

$$ J = \int_0^\infty \left( \mathbf{x}^T \mathbf{Q} \mathbf{x} + R u^2 \right) dt $$

Solving the Continuous Algebraic Riccati Equation (CARE) generates the optimal state-feedback gain vector $\mathbf{K}$ executed in real time:

$$ u(t) = -\mathbf{K} \mathbf{x}(t) = - (k_1 x + k_2 \dot{x} + k_3 \theta + k_4 \dot{\theta}) $$

{% include image-gallery.html images="derivation_preview-1.png" height="350" %}
*Snippet of the handwritten Lagrangian mechanics and voltage-input state-space derivation. [View the complete 5-page derivation document (PDF)](/assets/projects/Inverted_Pendulum_Robot/Inverted_Pendulum_Control_Derivation.pdf).*

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
