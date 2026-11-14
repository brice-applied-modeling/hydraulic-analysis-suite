# Hydraulic Analysis Suite (HAS)
## Version 1 Development Roadmap

**Status:** Active development  
**Primary application:** Wastewater force main and lift station hydraulic analysis  
**Development philosophy:** Engineering understanding first; verified software second  
**Version:** Roadmap revision 1.2 — November 2026 (comprehensive, repository-aligned)

---

## 1. Mission and guiding principles

Hydraulic Analysis Suite (HAS) is an engineering-focused Python package for analyzing wastewater force mains, pump performance, and lift station operations. Its purpose is to develop deeper engineering understanding while producing transparent, reproducible, independently verifiable calculations.

Version 1 will provide an integrated workflow from hydraulic inputs and boundary conditions to force main system curves, pump operating points, wet well behavior, design checks, and calculation reports.

**Guiding principles**

1. **Engineering decisions drive features.** Implement calculations that answer practical design questions, not disconnected formulas.
2. **Two parallel tracks.** Each phase includes an engineering knowledge objective and a software implementation objective.
3. **Traceability.** Record units, data sources, assumptions, equation references, warnings, and calculation versions.
4. **Verification before reliance.** Compare calculations with independently developed hand calculations, trusted examples, and appropriate established tools.
5. **Incremental delivery.** Each phase produces a usable, testable capability.
6. **Explicit limitations.** HAS supports professional engineering analysis; it does not replace project-specific criteria, independent checking, specialist evaluation, or EOR judgment.
7. **Avoid premature infrastructure.** Prioritize numerical correctness and engineering workflows over GUI, cloud, authentication, and ML.

## 2. Version 1 outcomes

At completion, HAS should be able to:

- Represent force mains composed of multiple pipe segments, fittings, and valves.
- Calculate flow velocity, pipe volume, friction loss, and minor losses using consistent units.
- Calculate static and dynamic head for explicitly defined hydraulic boundary conditions.
- Generate system curves across a specified flow range and multiple operating conditions.
- Import or enter manufacturer pump curves, determine single-pump operating points, and assess parallel-pump operation.
- Calculate hydraulic power, brake horsepower, and efficiency-related performance metrics with clearly stated assumptions.
- Size preliminary wet well operating volume and simulate pump cycling under defined inflow and control scenarios.
- Evaluate screening checks for velocities, pump operating region, NPSH, retention time, pressure conditions, air management, and transient risk.
- Generate a reproducible engineering calculation summary with assumptions, inputs, results, warnings, references, and verification evidence.

**Version 1 is not a substitute for detailed transient modeling, manufacturer selection validation, full collection-system hydraulic modeling, or a sealed engineering design.**

## 3. Actual repository baseline (November 2026)

The following files **exist in the project screenshot**. Their existence does not establish that implementation or tests are complete.

```text
src/hydraulic_analysis_suite/
├── calculators/
│   ├── __init__.py
│   └── friction_loss.py
├── cli/
│   ├── __init__.py
│   └── user_input.py
├── data/
│   ├── __init__.py
│   ├── constants.py
│   ├── fitting_coefficients.py
│   ├── fittings_data.py
│   └── pipe_materials.py
├── equations/
│   ├── __init__.py
│   └── hazen_williams.py
├── models/
│   ├── __init__.py
│   ├── fitting.py
│   └── pipe.py
├── utils/
│   ├── __init__.py
│   └── fitting_data_helper_calcs.py
└── __init__.py
```

**Confirmed correction:** `models/pump.py` has **not** been started. It must not be marked complete. Pipe and Fitting have existing implementation files but require code and test audits before a completion claim.

### Preserve and evolve

Keep the existing `src` package layout. Retain existing filenames initially. Introduce a cleaner domain structure incrementally where it improves clarity, without breaking imports or rewriting working functionality for cosmetic reasons.

- `models/`: engineering entities and validated state.
- `equations/`: pure numerical calculations with documented units and assumptions.
- `calculators/`: focused calculations assembled from models and equations.
- `analysis/` **(new when needed)**: system curves, pump operating points, operating scenarios, cycling simulations.
- `design_checks/` **(new when needed)**: design criteria and screening rules, with configurable thresholds.
- `data/`: reference tables, engineering constants, source/version traceability.
- `reporting/` **(new when needed)**: structured engineering results and export.
- `cli/`: user input interface, not part of the calculation core.
- `utils/`: genuinely generic helpers; domain-specific fitting calculations may eventually move to equations or data validation.

### Implementation status and next steps by existing file

| Existing file | Intended responsibility | Required review or next development step |
|---|---|---|
| `models/pipe.py` | Physical pipe data and derived hydraulic properties | Check internal diameter conventions, units, validation, material defaults, and calculated quantities |
| `models/fitting.py` | Fitting specification and count | Confirm K-value selection, geometry dependence, and applicability |
| `equations/hazen_williams.py` | Pure Hazen-Williams calculation | Check unit system, equation coefficients, domain limits, and independent numerical examples |
| `calculators/friction_loss.py` | Orchestrate pipe friction calculations | Separate calculation from presentation; check results and tests |
| `data/constants.py` | Constants and common engineering parameters | Document units and sources; avoid duplicating constants elsewhere |
| `data/pipe_materials.py` | Material assumptions and roughness data | Document provenance, age/condition sensitivity, and selection logic |
| `data/fitting_coefficients.py` | Fitting loss coefficient reference values | Verify source, applicability, and dimensional dependencies |
| `data/fittings_data.py` | Fitting metadata | Check consistency with fitting model and coefficient source |
| `utils/fitting_data_helper_calcs.py` | Existing fitting helper calculations | Review for domain logic better placed in equations; migrate only with tests |
| `cli/user_input.py` | User interaction | Keep isolated from models and numerical functions; deprioritize UI work |

**Status discipline:** Files shown in the screenshot are *present*, not automatically *complete*. `Pump` is **not started**. `Valve`, `ForceMain`, `WetWell`, and system-curve analysis are planned, not existing.

## 4. Two-track delivery method

Each phase has two required tracks:

- **Engineering knowledge:** Read design references, derive or explain equations, identify applicability and limitations, and complete a worked example.
- **Software implementation:** Implement the minimum reusable components, test them, and document their behavior.

**Definition of done for each phase:** (1) documented design question; (2) engineering basis and assumptions; (3) implemented capability; (4) independent calculation verification; (5) unit and integration tests; (6) worked example; and (7) documented limitations.

---

## Phase 0 — Design basis and verification framework

**Objective:** Define the engineering scope, project conventions, and evidence required to trust results.

### Engineering knowledge
- Establish governing owner criteria and applicable regulatory requirements for the intended project context.
- Review pump and wastewater design references, including appropriate Hydraulic Institute and manufacturer guidance.
- Define units, elevation datums, hydraulic boundaries, design flows, fluid assumptions, and pressure conventions.
- Distinguish static lift, friction head, minor losses, total dynamic head, HGL, and EGL.
- Document where specialist review or more sophisticated models are required.

### Software implementation
- Establish documented input/output units and conversion policy; select a consistent internal unit system.
- Set up unit tests, numerical tolerances, and verification fixtures.
- Create an engineering assumptions and reference register.
- Establish a fictitious, non-client-specific lift station replacement case study.

### Deliverables
- `docs/engineering/design_basis.md`
- `docs/engineering/engineering_assumptions.md`
- `docs/engineering/references.bib`
- `docs/verification/verification_plan.md`
- `examples/lift_station_replacement/README.md`

### Acceptance criteria
- [ ] A reviewer can identify all primary units, boundary conditions, and assumptions.
- [ ] At least one independently calculated baseline example is documented.
- [ ] A verification strategy distinguishes unit tests from engineering validation.

## Phase 1 — Core hydraulic component models

**Objective:** Complete reliable representations of hydraulic components before composing systems.

### Engineering knowledge
- Pipe internal diameter versus nominal diameter; material and roughness assumptions.
- Fitting and valve loss coefficients, equivalent-length methods, and their applicability.
- Difference between fully open valve losses and control-valve/throttled operation.
- Physical meaning of flow, velocity, pressure, and elevation data.

### Software implementation
- Audit and finish `Pipe`; test area, velocity, volume, and validation behavior.
- Finish `Fitting` and coefficient data with explicit source/provenance.
- Add `Valve` where needed, without duplicating fitting behavior unnecessarily.
- Add `PipeSegment` or an equivalent representation that combines geometry and components.
- Avoid embedding workflow logic in data models.

### Deliverables
- Verified component models and tests.
- Worked example for a single pipe segment with fittings.

### Acceptance criteria
- [ ] Inputs reject physically invalid values and incompatible units.
- [ ] Calculations reproduce independent reference values within stated tolerances.
- [ ] Fitting/valve assumptions and coefficient sources are inspectable.

## Phase 2 — Hydraulic equations and force main system curves

**Objective:** Analyze complete force main head requirements over a range of flows.

### Engineering knowledge
- Hazen-Williams: empirical assumptions, limits, and coefficient sensitivity.
- Darcy-Weisbach: Reynolds number, friction factor, fluid viscosity, and roughness.
- Minor loss equation and proper treatment of different pipe diameters.
- Static head from hydraulic boundary conditions, not simply the highest pipe elevation.
- HGL/EGL, pressure profiles, intermediate high points, and downstream boundary effects.
- Low-flow and high-flow scenarios, future roughness, and alternative alignments.

### Software implementation
- Implement pure functions for Hazen-Williams, Darcy-Weisbach, and minor losses.
- Implement force main aggregation across multiple segments.
- Calculate head requirements as a function of flow, with clearly defined boundary heads.
- Produce tabulated system curves and optional plots.
- Include head breakdowns by segment and loss category.

### Deliverables
- Force main hydraulic calculator.
- System curve table and plot.
- Independent hand-check and equation comparison cases.

### Acceptance criteria
- [ ] Friction and minor losses are correctly aggregated for different diameters.
- [ ] Static and dynamic head are separately reported.
- [ ] Zero-flow and low-flow behavior are explicitly defined.
- [ ] System curves are reproducible from stored inputs.

## Phase 3 — Pump performance and operating points

**Objective:** Predict how selected pumps interact with the force main.

**Pump class status at roadmap revision: NOT STARTED.**

### Engineering knowledge
- Pump characteristic curves: head, efficiency, power, and NPSHr versus flow.
- Best efficiency point, preferred/allowable operating region, minimum continuous flow, and manufacturer limitations.
- Pump/system curve intersection; minimum and maximum system head.
- Parallel pumps: combined flow at common head, not automatic doubling of flow.
- Affinity laws, variable speed limitations, and power implications.
- Hydraulic power, brake horsepower, motor selection considerations, and wire-to-water efficiency distinctions.

### Software implementation
- Implement `Pump` model and curve data validation.
- Support manufacturer curve-point ingestion and documented interpolation rules.
- Implement robust operating point calculation and no-intersection warnings.
- Model parallel identical pumps first; extend to unequal pumps if justified.
- Compute power and efficiency metrics using the appropriate curve data.
- Generate overlay plots for pump and system curves.

### Deliverables
- Single-pump and parallel-pump operating point analyzer.
- Pump/system curve visualization.
- Pump selection comparison example.

### Acceptance criteria
- [ ] Operating points satisfy both curves within a documented tolerance.
- [ ] Extrapolation is prohibited or clearly flagged.
- [ ] Multiple intersections and absent intersections are handled explicitly.
- [ ] Parallel pump results are verified against independent examples.

## Phase 4 — Wet well hydraulics and lift station operation

**Objective:** Connect influent conditions, controls, storage, and pumping behavior.

### Engineering knowledge
- Average, peak, and minimum influent flows and their basis.
- Wet well geometry, operating volume, control elevations, and pump submergence.
- Starts per hour, minimum run times, and manufacturer operating constraints.
- Duty/standby, lead/lag, firm capacity, and pump failure scenarios.
- Constant-speed versus variable-speed operation and level-control logic.
- Emergency storage versus normal cycling storage; separate design criteria.

### Software implementation
- Implement `WetWell` geometry and volume-elevation relationship.
- Implement analytical cycling checks for simplified cases.
- Add a time-step wet well simulation with configurable inflow and controls.
- Connect pumping rates to hydraulic operating points where appropriate.
- Report starts, runtime, level extremes, and unmet inflow conditions.

### Deliverables
- Preliminary wet well sizing calculator.
- Time-series simulation for a duplex station.
- Control-level and pump-cycling comparison.

### Acceptance criteria
- [ ] Analytical cases match independently derived cycle calculations.
- [ ] Simulation preserves mass balance within a defined numerical tolerance.
- [ ] Pump starts/stops and level transitions behave as specified.
- [ ] Firm-capacity checks clearly distinguish installed from available capacity.

## Phase 5 — Operating envelope and design screening

**Objective:** Identify important risks that a single design-point calculation can miss.

### Engineering knowledge
- Minimum/maximum operating head and force main velocities.
- Wastewater retention, solids transport, and applicable owner criteria.
- NPSH available, NPSH required, submergence, and cavitation risk.
- Pressure class, low-pressure/high-point behavior, and air/vacuum management.
- Power failure, check valve behavior, surge exposure, and need for transient analysis.
- Reliability, redundancy, and operational constraints.

### Software implementation
- Create configurable design checks, each with criteria source and severity.
- Report pass/warn/fail/not evaluated without implying regulatory approval.
- Add preliminary surge screening flags; **do not claim to perform full transient analysis**.
- Create scenario comparisons for minimum, normal, and maximum conditions.

### Deliverables
- Operating-envelope report and screening checklist.
- Documented referrals to detailed analysis when screening indicates risk.

### Acceptance criteria
- [ ] Every threshold identifies its source or user-supplied assumption.
- [ ] Missing information produces an explicit not-evaluated result.
- [ ] The software never treats screening as proof that a design is safe.

## Phase 6 — Integrated lift station replacement case study

**Objective:** Demonstrate a complete, realistic engineering workflow.

### Engineering knowledge
- Existing-system assessment and design flow basis.
- Existing versus proposed force main options.
- Pump selection under operating extremes.
- Wet well controls, redundancy, and operating reliability.
- Interfaces with downstream systems and construction/operational constraints.

### Software implementation
- Define a structured `LiftStationScenario` or equivalent orchestration layer.
- Run all calculations from one documented input set.
- Compare alternative pumps, force main sizes, and control settings.
- Produce scenario tables, plots, and design-check results.

### Deliverables
- Fully worked fictional LS-001 replacement study.
- Existing/proposed and sensitivity comparison.

### Acceptance criteria
- [ ] A new user can reproduce the complete study from repository instructions.
- [ ] Each reported result can be traced to its inputs and calculation method.
- [ ] Alternative scenarios can be compared without changing core equations.

## Phase 7 — Reporting, validation, and Version 1 release

**Objective:** Produce reviewable engineering outputs and a stable Python release.

### Engineering knowledge
- Calculation-package organization and independent checking.
- Communication of design assumptions, uncertainties, warnings, and limitations.
- Interpretation of model results versus design decisions.

### Software implementation
- Export machine-readable results to JSON and CSV.
- Generate a human-readable engineering report (PDF optional after report content is stable).
- Include inputs, assumptions, references, method, outputs, warnings, and verification status.
- Add documentation examples, CI checks, and release notes.

### Deliverables
- Reproducible engineering report.
- Version 1 example package and release checklist.

### Acceptance criteria
- [ ] Automated tests and engineering verification cases pass.
- [ ] Reports include calculation traceability and limitations.
- [ ] The fictional case study runs end to end from a clean environment.

---

## 5. Recommended implementation order

1. Document the engineering design basis and verification method.
2. Audit existing `Pipe`, `Fitting`, `hazen_williams.py`, `friction_loss.py`, fitting helpers, data modules, and tests.
3. Implement and verify Hazen-Williams and Darcy-Weisbach equations.
4. Implement and verify minor losses and multi-segment force main calculations.
5. Generate a force main system curve.
6. **Start the Pump class** and ingest manufacturer performance curves.
7. Calculate single-pump operating points and plot pump/system curves.
8. Extend to parallel pumps and power/efficiency analysis.
9. Implement wet well sizing and cycling calculations.
10. Implement level-control time-step simulation.
11. Add operating-envelope screening checks.
12. Complete the integrated lift station case study.
13. Add reporting and release validation.

## 6. Repository-aligned target package layout

Preserve the existing package and use the more structured domain organization as a **target state**. Existing files are marked `[EXISTING]`; proposed files/packages are `[PLANNED]`. `__init__.py` files are omitted below for readability.

```text
src/hydraulic_analysis_suite/
├── models/
│   ├── pipe.py                         [EXISTING]
│   ├── fitting.py                      [EXISTING]
│   ├── valve.py                        [PLANNED]
│   ├── force_main.py                   [PLANNED]
│   ├── pump.py                         [PLANNED — NOT STARTED]
│   └── wet_well.py                     [PLANNED]
├── equations/
│   ├── hazen_williams.py               [EXISTING]
│   ├── darcy_weisbach.py               [PLANNED]
│   ├── minor_losses.py                 [PLANNED]
│   └── pump_affinity.py                [PLANNED]
├── calculators/
│   ├── friction_loss.py                [EXISTING]
│   ├── minor_loss.py                   [PLANNED]
│   └── tdh.py                          [PLANNED]
├── analysis/                            [PLANNED PACKAGE]
│   ├── force_main.py
│   ├── system_curve.py
│   ├── pump_performance.py
│   ├── pump_combination.py
│   ├── wet_well_sizing.py
│   └── pump_cycling.py
├── design_checks/                       [PLANNED PACKAGE]
│   ├── velocity.py
│   ├── npsh.py
│   ├── pump_operation.py
│   └── surge_screening.py
├── data/
│   ├── constants.py                    [EXISTING]
│   ├── pipe_materials.py               [EXISTING]
│   ├── fitting_coefficients.py         [EXISTING]
│   ├── fittings_data.py                [EXISTING]
│   └── pump_data.py                    [PLANNED]
├── utils/
│   └── fitting_data_helper_calcs.py    [EXISTING]
├── cli/
│   └── user_input.py                   [EXISTING]
└── reporting/                           [PLANNED PACKAGE]

tests/
├── unit/
├── integration/
└── verification/

docs/
├── engineering/
├── architecture/
└── verification/

examples/
└── lift_station_replacement/
```

### Architecture responsibilities

- **Models** represent physical equipment and validated properties; they do not prompt users or generate reports.
- **Equations** are deterministic numerical functions, independently testable without model construction where practical.
- **Calculators** implement focused calculation workflows such as friction loss and TDH.
- **Analysis** evaluates a range of flows, operating conditions, and interacting components; it generates system curves, operating points, and cycling scenarios.
- **Design checks** compare computed conditions with *documented and configurable* project criteria; warnings are not universal design approvals.
- **Data** contains reference information and provenance, including limitations of coefficients and assumed values.
- **Reporting** serializes analysis results and verification metadata; it should not silently recalculate values.
- **CLI** handles interaction and stays outside the calculation engine.

### Incremental refactoring strategy

1. **Audit before moving:** document public interfaces and imports for existing `Pipe`, `Fitting`, Hazen-Williams, and friction loss modules.
2. **Add regression tests:** preserve currently correct outputs and validate units before changing implementation.
3. **Refactor only when a capability requires it:** for example, move domain-specific helper calculations out of `utils/` if they belong in `equations/`, with compatibility imports or a deliberate breaking-change note.
4. **Introduce `analysis/` with the first system curve:** avoid creating empty packages just to match the target tree.
5. **Introduce `design_checks/` with actual criteria and tests:** do not hard-code a single owner's limits as universal standards.
6. **Build `Pump` only in Phase 3:** begin with manufacturer curve data, units, and interpolation tests, then add operating-point logic.
7. **Document every migration:** update tests, imports, examples, and README together.

### Domain boundaries that need special care

A `ForceMain` model describes physical segments and fittings; `analysis/force_main.py` calculates performance across conditions. A `Pump` model stores manufacturer performance data; `analysis/pump_performance.py` determines operating points. This distinction avoids mixing equipment data with analysis workflows.

---

## 7. Fictional design case study: LS-001

Start with a deliberately simple, fictional replacement station, then increase complexity as capabilities mature.

| Input | Initial illustrative value |
|---|---|
| Configuration | Duplex submersible, duty/standby |
| Design discharge | 750 gpm |
| Force main | 8-inch nominal, 3,500 ft |
| Static head | 35 ft, based on defined upstream/downstream hydraulic boundaries |
| Downstream condition | Specified receiving hydraulic grade |
| Pump curves | Added when Phase 3 begins |
| Wet well dimensions/controls | Added when Phase 4 begins |

**These are software-development example inputs, not project design criteria or recommended selections.**

Case-study questions:
1. How do pipe size, material, and roughness affect system head?
2. How do minimum and maximum boundary levels shift the system curve?
3. Where will each candidate pump operate?
4. What happens when two pumps operate together?
5. How do wet well levels and control settings affect cycling?
6. What operating constraints or risks require additional analysis?
7. Which design option is most defensible under documented criteria?

## 8. Engineering reference strategy

Maintain a source register rather than embedding unsupported universal rules. Candidate reference categories include:

- Applicable Florida wastewater collection and pumping regulations.
- Specific utility/owner design standards and approved materials criteria.
- Hydraulic Institute pump selection, operating region, NPSH, and related guidance.
- Pump manufacturer certified performance data and installation manuals.
- Recognized hydraulics references for friction, minor losses, HGL/EGL, and transient fundamentals.
- Applicable engineering codes, permitting requirements, and electrical/control standards as project scope requires.

For each criterion or coefficient, record **reference title, edition/date, section, applicability, assumption, and any owner-specific override**. Verify the current governing edition before use on a live project.

## 9. Project-specific EOR readiness track (separate from HAS development)

The actual lift station replacement must not wait for HAS to be finished. Maintain a separate project design-basis and review process covering:

- Existing record drawings, survey, condition, and utility conflicts.
- Existing and projected influent flows, peaking basis, and future growth.
- Downstream force main and receiving-system constraints.
- Pump duty points across operating conditions and manufacturer confirmation.
- Wet well dimensions, controls, emergency storage, and standby power requirements.
- Electrical, controls, instrumentation, access, maintenance, and safety coordination.
- Bypass pumping, sequencing, outage management, and constructability.
- Surge/transient evaluation needs and air/vacuum management.
- Applicable owner standards, permitting, regulatory requirements, and independent reviews.

**Escalate gaps early:** identify where a senior pump specialist, electrical/control engineer, transient modeler, or independent technical reviewer should support the design.

## 10. Explicitly out of scope for Version 1

- GUI or browser application.
- Cloud hosting, authentication, multi-user accounts, and databases.
- AI/ML features and predictive maintenance.
- Full hydraulic transient solver and automated surge protection design.
- Comprehensive wastewater collection network modeling.
- Full CFD or detailed pump mechanical design.
- Automatic regulatory compliance certification or replacement of engineering judgment.

Interactive visualizations and simple plots are **in scope** because they directly support engineering understanding. A web platform is not required.

## 11. Version 1 release gate

Version 1 is ready when all of the following are true:

- [ ] Core component models and numerical methods have documented tests.
- [ ] Engineering verification examples have independent expected results.
- [ ] Multi-segment force main head and system curves are reproducible.
- [ ] Single and parallel pump operating points are calculated and verified.
- [ ] Wet well sizing and pump cycling are analyzed for defined conditions.
- [ ] Important operating constraints are checked or explicitly marked not evaluated.
- [ ] A complete fictional lift station replacement example runs end to end.
- [ ] Reports disclose inputs, assumptions, references, warnings, and limitations.
- [ ] Installation, usage, and developer documentation are complete.

## 12. Immediate next development sprint

**Sprint goal:** Establish a verified baseline and prepare for the first complete force main system curve.

- [ ] Review current `Pipe` and `Fitting` code, existing data, and tests.
- [ ] Correct roadmap statuses against actual repository contents.
- [ ] Create `design_basis.md` and `verification_plan.md`.
- [ ] Select the initial LS-001 hydraulic boundaries and internal unit convention.
- [ ] Define independent Hazen-Williams and Darcy-Weisbach test cases.
- [ ] Implement the first verified equation module without beginning the Pump class.

**Sprint completion evidence:** reviewed component inventory, documented engineering assumptions, and at least one independently verified hydraulic calculation running through automated tests.
