# Numerical Conventions, Results, and Error Handling

**Project:** Hydraulic Analysis Suite (HAS)  
**Status:** Phase 0 design specification — revised for U.S. customary calculation units  
**Scope:** All hydraulic and lift station calculation modules

## 1. Purpose

Define consistent internal units, conversion boundaries, numerical precision, result structures, and error/warning behavior. This is a **target specification**, not a statement that the current `Pipe` or `Fitting` classes already comply.

## 2. Unit policy

**Decision:** Use a consistent U.S. customary **ft–s–lbf** calculation basis. Convert convenient engineering inputs (gpm, inches, psi, hp) into the calculation units at controlled boundaries. SI is supported through explicit conversion utilities, not by mixing SI and U.S. customary quantities inside equations.

| Quantity | Internal calculation unit | Typical input/output |
|---|---|---|
| Length, elevation, hydraulic head | ft | ft |
| Pipe inside diameter | ft | in |
| Flow rate | ft³/s | US gpm, MGD |
| Velocity | ft/s | ft/s |
| Pressure | lbf/ft² (psf) | psi (gauge or absolute, labeled) |
| Specific weight, γ | lbf/ft³ | lbf/ft³ |
| Mass density, ρ | slug/ft³ | lbm/ft³, kg/m³ |
| Dynamic viscosity, μ | slug/(ft·s) | cP |
| Kinematic viscosity, ν | ft²/s | ft²/s |
| Acceleration, g | ft/s² | ft/s² |
| Force | lbf | lbf |
| Power | ft·lbf/s | mechanical hp |
| Time | s | s, min, hr |

**Important:** `lbm` (mass), `lbf` (force), and `slug` (mass) are different units. Use `slug/ft³` when the equation requires mass density in the coherent ft–s–lbf system. Do not substitute a numerical value in `lbm/ft³` into a formula requiring `slug/ft³` without conversion. Specific weight is γ = ρg when ρ is in slug/ft³.

### Exact or defined conversions

- 1 ft = 12 in (exact).
- 1 ft³ = 7.4805194805 US gal (approximate); 1 US gal = 231 in³ (exact).
- `Q_ft3_s = Q_gpm × 231 / (1728 × 60)`.
- `D_ft = D_in / 12`.
- `P_psf = P_psi × 144`.
- 1 psi = 6,894.757293168 Pa (approximately).
- 1 ft = 0.3048 m (exact).
- 1 mechanical hp = 550 ft·lbf/s (exact by definition).
- 1 slug ≈ 32.17405 lbm at the standard conversion convention; document the adopted conversion constant centrally.

Store constants and conversion factors centrally with source/precision notes. Avoid embedding unexplained conversion numbers throughout the hydraulic classes. Label **US** gallons explicitly when ambiguity is possible.

## 3. Pressure, head, and hydraulic grade

Pressure and pressure head are not interchangeable with hydraulic grade elevation.

- **Pressure:** force per area, expressed internally in psf and commonly displayed in psi.
- **Pressure head:** `h_p = P_gauge / γ` in ft, using consistent psf and lbf/ft³ units.
- **Hydraulic grade elevation (HGL):** `HGL = z + h_p`, in ft relative to the stated vertical datum.
- **Total head (simplified):** `H = z + h_p + V²/(2g)` in ft, with appropriate assumptions and any required correction factors.

For a reference fluid with γ ≈ 62.4 lbf/ft³:

`1 psi ≈ 144 / 62.4 = 2.3077 ft of pressure head`.

This **2.31 ft/psi rule is an approximation**, not a universal constant. Use case-specific specific weight when fluid properties differ. State whether a pressure is **gauge** or **absolute**; do not mix them. Absolute pressure is necessary for some vapor-pressure/NPSH calculations.

**Example:** At elevation 20 ft, a gauge pressure of 30 psi and γ = 62.4 lbf/ft³ yield pressure head `30 × 144 / 62.4 ≈ 69.23 ft` and HGL `≈ 89.23 ft` relative to the same datum.

## 4. Numerical policies

- Use Python `float` (binary64) for intermediate calculations unless a method requires otherwise.
- Do not round intermediate results; round only for display.
- Require finite numeric inputs and outputs; reject NaN and infinity.
- Set comparison tolerances for each independently verified case, not one universal tolerance.
- Document absolute/relative convergence tolerances, iteration limits, and nonconvergence behavior for iterative methods.
- Zero flow is permitted where physically and mathematically appropriate; friction and minor losses are zero at zero flow, while static head may remain nonzero.
- The initial force-main API assumes a specified forward-flow direction; signed reverse flow is a future capability.
- Keep elevations and head differences in feet, with one stated vertical datum per case.

## 5. Result schema

Use a predictable, versioned result envelope. Dataclasses or Pydantic can be chosen during implementation. The example below shows **illustrative schema shape, not calculated LS-001 results**.

```yaml
schema_version: "1.0"
case_id: "LS-001"
calculation: "force_main_hydraulics"
status: "ok"  # ok | warning | failed
inputs:
  flow_ft3_s: 0.0
outputs:
  velocity_ft_s: 0.0
  friction_head_ft: 0.0
  minor_head_ft: 0.0
  total_head_loss_ft: 0.0
warnings: []
method:
  friction_model: "darcy_weisbach"
  assumptions: []
```

Every public result must make units explicit, either in field names or validated unit metadata. Include calculation identifier, schema version, and method assumptions. Add software version, data provenance, and verification reference when relevant. Do not require nondeterministic timestamps in reproducible tests.

## 6. Errors versus engineering warnings

**Errors: calculations cannot proceed reliably.** Raise descriptive exceptions for nonpositive pipe inside diameter, negative pipe length, invalid physical properties, unsupported required material/fitting data, missing mandatory boundaries, nonfinite numbers, or solver nonconvergence. Begin with `ValueError`/`TypeError` as appropriate; introduce domain-specific exceptions (`HydraulicInputError`, `MissingEngineeringDataError`, `SolverConvergenceError`) when useful.

**Warnings: a valid numerical result may raise an engineering concern.** Return structured warnings for velocity outside case-specific criteria, operation beyond manufacturer-supported pump-curve bounds (and do not extrapolate silently), unverified roughness or fitting coefficients, insufficient NPSH margin, and transient screening concerns. Cite the criterion or assumption behind each warning; do not imply that one owner's criterion is universally applicable.

Result status: `ok` = completed without warnings; `warning` = completed with warnings; `failed` = exception captured in a batch/report envelope. Direct Python calculation APIs should ordinarily raise rather than quietly return a failed value. Never silently substitute zero for missing head-loss data.

## 7. Hydraulic sign and boundary conventions

- Initial force-main positive flow direction: wet well/pump discharge toward receiving system.
- Friction and minor losses are nonnegative for the defined flow magnitude.
- Static head = downstream hydraulic grade minus upstream hydraulic grade for the specified boundaries; it can be negative.
- Distinguish **static head**, **pressure head**, **velocity head**, **pump head**, and **total dynamic head**.
- State wet well levels, discharge HGL, pressure reference (gauge/absolute), and elevation datum explicitly.
- Use actual **inside** diameter derived from the selected pipe standard/DR; nominal diameter is not a hydraulic diameter.

## 8. Verification and regression tests

For each calculation method:

1. Record governing equation, units, assumptions, and applicable range.
2. Independently compute reference cases by hand or spreadsheet; do not use HAS output as its own expected result.
3. Test gpm↔ft³/s, in↔ft, psi↔psf, psi↔ft of head (for specified γ), and SI↔U.S. customary round trips.
4. Verify 30 psi at γ = 62.4 lbf/ft³ produces approximately 69.23 ft pressure head.
5. Test normal, zero-flow, invalid-input, and boundary conditions.
6. Establish per-case tolerances and document differences from external models where used.

## 9. Phase 0 acceptance criteria

- [ ] Approve ft–s–lbf internal calculation basis and boundary conversions.
- [ ] Approve pressure/pressure-head/HGL distinctions and fluid-property conventions.
- [ ] Approve result schema, exception/warning policies, and numerical precision rules.
- [ ] Identify existing `Pipe`/`Fitting` code and constants requiring alignment.
- [ ] Define independent reference cases and acceptance tolerances.

**Phase 0 completion:** Approval of the specification. Code refactoring, conversion utilities, and automated conformance tests belong to later implementation phases.
