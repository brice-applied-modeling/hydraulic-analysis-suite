# Hydraulic Analysis Suite — Architecture Overview

**Status:** Target architecture / Phase 0 specification  
**Scope:** Hydraulic Analysis Suite (HAS), Python 3.12

## 1. Purpose

HAS is a Python-based engineering analysis application for force-main and lift-station calculations, verification, and scenario comparison. Its first reproducible demonstration is the wholly illustrative **LS-001** case. This document describes intended component boundaries and data flow; it does **not** claim that every component has been implemented.

## 2. Architectural principles

1. Keep engineering equations separate from user interfaces, files, and reports.
2. Maintain a single documented internal calculation convention: consistent U.S. customary **ft–s–lbf** units, with mass in slugs where needed.
3. Convert common user inputs (gpm, inches, psi, hp) at explicit boundaries; never infer units from a bare number without a documented API contract.
4. Preserve the provenance and applicability of constants, pipe dimensions, fitting coefficients, and assumptions.
5. Separate calculation failures from engineering design warnings.
6. Make calculations deterministic and independently verifiable.
7. Keep demonstration data fictional and separate from employer/client information.

## 3. Logical layers

| Layer | Responsibility | Must not do |
|---|---|---|
| Interfaces | CLI and, eventually, other interfaces; receive input and present results | Implement hydraulic equations |
| Application / orchestration | Assemble scenarios, call calculations, coordinate checks | Hide units or silently substitute engineering assumptions |
| Domain models | Represent pipes, fittings, pumps, wet wells, operating conditions | Read files or format terminal output as a side effect |
| Engineering calculations | Flow, velocity, head loss, system curves, pump operating points, and later wet-well analyses | Depend on CLI, filesystem, or reporting code |
| Reference data | Constants, material properties, fitting coefficients, traceability | Treat approximate reference values as universally applicable |
| Validation and design checks | Validate inputs and flag criterion exceedances | Conflate warnings with calculation errors |
| Results and reporting | Structured numerical results, units, warnings, provenance, exports | Recompute hydraulics independently of the calculation layer |
| Verification | Unit, benchmark, regression, and independent case checks | Treat HAS output as its own independent ground truth |

These are **logical boundaries**, not a mandate to create one Python package for every row immediately.

## 4. Intended data flow

```text
User input / illustrative scenario file
                 |
                 v
        Parse and validate
                 |
                 v
  Convert to internal ft–s–lbf units
                 |
                 v
   Domain objects + reference data
                 |
                 v
      Hydraulic calculations
                 |
                 v
     Engineering design checks
                 |
                 v
Structured result (values, units, assumptions,
  warnings, calculation status, provenance)
                 |
                 v
    CLI output / reports / case comparison
```

Invalid inputs stop before calculation and raise descriptive errors. Valid but potentially unacceptable engineering results are returned with warnings and applicable criteria. A successful calculation is **not** an approval of the design.

## 5. Unit boundaries

Internal calculations use compatible U.S. customary units:

| Quantity | Internal unit | Common external unit |
|---|---|---|
| Length, elevation, head | ft | ft |
| Time | s | s, min, hr |
| Flow | ft³/s | gpm, MGD |
| Diameter | ft | in |
| Area | ft² | ft² |
| Velocity | ft/s | fps |
| Acceleration | ft/s² | ft/s² |
| Force | lbf | lbf |
| Pressure | lbf/ft² (psf) | psi |
| Specific weight | lbf/ft³ | pcf |
| Mass density | slug/ft³ | lbm/ft³ |
| Power | ft·lbf/s | hp |

Pressure head is `h_p = p / gamma` when pressure `p` is in lbf/ft² and fluid specific weight `gamma` is in lbf/ft³. Hydraulic grade elevation is `HGL = z + h_p`, with a consistent elevation datum. Distinguish gauge from absolute pressure whenever pressure is used. Do not assume a universal psi-to-feet factor for all fluids.

## 6. Core domain concepts

- **Pipe:** actual internal diameter, length, material, roughness assumptions, and applicable pressure class.
- **Fitting:** type, size, coefficient/source, and applicable minor-loss method.
- **Force-main segment/system:** pipes, fittings, boundary conditions, and flow-dependent head losses.
- **Pump:** manufacturer curve and operating constraints; future capability, not yet implemented.
- **Wet well:** levels, effective storage, and operating controls; future capability.
- **Operating scenario:** flow, pump status, boundary levels, and other explicitly specified conditions.
- **Calculation result:** values with units, assumptions, warnings, and traceability.

A model's nominal diameter is not automatically its hydraulic inside diameter. Material and dimensional-standard lookups must make that distinction explicit.

## 7. Results and failure behavior

A consistent result contract should capture at least:

```python
{
    "case_id": "LS-001",
    "calculation": "force_main_hydraulics",
    "status": "success",
    "values": {
        "flow": {"value": 0.0, "unit": "ft^3/s"},
        "velocity": {"value": 0.0, "unit": "ft/s"},
    },
    "warnings": [],
    "assumptions": [],
    "references": [],
}
```

**Illustrative contract only**, not an assertion that the current classes return this dictionary. Invalid geometry, unsupported inputs, or missing essential coefficients should raise descriptive exceptions. Criterion exceedances should normally generate warnings while preserving the computed result. Numerical failures should be distinguishable from input validation failures.

## 8. Verification strategy

- Unit tests: individual equations, unit conversions, and edge cases.
- Component tests: pipes, fittings, and their interactions.
- Independent benchmark calculations: hand calculations, documented spreadsheets, or validated external tools.
- Scenario/regression tests: stable, version-controlled LS-001 inputs and independently established expected values.
- Explicit tolerances: justified by method and quantity, not one global tolerance for every output.

Transient/surge behavior, NPSH, and pump-curve extrapolation require explicit limitations and, where appropriate, specialized analysis rather than implied certification by a steady-state tool.

## 9. Repository organization and dependencies

Maintain the existing `src/` layout and repository structure documented in `docs/01_architecture/00_project_structure.md`. Do not rename or move existing modules solely to match this conceptual diagram. Dependencies should generally flow from interfaces/application code toward domain calculations and reference data; the engineering core should not import CLI or reporting modules.

- `docs/01_architecture/02_design_decisions.md` records major choices and alternatives.
- `docs/01_architecture/03_numerical_conventions.md` is authoritative for detailed unit and numerical rules.
- `docs/02_engineering/09_verification_plan.md` governs verification procedures.
- `docs/02_engineering/10_ls001_design_basis.md` governs the fictional LS-001 assumptions.

If these documents conflict, reconcile them before implementing new functionality.

## 10. Implementation progression

1. Audit existing `Pipe`, `Fitting`, and reference-data code against the documented conventions.
2. Implement/test conversion and validation boundaries without rewriting sound hydraulic logic unnecessarily.
3. Standardize result contracts and warning/error categories.
4. Establish independently checked pipe/fitting and LS-001 benchmarks.
5. Extend to system curves, pump models, wet-well behavior, and scenario comparisons incrementally.

**Boundary:** HAS is an engineering analysis aid, not a substitute for professional judgment, owner requirements, manufacturer information, or independent design review.
