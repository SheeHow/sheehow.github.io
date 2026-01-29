---
title: "DM-MPPI: Datamodel for Efficient and Safe Model Path Integral Control"
excerpt: "Extending datamodels from machine learning to MPPI control—achieving 5× sample reduction while improving obstacle avoidance through learned influence prediction."
order: 9
collection: portfolio
header:
  teaser: images/dm-mppi-teaser.png
---

## Overview

Model Predictive Path Integral (MPPI) control is a powerful algorithm for controlling robots in complex environments because it handles non-linear dynamics and constraints easily. However, it is computationally expensive: it requires simulating thousands of random trajectories (samples) at every timestep to find the best path.

We introduced **DM-MPPI**, a framework that makes MPPI up to **5× more efficient**. By applying the "Datamodels" concept from machine learning to control theory, we trained a lightweight predictor to identify which trajectories actually matter. This allows the controller to instantly discard (prune) "useless" samples before wasting computation on them.

![DM-MPPI Architecture](/images/dm-mppi-fig1-architecture.png)
*The pipeline showing how the Datamodel acts as a gatekeeper, filtering samples based on predicted influence before the heavy MPPI computation occurs.*

## Motivation

In standard MPPI, thousands of noisy trajectories are averaged to calculate the control input. Most of these trajectories have near-zero weight (influence) on the final decision, yet we waste resources simulating them.

The key insight: **not all samples are created equal**. If we can predict which samples will matter, we can skip the rest entirely.

## The Core Logic: Learning Influence

DM-MPPI introduces an **Influence Predictor** with three steps:

| Step | Action | Description |
|------|--------|-------------|
| **Feature Extraction** | Extract cost features | Distance to goal, distance to obstacles for each sample |
| **Influence Prediction** | Apply trained model | Lightweight linear model predicts influence score |
| **Pruning** | Discard low-influence | Samples below threshold are removed before MPPI computation |

The influence predictor is trained offline using datamodel regression, then deployed for O(1) prediction at runtime.

## Safety via Influence-Aware Tuning

Beyond efficiency, we utilize the Datamodel for **Safety**. In traditional MPPI, tuning the penalty weight λ for obstacles is difficult:
- **Too Low:** The robot crashes
- **Too High:** The robot freezes or moves too conservatively

DM-MPPI monitors the **Total Influence of Violation Samples**. If trajectories that hit obstacles have too much influence on the final decision, the system detects this in real-time and dynamically increases the penalty parameter λ. This "reflexive" safety mechanism prevents crashes that static tuning misses.

## Results

Evaluated on a path tracking task with obstacle avoidance using a kinematic bicycle model.

### Sample Efficiency: 5× Reduction

![Sample Efficiency Comparison](/images/dm-mppi-fig2-efficiency.png)
*Sample efficiency comparison: DM-MPPI maintains stable performance at K=100, matching standard MPPI at K=500.*

Using the Influence Predictor to prune samples maintained trajectory quality with significantly fewer resources:

| Configuration | Samples | Position RMSE | Min. Obstacle Dist. | Iteration Time |
|---------------|:-------:|:-------------:|:-------------------:|:--------------:|
| Standard MPPI | 500 | 0.41 m | 1.24 m | 85.4 ms |
| DM-MPPI (Fixed ρ) | 100 | 0.44 m | 1.31 m | 18.2 ms |
| DM-MPPI (Adaptive ρ) | **100** | **0.43 m** | **1.42 m** | 18.5 ms |

### Trajectory Quality Comparison

![Trajectory Comparison](/images/dm-mppi-fig3-trajectories.png)
*Despite using only 100 samples, DM-MPPI produces control output comparable to standard MPPI with 500 samples.*

The adaptive variant achieves the largest minimum obstacle distance by dynamically adjusting ρ based on the violation influence ratio.

## Technical Approach

We define the "influence" of a sample trajectory on the optimal control input as the derivative of the solution with respect to the sample's weight. We approximate this using a linear Datamodel:

- **Offline:** Train regression model mapping trajectory costs to influence scores
- **Online:** Predict sample importance in O(1) time
- **Pruning:** Discard samples with predicted influence below threshold

This achieves the efficiency of reduced sampling with the safety of influence-aware constraint handling.

## Tools & Implementation

- **Control Framework:** Model Predictive Path Integral (MPPI)
- **Influence Prediction:** Linear datamodel regression
- **Application:** Robot navigation with obstacle avoidance
- **Performance:** 5× sample reduction, improved safety margins

## Publication

Li, J., **Li, S.**, Duan, X., & Chen, D. (2025). DM-MPPI: Datamodel for Efficient and Safe Model Path Integral Control. *arXiv preprint arXiv:2512.00759*.

<a href="https://arxiv.org/abs/2512.00759" target="_blank"><img src="https://img.shields.io/badge/arXiv-2512.00759-B31B1B.svg" alt="arXiv"></a>

```bibtex
@article{li2025dmmppi,
  title={DM-MPPI: Datamodel for Efficient and Safe Model Path Integral Control},
  author={Li, Jiachen and Li, Shihao and Duan, Xu and Chen, Dongmei},
  journal={arXiv preprint arXiv:2512.00759},
  year={2025}
}
```

## Related Projects

- [Influence Functions for System ID & LQR](/portfolio/influence-functions-lqr/) — Data attribution for linear system identification
- [Datamodel-Based Data Selection for DeePC](/portfolio/datamodel-deepc/) — Context-dependent data selection for predictive control
