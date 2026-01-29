---
title: "AURORA: Autonomous Updating of ROM and Controller via Recursive Adaptation"
excerpt: "A multi-agent LLM framework that autonomously maintains control of high-dimensional systems by continuously monitoring, re-identifying physics, and updating reduced-order models—achieving 100% success on dynamic benchmarks."
order: 5
collection: portfolio
header:
  teaser: images/aurora-teaser.png
---

## Overview

High-dimensional systems (like soft robots or thermal fluids) are notoriously difficult to control in real-time because their mathematical models are too heavy for fast computation. Engineers typically solve this by manually creating **Reduced-Order Models (ROMs)**—simplified approximations of the physics. However, these models are static; if the system changes (wear and tear, new environments), the controller fails.

We developed **AURORA**, a multi-agent LLM framework that automates the entire lifecycle of ROM-based control. It doesn't just design the controller once; it acts as an autonomous engineer that continuously monitors performance. If the system drifts, AURORA detects the degradation, re-identifies the physics, updates the reduced-order model, and retunes the controller—all without human intervention.

![AURORA Architecture](/images/aurora-fig1-architecture.png)
*The closed-loop workflow where the Evaluation Agent triggers re-modeling cycles upon detecting performance degradation.*

## Motivation

Traditional ROM-based control faces fundamental limitations:
- Manual ROM construction requires expert knowledge and significant effort
- Static models degrade when system dynamics change over time
- No automatic mechanism to detect and correct model mismatch
- Controller failures require manual diagnosis and re-tuning

AURORA addresses these through autonomous, continuous adaptation.

## The Recursive Adaptation Architecture

AURORA is not a single model but a team of five specialized AI agents orchestrated by a Manager:

| Agent | Role | Function |
|-------|------|----------|
| **SysID Agent** | Physics Discovery | Analyzes raw sensor data to identify governing equations |
| **ROM Agent** | Model Compression | Compresses high-dimensional dynamics using POD and related techniques |
| **Control Agent** | Controller Design | Designs optimal controllers (LQR/MPC) based on simplified ROM |
| **CodeGen Agent** | Implementation | Writes and verifies Python execution code |
| **Evaluation Agent** | The Watchdog | Monitors error metrics, diagnoses root causes, triggers update cycles |

The **Evaluation Agent** is the core of recursive adaptation—it continuously monitors performance and distinguishes between "Model Mismatch" vs. "Controller Aggression" to trigger targeted updates.

## Quality Assurance: Judge-Correction Loop

A major risk with automated design is "silent failure"—code that runs but produces wrong physics. AURORA implements a rigorous QA protocol:

**Validation Levels:**
1. **Execution:** Does it run without errors?
2. **Dimensions:** Do matrices match expected shapes?
3. **Quality:** Is the approximation error acceptably low?
4. **Physics:** Is energy conserved? Are constraints satisfied?

If any check fails, the error log is fed back to the generating agent for self-correction. This loop reduced code generation errors to near zero in experiments.

## Results

Validated on 8 diverse benchmarks ranging from simple systems (Building, n=48) to extreme complexity (Mobile Manipulator, n=156):

### Overall LLM Performance

| LLM | Success Rate | Avg. Improvement | Full Auto | Avg. Iterations |
|-----|:------------:|:----------------:|:---------:|:---------------:|
| **GPT-5** | **7/8** | **+8.9%** | **5/8** | **7** |
| Qwen-2.5-72B | 6/8 | +7.1% | 4/8 | 7 |
| DeepSeek-V3 | 6/8 | +3.2% | 3/8 | 10 |
| GPT-5 mini | 5/8 | -7.2% | 2/8 | 13 |
| Llama-4 Maverick | 4/8 | -18.6% | 1/8 | 18 |

### System Complexity Analysis

The framework scales across system complexity, with GPT-5 achieving success even on the most challenging Mobile Manipulator (n=156 states) where all other LLMs failed:

| Complexity | System | States | LLM Failures | Best LLM |
|------------|--------|:------:|:------------:|----------|
| Simple | Building | 48 | 0 | Qwen (+11.7%) |
| Simple | 7-DOF Manipulator | 42 | 0 | GPT-5 (+7.0%) |
| Moderate | ISS 1R | 270 | 2 | GPT-5 (+9.0%) |
| Moderate | Quadrotor | 288 | 2 | GPT-5 (+6.0%) |
| Complex | ISS 12A | 1412 | 3 | GPT-5 (+11.0%) |
| Extreme | Mobile Manipulator | 156 | 4 | GPT-5 (+12.0%) |

The framework demonstrated particular strength in high-dimensional systems where traditional ROM construction would require significant manual effort.

## Technical Approach

AURORA's recursive adaptation follows a principled workflow:

1. **Monitor:** Evaluation Agent tracks control error metrics in real-time
2. **Diagnose:** When error exceeds threshold, agent classifies root cause
3. **Update:** Targeted re-identification of affected components (ROM or controller)
4. **Validate:** QA loop ensures updated model passes all verification checks
5. **Deploy:** New controller seamlessly replaces old one

This enables **self-healing control** that maintains performance despite system drift.

## Tools & Implementation

- **ROM Techniques:** Proper Orthogonal Decomposition (POD), Dynamic Mode Decomposition
- **Control Methods:** LQR, MPC designed on reduced-order models
- **LLM Backend:** Compatible with GPT-4, Claude-3, and other frontier models
- **Validation:** Multi-level QA with physics-based consistency checks

## Publication

Li, J., **Li, S.**, & Chen, D. (2025). AURORA: Autonomous Updating of ROM and Controller via Recursive Adaptation. *In Preparation*.

```bibtex
@article{li2025aurora,
  title={AURORA: Autonomous Updating of ROM and Controller via Recursive Adaptation},
  author={Li, Jiachen and Li, Shihao and Chen, Dongmei},
  journal={In Preparation},
  year={2025}
}
```

## Related Projects

- [S2C: LLM Multi-Agent Control Synthesis](/portfolio/llm-control-synthesis/) — Certified H∞ controller synthesis with LLMs
- [LLM-Assisted Control for R2R](/portfolio/llm-r2r-control/) — Multi-agent framework for manufacturing systems
