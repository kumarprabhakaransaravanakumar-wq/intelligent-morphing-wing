# Intelligent Morphing-Wing Explorer

Intelligent Morphing-Wing Explorer is an aerospace autonomy research project investigating physics-informed adaptive flight control, safe autonomous decision-making, uncertainty-aware planning, and morphing aircraft for Mars and planetary exploration.

The goal is practical. First, simulation results show that the architecture works. Then the same autonomy stack flies on a real morphing-wing drone on Earth, so the gap between simulation and reality is measured rather than assumed.

**Keywords:** morphing wing, morphing aircraft, adaptive wing, variable camber, wing twist, Mars aircraft, Mars airplane, planetary aerial exploration, low Reynolds number aerodynamics, model predictive control (MPC), nonlinear MPC, physics-informed machine learning, residual learning, probabilistic ensembles, uncertainty quantification, conformal prediction, safe learning, runtime assurance, simplex architecture, safety filter, fallback control, sim-to-real, system identification, hardware-in-the-loop, software-in-the-loop, ArduPilot, companion computer, fixed-wing UAV, energy-efficient flight.

## Research Areas

- Autonomous flight and aerospace robotics
- Morphing wings and adaptive aircraft
- Physics-informed machine learning and simulation
- Safe autonomy, runtime assurance, and fallback control
- Uncertainty quantification and online system identification
- Mars aircraft and planetary aerial exploration

## Vision

The long-term concept is a compact autonomous vehicle delivered from orbit into a planetary atmosphere. It would perform sustained reconnaissance, respond to changing weather, avoid or shelter from severe storms, manage its own energy, and continue its mission with limited or delayed communication.

The central research question is:

> Can an aircraft use physics-informed intelligence to choose useful changes in wing geometry and trajectory while an independent safety system decides which risks are acceptable?

## Core Idea

The vehicle is organized around two distinct decision-making roles:

- An **opportunity system** searches for flight paths and wing configurations that improve science return, endurance, stability, or survivability.
- A **risk harness** evaluates uncertainty, physical limits, energy reserves, recoverability, and mission priorities before allowing a proposal to reach the vehicle.

This separation is intentional. Learning may suggest creative actions, but it does not get unrestricted control authority.

## Adaptive Capabilities

The research explores coordinated adaptation of:

- Wing span, chord, sweep, twist, camber, and airfoil family
- Angle of attack, speed, altitude, and route
- Survey, transit, storm avoidance, landing, shelter, and recovery behavior
- Energy collection, storage, use, and reserve strategy
- Online atmospheric and aerodynamic model estimates

Mars is a motivating reference environment because its thin atmosphere, terrain, dust, and weather make fixed assumptions especially limiting. The architecture is intended to remain general enough for other atmospheric bodies and high-altitude terrestrial research.

## Architecture

The concept uses a layered, physics-informed autonomy stack:

1. Observe the atmosphere, vehicle, terrain, weather, and mission state.
2. Estimate both the current state and uncertainty in that estimate.
3. Predict outcomes with a physical model corrected by bounded learned models.
4. Generate opportunities for geometry and trajectory adaptation.
5. Evaluate proposals against independent risk and recovery constraints.
6. Execute approved commands through conventional feedback control.
7. Fall back to a validated configuration whenever confidence or recoverability is inadequate.

See [docs/architecture-overview.md](docs/architecture-overview.md) for the public conceptual design and [docs/sim-to-real-roadmap.md](docs/sim-to-real-roadmap.md) for the path to a flying demonstrator.

## Public Scope

This repository intentionally contains only the project vision and high-level architecture. Flight software, model parameters, simulation code, detailed research notes, experimental data, validation results, and mission-specific values are maintained privately while the work is developed and reviewed.

## Current Status

The project is in simulation-first research and prototyping. A private implementation exercises the proposed separation between opportunity-seeking optimization and independent risk authority. The simulator is being upgraded so that the "true" environment and the controller's internal model come from independent sources. Without that separation, learning results could look better than they really are. Nothing in this repository describes a flight-qualified system.

## From Simulation to Real Flight

Mars flight cannot be reproduced on Earth in a single affordable test. Thin atmosphere, lower gravity, and different flow conditions cannot all be matched at once. The real-world experiment is therefore designed to validate the **autonomy architecture**, not Mars aerodynamics:

- A small electric fixed-wing drone flies in a comparable low-Reynolds-number flow regime, with a buildable subset of morphing (variable camber and wing twist).
- The same decision software runs onboard: opportunity search, risk harness, online learning, and fallback.
- Flight logs calibrate the simulator, so the remaining sim-to-real gap is measured and reported.
- Effects that cannot be matched on Earth are studied in simulation.

Staged path, where each stage must pass before the next begins:

1. **Credible simulation:** a truth model independent of the controller's model, plus actuator limits and flight dynamics, for both the Mars concept and the Earth demonstrator
2. **Controller in simulation:** an optimization-based controller with learned corrections, compared against fixed-geometry and scheduled-morphing baselines
3. **Simulation to flight stack:** the onboard software runs against autopilot software-in-the-loop, then hardware-in-the-loop with fault injection
4. **Ground and wind tests:** morphing mechanism characterization and static aerodynamic measurements
5. **Flight tests:** fixed-geometry baseline, open-loop morphing, system identification, then closed-loop adaptive morphing under the risk harness
6. **Mars relevance (stretch):** low-density testing through research partners

See [docs/sim-to-real-roadmap.md](docs/sim-to-real-roadmap.md) for more detail.

## Safety Authority in Flight

Authority is layered so that no learned or optimized component has the final word. Highest first:

1. Human safety pilot with a hardware override
2. Autopilot failsafes and geofence
3. Onboard morphing limit governor on the flight controller
4. Risk harness on the companion computer
5. Opportunity optimizer

The autopilot's own stabilization serves as the validated fallback controller. The autonomy software requests wing geometry and flight setpoints; it does not drive control surfaces directly.

## Compute Requirements

No cloud service and no large-scale reinforcement-learning training are required. Simulation, aerodynamic tables, and model training run on a laptop. In flight, every decision is made onboard, and the aircraft never depends on a network link.

## Public Roadmap

- Define measurable autonomy and safety requirements
- Separate the simulated truth environment from the controller's internal model
- Compare fixed-wing, scheduled-morphing, and adaptive-morphing baselines
- Develop uncertainty-aware atmosphere and vehicle estimation
- Evaluate joint wing-geometry and trajectory optimization in simulation
- Run the onboard software in autopilot software-in-the-loop and hardware-in-the-loop tests
- Build and ground-test a morphing-wing demonstrator
- Fly staged flight tests and report the measured sim-to-real gap
- Test fallback, landing, shelter, and storm-avoidance strategies in simulation
- Publish non-sensitive findings and reproducible benchmark definitions

## Responsible Development

This work is intended for scientific exploration, environmental observation, and aerospace autonomy research. Safety constraints, bounded adaptation, traceable decisions, and human-reviewed mission objectives are first-class design requirements.

## FAQ

**Is this a reinforcement-learning controller?**
No. End-to-end learned policies are hard to constrain and inspect. The project instead pairs physics-based prediction and optimization with bounded learned corrections, and an independent safety layer.

**Can a Mars aircraft be tested on Earth?**
Only partially. An Earth demonstrator can match the low-Reynolds-number flow regime but not Mars density, gravity, or flow speed relative to the speed of sound. The flight experiment tests the autonomy and safety architecture and calibrates the simulator. Mars-specific effects are studied in simulation.

**Which morphing is used on the demonstrator?**
Variable camber and wing twist, because they can be built and maintained on a small airframe. Span, sweep, and chord changes are studied in simulation.

**Does the aircraft need an internet or cloud connection?**
No. All flight decisions run onboard.

## Citation

If this work is useful in your research, please cite it using the metadata in [CITATION.cff](CITATION.cff). GitHub's "Cite this repository" button generates BibTeX and APA from that file.

## Follow The Project

- **Star** the repository to support the research and find future updates.
- **Watch** releases and repository activity for new public milestones.
- Use **GitHub Issues** or **Discussions** for research questions, related work, and collaboration ideas.

## Author

[kumarprabhakaransaravanakumar-wq](https://github.com/kumarprabhakaransaravanakumar-wq)

## License

The public overview is released under the [MIT License](LICENSE).
