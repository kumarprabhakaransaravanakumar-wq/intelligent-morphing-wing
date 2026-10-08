# Sim-to-Real Roadmap

This public roadmap describes how the morphing-wing autonomy architecture moves from simulation to a real flying demonstrator. Like the [architecture overview](architecture-overview.md), it omits implementation code, numerical limits, vehicle performance values, and test results.

## Objective

1. Produce simulation results showing that the opportunity optimizer, the independent risk harness, and online learning work together on a credible simulator.
2. Fly the same autonomy stack on a real morphing-wing aircraft, and measure how closely the simulator predicts reality.

## What an Earth Flight Can and Cannot Prove

A Mars aircraft operates in a very thin atmosphere at low gravity. On Earth, a small drone can reproduce the **low-Reynolds-number flow regime** that governs its wing aerodynamics. It cannot reproduce Mars density, gravity, or Mach number at the same time.

| An Earth demonstrator can validate | It cannot validate |
| --- | --- |
| The decision loop on real hardware with real sensor noise, latency, gusts, and unmodelled aerodynamics | Mars density and gravity effects on flight dynamics |
| Rate-limited geometry changes with real actuators | Compressibility at Mars flight speeds |
| The risk harness, limit governor, and fallback behaviour | CO₂ atmosphere properties and dust |
| Online residual learning and calibrated uncertainty on real data | Mars entry, deployment, and storm conditions |
| A measured sim-to-real gap after system identification | |

Effects in the right-hand column are studied in simulation, using a simulator whose credibility has been checked against real flight.

## Stages and Gates

Each stage has a go/no-go gate. Work proceeds only when the previous gate passes.

| Stage | Purpose | Gate (qualitative) |
| --- | --- | --- |
| 0. Decisions | Regulatory route, flying site, budget, morphing degrees of freedom, autopilot | Decisions recorded |
| 1. Credible simulation | Truth environment independent of the controller's model; actuator dynamics; longitudinal flight dynamics; Mars and Earth vehicle presets | Both presets fly; learning still helps when it cannot see the true model |
| 2. Controller in simulation | Optimization-based control with learned corrections and the risk harness, compared with fixed-geometry and scheduled-morphing baselines | Repeatable energy-efficiency gain with no hard-limit violations |
| 3. Simulation to flight stack | Onboard companion software against autopilot software-in-the-loop, then hardware-in-the-loop | Real-time budget met; injected faults trigger the expected fallbacks |
| 4. Ground and wind tests | Morphing mechanism characterization and static aerodynamic measurement | Measured actuator behaviour and aerodynamic trends agree with the simulator |
| 5. Flight tests | Fixed-geometry baseline → open-loop morphing → system identification → closed-loop adaptive morphing | Calibrated simulator predicts held-out flights; adaptive morphing improves energy per distance over the best fixed geometry |
| 6. Mars relevance (stretch) | Low-density testing, for example via high-altitude programmes | Only with a research partner |

## Demonstrator Concept

- A small electric fixed-wing drone with a rebuildable wing
- Morphing limited to **variable camber** and **wing twist**, which are buildable and repairable at small scale; span, sweep, and chord morphing remain simulation studies
- An open-source autopilot ([ArduPilot](https://ardupilot.org)) provides stabilization and serves as the validated fallback controller
- An onboard companion computer runs the opportunity optimizer, risk harness, and online learning
- Measured, not commanded, geometry is logged, together with airspeed and electrical power, so energy per distance can be computed

## Authority Chain

Highest authority first:

1. Human safety pilot with a hardware override
2. Autopilot failsafes and geofence
3. Morphing limit governor running on the flight controller, which holds a neutral geometry if the companion computer fails
4. Risk harness on the companion computer
5. Opportunity optimizer

One limits specification drives both the onboard governor and the software risk harness, so simulation and flight enforce the same boundaries.

## Flight-Test Discipline

- Fly at an approved site with an experienced safety pilot
- Write a test card for every flight: objective, one new variable, limits, abort criteria
- Expand the envelope one uncertainty and one geometry dimension at a time
- Compare morphing and fixed geometry on paired, back-to-back legs to cancel wind drift

## Compute

No cloud service or large-scale reinforcement-learning training is required. Simulation, aerodynamic tables, and small learned models run on a laptop. All flight decisions run onboard, and the aircraft never depends on a network link.

## Related Work

- Jeger et al., *Adaptive Morphing of Wing and Tail for Stable, Resilient, and Energy-Efficient Flight of Avian-Informed Drones* (2024). [arXiv:2403.08598](https://arxiv.org/abs/2403.08598)
- Contreras et al., *Safe, Out-of-Distribution-Adaptive MPC with Conformalized Neural Network Ensembles* (2024). [arXiv:2406.02436](https://arxiv.org/abs/2406.02436)
- Sharpe et al., *NeuralFoil: An Airfoil Aerodynamics Analysis Tool Using Physics-Informed Machine Learning* (2025). [arXiv:2503.16323](https://arxiv.org/abs/2503.16323)
- Chua et al., *Deep Reinforcement Learning in a Handful of Trials using Probabilistic Dynamics Models* (2018). [arXiv:1805.12114](https://arxiv.org/abs/1805.12114)
- Lakshminarayanan et al., *Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles* (2017). [arXiv:1612.01474](https://arxiv.org/abs/1612.01474)
