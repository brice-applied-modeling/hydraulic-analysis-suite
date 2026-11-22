# Hydraulic Analysis Suite — Version 1 MVP Development Checklist

**Revision:** 1.2 · November 2026  
**Companion roadmap:** `HAS_V1_Development_Roadmap_Comprehensive.md`  
**Release sequence:** **MVP 1A → MVP 1B → HAS v1.0**

## How to use this checklist

**This document has two independently defined release milestones.** Complete the entire MVP 1A section and its release gate before declaring the hydraulic foundation released. MVP 1B assumes MVP 1A is working and verified; it adds lift station functionality without reopening completed tasks unless a regression is discovered.

- `[x]` = implementation and stated verification confirmed; `[ ]` = incomplete or not yet verified.
- **Existing file** = visible in repository; this alone is not evidence of functionality.
- Each release includes engineering learning, software implementation, verification, documentation, and an explicit acceptance gate.
- Phase numbering (0–7) remains aligned to the comprehensive roadmap. Phase 7 is split into **7A** and **7B** so reporting and release tasks belong to the correct MVP.
- Engineering criteria and calculations must be reviewed independently before use in actual EOR work.

## Release scope at a glance

| | MVP 1A: Hydraulic Foundation | MVP 1B: Lift Station Analysis |
|---|---|---|
| Primary goal | Verified force-main hydraulic analysis | Verified pump and lift station operating analysis |
| Roadmap phases | 0, 1, 2, 7A | 3, 4, 5, 6, 7B |
| Main output | Friction/minor losses, TDH, multi-segment system curve | Pump operating points, wet well operation, design screening, LS-001 |
| User workflow | CLI **or** reproducible example script | Integrated case-study workflow and engineering report |
| Release gate | Independent hydraulic verification | Independent pump, cycling, and integrated verification |
| Dependency | Existing repository audit | **MVP 1A released** |

## Repository baseline (observed, not verified)
| Existing path under `src/hydraulic_analysis_suite/` | Observed state | Next action |
|---|---|---|
| `models/pipe.py` | Present; prior checklist called complete | Audit, test, document assumptions |
| `models/fitting.py` | Present; prior checklist called partial | Finish and test |
| `equations/hazen_williams.py` | Present; prior checklist said not started | Inspect before changing status |
| `calculators/friction_loss.py` | Present; prior checklist said not started | Inspect before changing status |
| `data/constants.py` | Present | Check units and provenance |
| `data/pipe_materials.py` | Present | Check internal diameters and roughness assumptions |
| `data/fitting_coefficients.py` | Present | Check source and applicability |
| `data/fittings_data.py` | Present | Check data integrity |
| `utils/fitting_data_helper_calcs.py` | Present | Audit and retain until tested refactor |
| `cli/user_input.py` | Present; previously described as stub | Defer enhancements until calculations work |
| `models/pump.py` | **Not started** | Create in Phase 3 |
| `models/valve.py`, `models/force_main.py`, `models/wet_well.py` | Not shown / planned | Create when needed |
| `analysis/`, `design_checks/`, `reporting/` | Planned | Introduce incrementally |

**Architecture rule:** Keep current `models/`, `equations/`, `calculators/`, `data/`, `utils/`, and `cli/` while introducing `analysis/`, `design_checks/`, and `reporting/` when warranted. Move existing logic only with regression tests.

---

# PART I — MVP 1A: HYDRAULIC FOUNDATION

**Release objective:** A verified, reproducible hydraulic analysis workflow that computes flow velocity, pipe friction, fitting/valve losses, boundary-condition-aware TDH, and a multi-segment force-main system curve. **No pump model is required for this release.**

**Included:** Phases 0–2, 7A. **Excluded:** Pump curves, parallel pumping, wet well, NPSH, LS-001 integrated pump design.

## Phase 0 —Engineering design basis and verification framework

### Engineering Knowledge
- [x] Establish intended use, limitations, fluid properties, and steady-state assumptions.
- [x] Define force-main boundary conditions: suction/wet-well level, discharge HGL, pressure conditions, and elevation datum.
- [x] Define units (gpm, inches, feet, fps, psi, hp) and conversions; avoid mixing inside calculations.
- [x] Identify owner criteria, Florida requirements, manufacturer information, and engineering references relevant to the case study.
- [x] Define independent verification methods, and tolerances.
- [x] Define pump selection process, including BEP, operating region, and NPSH requirements.
- [ ] Define wet well sizing, cycling, and control assumptions.
- [ ] Define design checks for velocity, NPSH, pump operation, and surge screening.
- [ ] Define operating scenarios for low/high head, pump combinations, and off-design conditions.
- [ ] Define surge/transient screening criteria and study triggers.

### Software Implementation
- [x] Create `docs/engineering/design_basis.md`.
- [ ] Create `docs/verification/verification_plan.md` and a case-result template.
- [ ] Audit `data/constants.py` for units, gravity, viscosity, and traceable sources.
- [ ] Audit `data/pipe_materials.py`, `fitting_coefficients.py`, and `fittings_data.py` for applicability and provenance.
- [ ] Decide numerical output schema, error handling, and internal unit conventions.
- [ ] Define LS-001 illustrative scenario and separate it from any real project information.

### Acceptance / evidence
- [ ] Design basis reviewed and version-controlled.
- [ ] At least one independent benchmark defined before implementing the corresponding equation.
- [ ] Input assumptions and limitations are visible in outputs or supporting documentation.
- [ ] The engineering design basis is documented and committed. 
- [ ] The numerical conventions and output schema are defined. 
- [ ] Existing engineering reference data have been audited, with findings documented. 
- [ ] At least one independent benchmark has been established with expected numerical results and tolerances. 
- [ ] The verification plan and reusable case template are complete. 
- [ ] The fictional LS-001 scenario has documented assumptions and boundaries.

---

## Phase 1 — Core hydraulic component models

### 1.1 Pipe model — `models/pipe.py`
- [ ] Audit constructor, input validation, materials lookup, derived area/volume/velocity, and representation.
- [ ] Confirm internal vs nominal diameter, pressure class, and dimensional conventions.
- [ ] Verify zero flow, negative/invalid inputs, units, and material defaults.
- [ ] Add or review `tests/models/test_pipe.py` with independent expected values.

### 1.2 Fitting model — `models/fitting.py`
The following tasks are carried forward from the original checklist; inspect existing implementation before marking them complete.

#### Detailed Fitting Class Tasks (`models/fitting.py`)

The Fitting class requires completion before the minor loss calculator can be built.

- [ ] Add `_lookup_fitting_data()` method to retrieve full fitting record from `fittings_data.py`
- [ ] Add `fitting_category` derived property (from database lookup)
- [ ] Add `k_factor` derived property (from database lookup)
- [ ] Add `equivalent_length_ratio` derived property (from database lookup)
- [ ] Add `source` derived property (engineering reference source)
- [ ] Add `source_reference` derived property (specific table, page, or section)
- [ ] Add `engineering_notes` derived property (assumptions and limitations)
- [ ] Add `diameter_ft` computed property (convert `diameter_in` to feet)
- [ ] Add `equivalent_length_ft` computed property (`equivalent_length_ratio × diameter_ft`)
- [ ] Add `total_equivalent_length_ft` computed property (`equivalent_length_ft × quantity`)
- [ ] Add `description` property (human-readable fitting summary string)
- [ ] Add `_validate_fitting_name()` method (check name exists in database)
- [ ] Add `_validate_diameter()` method (check diameter > 0)
- [ ] Add `_validate_quantity()` method (check quantity > 0)
- [ ] Refactor `_validate_data()` to call the three individual validators above
- [ ] Add `summary()` method (formatted multi-line engineering summary output)
- [ ] Update `__str__()` to return a concise single-line description
- [ ] Update `__repr__()` to return a developer-readable representation

#### Detailed Fitting Model Tests (`tests/models/test_fitting.py`)

- [ ] Test valid fitting creation with all required attributes
- [ ] Test `_validate_fitting_name()` raises error for unrecognized fitting name
- [ ] Test `_validate_diameter()` raises error for diameter ≤ 0
- [ ] Test `_validate_quantity()` raises error for quantity ≤ 0
- [ ] Test `k_factor` returns correct value from database
- [ ] Test `equivalent_length_ratio` returns correct value from database
- [ ] Test `equivalent_length_ft` computes correctly
- [ ] Test `total_equivalent_length_ft` scales correctly with quantity
- [ ] Test `fitting_category` returns correct category string
- [ ] Test `source` and `source_reference` return correct engineering references
- [ ] Test `engineering_notes` returns correct notes
- [ ] Test `summary()` returns a non-empty formatted string
- [ ] Test `__str__()` returns a single-line string
- [ ] Test `__repr__()` returns a valid developer string

### 1.4 Valve model — `models/valve.py` (planned)
- [ ] Define valve type, size, state/position, quantity, and coefficient source.
- [ ] Support a clearly documented K-based loss model where appropriate.
- [ ] Identify cases requiring manufacturer-specific loss curves or Cv-based treatment.
- [ ] Prevent double counting valves as both fittings and dedicated valve losses.
- [ ] Add `tests/models/test_valve.py`.

### 1.5 Model/data integration
- [ ] Check fitting coefficients for diameter, geometry, flow regime, and source limitations.
- [ ] Review `utils/fitting_data_helper_calcs.py`; relocate engineering formulas only if tests justify it.
- [ ] Define how multiple fittings and valves attach to a specific pipe segment.

### Acceptance / evidence
- [ ] Pipe/Fitting/Valve models validate inputs and produce consistent units.
- [ ] Coefficients and assumptions are traceable.
- [ ] Model unit tests pass, with representative independent examples.

---

## Phase 2 — Equations, force main hydraulics, and system curves

### Engineering Knowledge
- [ ] Explain Hazen-Williams applicability and C-factor sensitivity.
- [ ] Explain Darcy-Weisbach, Reynolds number, laminar/transitional/turbulent regimes, and friction-factor methods.
- [ ] Explain local losses, reference velocity, and fitting location/diameter.
- [ ] Distinguish static boundary head from high-point elevation; interpret HGL/EGL and pressure.
- [ ] Derive a system head curve as a function of flow and define the zero-flow boundary case.

### 2.1 Hazen-Williams — `equations/hazen_williams.py` (existing file)
- [ ] Inspect existing implementation before changing its status.
- [ ] Implement/verify `headloss(flow_rate_gpm, diameter_in, length_ft, c_factor)` with a **unit-consistent** equation and reference.
- [ ] Implement/verify `headloss_per_1000ft(...)`.
- [ ] Implement/verify `velocity(flow_rate_gpm, diameter_in)` or reuse a shared geometry utility.
- [ ] Document coefficient units, flow sign convention, valid inputs, and limitations.
- [ ] Test zero flow, invalid inputs, and hand-calculated cases in `tests/equations/test_hazen_williams.py`.
- [ ] **Correct prior checklist ambiguity:** the coefficient `4.727` is not valid with unconverted gpm and inch inputs; verify conversions before use.

### 2.2 Darcy-Weisbach — `equations/darcy_weisbach.py` (planned)
- [ ] Implement `reynolds_number(...)` with explicit viscosity units.
- [ ] Implement `friction_factor(...)` for laminar flow and an appropriate turbulent solver/approximation.
- [ ] Define handling for transitional flow and numerical convergence.
- [ ] Implement `headloss(...) = f(L/D)V²/(2g)`.
- [ ] Test laminar `64/Re`, turbulent benchmark, zero velocity, and invalid values.
- [ ] Compare friction factor against an independent Moody/Colebrook reference.

### 2.3 Minor losses — `equations/minor_losses.py` (planned)
- [ ] Implement pure `headloss(k_factor, velocity_fps)` function.
- [ ] Aggregate losses by segment using each fitting's local velocity and quantity.
- [ ] Verify K method vs equivalent-length method is not counted twice.
- [ ] Test zero velocity, zero K, multiple fittings, and differing diameters.

### 2.4 Calculators — `calculators/friction_loss.py`, `minor_loss.py`, `tdh_calculator.py`
- [ ] Inspect and complete existing friction-loss calculator.
- [ ] Accept `Pipe` inputs and return typed/structured friction results for both methods.
- [ ] Implement minor-loss calculator without CLI or printing.
- [ ] Implement TDH calculator with a complete energy-balance boundary definition.
- [ ] Report static, friction, minor, and other applicable energy terms separately.
- [ ] **Do not automatically add a terminal velocity-head term** to static + friction + minor losses; include it only when required by the chosen boundary conditions.
- [ ] Test component breakdown, zero static head, no fittings, and independently checked TDH.

### 2.5 Force main model — `models/force_main.py` (planned)
- [ ] Represent ordered pipe segments, fittings/valves, elevations, and discharge boundary.
- [ ] Support different diameters, lengths, materials, and roughness values.
- [ ] Aggregate losses by segment and expose pressure/HGL information where inputs permit.
- [ ] Evaluate minimum, normal, and maximum discharge boundary conditions.
- [ ] Verify segment-level and total results against hand calculations.

### 2.6 System curves — `analysis/system_curve.py` (planned)
- [ ] Calculate required head for an array of flows, including zero flow.
- [ ] Generate tabular system curve and head component breakdown.
- [ ] Plot head vs flow for selected operating conditions.
- [ ] Test monotonic behavior under appropriate assumptions and verify sample points independently.
- [ ] Save reproducible example inputs/outputs for LS-001.

### Acceptance / evidence
- [ ] Pure equation functions are independent of CLI and presentation.
- [ ] Multi-segment head losses are correctly summed at each segment's diameter.
- [ ] System curve and boundary assumptions are independently verified.
- [ ] No duplicate velocity-head or fitting losses.

---

## Phase 7A — MVP 1A workflow, verification, and documentation

### 7A.1 Runnable hydraulic workflow
- [ ] Review existing `cli/user_input.py` before extending it.
- [ ] Provide a repeatable CLI command **or** documented example script for MVP 1A.
- [ ] Accept pipe geometry/material, flow, boundary head, and fittings; validate inputs and re-prompt if using interactive CLI.
- [ ] Display velocity, friction comparison, minor losses, TDH, and system curve with explicit units.
- [ ] Configure `pyproject.toml` entry point if CLI is chosen; verify package imports and `pytest`.

### 7A.2 Engineering validation — original hydraulic cases
The following scenarios are preserved from the original MVP checklist. Where the original assumed a universal velocity-head addition or unqualified tolerance, verify the governing energy balance and set documented, case-specific acceptance criteria instead.

### 6.1 Hand Calculation Verification

Hand calculations serve as the primary baseline. Results must match within an acceptable engineering tolerance (documented case-specific tolerances).

**Scenario A — Basic Friction Loss**

```text
Flow Rate:  500 gpm
Pipe:       8-inch PVC (C = 150)
Length:     1,000 ft
```

- [ ] Calculate velocity by hand and compare to application output
- [ ] Calculate Hazen-Williams headloss by hand and compare to application output
- [ ] Calculate Darcy-Weisbach headloss by hand and compare to application output
- [ ] Document results in a verification table

**Scenario B — TDH with Minor Losses (boundary-condition aware)**

```text
Flow Rate:  500 gpm
Pipe:       8-inch PVC (C = 150)
Length:     1,000 ft
Static Head: 25 ft
Fittings:   2 × 90° Standard Radius Bend, 1 × Gate Valve (Fully Open)
```

- [ ] Calculate minor losses by hand (K × V²/2g for each fitting) and compare to application output
- [ ] Calculate TDH by hand using the full energy equation, adding terminal velocity head only when applicable and compare to application output
- [ ] Document results in a verification table

**Scenario C — Low Flow / Small Diameter**

```text
Flow Rate:  50 gpm
Pipe:       4-inch DIP (New)
Length:     500 ft
```

- [ ] Verify Hazen-Williams headloss matches hand calculation
- [ ] Verify Darcy-Weisbach friction factor is reasonable (check against Moody chart)
- [ ] Document results in a verification table

**Scenario D — High Flow / Large Diameter**

```text
Flow Rate:  2,500 gpm
Pipe:       16-inch DIP (New)
Length:     5,000 ft
```

- [ ] Verify Hazen-Williams headloss matches hand calculation
- [ ] Verify velocity is within a typical design range (2–8 fps)
- [ ] Document results in a verification table

---

### 6.2 Comparison Against EPA Reference Spreadsheets

The EPA publishes hydraulic design tools and technical guidance used in water and wastewater engineering practice.

- [ ] Identify a relevant EPA friction loss or hydraulic design spreadsheet (e.g., from the EPA SWMM documentation or WaterSense technical resources)
- [ ] Run the same input scenario through the EPA tool and the application
- [ ] Record both results side-by-side
- [ ] Document any discrepancies and identify root cause (formula variation, unit difference, assumption difference)
- [ ] Confirm results agree within acceptable engineering tolerance

---

### 6.3 Comparison Against Bentley WaterGEMS or WaterCAD

WaterGEMS and WaterCAD are industry-standard hydraulic modeling platforms used in water distribution and force main design. Matching output from these tools demonstrates that the application produces credible engineering results.

- [ ] Set up an equivalent pipe segment in WaterGEMS or WaterCAD matching a verification scenario
- [ ] Run steady-state hydraulic analysis
- [ ] Record headloss, velocity, and pressure results from WaterGEMS/WaterCAD
- [ ] Run the same scenario through the application
- [ ] Compare results in a side-by-side table
- [ ] Document any discrepancies and identify root cause
- [ ] Confirm Hazen-Williams results agree within acceptable engineering tolerance
- [ ] Confirm Darcy-Weisbach results agree within acceptable engineering tolerance

---

### 6.4 Comparison Against Utility Design Examples

Utility design standards and engineering manuals often include worked examples that can serve as independent validation benchmarks.

- [ ] Identify at least one worked example from a recognized reference:
  - Pumping Station Design (3rd Edition) — Sanks et al.
  - AWWA M11, M23, or M41 design examples
  - Hydraulic Institute Engineering Data Book
  - Cameron Hydraulic Data worked examples
  - WEF Manual of Practice No. 8 or MOP 11
- [ ] Reproduce the worked example inputs in the application
- [ ] Compare application output to the published solution
- [ ] Document the reference, inputs, expected output, and actual output
- [ ] Note any formula or assumption differences between the reference and the application

---

### 6.5 Validation Documentation

All validation results must be captured and stored for traceability.

- [ ] Create a `docs/06_validation/` directory
- [ ] Create `docs/06_validation/00_validation_summary.md` with:
  - Summary table of all validation scenarios
  - Pass/Fail status for each comparison
  - Reference sources used
  - Acceptable tolerance criteria
- [ ] Create one validation record per scenario (e.g., `01_hand_calc_scenario_a.md`)
  - Input parameters
  - Expected result (from reference)
  - Application result
  - Difference (absolute and percent)
  - Pass/Fail determination
  - Notes on any assumptions or discrepancies
- [ ] Update the validation summary table after each scenario is completed

---

### 7A.3 Documentation and packaging
- [ ] Update README with **only MVP 1A supported** features, installation, example commands, and limitations.
- [ ] Document model and equation APIs, units, coefficients, and assumptions.
- [ ] Record independent comparison results, tolerances, and discrepancies.
- [ ] Run unit, integration, and verification tests for MVP 1A.
- [ ] Record known limitations and out-of-scope MVP 1B capabilities.

## MVP 1A release gate — must all pass
- [ ] Models and equations have verified tests and documented units.
- [ ] Friction and minor losses work for multi-segment force mains.
- [ ] TDH uses a correct, explicit energy balance.
- [ ] System curve is independently verified and reproducible.
- [ ] A CLI or example workflow runs with documented inputs/outputs.
- [ ] Original hand-calculation scenarios and at least one independent published/tool comparison are documented.

## MVP 1A immediate next sprint
- [ ] Inspect `pipe.py`, `fitting.py`, `hazen_williams.py`, `friction_loss.py`, and existing tests.
- [ ] Reconcile each existing file's status with actual implementation and tests.
- [ ] Create design basis and verification plan.
- [ ] Fix any unit/equation discrepancies before adding functionality.
- [ ] Verify one Hazen-Williams case with an independent calculation.
- [ ] Define the first multi-segment system-curve test case.
- [ ] **Do not begin the Pump class yet.**

---

# PART II — MVP 1B: LIFT STATION ANALYSIS

**Entry condition:** MVP 1A release gate passed and baseline tests retained as regression tests.

**Release objective:** Extend the hydraulic engine to analyze pump/system intersections, multiple pumps, wet well storage and cycling, operating limits, and a complete fictional LS-001 lift station replacement study.

**Included:** Phases 3–6, 7B.

## Phase 3 —Pump performance and operating points

**Status: Pump class NOT STARTED.** Begin only after a verified system curve exists.

### Engineering Knowledge
- [ ] Study pump head-flow, efficiency, power, and NPSHr curves.
- [ ] Explain BEP, preferred/allowable operating regions, minimum flow, and runout.
- [ ] Explain system/pump curve intersection, multiple intersections, and no-intersection cases.
- [ ] Explain parallel pump behavior, combined curves, and non-doubling of flow.
- [ ] Explain affinity laws, VFD limitations, and power/motor margin.

### Software Implementation
- [ ] Create `models/pump.py` with pump identity, rated/design point, speed, and performance data.
- [ ] Implement strict validation of ordered curve points and unit consistency.
- [ ] Create `data/pump_data.py` for sample/manufacturer-source curve data with provenance.
- [ ] Create `analysis/pump_performance.py` for curve interpolation and intersection solver.
- [ ] Avoid unjustified extrapolation beyond the provided manufacturer curve.
- [ ] Compute hydraulic horsepower, brake horsepower, and efficiency where inputs support them.
- [ ] Create `analysis/pump_combination.py` for parallel pump curves at equal head.
- [ ] Implement single-pump and two-pump operating points for multiple system curves.
- [ ] Plot pump/system curves and label operating points and known operating limits.
- [ ] Add `tests/models/test_pump.py`, `tests/analysis/test_pump_performance.py`, and `test_pump_combination.py`.
- [ ] Independently verify one single-pump and one parallel-pump case.

### Acceptance / evidence
- [ ] Curve interpolation and root finding are numerically verified.
- [ ] Invalid/nonphysical data and no-intersection cases are handled explicitly.
- [ ] Pump power/efficiency calculations disclose assumptions.

---

## Phase 4 — Wet well hydraulics and lift station operation

### Engineering Knowledge
- [ ] Establish average, peak, minimum, and future influent design flow assumptions.
- [ ] Study wet well operating volume, geometric constraints, pump submergence, and control elevations.
- [ ] Derive idealized cycle-time and starts-per-hour relationships and their assumptions.
- [ ] Understand duty/standby operation, firm capacity, alternating duty, and lead/lag controls.
- [ ] Examine variable-speed pumping and time-varying inflow.

### Software Implementation
- [ ] Create `models/wet_well.py` with geometry, elevation-volume relationship, and control levels.
- [ ] Implement `analysis/wet_well_sizing.py` for usable volume and idealized cycling calculations.
- [ ] Create `analysis/pump_cycling.py` for time-step wet well level simulation.
- [ ] Model pump start/stop controls, lead/lag, and pump availability.
- [ ] Use pump discharge consistent with operating conditions; do not always assume constant design flow.
- [ ] Report starts/hour, run times, levels, overflow or low-level conditions.
- [ ] Add unit and scenario tests, including zero inflow, high inflow, and pump unavailable.
- [ ] Verify a constant-inflow analytical case against the simulation.

### Acceptance / evidence
- [ ] Storage and cycle calculations are independently checked.
- [ ] Control logic behaves predictably at level thresholds.
- [ ] Model assumptions and limitations are explicit.

---

## Phase 5 — Operating envelope and design screening

### Engineering Knowledge
- [ ] Evaluate force-main velocity, detention/retention time, and low-flow conditions.
- [ ] Evaluate maximum pressure and intermediate high-point pressure under steady state.
- [ ] Study NPSHa vs manufacturer NPSHr, submergence, and cavitation considerations.
- [ ] Study air release, vacuum, high points, and discharge conditions.
- [ ] Identify surge/transient triggers and the need for dedicated transient analysis.
- [ ] Evaluate pump operating regions, redundancy, and firm-capacity criteria.

### Software Implementation
- [ ] Create `design_checks/velocity.py` and configurable criteria, not universal pass/fail thresholds.
- [ ] Create `design_checks/npsh.py` with required physical inputs and margin reporting.
- [ ] Create `design_checks/pump_operation.py` for manufacturer operating range checks.
- [ ] Create `design_checks/surge_screening.py` for documented risk flags, **not** a full transient solver.
- [ ] Add detention/retention screening where flow and volume are adequately defined.
- [ ] Distinguish `PASS`, `WARNING`, `FAIL`, and `NOT_EVALUATED` with reasons.
- [ ] Test missing data, thresholds, and warning logic.

### Acceptance / evidence
- [ ] No screening result is presented as a complete engineering safety certification.
- [ ] Each check records criteria source, assumptions, and unassessed risks.

---

## Phase 6 — Integrated LS-001 replacement case study

### Engineering Knowledge
- [ ] Develop existing/proposed station design basis and system boundary conditions.
- [ ] Evaluate current and future flow conditions and firm capacity.
- [ ] Compare force main alternatives and roughness/age sensitivity.
- [ ] Compare candidate pumps over minimum/normal/maximum system curves.
- [ ] Evaluate wet well volume, controls, cycling, and off-design conditions.
- [ ] Document unresolved design risks and required specialist studies.

### Software Implementation
- [ ] Create `examples/lift_station_replacement/` with fictional, version-controlled inputs.
- [ ] Use the illustrative LS-001 scenario from the roadmap; verify all values and assumptions before calculation.
- [ ] Run pipe/fitting/valve → force main → system curve → pump → wet well → design checks.
- [ ] Add reproducible scenario results and integration tests.
- [ ] Compare at least two design alternatives or operating scenarios.
- [ ] Document sensitivity to roughness, discharge head, inflow, and pump availability.

### Acceptance / evidence
- [ ] Case study runs end-to-end without manual patching.
- [ ] Outputs are traceable to the input scenario and calculation version.
- [ ] Results include limitations and unanswered engineering questions.

---

## Phase 7B — MVP 1B reporting, verification, and documentation

### 7B.1 Engineering reporting
- [ ] Create `reporting/` once calculation result schemas are stable.
- [ ] Export JSON and CSV with units, metadata, and assumptions.
- [ ] Generate force-main and pump/system plots, wet well summary, and design-check results.
- [ ] Produce a human-readable engineering calculation package (PDF optional).
- [ ] Include sources, software version, warnings, and verification status.

### 7B.2 Integrated workflow
- [ ] Extend CLI or documented scripts to accept LS-001 pump, wet well, and control inputs.
- [ ] Run integrated force-main, pump, wet well, and operating-condition analyses.
- [ ] Re-run MVP 1A tests to detect regressions.

### 7B.3 Additional pump and lift station verification
- [ ] Multi-segment force main with two diameters and distinct local losses.
- [ ] Boundary-condition/HGL scenario with intermediate high point.
- [ ] System curve with independently computed sample flows.
- [ ] Manufacturer pump curve interpolation and single-pump intersection.
- [ ] Parallel-pump combined curve and operating point.
- [ ] Wet well analytical cycling vs time-step simulation.
- [ ] NPSH/design-check warning with explicit not-evaluated conditions.
- [ ] End-to-end LS-001 reproducibility and regression test.

### 7B.4 Documentation and release hygiene
- [ ] Update README with verified MVP 1B capabilities, installation, and LS-001 example.
- [ ] Document pump curves, parallel pump assumptions, cycling/control model, and limitations.
- [ ] Record source references for manufacturer curves and design screening criteria.
- [ ] Run all unit, integration, and engineering verification tests.
- [ ] Check stale imports and module references; do not require unimplemented future files.
- [ ] Publish known limitations, validation coverage, and excluded functionality.

## MVP 1B release gate — must all pass
- [ ] Pump and parallel-pump operating points are verified.
- [ ] Wet well sizing, cycling, and controls are verified for defined conditions.
- [ ] Operating checks report criteria and limits.
- [ ] LS-001 runs end-to-end with traceable input and output files.
- [ ] Engineering reporting and validation records are complete.
- [ ] Tests pass and README accurately reflects capabilities.

---

# PART III — PARALLEL ENGINEERING READINESS (NOT A RELEASE GATE)

These tasks support the actual EOR assignment independently of the HAS development schedule. They must not wait for MVP 1A or 1B completion.

These items support professional preparation for the actual replacement project and must be performed under the project's applicable requirements, schedule, and review procedures.

- [ ] Review owner standards, regulatory criteria, existing station records, and hydraulic data.
- [ ] Establish influent flows, receiving system constraints, and design boundaries.
- [ ] Identify pump selection, standby capacity, wet well, controls, electrical, and emergency operation requirements.
- [ ] Review bypass pumping, constructability, sequencing, permitting, and commissioning needs.
- [ ] Determine whether surge, air/vacuum, geotechnical, structural, electrical, or other specialist analyses are required.
- [ ] Arrange independent engineering checks and appropriate discipline reviews.
- [ ] Keep actual project/client data separate from public HAS examples.

## Deferred beyond Version 1
- Full transient solver; full hydraulic network solver; production GUI/web app; hosted database; authentication; multi-user features; AI/ML; cloud deployment; digital twin integration.
