# Hydraulic Analysis Suite (HAS)

![Python](https://img.shields.io/badge/Python-3.12-blue.svg)
![Package Manager](https://img.shields.io/badge/package%20manager-uv-purple.svg)
![Status](https://img.shields.io/badge/status-In%20Development-yellow.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

**Hydraulic Analysis Suite (HAS)** is an open-source Python engineering project focused on the analysis of pressurized piping systems, wastewater force mains, and lift stations.

HAS is being developed to connect fundamental hydraulic principles with practical engineering workflows. Rather than assembling isolated equations, the project aims to model how pipes, fittings, pumps, wet wells, and system operating conditions interact in a wastewater pumping system.

The project also serves as a structured opportunity to deepen technical understanding of force main and lift station design while applying sound Python software engineering practices.

> **Development status:** HAS is under active development. The capabilities described in the roadmap are development goals, not a claim that the software is complete, verified, or suitable for production design.

## Project Objectives

HAS has three complementary objectives:

1. **Engineering knowledge:** Strengthen understanding of pipe hydraulics, pump selection, system curves, wet well behavior, and wastewater lift station operation.
2. **Engineering analysis:** Develop transparent, reproducible computational tools that support the evaluation of hydraulic design alternatives and operating conditions.
3. **Software engineering:** Build a maintainable, tested Python package with reusable models, independent calculation functions, documented assumptions, and traceable verification.

The guiding principle is to **build useful engineering workflows rather than a collection of disconnected calculations**.

## Version 1 Development Strategy

Version 1 is divided into two independently defined milestones. Detailed scope, task status, and acceptance criteria are maintained in the development roadmap and MVP checklist, rather than duplicated here.

### MVP 1A: Hydraulic Foundation

Establish the verified hydraulic calculation foundation needed to evaluate pressurized pipe and force main systems.

Planned scope includes pipe and fitting properties, Hazen-Williams and Darcy-Weisbach friction loss, minor losses, total dynamic head, multi-segment force main calculations, and system head curves.

**Target outcome:** A usable, independently verified force main hydraulic analysis workflow that does not depend on pump modeling.

### MVP 1B: Lift Station Analysis

Extend the hydraulic foundation to evaluate the interaction between pumps, force mains, wet wells, and operating conditions.

Planned scope includes manufacturer pump curves, pump-system operating points, parallel pump operation, efficiency and power analysis, wet well storage and cycling, selected engineering design checks, and an integrated lift station replacement case study.

**Target outcome:** A reproducible lift station hydraulic analysis workflow with documented assumptions, limitations, and verification results.

More advanced capabilities may be considered after the Version 1 engineering foundation has been established and validated.

## Documentation and Project Navigation

The `docs/` directory is the authoritative source for detailed project information. Begin with the documents relevant to the question you are trying to answer:

| Topic | Where to look |
|---|---|
| Project purpose, scope, and design decisions | Project overview and design documentation in `docs/` |
| Current package and directory structure | [`docs/01_architecture/00_project_structure.md`](docs/01_architecture/00_project_structure.md) |
| Version 1 phases, objectives, and deliverables | Version 1 development roadmap in `docs/` |
| Implementation tasks and MVP 1A/1B release gates | MVP development checklist in `docs/` |
| Hydraulic principles, assumptions, and technical references | Engineering documentation in `docs/` |
| Calculation accuracy and independent comparisons | Verification and validation documentation in `docs/` |

The roadmap defines **what and why**; the MVP checklist tracks **what remains to be completed**; the architecture documentation describes **how the code is organized**. These documents are maintained separately so the README can remain useful as the implementation evolves.

## Engineering Approach

HAS is developed around the following principles:

- **Engineering first:** Models and workflows should reflect the underlying physical system and its boundary conditions.
- **Explicit assumptions:** Input units, equations, source data, applicability, and limitations must be documented.
- **Separation of concerns:** Engineering models, numerical calculations, analysis workflows, and presentation should remain logically distinct.
- **Traceability:** Material properties, fitting coefficients, pump data, and other engineering inputs should identify their sources where applicable.
- **Verification before reliance:** Calculations should be checked against independent hand calculations, published examples, and appropriate reference tools.
- **Incremental delivery:** Each development milestone should provide a useful and testable engineering capability.

## Installation

HAS uses **Python 3.12** and [uv](https://docs.astral.sh/uv/) for environment and dependency management.

Clone the repository and enter the project directory:

```bash
git clone <repository-url>
cd hydraulic-analysis-suite
```

Install the project dependencies into a managed virtual environment:

```bash
uv sync
```

Run the test suite, when tests are available:

```bash
uv run pytest
```

For development tooling, consult `pyproject.toml` for the configured dependency groups and commands. The application entry point and supported user workflows are documented as they become available and verified.

## Verification and Intended Use

Hydraulic calculations are not treated as black boxes. Verification activities are intended to include:

- Independent hand calculations and dimensional checks.
- Published engineering examples and recognized technical references.
- Comparison with established hydraulic modeling tools where suitable.
- Manufacturer performance data for pump-related analyses.
- Automated unit, integration, and regression tests.
- Recorded inputs, assumptions, expected results, discrepancies, and acceptance criteria.

**HAS is an engineering learning and decision-support project, not a substitute for professional engineering judgment or project-specific design review.** Its outputs must be independently evaluated before use in design, procurement, permitting, construction, or operational decisions. Applicability of local regulations, owner criteria, and relevant professional standards must be established for each project.

## Engineering References

References guiding the project may include:

- *Pumping Station Design*, Third Edition.
- Hydraulic Institute standards and technical publications.
- Relevant AWWA manuals and publications.
- Wastewater engineering manuals and utility design criteria.
- Manufacturer pump performance curves and technical literature.
- Applicable state and local regulatory requirements.

Specific equations, coefficients, design criteria, and worked examples should cite the applicable source in the engineering documentation rather than relying on this overview.

## License

MIT License. See the repository license file for terms.

## Author

**Brice Nelson, P.E., MBA**  
Civil Engineer | Infrastructure Analytics | Python Development

Focused on the intersection of municipal infrastructure engineering, hydraulic analysis, data analytics, and engineering software development.
