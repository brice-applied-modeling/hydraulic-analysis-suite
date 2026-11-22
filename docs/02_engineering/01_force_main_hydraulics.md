# Force Main Hydraulics

## 1. Purpose and Scope

This document establishes the engineering methodology, hydraulic boundary conditions, governing equations, and verification requirements for force-main analysis within the **Hydraulic Analysis Suite (HAS)**.

The methodology is intended primarily for municipal wastewater lift stations and pressurized force-main systems, with potential application to other water and wastewater pumping infrastructure.

The document addresses:

- Upstream suction and wet-well conditions.
- Downstream hydraulic grade line (HGL).
- Pressure and elevation boundary conditions.
- Hydraulic grade line and energy grade line (EGL).
- Major and minor hydraulic losses.
- Total dynamic head (TDH).
- System head curves.
- Single-pump and multiple-pump operating scenarios.
- Hydraulic verification and limitations.
- Future HAS software implementation.

This document provides an engineering design reference. It does not imply that every described calculation or capability has already been implemented in HAS.

---

## 2. Hydraulic System Definition

### 2.1 Typical Force-Main System

A municipal wastewater pumping system generally consists of:

1. Wet well or suction structure.
2. One or more pumps.
3. Pump discharge piping.
4. Valves and fittings.
5. Common discharge header, where applicable.
6. Force main.
7. Downstream receiving structure or pressurized system.

The pump adds hydraulic energy to the wastewater, allowing it to overcome differences in hydraulic grade and losses associated with the piping system.

The force main conveys the pumped flow and introduces hydraulic losses associated with pipe friction, fittings, valves, and other appurtenances.

### 2.2 Hydraulic System Boundaries

For a typical lift station, the pumping system may be evaluated between:

**Upstream boundary:** Wet-well water surface elevation.

**Downstream boundary:** Receiving hydraulic grade elevation or other defined downstream hydraulic condition.

The physical force main generally begins downstream of the pump discharge connection. However, the overall pumping system analysis may include suction-side losses, pump discharge piping, and downstream piping losses.

Boundary definitions must be consistent to avoid omitting or double-counting hydraulic losses.

### 2.3 Initial Analysis Assumptions

Unless otherwise specified, the initial HAS force-main methodology assumes:

- Steady-state flow.
- Incompressible liquid.
- Single-phase flow.
- Completely filled pressurized piping.
- Defined upstream and downstream hydraulic conditions.
- Known pipe geometry and material properties.
- Known or estimated fitting loss coefficients.
- Fluid properties appropriate for the application.

Hydraulic transients, air-pocket movement, two-phase flow, and time-dependent solids deposition are outside the initial analysis scope.

---

## 3. Governing Hydraulic Principles

### 3.1 Conservation of Mass

For steady, incompressible flow through a single pipe:

$$
Q = AV
$$

Where:

- $Q$ = volumetric flow rate (ft³/s).
- $A$ = internal pipe cross-sectional area (ft²).
- $V$ = average fluid velocity (ft/s).

For a circular pipe:

$$
A = \frac{\pi D^2}{4}
$$

Where:

- $D$ = actual internal pipe diameter (ft).

Therefore:

$$
V = \frac{4Q}{\pi D^2}
$$

Pipe diameter must be converted from inches to feet before performing calculations using these units.

### 3.2 General Energy Equation

The steady-flow energy equation between two locations is:

$$
z_1 + \frac{P_1}{\gamma}
+ \alpha_1 \frac{V_1^2}{2g}
+ H_p
=
z_2 + \frac{P_2}{\gamma}
+ \alpha_2 \frac{V_2^2}{2g}
+ h_L
$$

Where:

| Symbol | Description | Units |
|---|---|---|
| $z$ | Elevation head | ft |
| $P$ | Gauge pressure | lbf/ft² |
| $\gamma$ | Fluid specific weight | lbf/ft³ |
| $V$ | Average velocity | ft/s |
| $\alpha$ | Kinetic energy correction coefficient | Dimensionless |
| $g$ | Gravitational acceleration | ft/s² |
| $H_p$ | Head added by the pump | ft |
| $h_L$ | Total hydraulic losses | ft |

This formulation assumes steady, incompressible flow with no additional shaft work or energy transfer requiring separate treatment.

### 3.3 Total Dynamic Head

For a pumping system between large reservoirs or hydraulic boundaries where endpoint velocity heads are negligible:

$$
TDH = HGL_{downstream} - WSEL_{wetwell} + h_L
$$

Where:

- $TDH$ = total dynamic head (ft).
- $HGL_{downstream}$ = downstream hydraulic grade elevation (ft).
- $WSEL_{wetwell}$ = wet-well water surface elevation (ft).
- $h_L$ = total hydraulic losses between the selected boundaries (ft).

For more general boundary conditions:

$$
TDH =
\left(
z_2 + \frac{P_2}{\gamma}
+ \alpha_2\frac{V_2^2}{2g}
\right)
-
\left(
z_1 + \frac{P_1}{\gamma}
+ \alpha_1\frac{V_1^2}{2g}
\right)
+ h_L
$$

**Engineering consideration:** TDH is the hydraulic head the pump must add at the evaluated flow rate. It is not equivalent to friction loss alone.

---

## 4. Force-Main Boundary Conditions

### 4.1 General

Boundary conditions define the hydraulic state at the limits of the modeled system.

HAS should distinguish between:

- Upstream hydraulic conditions.
- Downstream hydraulic conditions.
- Internal hydraulic losses.
- Pump energy addition.
- Pipeline elevations and geometry.

Each boundary condition should be documented with its value, units, source, and applicable operating scenario.

### 4.2 Boundary Condition Requirements

| Boundary Condition | Description | Typical Unit |
|---|---|---|
| Wet-well water surface | Upstream hydraulic boundary | ft |
| Discharge HGL | Downstream hydraulic boundary | ft |
| Discharge pressure | Pressure at a specified downstream location | psi |
| Discharge elevation | Elevation of downstream pressure reference | ft |
| Vertical datum | Reference for all elevations | NAVD88, NGVD29, or local |
| Flow rate | Evaluated system flow | gpm |
| Fluid properties | Density, viscosity, and specific weight | Defined units |

All elevation values must use a consistent vertical datum.

---

## 5. Suction and Wet-Well Conditions

### 5.1 Wet-Well Water Surface Elevation

For an open wet well, the upstream hydraulic boundary is generally the water surface elevation.

At the free surface:

$$
HGL_{upstream} = WSEL_{wetwell}
$$

This assumes atmospheric pressure at the water surface.

### 5.2 Variable Wet-Well Elevations

Wet-well levels vary during pumping operations.

Relevant elevations may include:

- High-level alarm.
- Lag pump start.
- Lead pump start.
- Lead pump stop.
- Low-level alarm.
- Minimum allowable operating level.

The specific elevations depend on the pump control strategy and lift station design.

### 5.3 Effect on Pumping Head

For constant downstream hydraulic conditions, decreasing the wet-well water surface increases the static head requirement.

The change in static head can be expressed as:

$$
\Delta H_{static} = -\Delta WSEL_{wetwell}
$$

Where the downstream HGL remains constant.

Therefore, separate system curves should be developed for relevant wet-well operating elevations.

### 5.4 Suction-Side Losses

Depending on the pump configuration, suction-side hydraulic losses may include:

- Suction piping friction.
- Entrance losses.
- Suction elbows.
- Intake restrictions.
- Other suction appurtenances.

For submersible pumps, minimum submergence and intake geometry should be evaluated using manufacturer recommendations.

Suction-side losses must be included when the selected energy boundaries encompass those components.

---

## 6. Discharge Hydraulic Grade Line

### 6.1 Definition

The hydraulic grade line is:

$$
HGL = z + \frac{P}{\gamma}
$$

Where:

- $HGL$ = hydraulic grade elevation (ft).
- $z$ = elevation of the pressure reference point (ft).
- $P$ = gauge pressure (lbf/ft²).
- $\gamma$ = fluid specific weight (lbf/ft³).

For pressure expressed in psi:

$$
HGL = z + \frac{144P_{psi}}{\gamma}
$$

The HGL represents the elevation to which water would rise in an ideal piezometer connected to the pressurized system.

### 6.2 Discharge to an Open Structure

For discharge into an open receiving structure, the downstream hydraulic boundary may be represented by the receiving water surface elevation.

Where the receiving water surface is exposed to atmospheric pressure:

$$
HGL_{downstream} = WSEL_{receiving}
$$

This relationship is appropriate when the energy boundary is established at the receiving water surface and the associated discharge losses are treated consistently.

### 6.3 Discharge to a Pressurized System

For discharge into a pressurized system:

$$
HGL_{downstream}
=
z_d + \frac{144P_d}{\gamma}
$$

Where:

- $z_d$ = discharge pressure reference elevation (ft).
- $P_d$ = downstream gauge pressure (psi).

The downstream HGL may vary with:

- System demand.
- Other pumping stations.
- Network operating conditions.
- Storage tank elevations.
- Pressure control facilities.

A fixed downstream HGL may be a simplifying assumption rather than a complete representation of the receiving network.

### 6.4 Downstream Hydraulic Scenarios

Where applicable, evaluate:

- Minimum downstream HGL.
- Normal downstream HGL.
- Maximum downstream HGL.
- Existing system conditions.
- Future system conditions.
- Other pumping facilities operating.

---

## 7. Pressure Conditions

### 7.1 Pressure and HGL Relationship

Pressure head is:

$$
\frac{P}{\gamma} = HGL - z
$$

Therefore:

$$
P_{psi} = \frac{\gamma(HGL-z)}{144}
$$

For water with a specific weight of approximately 62.4 lbf/ft³:

$$
P_{psi} \approx 0.433(HGL-z)
$$

The approximate factor varies with fluid specific weight.

### 7.2 Pressure Distribution

Pressure along a force main depends on:

- Hydraulic grade elevation.
- Pipe elevation.
- Friction losses.
- Minor losses.
- Changes in pipe diameter.
- Pump operation.
- Downstream hydraulic conditions.

A rising pipe profile can produce decreasing local pressure even when the HGL changes only slightly.

### 7.3 Minimum Pressure Evaluation

Minimum pressure conditions should be evaluated at relevant pipeline locations, particularly:

- High points.
- Long ascending segments.
- Air-release valve locations.
- Locations susceptible to air accumulation.
- Conditions with low downstream hydraulic grade.

Potential subatmospheric pressure conditions should be identified for further engineering evaluation.

### 7.4 Maximum Pressure Evaluation

Maximum operating pressure should be evaluated against applicable pipe and equipment pressure ratings.

Relevant considerations include:

- Pump shutoff head.
- Maximum steady-state operating pressure.
- Low pipeline elevations.
- Pipe pressure class.
- Valve pressure ratings.
- Fitting pressure ratings.

Transient pressures require separate hydraulic surge analysis.

---

## 8. Elevation Datum

### 8.1 General

All hydraulic elevations must reference a consistent vertical datum.

Common examples include:

- NAVD88.
- NGVD29.
- Project-specific local datum.

A benchmark provides a physical reference but does not independently define the vertical datum.

### 8.2 Required Elevation Data

Relevant elevations may include:

- Wet-well water surface.
- Pump centerline or reference elevation.
- Force-main centerline.
- Force-main high and low points.
- Discharge connection elevation.
- Receiving structure water surface.
- Downstream hydraulic grade.

### 8.3 Datum Consistency

HAS should not combine elevations from different datums without an appropriate transformation.

Datum transformations must be documented and should not be represented as universal constant offsets.

### 8.4 Documentation

Each analysis should identify:

- Vertical datum.
- Elevation units.
- Source of elevation data.
- Benchmark information, where available.
- Datum transformations, where applicable.

---

## 9. Hydraulic Grade Line and Energy Grade Line

### 9.1 Hydraulic Grade Line

The HGL represents elevation head plus pressure head:

$$
HGL = z + \frac{P}{\gamma}
$$

The HGL is used to evaluate pressure conditions along the force main.

### 9.2 Energy Grade Line

The energy grade line represents total mechanical energy head:

$$
EGL = z + \frac{P}{\gamma}
+ \alpha\frac{V^2}{2g}
$$

Therefore:

$$
EGL = HGL + \alpha\frac{V^2}{2g}
$$

For ordinary flow conditions, the EGL is above the HGL by the kinetic energy head.

### 9.3 Hydraulic Losses

For a passive pipe segment without energy addition or extraction:

$$
EGL_1 - EGL_2 = h_L
$$

Hydraulic losses cause the EGL to decrease in the direction of flow.

Where velocity changes, the HGL may increase or decrease independently of the local EGL slope.

### 9.4 Pump Energy Addition

A pump adds energy to the system.

Across a pump:

$$
EGL_{out} - EGL_{in} = H_p
$$

This simplified expression assumes that losses outside the manufacturer's defined pump head are accounted for separately.

### 9.5 Engineering Applications

HGL and EGL profiles can be used to:

- Evaluate pressure distribution.
- Identify potentially problematic high points.
- Assess hydraulic losses.
- Compare force-main alternatives.
- Understand pump energy requirements.
- Evaluate steady-state operating scenarios.

Steady-state HGL and EGL analysis does not replace transient hydraulic modeling.

---

## 10. Major and Minor Hydraulic Losses

### 10.1 Major Friction Losses

The Darcy-Weisbach equation is:

$$
h_f = f\frac{L}{D}\frac{V^2}{2g}
$$

Where:

| Symbol | Description | Units |
|---|---|---|
| $h_f$ | Friction head loss | ft |
| $f$ | Darcy friction factor | Dimensionless |
| $L$ | Pipe length | ft |
| $D$ | Actual internal diameter | ft |
| $V$ | Average velocity | ft/s |
| $g$ | Gravitational acceleration | ft/s² |

The Darcy friction factor depends on Reynolds number and relative roughness.

### 10.2 Reynolds Number

$$
Re = \frac{VD}{\nu}
$$

Where:

- $Re$ = Reynolds number.
- $V$ = velocity (ft/s).
- $D$ = internal diameter (ft).
- $\nu$ = kinematic viscosity (ft²/s).

### 10.3 Minor Losses

Minor losses are calculated using:

$$
h_m = \sum_{i=1}^{n} K_i\frac{V_i^2}{2g}
$$

Where:

- $K_i$ = loss coefficient for fitting $i$.
- $V_i$ = applicable velocity for fitting $i$.

Typical minor-loss components include:

- Elbows.
- Tees and wyes.
- Valves.
- Reducers.
- Expanders.
- Entrances.
- Exits.

### 10.4 Total Hydraulic Losses

For a force main containing multiple pipe segments and fittings:

$$
h_L =
\sum_{i=1}^{n}h_{f,i}
+
\sum_{j=1}^{m}h_{m,j}
$$

Each segment must use its own applicable geometry, flow, and hydraulic properties.

---

## 11. System Head Curves

### 11.1 General

A system head curve represents the head required to convey flow through the pumping system.

For simplified reservoir-to-reservoir conditions:

$$
H_{system}(Q) = H_{static} + h_L(Q)
$$

Where:

$$
H_{static} = HGL_{downstream} - WSEL_{wetwell}
$$

The static head may be positive, zero, or negative depending on the hydraulic boundaries.

### 11.2 Developing the System Curve

The calculation procedure is:

1. Define the flow range.
2. Establish upstream and downstream hydraulic boundaries.
3. Calculate velocity for each pipe segment.
4. Calculate major friction losses.
5. Calculate minor losses.
6. Sum total hydraulic losses.
7. Add the applicable static head.
8. Plot system head against flow.

### 11.3 Multiple System Curves

Separate system curves may be required for:

- Minimum wet-well level.
- Maximum wet-well level.
- Minimum downstream HGL.
- Maximum downstream HGL.
- Existing conditions.
- Future conditions.
- Alternative force-main configurations.

### 11.4 Pump Operating Point

The pump operating point occurs where:

$$
H_{pump}(Q) = H_{system}(Q)
$$

The operating point must be recalculated whenever the pump curve or system conditions change.

---

## 12. Force-Main Operating Scenarios

### 12.1 Single-Pump Operation

Evaluate the system flow and head with one pump operating.

### 12.2 Multiple-Pump Operation

For parallel pumps, the combined pump performance curve must be evaluated against the system curve.

For identical pumps operating at a common developed head:

$$
Q_{total}(H) = nQ_{single}(H)
$$

Where:

- $n$ = number of operating pumps.
- $Q_{single}(H)$ = flow from one pump at the specified head.

This relationship assumes compatible pump curves and hydraulic boundary conditions.

Two identical pumps operating in parallel do not necessarily deliver twice the flow of one pump operating alone.

### 12.3 Minimum Flow Conditions

Evaluate:

- Pump minimum operating flow.
- Force-main velocity.
- Solids transport.
- Potential deposition.
- Wastewater detention time.

### 12.4 Maximum Flow Conditions

Evaluate:

- Friction losses.
- Pump operating limits.
- Maximum steady-state pressure.
- Pipe pressure ratings.
- Pumping capacity.

### 12.5 Future Operating Conditions

Where applicable, evaluate:

- Increased design flow.
- Changes in downstream HGL.
- Force-main extensions.
- Additional pumping stations.
- Alternative force-main diameters.
- Future system expansion.

---

## 13. Engineering Verification

### 13.1 Analytical Verification

HAS calculations should be independently verified for:

- Pipe cross-sectional area.
- Flow velocity.
- Reynolds number.
- Friction factor.
- Major friction losses.
- Minor losses.
- Total dynamic head.
- Pressure head.
- HGL.
- EGL.

### 13.2 Reference Software Comparison

Where appropriate, HAS results may be compared against:

- EPANET.
- Bentley WaterGEMS.
- Bentley SewerGEMS.
- Independent engineering calculations.

Reference comparisons must use consistent:

- Boundary conditions.
- Pipe geometry.
- Pipe roughness.
- Loss coefficients.
- Fluid properties.
- Hydraulic methods.
- Units.

### 13.3 Numerical Acceptance Criteria

Verification tolerances should be established in the HAS verification plan.

Acceptance criteria should distinguish between:

- Analytical precision.
- Numerical convergence.
- Differences between calculation methods.
- Input uncertainty.
- Reference data precision.

### 13.4 Verification Documentation

Each verification case should record:

- Case identifier.
- Input data.
- Assumptions.
- Boundary conditions.
- Reference calculation.
- HAS calculation.
- Numerical difference.
- Acceptance criteria.
- Verification status.

---

## 14. HAS Software Implementation Considerations

### 14.1 Force-Main Data Model

A future force-main model may contain:

- Pipe segments.
- Pipe materials.
- Internal diameters.
- Pipe lengths.
- Fittings and valves.
- Elevation data.
- Fluid properties.
- Hydraulic boundary conditions.
- Flow conditions.

### 14.2 Boundary Condition Attributes

Potential attributes include:

- `wet_well_level`
- `discharge_hgl`
- `discharge_pressure`
- `discharge_elevation`
- `vertical_datum`
- `fluid_properties`

All numerical attributes should have documented units.

### 14.3 Hydraulic Analysis Functions

Potential functionality includes:

- `calculate_static_head()`
- `calculate_major_losses()`
- `calculate_minor_losses()`
- `calculate_total_dynamic_head()`
- `generate_system_curve()`
- `calculate_pressure_profile()`
- `calculate_hgl_profile()`
- `calculate_egl_profile()`

These names are conceptual and should be reconciled with the actual HAS package architecture.

### 14.4 Separation of Responsibilities

**Pipe and fitting components**

Calculate individual hydraulic properties and losses.

**Force-main system**

Aggregates pipe and fitting losses and evaluates system hydraulic requirements.

**Pump model**

Represents pump performance and energy addition.

**System analysis**

Determines operating points and evaluates pumping scenarios.

This separation supports modular development and independent testing.

---

## 15. Limitations and Engineering Judgment

The initial steady-state methodology does not directly evaluate:

- Water hammer.
- Pump trip transients.
- Pressure surges.
- Column separation.
- Air-pocket movement.
- Two-phase flow.
- Transient cavitation.
- Detailed wet-well hydraulics.
- Time-dependent solids deposition.

These conditions may require specialized analysis and additional engineering evaluation.

Steady-state results should not be interpreted as demonstrating protection against transient pressure conditions.

---

## 16. Engineering References

Relevant engineering references include:

1. Crane Technical Paper No. 410, *Flow of Fluids Through Valves, Fittings, and Pipe*.
2. Hydraulic Institute standards and technical guidelines.
3. Water Environment Federation wastewater pumping and collection system publications.
4. American Water Works Association manuals and applicable standards.
5. U.S. EPA wastewater pumping technical publications.
6. Florida Department of Environmental Protection regulations applicable to wastewater collection and transmission facilities.
7. Manufacturer technical manuals for pipes, valves, fittings, and pumps.
8. Recognized fluid mechanics and hydraulic engineering textbooks.

Current editions and applicability should be verified before use in formal engineering design.

---

## 17. Summary

Force-main hydraulic analysis requires consistent definition of hydraulic boundaries, pipe geometry, hydraulic losses, and pump energy addition.

The primary engineering requirements are:

1. Establish consistent elevation datums and boundary conditions.
2. Distinguish static head from hydraulic losses.
3. Calculate major and minor losses using appropriate methods.
4. Evaluate pressure using the hydraulic grade line.
5. Evaluate total mechanical energy using the energy grade line.
6. Develop system head curves for relevant operating conditions.
7. Determine pump operating points from pump and system curve intersections.
8. Independently verify hydraulic calculations.
9. Document assumptions, limitations, and source data.

These principles establish the technical foundation for force-main analysis within HAS and support future development of integrated pumping system and lift station modeling capabilities.
