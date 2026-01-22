---
title: "S2C: LLM Multi-Agent Framework for Certified H∞ Controller Synthesis"
excerpt: "A multi-agent LLM framework achieving 100% synthesis success on benchmark problems by integrating natural language reasoning with convex optimization for provably safe controller design."
order: 1
collection: portfolio
header:
  teaser: images/s2c-teaser.png
---

**Links:** [arXiv](https://arxiv.org/abs/2511.07894))

## Overview

**S2C (Specification-to-Certified-Controller)** is a multi-agent framework that bridges the gap between vague human intent and rigorous engineering guarantees. While Large Language Models excel at code generation, they struggle with the strict mathematical requirements of control systems—stability, robustness, and performance guarantees. S2C addresses this by using LLMs not as solvers, but as **architects** that formulate convex optimization problems for formal tools to solve.

![S2C Framework Architecture](/images/s2c-architecture.png)
*S2C iterative loop showing the interaction between SpecInt, Solv, Tester, and Adapt agents.*

## Motivation

Traditional H∞ controller synthesis requires significant domain expertise:
- Expert knowledge of Linear Matrix Inequality (LMI) formulations
- Manual translation of performance specifications to mathematical constraints
- Iterative refinement through trial-and-error

This creates a barrier for practitioners and limits accessibility to advanced robust control methods. S2C enables engineers to specify control objectives in natural language while automatically synthesizing controllers with **formal safety guarantees**.

## Multi-Agent Architecture

S2C orchestrates five specialized agents in a closed-loop system:

| Agent | Role | Function |
|-------|------|----------|
| **SpecIntAgent** | The Interpreter | Parses natural language (e.g., "respond quickly but don't oscillate") into formal numeric specifications |
| **SolvAgent** | The Mathematician | Formulates and solves LMIs using convex optimization (CVXPY/MOSEK) |
| **TesterAgent** | The Critic | Validates designs via Monte Carlo simulations and frequency-domain analysis |
| **AdaptAgent** | The Strategist | Analyzes failures and refines specifications using the Gamma-Floor Guardrail |
| **CodeGen** | The Engineer | Exports deployable Python code with embedded certificates |

## Key Innovation: Gamma-Floor Guardrail

One of the hardest challenges in automated control is preventing "over-optimization." If an agent pushes disturbance rejection (γ) too low, the system often becomes dangerously oscillatory.

S2C introduces a **severity-aware Gamma-Floor** that dynamically raises the lower bound on the H∞ norm based on time-domain violations, effectively forcing the solver to prioritize stability over raw performance when needed.

## Results

Evaluated on 14 COMPleib benchmark problems against classical baselines and single-shot LLM approaches:

![Synthesis Success Comparison](/images/s2c-fig3a-synthesis-success.png)
*Synthesis success rate comparison: S2C achieves 100% success vs. 57% for standard BRL and 50% for PID.*

| Metric | S2C (Full) | S2C (No Floor) | Baseline (BRL) |
|--------|:----------:|:--------------:|:--------------:|
| **Synthesis Success** | **100%** | 86% | 57% |
| **Convergence (<6 iterations)** | **100%** | 92.9% | 50% |
| **Robustness** | 92.5% | 89.7% | 100% |

**Key findings:**
- Achieved **100% synthesis success rate** with formal H∞ performance certificates
- Framework is model-agnostic—effective across GPT-4, DeepSeek-V3, and Llama models
- Cost-efficient: ~$0.08 per problem with GPT-4 mini

## Case Study: NN1 Benchmark

The NN1 academic benchmark is notoriously difficult to stabilize. Given the user request for fast settling time (<16s) with minimal overshoot (<10%):

- **Iteration 0:** Initial controller achieves settling time but has 93.9% overshoot
- **Iteration 1:** TesterAgent flags violation; AdaptAgent activates guardrail
- **Iteration 5:** Converges with 15.9s settling time and 64% overshoot reduction

![Design Evolution](/images/s2c-fig5-design-evolution.png)
*Iterative improvement of NN1 controller: overshoot reduced from 93.9% to 34.2%, settling time error improved from +16% to -0.6%.*

## Technical Approach

S2C solves the **Bounded Real Lemma** with regional pole placement constraints. The SolvAgent generates code to solve convex optimization problems that enforce:
- Lyapunov stability (P > 0)
- H∞ performance bounds
- Decay-rate constraints for settling time requirements

## Tools & Implementation

- **Optimization:** CVXPY with MOSEK solver
- **LLM Interface:** Compatible with GPT-4, DeepSeek-V3, Llama-4
- **Verification:** Monte Carlo simulation, frequency-domain analysis
- **RAG System:** Domain knowledge retrieval for control theory concepts

## Publication

**Li, S.**, Li, J., Xu, J., & Chen, D. (2026). From Natural Language to Certified H-infinity Controllers: Integrating LLM Agents with LMI-Based Synthesis. *Proceedings of Machine Learning Research (L4DC)*.

<a href="https://arxiv.org/abs/2511.07894" target="_blank"><img src="https://img.shields.io/badge/arXiv-2511.07894-B31B1B.svg" alt="arXiv"></a>

```bibtex
@inproceedings{li2026s2c,
  title={S2C: From Natural Language to Certified H-infinity Controllers},
  author={Li, Shihao and Li, Jiachen and Xu, Jiamin and Chen, Dongmei},
  booktitle={Proceedings of Machine Learning Research (L4DC)},
  year={2026}
}
```

## Related Projects

- [AURORA: LLM-based ROM Discovery](/portfolio/aurora-rom/) — Automated reduced-order modeling with LLMs
- [LLM-Assisted Control for R2R](/portfolio/llm-r2r-control/) — Multi-agent framework for manufacturing systems
