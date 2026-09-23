---
layout: post
title: Inverted Pendulum Self-Balancing Robot
description: Modeling, embedded firmware development, and hardware design for a two-wheeled self-balancing inverted pendulum robot. Features state-space dynamic modeling, Linear Quadratic Regulator (LQR) feedback control, real-time sensor feedback at 1 kHz, and custom KiCAD wiring.
skills:
  - Controls Engineering (LQR / State-Space)
  - Embedded Systems & C Programming
  - Electrical Schematics (KiCAD)
  - Dynamic System Simulation (Python)
main-image: /robot_thumbnail.jpg
---

<div style="margin: 10px 0 25px 0; display: flex; gap: 12px; flex-wrap: wrap;">
  <a href="https://github.com/uofu-emb-26/Pendulum-Robot" target="_blank" rel="noopener noreferrer" style="background-color: var(--link-color, #4a76ee); color: white; padding: 8px 16px; border-radius: 6px; text-decoration: none; font-weight: 500; display: inline-flex; align-items: center; gap: 6px;">
    <i class="fa-brands fa-github"></i> View Firmware Code on GitHub
  </a>
  <a href="/assets/projects/Inverted_Pendulum_Robot/Inverted_Pendulum_Control_Derivation.pdf" target="_blank" rel="noopener noreferrer" style="background-color: var(--light-background-color, #f3f5fb); color: var(--text-color, #1a1c20); border: 1px solid var(--border-color, #ddd); padding: 8px 16px; border-radius: 6px; text-decoration: none; font-weight: 500; display: inline-flex; align-items: center; gap: 6px;">
    <i class="fa-solid fa-file-pdf"></i> Download Handwritten Derivation (PDF)
  </a>
</div>

---

# Phase 1 – Dynamic Modeling & LQR Control Design

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

**Goal:** Derive equations of motion and synthesize optimal feedback controllers for stability and disturbance rejection.

## System Modeling & Formulation
- **Nonlinear Dynamics:** Derived governing nonlinear equations of motion using Lagrangian mechanics, coupling cart translational acceleration with pendulum tilt dynamics and wheel rotational inertia.
- **State-Space Linearization:** Linearized the continuous-time dynamics around the upright unstable equilibrium point $\mathbf{x} = [x, \dot{x}, \theta, \dot{\theta}]^T = \mathbf{0}$.
- **Linear Quadratic Regulator (LQR) Tuning:** Formulated quadratic cost weighting matrices ($\mathbf{Q}$ and $\mathbf{R}$) to penalize angular tilt deviation and cart position displacement while optimizing control effort and actuator saturation limits.
- **Simulation & Disturbance Rejection:** Validated closed-loop stability and recovery time against step force impulses and initial angle deflections in simulation.

### Derivation & State-Space Formulation
From my handwritten derivation using the determinant factor $q = (M+m)(I+ml^2) - (ml)^2$, the linearized state equations are:

$$ (M + m)\ddot{x} + b\dot{x} + ml\ddot{\theta} = F $$

$$ (I + ml^2)\ddot{\theta} + mgl\theta + ml\ddot{x} = 0 $$

Accounting for motor back-EMF damping ($b_{\text{eff}} = b + \frac{G^2 K_t K_e}{r^2 R}$) and input voltage $V$:

$$ \begin{bmatrix} \dot{x} \\ \ddot{x} \\ \dot{\theta} \\ \ddot{\theta} \end{bmatrix} = \begin{bmatrix} 0 & 1 & 0 & 0 \\ 0 & -7.084 & -0.1745 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 20.76 & 29.26 & 0 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \\ \theta \\ \dot{\theta} \end{bmatrix} + \begin{bmatrix} 0 \\ 0.6437 \\ 0 \\ -1.887 \end{bmatrix} V $$

{% include image-gallery.html images="derivation_preview-1.png" height="350" %}
*Snippet of the handwritten Lagrangian mechanics and voltage-input derivation. [Download the complete 5-page derivation (PDF)](/assets/projects/Inverted_Pendulum_Robot/Inverted_Pendulum_Control_Derivation.pdf).*

---

# Phase 2 – Robot Design and hardware selection

**Goal:** Select appropriate hardware for the robot around the STM32 microcontroller and a 24V brushed DC motor, ensuring sensors and drivers will be able to provide adequate control input and sensing of the robot's state at a high enough frequency and resolution for the LQR control law to work

## Hardware & Schematic Architecture
- **Power Train:** Selected a 24V brushed DC motor I had on hand,and used a 3:1 pulley gear ratio. The motor had a built-in 400 CPR quadrature encoder. We went with a POLULU TB8041FTG motor driver due to its high efficiency and ability to drive high current DC motors.
- **KiCAD wiring schematic:** Due to time constraints, we went with a perfboard and point-to-point wiring for the robot, using KiCAD to create a wiring schematic to help keep track of the wiring.
- **Pendulum encoder:** selected a magnetic rotary encoder to provide high resolution feedback on the angle of the pendulum.

---

# Phase 3 – Embedded Firmware & Real-Time Sensor Fusion

**Goal:** Implement real-time control algorithms and read sensors to provide state feedback at 1kHz on an embedded microcontroller platform.

## Firmware Implementation Highlights
- **Deterministic Control Loop:** Configured hardware timer interrupts running at 1 kHz to guarantee deterministic sampling and state updates.
- **Encoder Quadrature Decoding:** Utilized hardware timer encoder interfaces to track wheel positions ($x$) and velocities ($\dot{x}$) without processor overhead.
- **I2C communication:** Configured I2C to read pendulum encoder data within 150 microseconds. Used logic analyzer to diagnose and verify timing to ensure communication met the 1 kHz control loop requirements.
- **Full-State Feedback Computation:** Calculated real-time motor PWM command voltages via $u = -\mathbf{K}\mathbf{x}$, incorporating anti-windup deadband compensation for motor static friction.

---

# Demonstration & Hardware Testing

<div style="display: flex; gap: 20px; align-items: flex-start; justify-content: center; flex-wrap: wrap; margin: 25px 0;">
  <div style="flex: 1 1 340px; max-width: 480px; text-align: center;">
    <video style="width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" controls playsinline>
      <source src="/assets/projects/Inverted_Pendulum_Robot/robot_balancing_demo.webm" type="video/webm">
      <source src="/assets/projects/Inverted_Pendulum_Robot/robot_balancing_demo.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <p style="font-size: 0.85em; color: #666; margin-top: 8px;"><em>Physical demonstration of the self-balancing inverted pendulum robot in action.</em></p>
  </div>
  <div style="flex: 1 1 340px; max-width: 480px; text-align: center;">
    <video style="width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);" controls playsinline>
      <source src="/assets/projects/Inverted_Pendulum_Robot/robot_physical_demo.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <p style="font-size: 0.85em; color: #666; margin-top: 8px;"><em>Demonstration of system stability under model parameter mismatch by adding extra weight (a ruler) to the pendulum.</em></p>
  </div>
</div>

---

# Key Learnings

- **Filter phase delay:** Filtering of encoder signals was necessary to provide smooth state estimates, however this introduced a phase delay that made the robot unstable, requiring feedforward compensation for the phase delay.
- **LQR Tuning:** Gains tuned via LQR were necessary to account for the differences between my simplified model and the real world system with non-linear friction and backlash in the drivetrain.
