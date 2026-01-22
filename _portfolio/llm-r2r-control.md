---
title: "LLM-Assisted Multi-Agent Control for Roll-to-Roll Manufacturing"
excerpt: "A lifecycle multi-agent framework automating control engineering for R2R manufacturing—from system identification to sim-to-real adaptation with safety-guaranteed deployment."
order: 2
collection: portfolio
header:
  teaser: images/r2r-llm-teaser.png
---

## Overview

Roll-to-roll (R2R) manufacturing—used for flexible electronics, batteries, and sensors—requires precise tension and velocity control. Traditionally, commissioning these lines is a time-intensive manual process, especially when material properties change or hardware degrades.

We developed a **Lifecycle Multi-Agent Framework** that automates the entire control engineering pipeline: from system identification to controller synthesis, and finally to safe sim-to-real adaptation.

![R2R Framework Overview](/images/r2r-llm-fig1-framework.png)
*Overview of the LLM-assisted R2R control framework: closed-loop architecture spanning System ID, Design, Adaptation, and Monitoring phases.*

## Motivation

R2R manufacturing systems face several challenges:
- Manual controller tuning is time-intensive and requires domain expertise
- Material property changes and hardware degradation require frequent re-commissioning
- Safety constraints must be maintained during any adaptation process
- Model uncertainty (up to 50% parameter mismatch) is common in practice

This framework addresses these challenges through automated, safety-aware control synthesis.

## The 5-Phase Lifecycle Architecture

Unlike static code generation, this framework manages the entire lifecycle of the manufacturing system through five distinct phases:

| Phase | Agent | Function |
|-------|-------|----------|
| **System ID** | SysID Agent | Uses operational data and physics-informed regression to construct high-fidelity simulation model |
| **Initial Design** | Initial Control Agent | Compares PID, MPC, and LQR architectures in simulation to select best baseline controller |
| **Adaptation** | Adaptation Agent | Performs rapid sim-to-real adaptation, tuning controller on physical system |
| **Monitoring** | Monitoring Agent | Watches for degradation, distinguishes control issues from physical issues |
| **Model Update** | Update Agent | Refines simulation model based on operational data |

## RAG-Enhanced Domain Knowledge Retrieval

A critical component of the framework is the **Retrieval-Augmented Generation (RAG)** system that grounds LLM reasoning in domain-specific knowledge. Rather than relying solely on the LLM's pretrained knowledge, the agents retrieve relevant information from a curated knowledge base of R2R control principles, material properties, and tuning heuristics.

![RAG Integration](/images/r2r-llm-fig3-rag.png)
*RAG-enhanced agent architecture: domain knowledge retrieval augments LLM reasoning for control-specific decisions.*

The RAG system enables:
- **Context-aware tuning:** Agents retrieve relevant tuning strategies based on current system state
- **Physics-grounded reasoning:** Material properties and dynamics constraints inform parameter suggestions
- **Historical pattern matching:** Past adaptation successes guide current decisions

## Key Innovation: Simulation Safety Filter

In manufacturing, you cannot simply let an AI "try" random parameters on hardware. We introduced a rigorous **Simulation Safety Filter** in the Sim-to-Real phase.

Before any LLM-suggested parameter is deployed to real motor drivers, it must pass validation in the simulation model:
- Predicted performance must improve over current baseline
- Tension must remain within safety bounds: T_min ≤ T(t) ≤ T_max

If the adjustment violates safety constraints or fails to improve performance in simulation, it is rejected **before** it touches the physical machine.

![Adaptation Workflow](/images/r2r-llm-fig4-adaptation.png)
*The Adaptation Agent loop with Safety Filter acting as gatekeeper between LLM suggestions and real hardware.*

## Results

Validated on a multi-span web handling model with 6 rollers under significant model uncertainty (up to 50% parameter mismatch).

### Velocity Ramp-Up Scenario

The system handled a **10-fold acceleration** (0.01 → 0.10 m/s). The initial controller (trained on imperfect model) oscillated dangerously.

- **Adapt1:** Agent increased feedforward compensation
- **Adapt2:** Agent refined feedback gains to eliminate steady-state error
- **Result:** Tension deviations reduced from >4N to within ±1.5N

![Tension Regulation Results](/images/r2r-llm-fig12-tension.png)
*Comparison of tension regulation during velocity ramping: Adapt2 (green) shows significantly tighter control than oscillating Initial controller (red).*

## Dual-Layer Diagnostics

The Monitoring Agent doesn't just look at error rates—it diagnoses physics through dual-layer analysis:

1. **Layer 1 (Control Metrics):** Is the error exceeding the baseline threshold (2σ)?
2. **Layer 2 (Root Cause Analysis):** LLM analyzes sensor patterns to classify faults

In experiments simulating bearing wear (friction increasing), the agent correctly identified "Bearing Friction Degradation" with **72% confidence** and distinguished it from sensor noise.

## Tools & Implementation

- **Simulation:** Physics-informed R2R model with 6-roller configuration
- **Control Architectures:** PID, MPC, LQR comparison
- **Safety Validation:** Pre-deployment simulation checking
- **LLM Interface:** Multi-agent coordination for lifecycle management

## Publication

Li, J., **Li, S.**, Martin, C., Li, W., & Chen, D. (2026). An LLM-Assisted Multi-Agent Control Framework for Roll-to-Roll Manufacturing Systems. *Manufacturing Letters (NAMRC 54)*, Under Review.

```bibtex
@article{li2026r2r,
  title={An LLM-Assisted Multi-Agent Control Framework for Roll-to-Roll Manufacturing Systems},
  author={Li, Jiachen and Li, Shihao and Martin, Christopher and Li, Wei and Chen, Dongmei},
  journal={Manufacturing Letters},
  volume={00},
  year={2026}
}
```

## Related Projects

- [S2C: LLM Multi-Agent Control Synthesis](/portfolio/llm-control-synthesis/) — Certified H∞ controller synthesis with LLMs
- [Curriculum-Based SAC for R2R](/portfolio/curriculum-rl/) — Deep reinforcement learning for tension control
- [Adhesion Dynamics Modeling](/portfolio/adhesion-dynamics/) — Physics-based modeling for R2R lamination
