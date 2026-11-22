# LS-001 — Illustrative Municipal Lift Station Design Basis

**Project:** Hydraulic Analysis Suite (HAS)  
**Case identifier:** LS-001  
**Status:** Phase 0 hypothetical case definition  
**Classification:** Public, synthetic educational/verification example; not a construction design

## 1. Purpose

Provide a repeatable, fully fictional municipal wastewater lift station replacement scenario for developing and independently verifying force-main hydraulics, pump/system curves, wet well behavior, operating scenarios, and design checks in HAS.

All case information is invented for this software exercise. It must not be represented as a real municipal facility or an employer/client project.

## 2. Hypothetical system description

A duplex submersible wastewater lift station conveys flow from a wet well through a common force main to a receiving sewer. The initial model assumes incompressible, single-phase, steady-state flow and a prescribed downstream hydraulic grade. The operating scenario and pump selection will be developed after the basic force-main calculation is verified.

## 3. Preliminary illustrative inputs

| Parameter | Illustrative value | Notes |
|---|---:|---|
| Average daily inflow | 150,000 US gpd | Background planning assumption; not the design pumping rate |
| Preliminary peak inflow | 450 US gpm | Illustrative scenario flow; not a verified peak-factor derivation |
| Initial hydraulic evaluation flow | 450 US gpm | One point on the proposed system curve |
| Nominal force-main size | 6 in | Actual inside diameter depends on selected pipe standard/DR |
| Force-main length | 2,500 ft | Hydraulic centerline length, illustrative |
| Force-main material | PVC | Specific pipe standard, pressure class, and roughness TBD |
| Wet well reference water surface | Elev. 10.0 ft | Synthetic datum; reference operating condition only |
| Receiving-system HGL | Elev. 65.0 ft | Same synthetic datum; assumed constant for initial model |
| Preliminary static head | 55.0 ft | 65.0 - 10.0 ft, at the stated reference condition |
| Pump arrangement | Duplex submersible | Lead/lag and duty/standby philosophy TBD |
| Fluid | Municipal wastewater | Use preliminary water-like properties for early calculations, with limitations stated |

**Important:** The 6-inch nominal size is not the hydraulic inside diameter. The pump duty point, pump curves, minimum/maximum wet well elevations, fitting inventory, and design criteria have not yet been selected.

## 4. Hydraulic boundaries and datum

- **Upstream boundary:** Wet well liquid surface, at an explicitly identified operating elevation. For an initial simplified model, take surface velocity and gauge pressure as approximately zero.
- **Downstream boundary:** Prescribed receiving-system hydraulic grade line (HGL) of Elev. 65.0 ft in the same synthetic vertical datum.
- **Vertical datum:** `LS-001 synthetic datum`; not NAVD88 or another survey datum.
- **Flow direction:** Wet well to receiving sewer.
- **Static head at reference level:** 55.0 ft. Recalculate as wet well level varies.
- **System head:** Static head plus force-main friction and minor losses, with additional terms included where warranted by the chosen boundary definitions.

For the initial simplified model:

\[
H_{system}(Q,z_{ww}) = (HGL_{downstream}-z_{ww}) + h_f(Q) + h_m(Q)
\]

This relationship assumes the chosen hydraulic boundaries properly account for velocity and pressure terms. Document any revisions before using the equation for detailed pump selection.

## 5. Initial calculation sequence

1. Select a documented PVC pipe standard and calculate the actual inside diameter.
2. Establish fluid density, viscosity, temperature, and roughness assumptions with sources.
3. Calculate velocity and Reynolds number over a range of flows.
4. Calculate Darcy–Weisbach friction losses using a documented friction-factor method.
5. Define the fitting/valve inventory and calculate minor losses.
6. Produce a system-head curve for at least the reference wet well elevation.
7. Independently verify representative points using hand calculations or a separately implemented spreadsheet.
8. Add candidate pump curves and solve operating points only after curves and operating philosophy are defined.
9. Evaluate wet well cycling and lead/lag scenarios after pump controls and storage geometry are defined.
10. Screen velocity, pump operating range, NPSH, and transient risk using documented, case-specific criteria.

## 6. Assumptions and limitations

- The scenario is fictional and is not a substitute for a sealed engineering design.
- No actual client, utility, station, drawing, survey, operating data, or employer-owned model is used.
- Steady-state hydraulics do not predict water hammer, pump-start transients, air binding, or all wastewater solids behavior.
- Downstream HGL is initially fixed for simplicity; later scenarios may vary it.
- Wet well level varies in real operation; Elev. 10.0 ft is one reference condition.
- No pump capacity, efficiency, horsepower, NPSH margin, or wet well storage adequacy is established by the initial inputs alone.
- A design-flow criterion, redundancy philosophy, and governing owner/regulatory requirements must be selected before treating the case as a complete engineering design exercise.

## 7. Data separation and provenance

LS-001 is a synthetic example. All inputs and results intended for public release must be traceable to:

- Explicit fictional assumptions created for HAS.
- Publicly available standards, technical references, or manufacturer information used in accordance with applicable terms.
- Independently calculated reference results.

Do not copy or adapt confidential project plans, specifications, hydraulic models, operating records, client names, or identifiable facility layouts into the public case. If a real-world project inspires a question, formulate a fresh, generic scenario and document it as synthetic; merely renaming a client or changing a few numbers is insufficient.

## 8. Proposed case files

```text
examples/
└── ls_001/
    ├── README.md
    ├── design_inputs.yaml
    ├── operating_scenarios.yaml
    └── expected_results.yaml
```

`expected_results.yaml` must contain independently verified reference values and tolerances, not simply HAS-generated outputs. The YAML files are future implementation deliverables, not required to approve the Phase 0 design basis.

## 9. Verification plan

| Verification case | Expected independent check |
|---|---|
| Unit conversion | Hand-check US gpm to ft³/s and inches to feet; verify pressure/head conversions with case-specific specific weight |
| Zero flow | Velocity and friction/minor losses equal zero under applicable assumptions |
| Reference flow | Independent velocity, Reynolds number, friction, and minor-loss calculation |
| Static head | Confirm 65.0 - 10.0 = 55.0 ft |
| Variable wet well level | Verify changes to static head and system curve |
| Invalid inputs | Confirm meaningful exceptions rather than fabricated results |
| Pump curve intersection | Independent interpolation/root-solve after pump data is selected |
| Design warnings | Compare each check with its documented criterion |

Set numerical tolerances for each check before accepting software output. External models can provide additional comparisons, but differences in equations, input definitions, and solver settings must be documented.

## 10. Outstanding decisions

- [ ] Select the 6-inch PVC pipe standard, DR/pressure class, and actual inside diameter.
- [ ] Select the friction-factor method and roughness source.
- [ ] Define fittings, valves, and associated loss coefficients.
- [ ] Define minimum, normal, and maximum wet well operating levels.
- [ ] Define pump duty/standby or lead/lag philosophy and controls.
- [ ] Select candidate pump curves and efficiency information.
- [ ] Define the downstream HGL range for operating scenarios.
- [ ] Define wet well geometry and inflow variability for cycling analysis.
- [ ] Document applicable owner criteria and engineering reference sources.
- [ ] Approve independent reference calculations and tolerances.

## 11. Phase 0 acceptance criteria

- [x] Case identifier and fictional purpose established.
- [x] Preliminary flow, piping, and boundary assumptions documented.
- [x] Synthetic elevation datum identified.
- [x] Initial calculation objectives and verification approach defined.
- [x] Confidentiality and provenance restrictions documented.
- [ ] Engineering assumptions and outstanding selections reviewed and approved.

**Phase 0 completion:** Approve this hypothetical case definition, including its explicit unknowns. Do not mistake preliminary assumptions for a completed lift station design.
