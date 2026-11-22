# Engineering Design Basis

## 1. Purpose

This document establishes the engineering design basis for the **Hydraulic Analysis Suite (HAS)**.

The design basis defines the fundamental engineering assumptions, intended applications, calculation boundaries, fluid properties, hydraulic principles, and limitations governing development of the software.

The objectives are to:

- Establish a consistent engineering foundation.
- Define the intended use and limitations of HAS.
- Identify governing hydraulic principles.
- Establish assumptions for fluid properties and flow conditions.
- Define hydraulic boundary conditions.
- Establish unit conventions.
- Identify applicable engineering references.
- Support independent verification.
- Provide a technical foundation for future software development.

This document establishes engineering assumptions and calculation principles. Detailed calculation methods, regulatory criteria, and verification procedures are maintained in supporting documents.

---

## 2. Project Scope and Objectives

### 2.1 Project Description

HAS is a modular, Python-based engineering analysis suite intended to support hydraulic calculations and preliminary engineering evaluations associated with municipal water and wastewater infrastructure.

The initial development emphasis is on:

1. Pressurized pipe hydraulics.
2. Pipe and fitting head-loss calculations.
3. Force-main hydraulic analysis.
4. Pump performance and operating point analysis.
5. Wet-well operating calculations.
6. Integrated pumping system evaluation.

These represent the intended development scope and do not imply that all capabilities are currently implemented.

### 2.2 Primary Objectives

The primary objectives of HAS are to:

- Strengthen understanding of hydraulic engineering principles.
- Develop reusable engineering calculation components.
- Apply professional Python software development practices.
- Establish traceable engineering data and calculation methods.
- Develop independently verified hydraulic calculations.
- Support engineering alternatives analysis.
- Establish a foundation for future infrastructure decision analytics.

### 2.3 Long-Term Objectives

Potential future capabilities include:

- Pump selection and comparison.
- Multiple-pump operating scenarios.
- Variable-speed pumping analysis.
- Pump energy consumption.
- Lifecycle cost analysis.
- Capital improvement alternative evaluation.
- Sensitivity analysis.
- Monte Carlo simulation.
- Integration with external hydraulic models.

These capabilities are potential future development objectives rather than existing software functionality.

---

## 3. Intended Applications

### 3.1 Primary Applications

HAS is intended to support:

- Preliminary force-main sizing.
- Pipe velocity calculations.
- Reynolds number calculations.
- Pipe friction-loss calculations.
- Fitting and valve minor-loss calculations.
- Total dynamic head calculations.
- System head curve generation.
- Pump performance evaluation.
- Pumping system alternative comparisons.
- Engineering calculation documentation.

### 3.2 Future Applications

Future applications may include:

- Wet-well sizing and operating cycle analysis.
- Parallel pump analysis.
- Variable-speed pump analysis.
- Energy consumption estimation.
- Infrastructure rehabilitation alternatives.
- Capital cost comparisons.
- Uncertainty-based engineering decision support.

### 3.3 Excluded Applications

Unless separately developed and verified, HAS is not intended to provide:

- Complete water distribution network modeling.
- Complete gravity sewer network modeling.
- Hydraulic transient or water-hammer analysis.
- Computational fluid dynamics.
- Two-phase flow modeling.
- Detailed air-pocket movement analysis.
- Structural design.
- Electrical design.
- Automatic regulatory compliance certification.
- Manufacturer-certified pump selection.

These applications may require specialized software and additional engineering evaluation.

---

## 4. Engineering Design Principles

### 4.1 Engineering Accuracy

All calculations should be based on recognized hydraulic engineering principles and documented equations.

### 4.2 Dimensional Consistency

All calculations must use consistent units.

Conversions should be explicit and centrally maintained.

### 4.3 Modularity

Individual hydraulic components should be developed independently and integrated through clearly defined interfaces.

### 4.4 Traceability

Engineering constants, coefficients, and reference data should have identifiable sources and documented limitations.

### 4.5 Verification

Calculations should be independently verified against analytical solutions, recognized references, or established hydraulic software.

### 4.6 Reproducibility

Engineering analyses should be reproducible using documented inputs, assumptions, software versions, and calculation methods.

### 4.7 Engineering Judgment

HAS should support engineering judgment rather than replace it.

Results must be evaluated within the context of the applicable design requirements and system conditions.

---

## 5. Unit System

### 5.1 Primary Engineering Units

HAS initially uses U.S. customary engineering units.

| Quantity | Engineering Unit |
|---|---|
| Pipe length | ft |
| Pipe diameter | in |
| Flow rate | gpm |
| Velocity | ft/s |
| Pressure | psi |
| Hydraulic head | ft |
| Elevation | ft |
| Pump power | hp |
| Electrical power | kW |
| Volume | gal |
| Temperature | °F |

### 5.2 Internal Calculation Units

The proposed internal calculation conventions are:

| Quantity | Internal Unit |
|---|---|
| Length | ft |
| Diameter | ft |
| Flow | ft³/s |
| Area | ft² |
| Velocity | ft/s |
| Head | ft |
| Pressure | lbf/ft² |
| Time | s |
| Kinematic viscosity | ft²/s |

These conventions should be reconciled with the existing HAS implementation.

### 5.3 Unit Documentation

All calculation functions should clearly identify their input and output units.

Detailed conversions are maintained in the Units and Unit Conventions document.

---

## 6. Fluid Properties

### 6.1 General Assumptions

The initial HAS methodology assumes a single-phase, incompressible, Newtonian liquid.

For preliminary municipal water and wastewater hydraulic calculations, water properties may be used where the approximation is appropriate.

Fluid properties must be consistent with the selected temperature and fluid composition.

### 6.2 Fluid Density

Fluid mass density is represented by:

$$
\rho = \frac{m}{V}
$$

Where:

- $\rho$ = mass density.
- $m$ = mass.
- $V$ = volume.

For water near 20°C:

$$
\rho \approx 998.2 \ \mathrm{kg/m^3}
$$

In U.S. customary mass units:

$$
\rho \approx 1.937 \ \mathrm{slug/ft^3}
$$

These are representative values, not universal constants.

### 6.3 Specific Weight

Specific weight is:

$$
\gamma = \rho g
$$

Where:

- $\gamma$ = specific weight (lbf/ft³).
- $\rho$ = mass density (slug/ft³).
- $g$ = gravitational acceleration (ft/s²).

For water near typical ambient temperatures:

$$
\gamma \approx 62.3 \ \mathrm{lbf/ft^3}
$$

The commonly used value of 62.4 lbf/ft³ is a rounded engineering approximation.

### 6.4 Dynamic Viscosity

Dynamic viscosity is represented by:

$$
\mu
$$

Typical units include:

- Pa·s.
- cP.
- lbf·s/ft².

For water near 20°C:

$$
\mu \approx 1.002 \times 10^{-3} \ \mathrm{Pa \cdot s}
$$

### 6.5 Kinematic Viscosity

Kinematic viscosity is:

$$
\nu = \frac{\mu}{\rho}
$$

Where:

- $\nu$ = kinematic viscosity.
- $\mu$ = dynamic viscosity.
- $\rho$ = mass density.

For water near 20°C:

$$
\nu \approx 1.004 \times 10^{-6} \ \mathrm{m^2/s}
$$

Equivalent to approximately:

$$
\nu \approx 1.08 \times 10^{-5} \ \mathrm{ft^2/s}
$$

### 6.6 Temperature Dependence

Water viscosity and density vary with temperature.

HAS should not treat fluid properties as universal constants independent of operating conditions.

For the initial implementation, a documented reference temperature may be used for individual verification cases.

Future functionality may include temperature-dependent fluid properties.

### 6.7 Wastewater Characteristics

Municipal wastewater may contain:

- Suspended solids.
- Dissolved substances.
- Grease.
- Entrained gases.
- Other constituents.

The initial HAS methodology assumes that the liquid phase can be reasonably approximated using Newtonian water properties.

Applications involving significant non-Newtonian behavior, high solids concentrations, or multiphase conditions require separate evaluation.

---

## 7. Governing Hydraulic Principles

### 7.1 Conservation of Mass

For steady incompressible flow:

$$
Q = AV
$$

Where:

- $Q$ = flow rate (ft³/s).
- $A$ = cross-sectional area (ft²).
- $V$ = average velocity (ft/s).

For a circular pipe:

$$
A = \frac{\pi D^2}{4}
$$

### 7.2 Reynolds Number

The Reynolds number is:

$$
Re = \frac{VD}{\nu}
$$

Where:

- $Re$ = dimensionless Reynolds number.
- $V$ = velocity (ft/s).
- $D$ = internal pipe diameter (ft).
- $\nu$ = kinematic viscosity (ft²/s).

The Reynolds number is used to characterize flow regime.

Typical circular-pipe flow classifications are:

| Flow Regime | Approximate Reynolds Number |
|---|---|
| Laminar | $Re < 2300$ |
| Transitional | $2300 \leq Re \leq 4000$ |
| Turbulent | $Re > 4000$ |

These boundaries are approximate and should not be interpreted as universal thresholds for every flow configuration.

### 7.3 Conservation of Energy

The general steady-flow energy equation is:

$$
z_1 + \frac{P_1}{\gamma}
+ \alpha_1\frac{V_1^2}{2g}
+ H_p
=
z_2 + \frac{P_2}{\gamma}
+ \alpha_2\frac{V_2^2}{2g}
+ h_L
$$

Where:

- $z$ = elevation head (ft).
- $P/\gamma$ = pressure head (ft).
- $\alpha V^2/(2g)$ = kinetic energy head (ft).
- $H_p$ = pump head added (ft).
- $h_L$ = hydraulic losses (ft).

This equation forms the basis for force-main system head calculations.

---

## 8. Pipe Friction Losses

### 8.1 Darcy-Weisbach Equation

The primary friction-loss methodology is:

$$
h_f = f\frac{L}{D}\frac{V^2}{2g}
$$

Where:

- $h_f$ = friction head loss (ft).
- $f$ = Darcy friction factor.
- $L$ = pipe length (ft).
- $D$ = actual internal diameter (ft).
- $V$ = velocity (ft/s).
- $g$ = gravitational acceleration (ft/s²).

### 8.2 Friction Factor

The Darcy friction factor depends on:

- Reynolds number.
- Relative roughness.
- Flow regime.

For laminar flow in a circular pipe:

$$
f = \frac{64}{Re}
$$

For turbulent flow, appropriate correlations or iterative methods should be used.

### 8.3 Flow-Dependent Head Loss

Head loss is not universally proportional to the square of flow.

For Darcy-Weisbach:

$$
h_f \propto f(Q)Q^2
$$

Because the friction factor may vary with flow, the relationship is not necessarily exactly quadratic.

### 8.4 Hazen-Williams Method

The Hazen-Williams equation may be supported as an alternative empirical method where applicable.

Its use should be limited to appropriate fluids, temperatures, pipe conditions, and design applications.

The selected method and coefficient values must be documented.

---

## 9. Minor Hydraulic Losses

### 9.1 General

Minor losses result from hydraulic disturbances associated with:

- Elbows.
- Tees.
- Wyes.
- Valves.
- Reducers.
- Expanders.
- Entrances.
- Exits.
- Other appurtenances.

Minor losses may be significant in pump station discharge piping and should not automatically be neglected.

### 9.2 Loss Coefficient Method

Minor losses may be calculated using:

$$
h_m = K\frac{V^2}{2g}
$$

Where:

- $h_m$ = minor head loss (ft).
- $K$ = dimensionless loss coefficient.
- $V$ = applicable velocity (ft/s).

For multiple components:

$$
h_{m,total} = \sum_{i=1}^{n}K_i\frac{V_i^2}{2g}
$$

### 9.3 Coefficient Selection

Loss coefficients should be obtained from:

- Recognized engineering references.
- Manufacturer technical information.
- Documented experimental data.

The applicability of each coefficient must be evaluated.

### 9.4 Total Hydraulic Losses

$$
h_L = h_f + h_m
$$

For multiple pipe segments and fittings:

$$
h_L =
\sum_{i=1}^{n}h_{f,i}
+
\sum_{j=1}^{m}h_{m,j}
$$

---

## 10. Hydraulic Boundary Conditions

### 10.1 Upstream Boundary

For a typical open wet well:

$$
HGL_{upstream} = WSEL_{wetwell}
$$

The wet-well water surface establishes the upstream hydraulic boundary.

### 10.2 Downstream Boundary

The downstream boundary may be:

- Receiving water surface elevation.
- Defined downstream HGL.
- Pressure at a specified discharge elevation.

For a pressurized downstream system:

$$
HGL_{downstream}
=
z_d+\frac{144P_d}{\gamma}
$$

Where $P_d$ is expressed in psi.

### 10.3 Elevation Datum

All elevations must use a consistent vertical datum.

Potential datums include:

- NAVD88.
- NGVD29.
- Project-specific local datum.

### 10.4 Variable Boundary Conditions

Relevant scenarios may include:

- Minimum wet-well level.
- Maximum wet-well level.
- Minimum downstream HGL.
- Maximum downstream HGL.
- Existing conditions.
- Future conditions.

---

## 11. Total Dynamic Head

### 11.1 Simplified Equation

For appropriate reservoir-to-reservoir boundary conditions:

$$
TDH = H_{static}+h_L
$$

Where:

$$
H_{static}
=
HGL_{downstream}-WSEL_{wetwell}
$$

Therefore:

$$
TDH =
HGL_{downstream}-WSEL_{wetwell}+h_L
$$

### 11.2 General Equation

Where velocity-head differences are significant:

$$
TDH =
\left(
z_2+\frac{P_2}{\gamma}
+\alpha_2\frac{V_2^2}{2g}
\right)
-
\left(
z_1+\frac{P_1}{\gamma}
+\alpha_1\frac{V_1^2}{2g}
\right)
+h_L
$$

### 11.3 System Head Curve

The system head curve is:

$$
H_{system}(Q)=H_{static}+h_L(Q)
$$

The applicable curve depends on the selected hydraulic boundary conditions.

---

## 12. Pumping System Assumptions

### 12.1 Pump Performance

Pump performance should be represented using manufacturer data where available.

Relevant characteristics include:

- Head-capacity curve.
- Efficiency curve.
- Power curve.
- NPSH requirements.
- Operating limits.

### 12.2 Pump Operating Point

The pump operating point occurs where:

$$
H_{pump}(Q)=H_{system}(Q)
$$

### 12.3 Multiple-Pump Operation

For pumps operating in parallel, the combined pump curve must be evaluated against the system curve.

### 12.4 Pump Efficiency

Pump brake horsepower may be estimated as:

$$
BHP=\frac{QHSG}{3960\eta_p}
$$

Where:

- $Q$ = flow (gpm).
- $H$ = pump head (ft).
- $SG$ = specific gravity.
- $\eta_p$ = pump efficiency expressed as a decimal.

Detailed pump selection requirements are addressed in the Pump Selection document.

---

## 13. Steady-State Flow Assumptions

### 13.1 Definition

Steady-state flow assumes that hydraulic conditions at a given location do not vary with time during the evaluated condition.

Mathematically:

$$
\frac{\partial Q}{\partial t}=0
$$

For steady conditions, pressure and velocity at fixed locations are also time-independent.

### 13.2 Application

Steady-state analysis is appropriate for evaluating:

- Pipe velocity.
- Friction losses.
- Minor losses.
- Total dynamic head.
- System curves.
- Pump operating points.
- Steady-state pressure distribution.

### 13.3 Limitations

Steady-state analysis does not directly evaluate:

- Pump startup transients.
- Pump shutdown transients.
- Water hammer.
- Pressure surges.
- Column separation.
- Transient cavitation.
- Time-dependent air movement.

These phenomena require additional analysis.

---

## 14. Hydraulic Grade Line and Energy Grade Line

### 14.1 Hydraulic Grade Line

$$
HGL=z+\frac{P}{\gamma}
$$

The HGL represents elevation head plus pressure head.

### 14.2 Energy Grade Line

$$
EGL=z+\frac{P}{\gamma}
+\alpha\frac{V^2}{2g}
$$

Therefore:

$$
EGL=HGL+\alpha\frac{V^2}{2g}
$$

### 14.3 Application

HGL and EGL analysis may be used to:

- Evaluate pressure distribution.
- Identify potentially problematic high points.
- Evaluate hydraulic losses.
- Compare pipeline configurations.
- Understand pump energy requirements.

---

## 15. Engineering Design Constraints

### 15.1 Pipe Design

Pipe design evaluations should consider:

- Actual internal diameter.
- Pipe material.
- Pipe pressure rating.
- Operating velocity.
- Friction losses.
- Pipeline profile.
- Applicable owner criteria.

### 15.2 Pumping System Design

Pumping system evaluations should consider:

- Required design flow.
- Total dynamic head.
- Pump operating range.
- Pump efficiency.
- Redundancy.
- Minimum submergence.
- Operating controls.

### 15.3 Valves and Fittings

Valve and fitting evaluations should consider:

- Component geometry.
- Applicable loss coefficients.
- Flow direction.
- Operating configuration.
- Manufacturer limitations.

### 15.4 Engineering References

Applicable criteria and methods should be supported by traceable engineering references.

Project-specific requirements should be established separately from universal calculation principles.

---

## 16. Independent Verification

### 16.1 Verification Requirements

HAS calculations should be independently verified using:

- Hand calculations.
- Analytical solutions.
- Recognized engineering references.
- Manufacturer performance data.
- Established hydraulic software.

### 16.2 Reference Software

Where applicable, comparisons may be performed using:

- EPANET.
- Bentley WaterGEMS.
- Bentley SewerGEMS.
- Manufacturer pump selection software.

### 16.3 Verification Documentation

Each verification case should document:

- Input values.
- Units.
- Assumptions.
- Calculation methods.
- Reference results.
- HAS results.
- Numerical differences.
- Acceptance criteria.
- Verification status.

Detailed procedures and tolerances are maintained in the Verification Plan.

---

## 17. Software Development Requirements

### 17.1 Modular Architecture

HAS should maintain separation between:

**Pipe calculations**

- Geometry.
- Velocity.
- Reynolds number.
- Friction factor.
- Major head loss.

**Fitting calculations**

- Loss coefficients.
- Minor head losses.

**Force-main analysis**

- Hydraulic boundaries.
- Aggregated losses.
- Total dynamic head.
- System head curves.

**Pump analysis**

- Pump performance curves.
- Operating points.
- Efficiency.
- Power requirements.

**Wet-well analysis**

- Operating storage.
- Pump cycling.
- Operating water levels.

### 17.2 Data Traceability

Engineering data should include:

- Source.
- Units.
- Applicable conditions.
- Assumptions.
- Limitations.

### 17.3 Automated Testing

HAS should incorporate:

- Unit tests.
- Analytical verification cases.
- Numerical edge-case tests.
- Input validation tests.
- Regression tests.

### 17.4 Documentation

Engineering calculation methods should remain separate from software implementation details where practical.

The design basis establishes the governing principles, while module-specific documentation defines detailed methods.

---

## 18. Engineering Limitations

HAS is intended to support engineering analysis and preliminary design evaluation.

The software does not replace:

- Professional engineering judgment.
- Applicable regulations.
- Owner design criteria.
- Manufacturer requirements.
- Independent engineering review.
- Specialized hydraulic modeling where required.

Results should not be used for final engineering design without appropriate verification and professional review.

---

## 19. Engineering References

Relevant references include:

1. Crane Technical Paper No. 410, *Flow of Fluids Through Valves, Fittings, and Pipe*.
2. Hydraulic Institute standards and technical guidelines.
3. Water Environment Federation wastewater pumping and collection system design publications.
4. American Water Works Association manuals and applicable standards.
5. U.S. EPA wastewater pumping technical publications.
6. Florida Department of Environmental Protection regulations applicable to wastewater collection and transmission facilities.
7. Manufacturer technical manuals.
8. Recognized fluid mechanics and hydraulic engineering textbooks.

Current editions and applicability should be confirmed before use in formal engineering design.

---

## 20. Summary

The HAS engineering design basis establishes the fundamental assumptions and principles governing development of the Hydraulic Analysis Suite.

The primary requirements are:

1. Apply recognized hydraulic engineering principles.
2. Maintain consistent units and fluid properties.
3. Use appropriate friction-loss and minor-loss methods.
4. Define hydraulic boundary conditions explicitly.
5. Distinguish steady-state analysis from transient analysis.
6. Maintain traceable engineering reference data.
7. Independently verify hydraulic calculations.
8. Document assumptions and limitations.
9. Maintain a modular software architecture.
10. Support future development of integrated pumping system and infrastructure decision analytics capabilities.

This design basis provides the technical foundation for HAS development and supports consistent engineering methods throughout the software.
