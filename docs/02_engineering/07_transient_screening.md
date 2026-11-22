# Transient Screening and Surge Risk Assessment

## 1. Purpose and Scope

This document establishes the engineering basis for preliminary hydraulic transient screening within the Hydraulic Analysis Suite (HAS).

Transient screening is intended to identify potential pressure surge hazards associated with rapid changes in flow within pressurized piping systems, particularly municipal wastewater force mains and lift station discharge systems.

The methodology supports preliminary evaluation of:

- Pump startup and shutdown.
- Sudden power failure and pump trip.
- Rapid valve closure or opening.
- Check valve closure and potential slam.
- Positive and negative pressure excursions.
- Potential subatmospheric pressure and vapor-pressure conditions.
- Pipe pressure rating concerns.
- Force-main profile vulnerability.
- Conditions warranting detailed hydraulic transient analysis.

**Transient screening is not a substitute for a detailed surge analysis.**

The initial HAS implementation should use simplified analytical relationships to identify risks and support engineering decisions about additional investigation.

It should not attempt to predict the complete time-dependent pressure response of a pumping system.

---

## 2. Hydraulic Transient Fundamentals

### 2.1 Definition

A hydraulic transient is a time-dependent change in pressure and flow caused by a disturbance to the hydraulic system.

Unlike steady-state flow, transient conditions involve pressure waves propagating through the fluid and piping system.

These waves may reflect from boundaries and interact with other pressure disturbances.

### 2.2 Common Causes

Hydraulic transients may result from:

1. Pump shutdown.
2. Pump startup.
3. Sudden loss of electrical power.
4. Rapid valve closure.
5. Rapid valve opening.
6. Check valve closure.
7. Flow reversal.
8. Changes in downstream boundary conditions.
9. Air pocket movement or release.
10. Rapid changes in system operating configuration.

### 2.3 Potential Consequences

Hydraulic transients may produce:

- Excessive internal pressure.
- Subatmospheric pressure.
- Vapor cavity formation.
- Column separation and subsequent rejoining.
- Pipe or fitting failure.
- Joint separation.
- Damage to valves and pumps.
- Structural movement or thrust-related damage.
- Air entrainment and unstable operation.

The severity depends on the system configuration, operating conditions, material properties, and disturbance characteristics.

---

## 3. Relationship to Steady-State Hydraulics

### 3.1 Steady-State Conditions

Under steady-state conditions, hydraulic analysis assumes flow and pressure remain constant with time for the evaluated operating condition.

For a force main:

$$
H_{pump}(Q)=H_{system}(Q)
$$

This relationship establishes the pump operating point.

### 3.2 Transient Conditions

During a transient event:

$$
Q=Q(t)
$$

$$
P=P(x,t)
$$

Where:

- $Q$ = flow rate.
- $P$ = pressure.
- $x$ = position along the pipeline.
- $t$ = time.

Pressure and flow may vary with both location and time.

### 3.3 HAS Screening Approach

HAS should use steady-state hydraulic results to establish initial conditions for transient screening.

These may include:

- Initial flow rate.
- Initial velocity.
- Initial pressure.
- Initial HGL.
- Pipe geometry.
- Pipeline profile.
- Pump operating conditions.

Simplified transient equations can then be used to estimate potential pressure changes.

The resulting estimates should be reported separately from verified steady-state results.

---

## 4. Transient Screening Inputs

### 4.1 Hydraulic Inputs

| Parameter | Symbol | Typical Unit |
|---|---|---|
| Initial flow | $Q_0$ | gpm |
| Initial velocity | $V_0$ | ft/s |
| Final assumed velocity | $V_1$ | ft/s |
| Velocity change | $\Delta V$ | ft/s |
| Initial pressure | $P_0$ | psi |
| Initial hydraulic grade | $HGL_0$ | ft |
| Fluid density | $\rho$ | slug/ft³ |
| Fluid specific weight | $\gamma$ | lbf/ft³ |

### 4.2 Pipeline Inputs

| Parameter | Symbol | Typical Unit |
|---|---|---|
| Pipe length | $L$ | ft |
| Internal diameter | $D$ | ft |
| Pipe wall thickness | $e$ | ft |
| Pipe elastic modulus | $E$ | psi |
| Fluid bulk modulus | $K$ | psi |
| Wave speed | $a$ | ft/s |
| Pipe pressure rating | $P_{rated}$ | psi |

### 4.3 Operating Inputs

Additional inputs may include:

- Pump operating status.
- Pump shutdown characteristics.
- Valve closure time.
- Valve operating characteristics.
- Check valve type.
- Downstream hydraulic boundary.
- Pipeline elevation profile.
- Air and vacuum valve locations.
- Existing surge protection equipment.

### 4.4 Data Requirements

Each input should identify its source.

Acceptable sources may include:

- Manufacturer documentation.
- As-built drawings.
- Engineering calculations.
- Field measurements.
- Recognized engineering references.
- Explicitly documented preliminary assumptions.

Material properties and wave speeds should not be assigned without identifying their basis.

---

## 5. Pressure Wave Velocity

### 5.1 General

Pressure disturbances propagate through a pressurized pipeline at a wave speed dependent on:

- Fluid compressibility.
- Pipe wall elasticity.
- Pipe diameter.
- Pipe wall thickness.
- Pipe restraint conditions.
- Pipe material.
- Fluid properties.

### 5.2 Simplified Wave Speed Equation

A commonly used elastic-pipe relationship is:

$$
a=
\sqrt{
\frac{K/\rho}
{1+\frac{KD}{Ee}C}
}
$$

Where:

- $a$ = pressure wave speed (ft/s).
- $K$ = fluid bulk modulus.
- $\rho$ = fluid mass density.
- $D$ = pipe internal diameter.
- $E$ = pipe elastic modulus.
- $e$ = pipe wall thickness.
- $C$ = dimensionless restraint factor.

All quantities must use dimensionally consistent units.

For example, if $K$ and $E$ are entered in psi, their ratio is dimensionless, but $K/\rho$ must be converted to compatible units before calculating wave speed.

### 5.3 Restraint Factor

The restraint factor depends on the pipe's longitudinal restraint and elastic behavior.

HAS should not assume a universal value of $C$.

The appropriate relationship should be selected from a recognized reference consistent with the pipe material, wall geometry, and restraint conditions.

### 5.4 Wave Speed Uncertainty

Actual wave speed may differ from theoretical estimates because of:

- Pipe restraint.
- Material variability.
- Air or gas content.
- Pipe wall properties.
- Temperature.
- Fluid characteristics.

Entrained or entrapped gas can substantially alter effective wave speed and transient behavior.

### 5.5 Initial HAS Implementation

The initial module should permit:

1. User-specified wave speed with documented source.
2. Optional calculated wave speed when sufficient verified material information is available.
3. Sensitivity analysis over an explicitly defined wave-speed range.

User-specified wave speed should be the simplest initial implementation.

---

## 6. Joukowsky Pressure Change

### 6.1 Governing Equation

The Joukowsky equation estimates the pressure change associated with a rapid change in fluid velocity:

$$
\Delta P=-\rho a\Delta V
$$

Where:

- $\Delta P$ = pressure change.
- $\rho$ = fluid mass density.
- $a$ = pressure wave speed.
- $\Delta V$ = signed velocity change.

The equation must be evaluated with consistent units.

### 6.2 Pressure Head Form

The corresponding change in pressure head is:

$$
\Delta H=-\frac{a\Delta V}{g}
$$

For screening the magnitude of the change:

$$
|\Delta H|=\frac{a|\Delta V|}{g}
$$

Where:

- $\Delta H$ = pressure head change (ft).
- $g$ = gravitational acceleration (ft/s²).

### 6.3 Pressure Conversion

For a fluid with specific weight $\gamma$:

$$
|\Delta P_{psi}|=
\frac{\gamma|\Delta H|}{144}
$$

For water with an assumed specific weight of 62.4 lbf/ft³:

$$
|\Delta P_{psi}|\approx0.4333|\Delta H|
$$

### 6.4 Assumptions

The simplified Joukowsky relationship assumes an idealized rapid velocity change and one-dimensional elastic wave propagation.

It does not independently account for:

- Complex wave reflections.
- Pump rotational inertia.
- Nonlinear valve characteristics.
- Column separation.
- Air pocket behavior.
- Cavitation.
- Time-dependent friction.
- Complex network interactions.

### 6.5 Important Limitation

The Joukowsky estimate is not necessarily the maximum pressure that can occur during a real event.

Wave reflections, check valve slam, cavity collapse, and other system interactions may produce more severe conditions.

HAS should not label the calculated result as the guaranteed maximum surge pressure.

---

## 7. Critical Wave Travel Time

### 7.1 General

A pressure wave travels through the pipeline and reflects from hydraulic boundaries.

The approximate round-trip wave travel time is:

$$
T_r=\frac{2L}{a}
$$

Where:

- $T_r$ = round-trip wave travel time (s).
- $L$ = hydraulic path length to the relevant reflection boundary (ft).
- $a$ = wave speed (ft/s).

### 7.2 Interpretation

For a simple pipeline and rapid valve closure:

- A closure completed in less than or approximately equal to $T_r$ may produce a full Joukowsky-type pressure change.
- A slower closure may reduce the initial pressure rise.
- Actual results depend on valve characteristics and system configuration.

### 7.3 Closure Time Ratio

Define:

$$
R_t=\frac{T_c}{T_r}
$$

Where:

- $T_c$ = valve closure time.
- $T_r$ = round-trip wave travel time.

This dimensionless ratio may support preliminary classification of valve operating speed.

### 7.4 Limitations

The ratio does not independently determine transient safety.

Pump trip behavior cannot generally be represented using valve closure time alone.

For complex systems, the relevant reflection boundary may not be obvious.

---

## 8. Pump Trip Screening

### 8.1 General

Sudden pump shutdown is a major transient consideration for wastewater force mains.

A pump trip may result from:

- Electrical power failure.
- Motor protection shutdown.
- Control system failure.
- Mechanical malfunction.

### 8.2 Initial Hydraulic Response

Following a pump trip, the pump-developed head decreases while the fluid continues moving.

The initial disturbance on the discharge side may be a negative pressure wave.

The subsequent response depends on:

- Pump and motor inertia.
- System static head.
- Force-main profile.
- Check valve behavior.
- Potential flow reversal.
- Wave reflections.
- Surge protection devices.

### 8.3 Screening Approach

The initial HAS pump trip screening should:

1. Establish the pre-trip operating point.
2. Calculate initial pipeline velocity.
3. Establish an appropriate wave speed.
4. Calculate the magnitude of a hypothetical rapid velocity change.
5. Evaluate a negative pressure-head perturbation.
6. Compare the resulting screening HGL against the pipeline profile.
7. Identify potential subatmospheric or vapor-pressure conditions.
8. Flag the need for detailed transient analysis.

### 8.4 Limitations

The simplified screening method does not calculate actual pump rundown.

It does not model:

- Pump rotational inertia.
- Four-quadrant pump performance.
- Reverse flow.
- Check valve dynamic closure.
- Column separation.
- Subsequent pressure wave interactions.

These require more advanced transient modeling.

---

## 9. Valve Closure Screening

### 9.1 General

Rapid valve closure can generate positive pressure waves upstream of the closing valve and negative pressure waves downstream.

### 9.2 Screening Method

For a simplified single-pipeline configuration:

1. Determine initial flow velocity.
2. Determine the assumed velocity change.
3. Establish wave speed.
4. Calculate round-trip wave travel time.
5. Compare closure time with wave travel time.
6. Estimate the rapid-change pressure magnitude.
7. Compare the result with component pressure limitations.

### 9.3 Valve Characteristics

Actual valve behavior depends on:

- Valve type.
- Closure profile.
- Actuator characteristics.
- Flow conditions.
- Valve position versus flow relationship.

A valve's mechanical closure time does not necessarily correspond to a uniform change in flow velocity.

### 9.4 Screening Limitations

HAS should not apply a generic slow-closure reduction factor without a documented, applicable engineering method.

If valve characteristics are unavailable, the screening should identify the uncertainty rather than calculate a falsely precise pressure reduction.

---

## 10. Check Valve Slam

### 10.1 General

Check valve slam may occur when reverse flow develops before the check valve closes.

Rapid interruption of reverse flow can generate a significant positive pressure surge.

### 10.2 Relevant Factors

Check valve behavior depends on:

- Valve type.
- Disc or closure-element inertia.
- Spring assistance.
- Reverse flow velocity.
- System deceleration.
- Installation configuration.

### 10.3 Screening Requirements

HAS should identify:

- Presence of check valves.
- Valve type.
- Available manufacturer closure characteristics.
- Potential for flow reversal.
- Whether dynamic check valve analysis is required.

### 10.4 Limitation

HAS should not estimate check valve slam pressure from forward-flow velocity alone and represent it as a verified prediction.

Reverse-flow conditions and valve closure dynamics must be evaluated using appropriate methods.

---

## 11. Positive Pressure Screening

### 11.1 General

Positive transient pressures may exceed normal operating pressures.

### 11.2 Preliminary Pressure Estimate

For a defined location and an assumed positive pressure-head perturbation:

$$
P_{screen,max}=P_{steady}+\frac{\gamma|\Delta H|}{144}
$$

Where:

- $P_{screen,max}$ = screening pressure (psig).
- $P_{steady}$ = initial steady-state pressure (psig).
- $\gamma$ = fluid specific weight (lbf/ft³).
- $\Delta H$ = assumed positive pressure-head change (ft).

### 11.3 Pressure Rating Comparison

The screening pressure should be compared against the applicable limits of:

- Pipe.
- Fittings.
- Valves.
- Couplings.
- Appurtenances.
- Other pressure-containing components.

### 11.4 Pressure Rating Considerations

Pressure ratings may depend on:

- Material.
- Temperature.
- Pressure class.
- Allowable surge provisions.
- Manufacturer limitations.
- Applicable standards.
- Installation and restraint conditions.

The pipe pressure class should not automatically be interpreted as the allowable maximum transient pressure for every component.

### 11.5 Screening Outcome

If the estimated pressure exceeds an applicable component limit, the scenario should be flagged for detailed review.

A result below the limit does not prove the system is safe from all transient conditions.

---

## 12. Negative Pressure Screening

### 12.1 General

Negative pressure waves may reduce the internal pipeline pressure below atmospheric pressure.

Potential consequences include:

- Air intrusion.
- Structural concerns.
- Vapor cavity formation.
- Column separation.
- Subsequent pressure surges.

### 12.2 Preliminary Pressure Estimate

For an assumed negative pressure-head perturbation:

$$
P_{screen,min}=P_{steady}-\frac{\gamma|\Delta H|}{144}
$$

This equation uses gauge pressure.

### 12.3 Absolute Pressure

Absolute pressure is:

$$
P_{absolute}=P_{gauge}+P_{atmospheric}
$$

For screening against vapor pressure, atmospheric pressure must be appropriate for the site elevation and conditions.

### 12.4 Vapor Pressure Comparison

Potential vapor-pressure conditions may be identified when:

$$
P_{absolute}\leq P_{vapor}
$$

Where $P_{vapor}$ is the absolute vapor pressure at the applicable fluid temperature.

### 12.5 Subatmospheric Pressure

A negative gauge pressure does not automatically mean the liquid has reached vapor pressure.

However, subatmospheric conditions may still create operational or structural concerns.

### 12.6 Limitations

The simplified calculation does not simulate vapor cavity formation, column separation, or cavity collapse.

These conditions should trigger detailed transient investigation.

---

## 13. Pipeline Profile Screening

### 13.1 General

Pipeline elevation profiles are particularly important for force mains with significant elevation changes.

High points may be vulnerable to low or negative pressure during transient events.

### 13.2 Steady-State Hydraulic Grade

At a defined location:

$$
HGL_{steady}(x)=z(x)+\frac{P_{steady}(x)}{\gamma}
$$

Where:

- $z(x)$ = pipeline elevation at location $x$.
- $P_{steady}(x)$ = local gauge pressure in compatible pressure units.

### 13.3 Preliminary Transient HGL

For a screening perturbation:

$$
HGL_{screen}(x)=HGL_{steady}(x)\pm|\Delta H|
$$

The positive and negative cases should be evaluated separately.

This is a hypothetical local perturbation, not a predicted simultaneous transient HGL profile.

### 13.4 Pressure Head

At a pipeline location:

$$
h_p(x)=HGL_{screen}(x)-z(x)
$$

### 13.5 Profile Risk Indicators

Potential concerns include:

- Screening HGL below the pipe crown.
- Negative gauge pressure.
- Absolute pressure approaching vapor pressure.
- High points with limited steady-state pressure margin.
- Long ascending force-main segments.
- Known air accumulation locations.

The hydraulic elevation reference must be consistent throughout the analysis.

---

## 14. Preliminary Transient Screening Example

### 14.1 Assumed Conditions

Consider a hypothetical wastewater force main with:

| Parameter | Value |
|---|---:|
| Initial flow velocity | 4.0 ft/s |
| Assumed wave speed | 1,500 ft/s |
| Pipeline length | 3,000 ft |
| Assumed velocity change | 4.0 ft/s |
| Initial local pressure | 65 psig |
| Fluid specific weight | 62.4 lbf/ft³ |
| Gravitational acceleration | 32.174 ft/s² |

The assumed wave speed is illustrative and is not a default value for a particular pipe material.

### 14.2 Joukowsky Head Change

$$
|\Delta H|=\frac{a|\Delta V|}{g}
$$

$$
|\Delta H|=\frac{1500(4.0)}{32.174}
$$

$$
|\Delta H|\approx186.5\ \mathrm{ft}
$$

### 14.3 Pressure Change

$$
|\Delta P|=\frac{62.4(186.5)}{144}
$$

$$
|\Delta P|\approx80.8\ \mathrm{psi}
$$

### 14.4 Round-Trip Wave Travel Time

$$
T_r=\frac{2L}{a}
$$

$$
T_r=\frac{2(3000)}{1500}
$$

$$
T_r=4.0\ \mathrm{s}
$$

### 14.5 Positive Pressure Screening

$$
P_{screen,max}=65+80.8
$$

$$
P_{screen,max}=145.8\ \mathrm{psig}
$$

### 14.6 Negative Pressure Screening

$$
P_{screen,min}=65-80.8
$$

$$
P_{screen,min}=-15.8\ \mathrm{psig}
$$

### 14.7 Engineering Interpretation

The idealized positive-pressure case indicates a potentially significant surge.

The negative-pressure estimate is below the approximate physical lower bound of gauge pressure corresponding to a near-vacuum condition at ordinary atmospheric pressure.

It should therefore be interpreted as an indication that the single-phase Joukowsky assumptions are no longer physically adequate.

It is **not** a valid prediction of a sustained -15.8 psig liquid pressure.

This screening case should be flagged for detailed transient analysis because vapor cavity formation or column separation may occur.

The example demonstrates why screening calculations should include explicit physical validity checks.

---

## 15. Transient Screening Scenarios

HAS should eventually support a defined screening matrix.

| Scenario | Description | Primary Concern |
|---|---|---|
| TS-01 | Sudden pump trip | Negative pressure |
| TS-02 | Rapid pump startup | Positive pressure |
| TS-03 | Rapid valve closure | Positive and negative waves |
| TS-04 | Rapid valve opening | Pressure redistribution |
| TS-05 | Check valve slam | Positive pressure |
| TS-06 | Minimum wet-well level | High-head initial condition |
| TS-07 | Maximum wet-well level | Potential high-flow initial condition |
| TS-08 | Maximum downstream HGL | Pump trip and high-head response |
| TS-09 | Maximum pump flow | Large velocity change |
| TS-10 | High-point profile screening | Subatmospheric pressure |
| TS-11 | Parallel pump trip | Combined flow disturbance |
| TS-12 | Reduced force-main capacity | Altered initial hydraulic conditions |

Not every scenario can be evaluated quantitatively using the initial simplified methodology.

Unsupported scenarios should be clearly identified rather than assigned misleading numerical results.

---

## 16. Screening Classification

### 16.1 Purpose

The screening process should identify conditions requiring further engineering review.

### 16.2 Suggested Classifications

**No immediate screening trigger identified**

The evaluated simplified checks do not identify an obvious exceedance.

This does not establish transient safety.

**Engineering review required**

The screening identifies uncertainty, limited pressure margin, or a potentially vulnerable system configuration.

**Detailed transient analysis recommended**

The screening identifies a significant pressure concern or a phenomenon that cannot be reliably evaluated using the simplified equations.

**Insufficient data**

The available information does not support a meaningful quantitative screening.

### 16.3 Automatic Review Triggers

Potential triggers include:

- Estimated positive pressure exceeding an applicable limit.
- Estimated absolute pressure reaching or falling below vapor pressure.
- Significant subatmospheric pressure risk.
- Known check valve slam concerns.
- High-point vulnerability.
- Potential column separation.
- Complex parallel force-main configurations.
- Multiple interacting pumps and valves.
- Existing surge-related operational problems.

These triggers should be configurable and traceable to applicable engineering criteria.

---

## 17. Recommended HAS Outputs

### 17.1 Calculation Results

The screening module should report:

- Initial flow.
- Initial velocity.
- Assumed velocity change.
- Wave speed.
- Wave-speed source.
- Round-trip wave travel time.
- Joukowsky head change.
- Joukowsky pressure change.
- Initial local pressure.
- Positive pressure screening estimate.
- Negative pressure screening estimate.
- Relevant pressure limits.
- Screening classification.

### 17.2 Engineering Warnings

Warnings should identify:

- Missing material information.
- Assumed wave speed.
- Unknown restraint conditions.
- Unknown valve characteristics.
- Unknown pump inertia.
- Potential vapor-pressure conditions.
- Unverified component pressure ratings.
- Incomplete pipeline profile.
- Applicability limitations.

### 17.3 Reporting

Each screening report should document:

1. Scenario identifier.
2. Engineering assumptions.
3. Input values and units.
4. Calculation methodology.
5. Reference sources.
6. Numerical results.
7. Physical validity checks.
8. Identified risks.
9. Recommended next steps.
10. Verification status.

---

## 18. Engineering Verification

### 18.1 Independent Calculations

Initial verification should include independent calculations for:

- Joukowsky head change.
- Pressure conversion.
- Round-trip wave travel time.
- Positive pressure screening.
- Negative pressure screening.
- Vapor-pressure comparison.
- Pipeline profile pressure.

### 18.2 Numerical Test Cases

Recommended verification cases include:

1. Zero velocity change.
2. Positive velocity change.
3. Negative velocity change.
4. Different assumed wave speeds.
5. Different pipeline lengths.
6. Different initial pressures.
7. Different fluid specific weights.
8. Pressure rating exceedance.
9. Subatmospheric pressure.
10. Vapor-pressure threshold.
11. Invalid negative wave speed.
12. Invalid pipe geometry.
13. Missing material data.
14. Inconsistent units.
15. Screening estimates outside physical applicability.

### 18.3 External Verification

Where suitable, calculations may be compared with:

- Independent hand calculations.
- Spreadsheet calculations.
- Published analytical examples.
- Recognized engineering references.
- Results from established transient analysis software.

Agreement with a simple analytical equation verifies the implementation of that equation, not the complete transient behavior of a real system.

### 18.4 Acceptance Criteria

Numerical tolerances and verification requirements should be established in the HAS Verification Plan.

---

## 19. HAS Software Architecture

### 19.1 Proposed Module

A future `TransientScreening` component may provide preliminary surge-risk calculations.

### 19.2 Dependencies

The module may interact with:

**Pipe**

Provides geometry, material properties, and pressure rating information.

**ForceMain**

Provides pipeline configuration, initial hydraulic conditions, and profile information.

**Pump**

Provides initial operating flow and pump performance data.

**OperatingScenario**

Defines the hydraulic conditions being evaluated.

**LiftStation**

Provides integrated system context.

### 19.3 Potential Functions

Possible calculation functions include:

- `calculate_wave_speed()`
- `calculate_wave_travel_time()`
- `calculate_joukowsky_head()`
- `calculate_joukowsky_pressure()`
- `screen_positive_pressure()`
- `screen_negative_pressure()`
- `screen_vapor_pressure()`
- `screen_pipeline_profile()`
- `classify_transient_risk()`

These names are conceptual and should be reconciled with the actual HAS architecture before implementation.

### 19.4 Separation of Responsibilities

The transient screening module should not duplicate the existing steady-state hydraulic calculations.

It should consume verified hydraulic results and perform additional screening calculations.

### 19.5 Future Development

Potential future extensions include:

- More advanced wave-speed models.
- Pipeline profile visualization.
- Sensitivity analysis.
- Automated screening reports.
- Comparison with external transient simulation results.

A full method-of-characteristics transient solver would represent a separate, substantially more complex development effort.

---

## 20. Limitations and Engineering Judgment

The initial HAS transient screening methodology is limited to preliminary analytical estimates.

It does not simulate:

- Full pressure wave propagation.
- Multiple wave reflections.
- Detailed pump rundown.
- Dynamic check valve closure.
- Flow reversal.
- Vapor cavity formation.
- Column separation and rejoining.
- Air pocket compression.
- Surge tank behavior.
- Air valve dynamics.
- Transient friction.
- Complex network interactions.

A screening calculation should not be used to certify that a force main is protected against hydraulic transients.

The absence of a screening exceedance does not eliminate the need for detailed transient analysis when system complexity, operating conditions, or applicable criteria warrant it.

---

## 21. Engineering References

Relevant references include:

1. American Water Works Association, applicable pipe design manuals addressing pressure surge and water hammer.
2. Hydraulic Institute, applicable pumping system and transient guidance.
3. U.S. Army Corps of Engineers, engineering manuals addressing wastewater pumping stations and hydraulic surge.
4. Pump and valve manufacturer technical documentation.
5. Recognized hydraulic transient analysis textbooks and technical publications.
6. Applicable pipe manufacturer pressure rating and surge allowance documentation.

Specific editions, equations, and material properties should be verified before incorporation into production engineering calculations.

### Selected Technical Sources

- U.S. Army Corps of Engineers, *EM 1110-3-173*, wastewater pumping station guidance: https://www.publications.usace.army.mil/Portals/76/Publications/EngineerManuals/EM_1110-3-173.pdf
- NASA, technical discussion of Joukowsky pressure change and elastic-pipe wave speed: https://ntrs.nasa.gov/api/citations/20210022275/downloads/20210022275.pdf
- AWWA Manual M11, *Steel Pipe: A Guide for Design and Installation*, water hammer and pressure surge discussion. Consult the applicable licensed edition.

---

## 22. Summary

Transient screening provides an initial engineering assessment of potential pressure surge hazards in wastewater force mains and lift station discharge systems.

The principal requirements are:

1. Establish verified steady-state hydraulic conditions.
2. Identify credible transient events.
3. Determine appropriate pressure wave speeds.
4. Estimate rapid-change pressure magnitudes.
5. Calculate wave travel times.
6. Evaluate positive pressure concerns.
7. Evaluate negative pressure concerns.
8. Assess pipeline profile vulnerability.
9. Identify potential vapor-pressure conditions.
10. Compare results against applicable component limitations.
11. Document assumptions and uncertainty.
12. Identify conditions requiring detailed transient analysis.

The initial HAS implementation should focus on transparent, independently verifiable screening calculations.

Its purpose is to help engineers recognize potential transient risks and determine when more sophisticated analysis is necessary, not to replace specialized hydraulic transient modeling software.
