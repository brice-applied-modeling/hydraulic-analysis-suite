# Design Checks for Velocity, NPSH, Pump Operation, and Surge Screening

## 1. Purpose and Scope

This document establishes the engineering methodology for evaluating hydraulic design checks within the Hydraulic Analysis Suite (HAS).

The design-check framework is intended to evaluate calculated hydraulic results against documented engineering criteria and identify conditions requiring additional review.

The initial design-check categories include:

1. Pipe and force-main velocity.
2. Net Positive Suction Head (NPSH).
3. Pump operating conditions.
4. Preliminary hydraulic transient and surge screening.

Additional checks may be incorporated as HAS development progresses.

The framework is intended to support:

- Consistent engineering evaluations.
- Transparent acceptance criteria.
- Automated identification of potential design concerns.
- Traceable engineering assumptions.
- Independent calculation verification.
- Consistent reporting across operating scenarios.

This document defines the engineering basis for future software implementation. It does not imply that the described design-check functionality has been implemented.

---

## 2. Design-Check Philosophy

### 2.1 General

HAS should distinguish between hydraulic calculations and engineering design checks.

**Hydraulic calculations** determine quantities such as:

- Flow.
- Velocity.
- Pressure.
- Total dynamic head.
- Pump operating point.
- NPSH available.
- Estimated surge pressure.

**Engineering design checks** evaluate those quantities against applicable criteria.

For example, a force-main hydraulic model may calculate a velocity of 6.2 ft/s.

The design-check framework then determines whether that velocity satisfies the criteria established for the particular project.

### 2.2 Sources of Design Criteria

Design criteria may originate from:

1. Applicable laws and regulations.
2. Permitting authority requirements.
3. Owner or utility design standards.
4. Project-specific engineering requirements.
5. Manufacturer operating limitations.
6. Recognized engineering standards.
7. Documented engineering assumptions.

The controlling requirement should be identified for each check.

Generic screening values should not automatically override project-specific requirements.

### 2.3 Engineering Judgment

HAS should not independently determine whether a design is professionally acceptable.

The software should identify compliance with defined criteria, potential concerns, and limitations requiring engineering judgment.

### 2.4 Separation of Calculations and Criteria

Engineering criteria should be maintained separately from the underlying hydraulic calculation functions.

This allows the same hydraulic result to be evaluated against different owner requirements without modifying the calculation methodology.

---

## 3. Design-Check Classification

### 3.1 Recommended Status Values

Each check should return one of the following statuses.

| Status | Description |
|---|---|
| PASS | Calculated result satisfies the specified criterion |
| WARNING | Result approaches a limit or warrants engineering review |
| FAIL | Result violates a specified criterion |
| NOT_EVALUATED | Required inputs or calculations are unavailable |
| NOT_APPLICABLE | Check does not apply to the evaluated configuration |

### 3.2 PASS

A PASS indicates that the evaluated result satisfies the specified criterion.

It does not establish overall design adequacy.

### 3.3 WARNING

A WARNING indicates that a result:

- Approaches an applicable limit.
- Falls within a defined review range.
- Relies on uncertain assumptions.
- Requires additional engineering evaluation.

Warning thresholds should be documented and configurable.

### 3.4 FAIL

A FAIL indicates that a calculated result does not satisfy a specified criterion.

The result should identify:

- The failed criterion.
- Calculated value.
- Required value or range.
- Units.
- Applicable scenario.
- Reference source.

### 3.5 NOT_EVALUATED

This status applies when the required information is unavailable or the calculation cannot be completed reliably.

Missing information must not be interpreted as a passing result.

### 3.6 NOT_APPLICABLE

This status applies when the check is not relevant to the evaluated system.

For example, NPSH screening may not be meaningful using a conventional suction-piping model for a submersible pump without an appropriate manufacturer-specific intake representation.

---

## 4. General Design-Check Data Requirements

Each design check should include:

- Check identifier.
- Check name.
- Engineering category.
- Scenario identifier.
- Calculated value.
- Criterion type.
- Criterion value.
- Units.
- Source reference.
- Evaluation status.
- Engineering message.
- Verification status.

### 4.1 Example Check Record

| Field | Example |
|---|---|
| Check ID | VEL-001 |
| Check Name | Maximum Force-Main Velocity |
| Scenario | SCN-001 |
| Calculated Value | 6.2 ft/s |
| Criterion | Maximum 8.0 ft/s |
| Status | PASS |
| Source | Hypothetical project criterion |

The example criterion is illustrative and is not a universal force-main velocity limit.

---

# Part I: Velocity Design Checks

## 5. Pipe Velocity Fundamentals

### 5.1 Governing Equation

Pipe velocity is calculated using continuity:

$$
Q=AV
$$

Therefore:

$$
V=\frac{Q}{A}
$$

For a circular pipe:

$$
A=\frac{\pi D^2}{4}
$$

Thus:

$$
V=\frac{4Q}{\pi D^2}
$$

Where:

- $V$ = average flow velocity (ft/s).
- $Q$ = volumetric flow rate (ft³/s).
- $A$ = internal pipe area (ft²).
- $D$ = actual internal pipe diameter (ft).

### 5.2 Unit Conversion

For flow in gallons per minute:

$$
Q_{ft^3/s}=\frac{Q_{gpm}}{448.831}
$$

The velocity calculation must use actual internal diameter rather than nominal pipe diameter.

---

## 6. Minimum Velocity Check

### 6.1 Purpose

Minimum velocity screening may be used to identify conditions associated with:

- Solids deposition.
- Inadequate transport velocity.
- Extended low-flow operation.
- Potential maintenance concerns.

### 6.2 Evaluation

For a defined minimum criterion:

$$
V_{calculated}\geq V_{minimum}
$$

### 6.3 Result Classification

**PASS**

$$
V_{calculated}\geq V_{minimum}
$$

**FAIL**

$$
V_{calculated}<V_{minimum}
$$

### 6.4 Engineering Considerations

A minimum velocity criterion does not independently establish adequate solids transport.

Actual performance depends on:

- Solids characteristics.
- Flow duration.
- Pipe slope and profile.
- Wastewater characteristics.
- Pump cycling.
- Operating frequency.

The check should identify whether the velocity represents continuous, intermittent, or peak operating conditions.

---

## 7. Maximum Velocity Check

### 7.1 Purpose

Maximum velocity screening may identify conditions associated with:

- Excessive friction losses.
- Increased pumping energy.
- Elevated transient pressure potential.
- Equipment operating limitations.
- Owner-specific velocity restrictions.

### 7.2 Evaluation

$$
V_{calculated}\leq V_{maximum}
$$

### 7.3 Result Classification

**PASS**

$$
V_{calculated}\leq V_{maximum}
$$

**FAIL**

$$
V_{calculated}>V_{maximum}
$$

### 7.4 Engineering Considerations

Maximum velocity limits should be established using applicable owner criteria, engineering standards, and project requirements.

The framework should not impose a universal maximum velocity without a documented basis.

---

## 8. Velocity Check Scenarios

Velocity should be evaluated under relevant operating conditions.

Recommended scenarios include:

1. Minimum pumping flow.
2. Normal single-pump operation.
3. Maximum single-pump flow.
4. Parallel-pump operation.
5. Maximum anticipated station discharge.
6. Future design flow.
7. Alternative force-main configurations.

### 8.1 Governing Results

The minimum and maximum velocity conditions may occur under different operating scenarios.

HAS should retain the scenario responsible for each governing result.

### 8.2 Example

Consider a hypothetical 8-inch internal-diameter force main carrying 500 gpm.

Convert flow:

$$
Q=\frac{500}{448.831}
$$

$$
Q\approx1.114\ \mathrm{ft^3/s}
$$

Pipe diameter:

$$
D=\frac{8}{12}=0.6667\ \mathrm{ft}
$$

Area:

$$
A=\frac{\pi(0.6667)^2}{4}
$$

$$
A\approx0.3491\ \mathrm{ft^2}
$$

Velocity:

$$
V=\frac{1.114}{0.3491}
$$

$$
V\approx3.19\ \mathrm{ft/s}
$$

The calculated velocity should then be evaluated against the applicable project-specific criteria.

---

# Part II: NPSH Design Checks

## 9. NPSH Fundamentals

### 9.1 General

Net Positive Suction Head is used to evaluate the pressure conditions available at a pump inlet relative to the liquid vapor pressure.

Two principal quantities are:

- Net Positive Suction Head Available (NPSHA).
- Net Positive Suction Head Required (NPSHR).

### 9.2 NPSH Available

For a general suction system, NPSHA may be expressed as:

$$
NPSHA=
\frac{P_{s,abs}}{\gamma}
+
\frac{V_s^2}{2g}
-
\frac{P_v}{\gamma}
$$

Where:

- $P_{s,abs}$ = absolute static pressure at the pump suction reference section.
- $V_s$ = average velocity at that section.
- $P_v$ = absolute vapor pressure.
- $\gamma$ = fluid specific weight.

Pressure terms must use units compatible with the head calculation.

### 9.3 Open Wet-Well Formulation

For a pump supplied from an open wet well, using an appropriate suction reference elevation:

$$
NPSHA=
H_{atm}
+
H_{static,suction}
-
h_{suction}
-
H_{vapor}
$$

Where:

- $H_{atm}$ = atmospheric pressure head.
- $H_{static,suction}$ = water surface elevation relative to the pump suction reference.
- $h_{suction}$ = suction-side hydraulic losses.
- $H_{vapor}$ = vapor pressure head.

This formulation assumes negligible approach velocity at the wet-well free surface.

### 9.4 NPSH Required

NPSHR should be obtained from manufacturer pump performance data for the applicable:

- Pump model.
- Impeller.
- Speed.
- Operating flow.
- Test definition.

Published NPSHR commonly represents NPSH3, associated with a specified head-drop criterion.

NPSHR should not automatically be interpreted as a cavitation-free operating threshold.

---

## 10. NPSH Margin Check

### 10.1 Basic Evaluation

The basic comparison is:

$$
NPSHA\geq NPSHR
$$

### 10.2 NPSH Margin

Absolute margin:

$$
M_{NPSH}=NPSHA-NPSHR
$$

Ratio:

$$
R_{NPSH}=\frac{NPSHA}{NPSHR}
$$

The ratio is defined only when NPSHR is positive.

### 10.3 Engineering Requirements

The required margin should be established using applicable manufacturer guidance and Hydraulic Institute recommendations.

A simple comparison of NPSHA and NPSHR may not provide sufficient protection against cavitation-related damage.

### 10.4 Design-Check Evaluation

For a required absolute margin:

$$
NPSHA-NPSHR\geq M_{required}
$$

Alternatively, where a ratio criterion is applicable:

$$
\frac{NPSHA}{NPSHR}\geq R_{required}
$$

HAS should use the criterion specified for the project rather than assume a universal NPSH safety factor.

---

## 11. Governing NPSH Scenarios

NPSH should be evaluated under conditions that may reduce available suction head or increase required NPSH.

Potential governing conditions include:

- Minimum wet-well elevation.
- Maximum pump operating flow.
- Maximum liquid temperature.
- Reduced atmospheric pressure.
- Increased suction losses.
- Maximum pump speed.
- Parallel-pump operation.

### 11.1 Minimum Wet-Well Elevation

Lower wet-well elevations generally reduce NPSHA.

### 11.2 Maximum Pump Flow

Higher flow may increase suction losses and NPSHR.

### 11.3 Fluid Temperature

Increasing fluid temperature generally increases vapor pressure and reduces NPSHA, all else equal.

### 11.4 Atmospheric Pressure

Site elevation and atmospheric conditions affect available atmospheric pressure head.

### 11.5 Submersible Pumps

Submersible pump evaluation requires attention to manufacturer-defined suction geometry, submergence, and operating requirements.

Conventional suction-piping equations should not be applied indiscriminately.

---

# Part III: Pump Operating Checks

## 12. Pump Operating Point

### 12.1 General

The operating point occurs at the intersection of the pump performance curve and system head curve.

$$
H_{pump}(Q)=H_{system}(Q)
$$

### 12.2 Required Inputs

- Pump head-flow curve.
- System head curve.
- Pump operating speed.
- Wet-well elevation.
- Downstream HGL.
- Pump configuration.
- Relevant manufacturer limits.

### 12.3 Required Outputs

- Operating flow.
- Developed head.
- Efficiency.
- Brake horsepower.
- Motor loading.
- Operating range status.

---

## 13. Best Efficiency Point Check

### 13.1 Definition

The Best Efficiency Point (BEP) is the operating condition at which the pump achieves its maximum hydraulic efficiency for a specified configuration and speed.

### 13.2 Relative Flow

A useful comparison is:

$$
R_{BEP}=\frac{Q_{operating}}{Q_{BEP}}
$$

Where:

- $Q_{operating}$ = calculated operating flow.
- $Q_{BEP}$ = flow at BEP.

### 13.3 Interpretation

The BEP ratio provides an indication of how far the operating flow is from the pump's best efficiency point.

It does not independently establish acceptable pump operation.

### 13.4 Criteria

Acceptable operating ranges should be obtained from manufacturer information and applicable Hydraulic Institute guidance.

HAS should not impose a universal BEP operating band without identifying its source.

---

## 14. Preferred Operating Region Check

### 14.1 General

The Preferred Operating Region (POR) identifies the range of operation generally preferred for pump reliability and performance.

### 14.2 Evaluation

$$
Q_{POR,min}\leq Q_{operating}\leq Q_{POR,max}
$$

### 14.3 Classification

**PASS:** Operating flow is within the specified POR.

**WARNING:** Operating flow is outside the POR but within the applicable Allowable Operating Region (AOR), where documented.

**FAIL:** Operating flow is outside the applicable AOR or violates another mandatory operating limitation.

### 14.4 Data Requirements

POR and AOR boundaries should be established using applicable manufacturer data and engineering guidance.

---

## 15. Minimum and Maximum Flow Checks

### 15.1 Minimum Continuous Flow

Pumps may have minimum continuous operating flow requirements.

Potential concerns include:

- Recirculation.
- Excessive vibration.
- Temperature rise.
- Unstable hydraulic operation.

### 15.2 Minimum Flow Check

$$
Q_{operating}\geq Q_{minimum,continuous}
$$

### 15.3 Maximum Flow Check

$$
Q_{operating}\leq Q_{maximum,allowable}
$$

### 15.4 Engineering Considerations

Minimum and maximum operating limits should be based on manufacturer documentation.

A pump's published curve endpoint does not automatically establish an acceptable continuous operating condition.

---

## 16. Pump Power and Motor Loading Checks

### 16.1 Hydraulic Power

For U.S. customary units:

$$
HP_{hydraulic}=\frac{QHSG}{3960}
$$

Where:

- $Q$ = flow (gpm).
- $H$ = developed head (ft).
- $SG$ = fluid specific gravity.

### 16.2 Brake Horsepower

$$
BHP=\frac{QHSG}{3960\eta_p}
$$

Where $\eta_p$ is pump efficiency expressed as a decimal.

### 16.3 Motor Loading

A simplified motor loading ratio is:

$$
R_{motor}=\frac{BHP}{HP_{motor,rated}}
$$

### 16.4 Motor Capacity Check

$$
BHP\leq HP_{motor,allowable}
$$

The allowable motor loading must account for applicable motor rating, service conditions, and manufacturer requirements.

### 16.5 Operating Scenarios

Motor loading should be evaluated at relevant operating points, including potential maximum-flow conditions.

Maximum pump head does not necessarily correspond to maximum brake horsepower.

---

## 17. Pump Capacity Checks

### 17.1 Required Pumping Capacity

A basic capacity comparison is:

$$
Q_{available}\geq Q_{required}
$$

### 17.2 Firm Capacity

Where applicable, firm capacity should be evaluated with the largest pumping unit unavailable.

### 17.3 Parallel-Pump Capacity

Combined pump capacity must be calculated from the pump and system hydraulic relationships.

It should not be assumed equal to the sum of independently rated pump capacities.

### 17.4 Required Scenarios

Potential scenarios include:

- One pump operating.
- Multiple pumps operating.
- Largest pump unavailable.
- Peak wastewater inflow.
- Maximum downstream HGL.
- Minimum wet-well elevation.
- Future design conditions.

---

## 18. Pump Cycling Checks

### 18.1 General

Pump cycling should be evaluated using the wet-well design methodology.

### 18.2 Starts per Hour

For repeated identical cycles:

$$
N=\frac{60}{t_{cycle}}
$$

### 18.3 Allowable Starts

$$
N_{calculated}\leq N_{allowable}
$$

### 18.4 Minimum Run Time

$$
t_{run}\geq t_{run,min}
$$

### 18.5 Minimum Off Time

$$
t_{off}\geq t_{off,min}
$$

### 18.6 Limitations

Simplified constant-flow cycling equations may not adequately represent variable-speed pumps or complex lead/lag control systems.

---

# Part IV: Surge and Transient Screening Checks

## 19. Surge Screening Fundamentals

### 19.1 General

Hydraulic transient screening evaluates potential pressure changes caused by rapid flow disturbances.

Examples include:

- Pump trips.
- Rapid valve closure.
- Pump startup.
- Check valve slam.
- Flow reversal.

### 19.2 Screening Limitation

The initial HAS transient screening methodology does not predict complete transient pressure histories.

Its purpose is to identify potential hazards requiring further engineering investigation.

---

## 20. Joukowsky Pressure Screening

### 20.1 Pressure Head Change

$$
|\Delta H|=\frac{a|\Delta V|}{g}
$$

Where:

- $a$ = wave speed (ft/s).
- $\Delta V$ = velocity change (ft/s).
- $g$ = gravitational acceleration (ft/s²).

### 20.2 Pressure Change

$$
|\Delta P|=\frac{\gamma|\Delta H|}{144}
$$

Where:

- $\gamma$ = fluid specific weight (lbf/ft³).
- $\Delta P$ = pressure change (psi).

### 20.3 Positive Pressure Screening

$$
P_{screen,max}=P_{steady}+|\Delta P|
$$

### 20.4 Negative Pressure Screening

$$
P_{screen,min}=P_{steady}-|\Delta P|
$$

These are simplified screening estimates and should not be interpreted as guaranteed transient extrema.

---

## 21. Maximum Pressure Check

### 21.1 Evaluation

For an applicable pressure limit:

$$
P_{screen,max}\leq P_{allowable}
$$

### 21.2 Engineering Considerations

Applicable pressure limits may depend on:

- Pipe material.
- Pressure class.
- Manufacturer ratings.
- Temperature.
- Surge allowance.
- Fittings and valves.
- Joint restraint.
- Applicable engineering standards.

### 21.3 Classification

**PASS:** Screening estimate satisfies the specified pressure criterion.

**FAIL:** Screening estimate exceeds the specified criterion.

A PASS does not establish complete transient safety.

---

## 22. Minimum Pressure Check

### 22.1 General

Negative pressure screening identifies potential subatmospheric conditions and vapor-pressure concerns.

### 22.2 Absolute Pressure

$$
P_{absolute}=P_{gauge}+P_{atmospheric}
$$

### 22.3 Vapor Pressure Criterion

$$
P_{absolute}>P_{vapor}
$$

### 22.4 Classification

**WARNING:** Screening indicates subatmospheric pressure requiring engineering review.

**FAIL:** Screening indicates a vapor-pressure threshold violation or another mandatory minimum-pressure criterion is not satisfied.

### 22.5 Physical Validity

If a simplified single-phase pressure estimate falls below vapor pressure, HAS should flag the breakdown of the assumed calculation model.

The result should not be represented as a physically valid sustained liquid pressure.

---

## 23. Wave Travel Time Check

### 23.1 Round-Trip Wave Travel Time

$$
T_r=\frac{2L}{a}
$$

Where:

- $L$ = relevant hydraulic path length (ft).
- $a$ = wave speed (ft/s).

### 23.2 Closure Time Ratio

$$
R_t=\frac{T_c}{T_r}
$$

Where $T_c$ is the valve closure time.

### 23.3 Interpretation

The ratio may be used to identify potentially rapid closure conditions.

However, it does not independently establish transient safety.

### 23.4 Design Check

HAS may classify valve closure as:

- Rapid relative to wave travel time.
- Intermediate.
- Slow relative to wave travel time.

Classification thresholds should be based on documented engineering methodology.

---

## 24. Pipeline Profile Screening

### 24.1 General

Force-main elevation profiles should be evaluated for potential low-pressure conditions.

### 24.2 Hydraulic Grade

$$
HGL=z+\frac{P}{\gamma}
$$

Where pressure is expressed in units compatible with specific weight.

### 24.3 Screening Evaluation

At a defined pipeline location:

$$
HGL_{screen}=HGL_{steady}\pm|\Delta H|
$$

### 24.4 Potential Concerns

- High points with limited pressure margin.
- Subatmospheric pressure.
- Potential vapor-pressure conditions.
- Air accumulation.
- Column separation risk.

### 24.5 Limitation

The simplified screening HGL is not a complete transient pressure envelope.

---

# Part V: Integrated Design-Check Framework

## 25. Scenario-Based Design Checks

### 25.1 General

Design checks should be evaluated for each applicable operating scenario.

A single hydraulic configuration may pass under normal conditions and fail under another operating condition.

### 25.2 Example Scenario Matrix

| Scenario | Velocity | NPSH | Pump Operation | Surge |
|---|---|---|---|---|
| Normal single-pump | Evaluate | Evaluate | Evaluate | Screen |
| Minimum wet-well level | Evaluate | Evaluate | Evaluate | Screen |
| Maximum wet-well level | Evaluate | Evaluate | Evaluate | Screen |
| Maximum downstream HGL | Evaluate | Evaluate | Evaluate | Screen |
| Parallel pumps | Evaluate | Evaluate | Evaluate | Screen |
| Peak wastewater inflow | Evaluate | Evaluate | Evaluate | Screen |
| Largest pump unavailable | Evaluate | Evaluate | Evaluate | As applicable |
| Pump trip | Initial condition | As applicable | Initial condition | Screen |

### 25.3 Governing Conditions

HAS should identify the scenario producing the most critical result for each criterion.

Examples include:

- Maximum velocity.
- Minimum NPSH margin.
- Maximum motor loading.
- Minimum pumping capacity.
- Maximum screening pressure.
- Minimum screening pressure.

---

## 26. Engineering Criteria Configuration

### 26.1 Purpose

Criteria should be configurable without modifying hydraulic calculation code.

### 26.2 Potential Criteria

The criteria configuration may include:

- Minimum velocity.
- Maximum velocity.
- Minimum NPSH margin.
- Minimum NPSH ratio.
- Preferred operating range.
- Allowable operating range.
- Minimum pump flow.
- Maximum pump flow.
- Maximum motor loading.
- Minimum firm capacity.
- Maximum starts per hour.
- Minimum run time.
- Minimum off time.
- Maximum allowable pressure.
- Minimum allowable pressure.

### 26.3 Traceability

Each criterion should identify:

- Source.
- Applicable project.
- Revision or edition.
- Effective date, where applicable.
- Engineering justification.
- Whether the criterion is mandatory or advisory.

---

## 27. Design-Check Results

### 27.1 Recommended Result Structure

A future `DesignCheckResult` data structure may contain:

- `check_id`
- `check_name`
- `category`
- `scenario_id`
- `calculated_value`
- `criterion_value`
- `criterion_operator`
- `units`
- `status`
- `message`
- `source_reference`
- `verification_status`

### 27.2 Example Output

| Check | Calculated | Criterion | Status |
|---|---:|---:|---|
| Minimum velocity | 3.2 ft/s | ≥ 2.0 ft/s | PASS |
| Maximum velocity | 3.2 ft/s | ≤ 8.0 ft/s | PASS |
| NPSH margin | 5.0 ft | ≥ 3.0 ft | PASS |
| Pump POR | 105% BEP | Manufacturer POR | PASS |
| Surge pressure | 145 psi | ≤ 150 psi | PASS |

All criteria and results are hypothetical examples.

### 27.3 Engineering Messages

Messages should be specific and actionable.

Example:

"Calculated force-main velocity exceeds the project maximum velocity criterion under SCN-004."

A message should identify the scenario and applicable criterion rather than simply state that a check failed.

---

## 28. HAS Software Architecture

### 28.1 Proposed Components

The future design-check framework may include:

**DesignCriteria**

Stores engineering acceptance requirements.

**DesignCheck**

Evaluates a specific engineering condition.

**DesignCheckResult**

Stores the evaluation outcome.

**DesignCheckEngine**

Coordinates applicable checks across hydraulic scenarios.

### 28.2 Integration

The design-check framework may consume results from:

- Pipe.
- Fitting.
- ForceMain.
- Pump.
- WetWell.
- OperatingScenario.
- TransientScreening.
- LiftStation.

### 28.3 Separation of Responsibilities

Hydraulic components should calculate engineering quantities.

Design-check components should evaluate those quantities against criteria.

Reporting components should communicate the results and limitations.

This separation improves maintainability and independent verification.

### 28.4 Future Development

Potential extensions include:

- Owner-specific criteria profiles.
- Automated scenario comparison.
- Engineering review dashboards.
- PDF and spreadsheet reporting.
- Sensitivity analysis.
- Monte Carlo design reliability analysis.
- Capital improvement alternative evaluation.

---

## 29. Verification Requirements

### 29.1 General

Design checks should be independently verified against known inputs and expected results.

### 29.2 Boundary Conditions

Each check should be tested at:

1. Values below the criterion.
2. Values equal to the criterion.
3. Values above the criterion.
4. Missing values.
5. Invalid values.
6. Unit conversion boundaries.

### 29.3 Recommended Tests

**Velocity**

- Minimum velocity satisfied.
- Minimum velocity violated.
- Maximum velocity satisfied.
- Maximum velocity violated.
- Invalid pipe diameter.
- Zero flow.

**NPSH**

- Positive margin.
- Zero margin.
- Negative margin.
- Insufficient margin.
- Missing manufacturer NPSHR.
- Invalid vapor pressure data.

**Pump Operation**

- Within POR.
- Outside POR but within AOR.
- Outside AOR.
- Below minimum continuous flow.
- Above maximum allowable flow.
- Motor overload.
- Insufficient firm capacity.

**Surge Screening**

- Pressure within applicable limit.
- Pressure above applicable limit.
- Subatmospheric pressure.
- Vapor-pressure threshold.
- Invalid wave speed.
- Missing pressure rating.
- Incomplete pipeline profile.

### 29.4 Acceptance Criteria

Numerical tolerances and verification methods should be documented in the HAS Verification Plan.

---

## 30. Limitations and Engineering Judgment

The design-check framework evaluates calculated results against specified criteria.

It does not independently establish:

- Regulatory compliance.
- Final design adequacy.
- Manufacturer approval.
- Structural integrity.
- Complete hydraulic transient safety.
- Operational reliability.
- Constructability.

A PASS indicates compliance with the specific evaluated criterion only.

A WARNING identifies a condition requiring review.

A FAIL identifies a violation of a specified criterion.

A NOT_EVALUATED result identifies an incomplete evaluation.

Final engineering decisions remain dependent on applicable standards, owner requirements, manufacturer guidance, and professional engineering judgment.

---

## 31. Engineering References

Relevant references include:

1. Hydraulic Institute standards and pump application guidance.
2. Water Environment Federation wastewater pumping station design publications.
3. American Water Works Association pipe design manuals and applicable standards.
4. Florida Department of Environmental Protection wastewater collection and transmission requirements.
5. Pump manufacturer performance and installation documentation.
6. Pipe and valve manufacturer pressure rating documentation.
7. Recognized fluid mechanics and hydraulic engineering references.
8. HAS engineering design basis.
9. HAS force-main hydraulics documentation.
10. HAS pump selection documentation.
11. HAS wet-well design documentation.
12. HAS operating scenarios documentation.
13. HAS transient screening documentation.

Applicable editions and project-specific requirements should be confirmed before formal engineering use.

---

## 32. Summary

The HAS design-check framework establishes a consistent methodology for evaluating hydraulic calculations against documented engineering criteria.

The initial framework should:

1. Evaluate minimum and maximum force-main velocity.
2. Evaluate NPSH availability and required margin.
3. Evaluate pump operating range.
4. Evaluate minimum and maximum pump flow.
5. Evaluate motor loading.
6. Evaluate pumping capacity and redundancy.
7. Evaluate pump cycling.
8. Screen positive and negative transient pressures.
9. Identify potential vapor-pressure conditions.
10. Evaluate relevant operating scenarios.
11. Record criterion sources and assumptions.
12. Return consistent PASS, WARNING, FAIL, NOT_EVALUATED, and NOT_APPLICABLE statuses.
13. Support independent verification and engineering review.

The design-check system should remain separate from the underlying hydraulic calculation modules.

This architecture will allow HAS to evaluate the same hydraulic model against different project requirements while maintaining traceability, consistency, and engineering transparency.
