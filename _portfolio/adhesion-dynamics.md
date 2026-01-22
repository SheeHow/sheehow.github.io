---
title: "Physics-Based Modeling of Adhesion Dynamics in R2R Lamination"
excerpt: "A first-principles process model integrating Hertzian contact theory and transient heat transfer to predict adhesion quality—enabling 62.6% faster production through optimized dual-heating strategies."
order: 4
collection: portfolio
header:
  teaser: images/adhesion-teaser.png
---

## Overview

In flexible electronics and solar cell manufacturing, **lamination** is the critical step where multi-layered materials are bonded under heat and pressure. Adhesion force is the metric of success, but it is notoriously difficult to predict because it depends on a complex interplay of viscoelasticity, thermodynamics, and contact mechanics.

We developed a **Physics-Based Process Model** that explicitly integrates **Hertzian Contact Theory** with transient heat transfer equations. Unlike black-box empirical models, this framework predicts exactly how machine parameters (roller speed, temperature, compression force) translate into bonding strength.

![R2R Lamination System](/images/adhesion-fig1-schematic.png)
*Schematic of the R2R lamination process showing key inputs: compression force (F), temperature (T), and web velocity (v).*

## Motivation

Traditional approaches to lamination process optimization rely on:
- Expensive trial-and-error experimentation
- Empirical models that don't generalize across materials
- Conservative operating parameters that limit throughput

A physics-based model enables:
- Predictive process design before physical trials
- Understanding of parameter interactions
- Identification of optimal operating strategies

## The Physics Engine

The model couples two physical domains:

### Mechanical Domain: Hertzian Contact

We model the nip (contact zone) between rollers as a cylindrical Hertzian contact, calculating exact effective contact pressure and width rather than assuming flat interaction:

- **Contact width** depends on total force, effective radius, and material properties
- **Maximum pressure** distribution follows Hertzian profile
- **Contact time** determined by web velocity and contact width

### Thermal Domain: Transient Heat Transfer

Adhesion is treated as a thermally activated process following an Arrhenius-type law. The model tracks temperature evolution through the multi-layer stack (PET-EVA-PET) as it moves through the heated zone:

- **Conduction** through substrate layers
- **Interface temperature** at bonding surface
- **Melt transition** of adhesive layer (EVA)

## Results: The Adhesion Surface

The model generates a 3D "Adhesion Surface" that serves as a manufacturing process map, visualizing the safe operating zone where temperature and pressure ensure adequate bonding.

![Adhesion Surface](/images/adhesion-fig5-surface.png)
*3D surface plots predicting adhesion force as a function of temperature and pressure across different speeds. Note how the "safe zone" shrinks at higher speeds.*

Key observations:
- **Low Speed (0.01 m/s):** High adhesion achievable across wide range of pressures
- **High Speed (0.10 m/s):** Safe zone shrinks drastically—only maximal temperatures (200°C+) and high compression maintain bond integrity

### Model Validation

![Model Validation](/images/adhesion-fig4-validation.png)
*2D contour plots with experimental data points overlay, demonstrating model accuracy across operating conditions.*

## Optimization: Dual-Heating Strategy

A key insight from thermal analysis was the bottleneck in heat transfer through substrate layers. We simulated a **Dual-Roller Heating** strategy (heating both top and bottom rollers):

| Configuration | Time to Target (87°C) | Improvement |
|---------------|:---------------------:|:-----------:|
| Single Roller | 0.337 s | Baseline |
| Dual Roller | **0.126 s** | **62.6% faster** |

![Temperature Evolution](/images/adhesion-fig7-heating.png)
*Comparison of EVA layer temperature rise: dual-heated configuration (red) shows significantly steeper slope, reaching target temperature 62.6% faster.*

This reduction in heating time means production line speed can be **more than doubled** while maintaining the same adhesion quality.

## Impact

The physics-based model enables:
- **Process optimization** without extensive physical trials
- **Material selection** guidance based on thermal properties
- **Equipment design** insights for dual-heating configurations
- **Quality prediction** across operating conditions

## Tools & Implementation

- **Contact Mechanics:** Hertzian theory for cylinder-on-cylinder contact
- **Heat Transfer:** Transient conduction through multi-layer stack
- **Validation:** Custom R2R lamination testbed + Instron peel testing
- **Application:** Flexible electronics, solar cell manufacturing

## Publication

**Li, S.**, Martin, C., Morquecho, E.V., Chen, Z., Chen, D., & Li, W. (2025). Modeling of Adhesion Dynamics in Roll-to-Roll Lamination Processes. *Manufacturing Letters*, 44, pp.552-558. **[Published]**

```bibtex
@article{li2025adhesion,
  title={Modeling of Adhesion Dynamics in Roll-to-Roll Lamination Processes},
  author={Li, Shihao and Martin, Christopher and Velasquez Morquecho, Enrique and Chen, Zijun and Chen, Dongmei and Li, Wei},
  journal={Manufacturing Letters},
  volume={44},
  pages={552--558},
  year={2025}
}
```

## Related Projects

- [RLMPC: Repetitive Learning Control](/portfolio/rl-mpc/) — Real-time learning control for R2R manufacturing
- [LLM-Assisted Control for R2R](/portfolio/llm-r2r-control/) — Multi-agent framework for R2R systems
- [Curriculum-Based SAC for R2R](/portfolio/curriculum-rl/) — Deep reinforcement learning for tension control
