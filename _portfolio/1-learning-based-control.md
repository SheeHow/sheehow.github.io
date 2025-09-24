---
title: "Data-Driven and Learning-Based Control Systems"
excerpt: "Developing safe reinforcement learning, data-driven predictive control, and intelligent system identification methods<br/><img src='/images/learning-control-thumbnail.png'>"
collection: portfolio
---

## Overview

This research program develops advanced data-driven control methodologies that bridge machine learning and control theory. The work addresses critical challenges in modern autonomous systems: ensuring safety under adversarial attacks, optimizing control-relevant system identification, quantifying data influence on control performance, and achieving robust control with partial model knowledge.

<img src='/images/learning-control-overview.png' alt='Research Overview' style='width:100%;'>

## Key Research Contributions

### 1. SafeTrajRL: Resilient Control Under Cyber-Physical Attacks

<img src='/images/safetrajrl-framework.png' alt='SafeTrajRL Framework' style='width:80%; display:block; margin:auto;'>

**Problem**: Cyber-physical systems face adversarial attacks on sensors and actuators while operating with unknown dynamics.

**Solution**: SafeTrajRL provides:
- Unified IO-only safe RL architecture with differentiable QP safety filters
- Formal guarantees for robust forward invariance under bounded attacks
- Implicit attack detection via QP residual (detection delay: 0.60s)
- Graceful performance degradation with quadratic scaling

**Results**: 0% safety violations under attack vs. 100% for baseline PPO

### 2. Smart Predict-Then-Control (SPC)

<div style='display:flex; justify-content:space-between; margin:20px 0;'>
  <img src='/images/spc-framework.png' alt='SPC Framework' style='width:48%;'>
  <img src='/images/spc-results.png' alt='SPC Performance' style='width:48%;'>
</div>

**Innovation**: Integrates system identification and control design through decision regret minimization, prioritizing control-relevant dynamics over pure prediction accuracy.

**Key Results**:
- 58.1% reduction in tracking error for linear systems
- 68.9% reduction in decision regret
- Theoretical regret bounds with convergence guarantees

### 3. Influence Functions for Control Data Attribution

<img src='/images/influence-functions.png' alt='Influence Functions' style='width:100%;'>

**Contribution**: First framework to quantify how individual training trajectories affect:
- **IF1**: Learned dynamics accuracy (correlation: 0.93)
- **IF2**: LQR controller performance (correlation: 0.76)

Enables efficient data curation without expensive retraining.

### 4. MDR-DeePC: Distributionally Robust Predictive Control

<img src='/images/mdr-deepc-results.png' alt='MDR-DeePC Performance' style='width:100%;'>

**Approach**: Combines model-based constraints for known dynamics with Hankel matrix representations for unknown dynamics under distributionally robust optimization.

**Performance**:
- 58.75% cost reduction vs. standard DeePC
- 33.55% reduction in maximum output deviation
- Real-time capable with manageable computational complexity

## Technical Methods

<img src='/images/technical-methods.png' alt='Technical Methods' style='width:100%;'>

### Core Techniques
- **Safe RL**: Input-Output Control Lyapunov-Barrier Functions (IO-CLBF)
- **Data-Driven Control**: Hankel matrices, behavioral theory, DeePC
- **Robust Optimization**: Distributionally robust optimization, Wasserstein ambiguity sets
- **System Identification**: Control-oriented learning with decision regret
- **Data Attribution**: Influence functions through DARE sensitivity analysis

### Implementation
- **Languages**: Python (PyTorch, NumPy), MATLAB/Simulink
- **Optimization**: CVX, differentiable QP solvers
- **Validation**: Hardware-in-the-loop testing, Monte Carlo simulations

## Publications

1. **SafeTrajRL: A Data-Driven Reinforcement Learning Framework for Resilient Control of Cyber-Physical Systems**  
   S. Li, J. Li, W. Li, D. Chen. *Under review* <!-- PLACEHOLDER: Add venue -->

2. **Smart Predict-Then-Control: Integrating Identification and Control via Decision Regret**  
   J. Li, S. Li, D. Chen. *arXiv:2506.11279* (2025)

3. **Influence Functions for Data Attribution in Linear System Identification and LQR Control**  
   J. Li, S. Li, J. Xu, S. Bakshi, D. Chen. *arXiv:2506.11293* (2025)

4. **MDR-DeePC: Model-Inspired Distributionally Robust Data-Enabled Predictive Control**  
   S. Li, J. Li, C. Martin, S. Bakshi, D. Chen. *MECC 2025* (accepted)

## Impact and Applications

<div style='display:flex; justify-content:space-around; text-align:center; margin:30px 0;'>
  <div style='width:22%;'>
    <img src='/images/icon-robotics.png' alt='Robotics' style='width:60px; height:60px;'>
    <h4>Robotics</h4>
    <p>Safe manipulation under sensor attacks</p>
  </div>
  <div style='width:22%;'>
    <img src='/images/icon-power.png' alt='Power Systems' style='width:60px; height:60px;'>
    <h4>Smart Grids</h4>
    <p>Resilient control under false data injection</p>
  </div>
  <div style='width:22%;'>
    <img src='/images/icon-autonomous.png' alt='Autonomous Systems' style='width:60px; height:60px;'>
    <h4>Autonomous Vehicles</h4>
    <p>Robust navigation with GPS spoofing</p>
  </div>
  <div style='width:22%;'>
    <img src='/images/icon-manufacturing.png' alt='Manufacturing' style='width:60px; height:60px;'>
    <h4>Manufacturing</h4>
    <p>Optimal control with partial models</p>
  </div>
</div>

## Open-Source Tools

- **SafeTrajRL Framework**: [GitHub](#) <!-- PLACEHOLDER -->
- **SPC Toolbox**: [GitHub](#) <!-- PLACEHOLDER -->
- **Influence Functions for Control**: [GitHub](#) <!-- PLACEHOLDER -->

## Future Directions

- Extension to nonlinear systems and neural network controllers
- Distributed multi-agent resilient control
- Online adaptive learning with safety guarantees
- Integration with foundation models for control
