---
title: "RLMPC: Real-Time Repetitive Learning Control for High-Speed Manufacturing"
excerpt: "A control framework that gives machines 'muscle memory'—learning from previous cycles to reduce tracking error by 64% while maintaining real-time 3ms computation."
order: 3
collection: portfolio
header:
  teaser: images/rlmpc-teaser.png
---

## Overview

In mass manufacturing, machines perform the same task millions of times. Yet, traditional controllers suffer from "amnesia"—they treat every cycle as a new event, repeating the same errors over and over due to unmodeled dynamics.

We developed **RLMPC (Repetitive Learning Model Predictive Control)**, a control strategy that gives machines "muscle memory." By analyzing error patterns from previous cycles, the controller updates its internal model in real-time, progressively eliminating periodic disturbances that standard controllers cannot see.

![RLMPC Algorithm Flow](/images/rlmpc-fig3-algorithm.png)
*The controller uses a memory buffer of previous cycles to update the LTV model along the prediction horizon, effectively linearizing along the learned optimal trajectory.*

## Motivation

Traditional Model Predictive Control (MPC) approaches face a fundamental trade-off:
- **Linear MPC:** Fast computation but cannot handle nonlinear dynamics accurately
- **Nonlinear MPC:** Accurate but computationally expensive for real-time control

RLMPC bridges this gap by using a **Linear Time-Varying (LTV)** internal model whose sensitivities are updated online using real-time measurements—achieving nonlinear accuracy at linear computational cost.

## The Algorithm: Learning via LTV Sensitivities

Instead of solving a non-convex optimization problem at every step, RLMPC uses an LTV internal model with online-updated Jacobians:

| Step | Action | Description |
|------|--------|-------------|
| **Recall** | Retrieve previous cycle | Access state/input trajectory from the previous cycle |
| **Update** | Calculate new dynamics | Compute linearized A, B matrices based on actual system state |
| **Solve** | Single QP optimization | Find optimal correction via convex quadratic program |
| **Execute & Store** | Apply and memorize | Execute trajectory and store for next cycle refinement |

The key insight: linearize along the **learned optimal trajectory** rather than a static reference, bridging linear efficiency with nonlinear accuracy.

## Results

Tested on a simulated R2R mechanical dry transfer process where adhesion forces create complex, periodic disturbances.

![Tension Convergence](/images/rlmpc-fig6-convergence.png)
*Evolution of tension control over 32 cycles: oscillating error (Cycle 1) flattens out (Cycle 32) as the system learns the disturbance profile.*

| Metric | Linear MPC (Baseline) | RLMPC (Ours) | Improvement |
|--------|:---------------------:|:------------:|:-----------:|
| **RMSE (Cycle 1)** | 2.19 N | 2.19 N | 0% |
| **RMSE (Cycle 32)** | 2.19 N | **0.79 N** | **64%** |
| **Computation Time** | ~3 ms | **~3 ms** | Same Cost |

The "learning" effect is clearly visible: in Cycle 1, error is high (RMSE: 2.19 N). By Cycle 32, the controller has learned the disturbance profile, reducing RMSE to 0.79 N—a **64% improvement** at no additional computational cost.

## Technical Approach

At each timestep, RLMPC solves a Convex Quadratic Program (QP) where the system matrices A and B are updated every cycle using the full nonlinear process model. This approach:

- Maintains **real-time feasibility** (~3ms per solve)
- Achieves **nonlinear accuracy** through iterative linearization
- Provides **constraint satisfaction** via QP formulation
- Enables **progressive learning** of periodic disturbances

## Application: Roll-to-Roll Dry Transfer

The R2R mechanical dry transfer process involves precise tension control during material transfer between rollers. Adhesion forces create complex, periodic disturbances that standard MPC cannot anticipate. RLMPC learns these patterns cycle-by-cycle, effectively building a "muscle memory" of the process dynamics.

## Tools & Implementation

- **Optimization:** Convex QP solver for real-time control
- **Model:** Linear Time-Varying (LTV) with online Jacobian updates
- **Application:** Roll-to-roll dry transfer manufacturing
- **Performance:** 3ms computation time, suitable for high-speed loops

## Publication

Martin, C., **Li, S.**, Li, J., Li, W., & Chen, D. (2026). A Repetitive Learning Model Predictive Control Method for Nonlinear Systems with Application to Roll-to-Roll Manufacturing. *American Control Conference (ACC)*, Accepted.

```bibtex
@inproceedings{martin2026rlmpc,
  title={A Repetitive Learning Model Predictive Control Method for Nonlinear Systems with Application to Roll-to-Roll Manufacturing},
  author={Martin, Christopher and Li, Shihao and Li, Jiachen and Li, Wei and Chen, Dongmei},
  booktitle={American Control Conference (ACC)},
  year={2026}
}
```

## Related Projects

- [LLM-Assisted Control for R2R](/portfolio/llm-r2r-control/) — Multi-agent framework for R2R manufacturing
- [Curriculum-Based SAC for R2R](/portfolio/curriculum-rl/) — Deep reinforcement learning for tension control
- [Adhesion Dynamics Modeling](/portfolio/adhesion-dynamics/) — Physics-based modeling for R2R lamination
