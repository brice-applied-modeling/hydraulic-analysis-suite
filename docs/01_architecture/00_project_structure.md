# Hydraulic Analysis Suite — Project Structure

## Purpose

This document defines the **target Version 1 project structure** for the Hydraulic Analysis Suite (HAS), spanning **MVP 1A: Hydraulic Foundation** and **MVP 1B: Lift Station Analysis**. It describes architectural responsibilities, not implementation status. Files and packages should be introduced or refactored as development requires; the tree is the intended full buildout.

## Full Version 1 Structure

```text
hydraulic_analysis_suite/
├── pyproject.toml                         # Package metadata, dependencies, tool configuration
├── uv.lock                                # Reproducible dependency resolution
├── README.md                              # Project purpose, setup, documentation navigation
├── LICENSE                                # MIT license
├── .gitignore                             # Excluded local, build, and IDE files
├── .python-version                        # Preferred Python 3.12 interpreter
│
├── src/
│   └── hydraulic_analysis_suite/
│       ├── __init__.py                    # Public package interface and version
│       ├── __main__.py                    # Package command-line entry point
│       │
│       ├── models/                        # Physical assets and engineering input models
│       │   ├── __init__.py
│       │   ├── pipe.py                     # Pipe geometry, material, and hydraulic properties
│       │   ├── fitting.py                  # Fitting geometry, quantities, and loss data
│       │   ├── valve.py                    # Valve properties, operating state, and losses
│       │   ├── pump.py                     # Pump identity, rated data, and performance curves
│       │   ├── wet_well.py                 # Wet well geometry, elevations, and storage
│       │   ├── force_main.py               # Ordered pipe segments and appurtenances
│       │   ├── lift_station.py             # Pumps, wet well, controls, and discharge system
│       │   ├── operating_scenario.py       # Flow, boundary heads, and pump operating states
│       │   └── fluid.py                    # Density, viscosity, and temperature assumptions
│       │
│       ├── data/                          # Reference datasets and engineering constants
│       │   ├── __init__.py
│       │   ├── constants.py                # Units, gravity, and numerical constants
│       │   ├── pipe_materials.py           # Material roughness and C-factor records
│       │   ├── fitting_coefficients.py     # Loss coefficient reference values
│       │   ├── fittings_data.py            # Fitting catalog and reference metadata
│       │   ├── valves_data.py              # Valve catalog and loss coefficient references
│       │   └── pump_data.py                # Pump curve import schemas and sample records
│       │
│       ├── equations/                     # Pure numerical hydraulic relationships
│       │   ├── __init__.py
│       │   ├── continuity.py               # Flow, velocity, and cross-sectional area
│       │   ├── hazen_williams.py           # Hazen-Williams friction headloss
│       │   ├── darcy_weisbach.py           # Reynolds number, friction factor, and headloss
│       │   ├── minor_losses.py             # Local losses from K values and velocity
│       │   ├── energy.py                   # Bernoulli and energy-grade relationships
│       │   └── pump_affinity.py            # Pump speed, flow, head, and power scaling
│       │
│       ├── calculators/                   # Focused engineering calculations
│       │   ├── __init__.py
│       │   ├── friction_loss.py            # Friction losses for a specified pipe and flow
│       │   ├── minor_loss.py               # Aggregate fitting and valve losses
│       │   ├── tdh_calculator.py           # Head requirements between defined boundaries
│       │   ├── power.py                    # Hydraulic and brake horsepower calculations
│       │   └── wet_well_volume.py          # Geometry-based volume and drawdown storage
│       │
│       ├── analysis/                      # Multi-component hydraulic system workflows
│       │   ├── __init__.py
│       │   ├── force_main.py               # Segment-by-segment force main evaluation
│       │   ├── hydraulic_profile.py        # HGL/EGL and pressure profile calculations
│       │   ├── system_curve.py             # Required system head across a flow range
│       │   ├── pump_curve.py               # Curve interpolation and data validation
│       │   ├── pump_performance.py         # Operating point, efficiency, and power
│       │   ├── pump_combination.py         # Parallel pump performance and firm capacity
│       │   ├── wet_well_sizing.py          # Operating storage and cycling requirements
│       │   ├── pump_cycling.py             # Time-step wet well and pump-control simulation
│       │   └── scenario_comparison.py      # Compare hydraulic design alternatives
│       │
│       ├── design_checks/                 # Screening checks and engineering warnings
│       │   ├── __init__.py
│       │   ├── velocity.py                 # Minimum/maximum pipe velocity screening
│       │   ├── pressure.py                 # Steady-state pressure and rating screening
│       │   ├── pump_operation.py           # Pump operating region and power checks
│       │   ├── npsh.py                     # NPSHa versus manufacturer NPSHr checks
│       │   ├── wet_well.py                 # Cycling and operating level checks
│       │   ├── air_management.py           # High-point and air/vacuum review flags
│       │   ├── detention_time.py           # Force main retention-time screening
│       │   └── surge_screening.py          # Flags requiring transient specialist analysis
│       │
│       ├── reporting/                     # Reproducible engineering outputs
│       │   ├── __init__.py
│       │   ├── result_models.py            # Structured calculation and warning records
│       │   ├── summaries.py                # Human-readable calculation summaries
│       │   ├── csv_export.py               # Tabular data export
│       │   ├── json_export.py              # Machine-readable result export
│       │   ├── pdf_report.py               # Engineering calculation report generation
│       │   └── plots.py                    # System/pump curves and hydraulic profiles
│       │
│       ├── cli/                           # User interaction, separate from calculations
│       │   ├── __init__.py
│       │   ├── user_input.py               # Input prompts and validation messages
│       │   ├── display.py                  # Terminal result presentation
│       │   └── commands.py                 # CLI workflows and command dispatch
│       │
│       └── utils/                         # Shared, non-domain-specific helpers
│           ├── __init__.py
│           ├── fitting_data_helper_calcs.py # Fitting reference-data helper calculations
│           ├── units.py                    # Explicit engineering unit conversions
│           └── interpolation.py            # Shared numerical interpolation helpers
│
├── tests/                                 # Automated software and numerical tests
│   ├── conftest.py                        # Reusable test fixtures and sample assets
│   ├── models/                            # Model validation and derived-property tests
│   │   ├── test_pipe.py
│   │   ├── test_fitting.py
│   │   ├── test_valve.py
│   │   ├── test_pump.py
│   │   ├── test_wet_well.py
│   │   ├── test_force_main.py
│   │   └── test_lift_station.py
│   ├── equations/                         # Formula and numerical edge-case tests
│   │   ├── test_continuity.py
│   │   ├── test_hazen_williams.py
│   │   ├── test_darcy_weisbach.py
│   │   ├── test_minor_losses.py
│   │   ├── test_energy.py
│   │   └── test_pump_affinity.py
│   ├── calculators/                       # Single-calculation workflow tests
│   │   ├── test_friction_loss.py
│   │   ├── test_minor_loss.py
│   │   ├── test_tdh_calculator.py
│   │   ├── test_power.py
│   │   └── test_wet_well_volume.py
│   ├── analysis/                          # System behavior and scenario tests
│   │   ├── test_force_main.py
│   │   ├── test_hydraulic_profile.py
│   │   ├── test_system_curve.py
│   │   ├── test_pump_curve.py
│   │   ├── test_pump_performance.py
│   │   ├── test_pump_combination.py
│   │   ├── test_wet_well_sizing.py
│   │   └── test_pump_cycling.py
│   ├── design_checks/                     # Engineering screening and warning tests
│   │   ├── test_velocity.py
│   │   ├── test_pressure.py
│   │   ├── test_npsh.py
│   │   └── test_surge_screening.py
│   ├── integration/                       # End-to-end subsystem interactions
│   │   ├── test_mvp_1a_workflow.py
│   │   └── test_mvp_1b_workflow.py
│   └── verification/                      # Independently established benchmark cases
│       ├── test_reference_headloss.py
│       ├── test_reference_tdh.py
│       ├── test_reference_system_curve.py
│       └── test_reference_pump_operation.py
│
├── examples/                              # Reproducible engineering demonstrations
│   ├── mvp_1a_force_main/                 # Pipe losses, TDH, and system curve example
│   │   ├── inputs.json                    # Defined hydraulic input scenario
│   │   └── run_example.py                 # Run and summarize the scenario
│   └── ls_001_replacement/                # Fictional lift station replacement study
│       ├── design_basis.md                # Case study assumptions and design criteria
│       ├── inputs.json                    # Force main, pumps, wet well, and scenarios
│       ├── pump_curve.csv                 # Illustrative pump performance points
│       └── run_example.py                 # Integrated lift station evaluation
│
├── docs/                                  # Technical and project documentation
│   ├── 00_project_overview/               # Mission, scope, and development strategy
│   │   ├── 00_project_overview.md
│   │   └── 01_class_template.md
│   ├── 01_architecture/                   # Architecture and design decisions
│   │   ├── 00_project_structure.md        # This target package and repository structure
│   │   ├── 01_architecture_overview.md    # Layer boundaries and data flow
│   │   └── 02_design_decisions.md         # Major technical decisions and tradeoffs
│   ├── 02_engineering/                    # Engineering methods and design criteria
│   │   ├── 00_design_basis.md             # Conventions, assumptions, and units
│   │   ├── 01_force_main_hydraulics.md    # Pipe, boundary, and HGL/EGL methods
│   │   ├── 02_units.md                    # Units and conversions
│   │   ├── 03_criteria_and_requirements.md  # Criteria and requirements
│   │   ├── 04_pump_selection.md           # Curves, BEP, operating regions, and NPSH
│   │   ├── 05_wet_well_design.md          # Storage, cycling, and controls
│   │   ├── 06_operating_scenarios.md      # Low/high head and pump combinations
│   │   ├── 07_transient_screening.md      # Surge concerns and study triggers
│   │   └── 08_design_checks.md            # Design checks for velocity, NPSH, pump operation, and surge screening
│   ├── 03_models/                         # Model attributes, behavior, and constraints
│   │   ├── pipe.md
│   │   ├── fitting.md
│   │   ├── pump.md
│   │   ├── force_main.md
│   │   └── wet_well.md
│   ├── 04_calculations/                   # Equations, units, and worked examples
│   │   ├── friction_losses.md
│   │   ├── minor_losses.md
│   │   ├── total_dynamic_head.md
│   │   ├── system_curves.md
│   │   └── pump_operating_points.md
│   ├── 05_checklists/                    # checklists for MVP workflows and independent verification
│   │   ├── 00_high_level_roadmap.md
│   │   └── 01_v1_mvp_checklist_1A_1B.md
│   ├── 06_strategy/                      # Software development and verification strategy
│   │   ├── 00_pipe_class.md
│   │   ├── 01_pipe_materials.md
│   │   ├── 02_constants.md
│   │   ├── 03_pipe_fittings_coef.md
│   │   └── 04_fitting_class.md
│   ├── 07_user_guides/                    # Setup and example workflows
│   │   ├── installation.md
│   │   ├── mvp_1a_workflow.md
│   │   └── mvp_1b_workflow.md
│   ├── 08_validation/                     # Independent engineering verification records
│   │   ├── 00_validation_summary.md      # Benchmark inventory and pass/fail criteria
│   │   ├── 01_hand_calculations.md        # Independently derived numerical examples
│   │   ├── 02_reference_comparisons.md    # Published-reference comparisons
│   │   └── 03_industry_tool_comparison.md # Equivalent-model comparison records
│   ├── 09_references/                     # Independent engineering verification records
│   │   └── references.bib                 # Traceable engineering bibliography
│   ├── 10_audits/                         # Independent engineering verification records
│   │   └── 20261122_src_data_audit.md     # Audit of source data for units, gravity, viscosity, and traceable sources 
│   ├── 99_archive/                        # Archived or deprecated content
│   │   └── README.md 
│   └── 100_lessons_learned/               # Independent engineering verification records
│       └── 00_pipe_class_design_vs_oop_fund       
│
└── .github/
    └── workflows/
        └── tests.yml                      # Automated tests and quality checks
```

## Architectural Boundaries

| Layer | Responsibility | Should not do |
|---|---|---|
| `models/` | Represent engineering assets and validated inputs | Prompt users or generate reports |
| `data/` | Hold engineering reference values and their provenance | Silently select unsupported design assumptions |
| `equations/` | Calculate individual mathematical relationships | Depend on CLI, reporting, or workflow orchestration |
| `calculators/` | Compose equations for a focused calculation | Own interactive application state |
| `analysis/` | Evaluate connected systems and operating scenarios | Embed presentation logic |
| `design_checks/` | Flag conditions needing engineering review | Declare regulatory compliance automatically |
| `reporting/` | Format traceable outputs, assumptions, and warnings | Recompute hydraulics independently |
| `cli/` | Gather inputs, invoke workflows, present outputs | Contain hydraulic equations |
| `utils/` | Provide shared conversion and numerical helpers | Become a dumping ground for domain logic |

## MVP Alignment

**MVP 1A — Hydraulic Foundation:** Pipe/fitting/valve models, hydraulic reference data, continuity and friction equations, minor losses, TDH, force main analysis, system curves, CLI workflow, and independent verification.

**MVP 1B — Lift Station Analysis:** Pump and wet well models, pump curves, operating points, parallel pumps, cycling simulation, NPSH and other screening checks, LS-001 integrated study, and engineering reporting.

## Design Conventions

- **Python:** 3.12, with dependencies managed by `uv`.
- **Units:** Every public engineering input and result must document its units. Use explicit conversion boundaries; never silently mix US customary and SI formula constants.
- **Traceability:** Reference data must retain sources, applicable conditions, and limitations.
- **Verification:** Unit tests check code behavior; engineering verification uses independently established numerical results.
- **TDH:** Compute from clearly specified energy boundaries; do not automatically add velocity head as a separate term when already accounted for in the boundary formulation.
- **Pump curves:** Distinguish manufacturer data, interpolation assumptions, and extrapolation limits.
- **Screening checks:** Warnings are decision support, not substitutes for detailed hydraulic transient analysis, regulatory review, or professional engineering judgment.
- **Scope:** This structure is for the Version 1 command-line analysis package, not a web application, database service, or multiuser platform.

> **Maintenance rule:** Update this document when the target architecture changes materially. Track task completion in the MVP checklist, not in this document.
