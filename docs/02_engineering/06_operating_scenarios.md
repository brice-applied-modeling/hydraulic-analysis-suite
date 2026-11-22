# Operating Scenarios and Lift Station Performance

## 1. Purpose and Scope

This document establishes the engineering methodology, operating conditions, scenario definitions, and verification requirements for evaluating lift station and force-main performance within the Hydraulic Analysis Suite (HAS).

Operating scenario analysis allows engineers to evaluate how changes in hydraulic conditions, pump availability, system configuration, and operating controls affect lift station performance.

The methodology is intended to support:

- Single-pump operation.
- Multiple-pump operation.
- Minimum and maximum wet-well elevations.
- Variable downstream hydraulic grade line (HGL).
- Minimum, average, and peak wastewater inflow.
- Existing and future hydraulic conditions.
- Pump failure and reduced-capacity operation.
- Force-main configuration alternatives.
- Variable-speed pump operation.
- Pump cycling and wet-well storage.
- Preliminary energy and operational comparisons.

This document defines the engineering basis for future HAS functionality. It does not imply that all described capabilities have been implemented.

---

## 2. Operating Scenario Fundamentals

### 2.1 Definition

An operating scenario represents a defined combination of hydraulic boundary conditions, equipment availability, system configuration, and operating assumptions.

Each scenario should identify:

1. Hydraulic boundary conditions.
2. Wastewater inflow.
3. Available pumps.
4. Pump operating configuration.
5. Force-main configuration.
6. Pump control assumptions.
7. Applicable design criteria.
8. Required engineering outputs.

### 2.2 Scenario Analysis Objectives

Operating scenario analysis is intended to:

- Identify governing hydraulic conditions.
- Evaluate pumping capacity.
- Determine pump operating points.
- Assess system pressure.
- Evaluate operating flexibility.
- Identify hydraulic constraints.
- Compare alternative infrastructure configurations.
- Support preliminary engineering decisions.

### 2.3 Analysis Categories

HAS should distinguish between three primary analysis categories.

**Steady-state hydraulic scenarios**

Evaluate hydraulic performance at a defined operating condition without explicitly simulating changes over time.

**Time-dependent operational simulations**

Evaluate changes in wet-well levels, pump status, and system conditions over time.

**Engineering alternatives analysis**

Compare different equipment, piping, or operating configurations using consistent performance criteria.

---

## 3. Scenario Inputs

### 3.1 Hydraulic Boundary Conditions

| Parameter | Symbol | Typical Unit |
|---|---|---|
| Wet-well water surface elevation | $WSEL$ | ft |
| Downstream hydraulic grade elevation | $HGL_d$ | ft |
| Static head | $H_{static}$ | ft |
| Wastewater inflow | $Q_{in}$ | gpm |
| Pump discharge flow | $Q_p$ | gpm |
| Fluid temperature | $T$ | °F |
| Fluid specific gravity | $SG$ | Dimensionless |

### 3.2 Pump Configuration

Scenario inputs should identify:

- Number of installed pumps.
- Number of available pumps.
- Number of operating pumps.
- Pump performance curves.
- Pump efficiency curves.
- Pump speed.
- Pump operating limitations.
- Pump control configuration.

### 3.3 Force-Main Configuration

Scenario inputs should identify:

- Pipe lengths.
- Actual internal diameters.
- Pipe materials.
- Pipe roughness.
- Fittings.
- Valves.
- Pipeline elevations.
- Available parallel force mains.
- Open or closed flow paths.

### 3.4 Scenario Metadata

Each scenario should include:

- Scenario identifier.
- Scenario name.
- Description.
- Design condition.
- Data sources.
- Engineering assumptions.
- Calculation method.
- Verification status.

---

## 4. Hydraulic Operating Scenarios

### 4.1 Minimum Wet-Well Elevation

The minimum wet-well elevation generally produces a higher static head requirement when downstream conditions remain constant.

For simplified reservoir-to-reservoir conditions:

$$
H_{static} = HGL_d-WSEL
$$

As $WSEL$ decreases, $H_{static}$ increases.

This scenario may be important for evaluating:

- Maximum required pumping head.
- Reduced pump discharge capacity.
- Pump operating range.
- Minimum submergence.
- Pump power requirements.

Maximum required head does not necessarily correspond to maximum pump power.

### 4.2 Maximum Wet-Well Elevation

A higher wet-well elevation generally reduces the static head requirement.

This may result in:

- Increased pump discharge flow.
- Reduced system head.
- Movement along the pump performance curve.
- Increased force-main velocity.
- Potential pump runout conditions.

Pump power must be evaluated using the actual pump performance characteristics.

### 4.3 Minimum Downstream HGL

A minimum downstream HGL generally reduces required static head.

Potential consequences include:

- Increased pump flow.
- Increased pipeline velocity.
- Increased friction losses.
- Movement toward higher-flow operating conditions.

### 4.4 Maximum Downstream HGL

A maximum downstream HGL generally increases required static head.

Potential consequences include:

- Reduced pump flow.
- Increased required pump head.
- Reduced available pumping capacity.
- Potential inability to satisfy required operating conditions.

### 4.5 Combined Boundary Conditions

Individual boundary conditions should not always be evaluated independently.

Relevant combinations may include:

| Scenario | Wet-Well Elevation | Downstream HGL | General Purpose |
|---|---|---|---|
| S-01 | Minimum | Maximum | Evaluate high-head conditions |
| S-02 | Maximum | Minimum | Evaluate potential high-flow conditions |
| S-03 | Normal | Normal | Evaluate typical operation |
| S-04 | Minimum | Minimum | Evaluate alternative operating condition |
| S-05 | Maximum | Maximum | Evaluate alternative operating condition |

These scenarios are illustrative. The governing condition depends on the actual hydraulic system and pump performance.

---

## 5. Wastewater Inflow Scenarios

### 5.1 Minimum Inflow

Minimum wastewater inflow conditions may be important for:

- Pump cycling.
- Minimum run time.
- Wet-well detention.
- Odor and septicity concerns.
- Operational efficiency.

### 5.2 Average Inflow

Average inflow may be used to evaluate:

- Typical pump operation.
- Daily pumping duration.
- Preliminary energy consumption.
- Operating costs.
- Normal wet-well cycling.

### 5.3 Peak Inflow

Peak inflow scenarios are used to evaluate whether the station can accommodate elevated wastewater loading.

Relevant considerations include:

- Peak hourly flow.
- Wet-weather inflow and infiltration.
- Required pumping capacity.
- Available operating storage.
- Lag pump operation.
- High-level alarms.

### 5.4 Inflow and Pump Capacity

The wet-well volume balance is:

$$
\frac{dV}{dt}=Q_{in}-Q_{out}
$$

Where:

- $V$ = stored wastewater volume.
- $Q_{in}$ = incoming wastewater flow.
- $Q_{out}$ = pumped wastewater flow.

When:

$$
Q_{in}>Q_{out}
$$

Wet-well storage increases.

When:

$$
Q_{in}<Q_{out}
$$

Wet-well storage decreases.

When:

$$
Q_{in}=Q_{out}
$$

Wet-well volume remains constant under the assumed conditions.

---

## 6. Single-Pump Operation

### 6.1 General

Single-pump scenarios evaluate the hydraulic performance of one operating pump against the system head curve.

### 6.2 Pump Operating Point

The operating point satisfies:

$$
H_{pump}(Q)=H_{system}(Q)
$$

Where:

- $H_{pump}$ = pump-developed head.
- $H_{system}$ = required system head.
- $Q$ = operating flow.

### 6.3 Required Outputs

Single-pump analysis should report:

- Operating flow.
- Total dynamic head.
- Pump efficiency, where available.
- Pump brake horsepower.
- Motor power requirements, where applicable.
- Force-main velocity.
- Major and minor losses.
- Pump operating range compliance.
- Relevant pressure conditions.

### 6.4 Operating Limitations

The analysis should identify conditions where the calculated operating point falls outside the manufacturer's allowable operating range.

---

## 7. Multiple-Pump Operation

### 7.1 General

Multiple-pump scenarios evaluate how two or more pumps interact with a common hydraulic system.

### 7.2 Parallel Pumps

For pumps operating in parallel, the combined flow at a common developed head is:

$$
Q_{total}(H)=\sum_{i=1}^{n}Q_i(H)
$$

Where:

- $Q_{total}$ = combined pump flow.
- $Q_i$ = flow from pump $i$ at the specified head.

This relationship assumes a compatible common-head representation. Individual branch losses must be included where applicable.

### 7.3 Combined Pump Curve

The combined pump curve is evaluated against the system head curve.

The operating point satisfies:

$$
H_{combined}(Q)=H_{system}(Q)
$$

### 7.4 Flow Distribution

Where pumps have different performance curves or branch losses, individual pump flows may differ.

HAS should not assume equal flow distribution unless the hydraulic configuration and pump characteristics justify that assumption.

### 7.5 Pump Redundancy

Operating scenarios should distinguish:

- Installed capacity.
- Available capacity.
- Firm capacity.
- Required design capacity.

Firm capacity is commonly evaluated with the largest pumping unit unavailable, subject to applicable owner and regulatory definitions.

---

## 8. Pump Failure and Reduced-Capacity Scenarios

### 8.1 Single Pump Unavailable

A pump-out-of-service scenario evaluates system performance when one pumping unit is unavailable.

Potential causes include:

- Mechanical failure.
- Electrical failure.
- Planned maintenance.
- Control system failure.

### 8.2 Firm Capacity

For a station with multiple pumps, firm capacity may be evaluated with the largest pump unavailable.

The remaining pumps must be evaluated against the actual system curve.

Installed pump nameplate capacities should not be added without accounting for hydraulic interactions.

### 8.3 Reduced Pump Performance

Pump degradation scenarios may evaluate:

- Reduced developed head.
- Reduced efficiency.
- Impeller wear.
- Increased internal clearances.
- Reduced pump speed.

Performance changes should be based on documented assumptions or manufacturer data.

### 8.4 Reduced Force-Main Capacity

Scenarios may also evaluate:

- Increased pipe roughness.
- Partial restrictions.
- Valve throttling.
- Reduced effective diameter.
- Additional hydraulic losses.

These conditions may increase required system head and reduce available flow.

---

## 9. Variable-Speed Pump Operation

### 9.1 General

Variable-frequency drives (VFDs) allow pump speed to change in response to operating conditions.

### 9.2 Pump Affinity Laws

For the same pump operating under dynamically similar conditions:

$$
\frac{Q_2}{Q_1}=\frac{N_2}{N_1}
$$

$$
\frac{H_2}{H_1}=\left(\frac{N_2}{N_1}\right)^2
$$

$$
\frac{P_2}{P_1}=\left(\frac{N_2}{N_1}\right)^3
$$

Where:

- $Q$ = flow.
- $H$ = head.
- $P$ = pump shaft power.
- $N$ = rotational speed.

These relationships are approximations subject to pump similarity and efficiency considerations.

### 9.3 Variable-Speed Scenarios

Potential scenarios include:

- Minimum allowable pump speed.
- Normal operating speed.
- Maximum operating speed.
- Constant wet-well level control.
- Flow-based speed control.
- Energy optimization.

### 9.4 Operating Limitations

Variable-speed operation should be evaluated against:

- Manufacturer speed limits.
- Minimum continuous flow.
- Motor cooling requirements.
- Pump efficiency.
- Available NPSH.
- Hydraulic system characteristics.

---

## 10. Force-Main Configuration Scenarios

### 10.1 Existing Force Main

The existing configuration establishes the baseline for hydraulic comparison.

### 10.2 Alternative Pipe Diameters

Alternative pipe diameters may be evaluated to compare:

- Velocity.
- Friction losses.
- Total dynamic head.
- Pump operating point.
- Energy consumption.
- Preliminary capital cost.

### 10.3 Alternative Pipe Materials

Material scenarios may evaluate differences in:

- Actual internal diameter.
- Pipe roughness.
- Pressure class.
- Hydraulic losses.
- Long-term operating assumptions.

### 10.4 Parallel Force Mains

Where multiple force mains operate in parallel, flow distribution depends on the hydraulic resistance of each path.

The model must satisfy continuity and energy compatibility across the parallel branches.

### 10.5 Valve and Fitting Configurations

Alternative configurations may evaluate:

- Valve selection.
- Fitting arrangements.
- Additional bends.
- Reducers.
- Check valves.
- Isolation arrangements.

---

## 11. Existing and Future Conditions

### 11.1 Existing Conditions

Existing conditions should be based on available information such as:

- As-built drawings.
- Pump performance data.
- Operating records.
- Flow measurements.
- Pressure measurements.
- Wet-well operating levels.

### 11.2 Future Conditions

Future scenarios may include:

- Increased wastewater flow.
- New service areas.
- Additional pumping stations.
- Force-main extensions.
- Changes in downstream HGL.
- Pump replacements.
- System rehabilitation.

### 11.3 Alternative Comparison

Alternatives should be evaluated using consistent hydraulic assumptions and performance criteria.

---

## 12. Time-Dependent Operational Simulation

### 12.1 General

Time-dependent simulation evaluates changes in wet-well storage, pump operation, and hydraulic conditions over time.

This differs from a single steady-state hydraulic calculation.

### 12.2 Wet-Well Volume Balance

$$
\frac{dV}{dt}=Q_{in}(t)-Q_{out}(t)
$$

For a constant-area wet well:

$$
A\frac{dh}{dt}=Q_{in}(t)-Q_{out}(t)
$$

Where:

- $A$ = wet-well plan area.
- $h$ = water surface elevation.
- $t$ = time.

Consistent units are required.

### 12.3 Pump Control Logic

Potential control inputs include:

- Lead pump start elevation.
- Lead pump stop elevation.
- Lag pump start elevation.
- Lag pump stop elevation.
- High-level alarm.
- Minimum run time.
- Minimum off time.
- Pump alternation.

### 12.4 Quasi-Steady Hydraulic Evaluation

For each operating time step, HAS may evaluate the pump and force-main hydraulics using steady-state equations.

The resulting pump discharge is then used to update wet-well storage.

This approach is a quasi-steady operational simulation and does not represent a full hydraulic transient model.

### 12.5 Simulation Outputs

Potential outputs include:

- Wet-well level versus time.
- Pump operating status.
- Pump starts.
- Pump run hours.
- Pump discharge flow.
- Total pumped volume.
- High-level alarm events.
- Estimated energy consumption.

---

## 13. Energy and Operating Cost Scenarios

### 13.1 Pump Hydraulic Power

Hydraulic power may be calculated as:

$$
HP_{hydraulic}=\frac{QHSG}{3960}
$$

Where:

- $Q$ = flow (gpm).
- $H$ = pump head (ft).
- $SG$ = specific gravity.

### 13.2 Pump Brake Horsepower

$$
BHP=\frac{QHSG}{3960\eta_p}
$$

Where $\eta_p$ is pump efficiency expressed as a decimal.

### 13.3 Electrical Power

For a motor with efficiency $\eta_m$:

$$
P_{electric}
=
\frac{0.7457BHP}{\eta_m}
$$

Where $P_{electric}$ is expressed in kW.

Additional drive losses should be included where applicable.

### 13.4 Energy Consumption

For constant electrical power:

$$
E=P_{electric}t
$$

Where:

- $E$ = energy consumption (kWh).
- $P_{electric}$ = electrical power (kW).
- $t$ = operating duration (hr).

For variable power:

$$
E=\int P_{electric}(t)\,dt
$$

With time expressed in hours.

### 13.5 Operating Cost

A simplified energy cost estimate is:

$$
C_{energy}=E \times R
$$

Where:

- $C_{energy}$ = energy cost.
- $E$ = electrical energy (kWh).
- $R$ = electricity rate ($/kWh).

Actual utility charges may include demand charges, time-of-use rates, and other adjustments.

---

## 14. Scenario Comparison Framework

### 14.1 Common Evaluation Metrics

| Metric | Typical Unit |
|---|---|
| Pump operating flow | gpm |
| Total dynamic head | ft |
| Force-main velocity | ft/s |
| Friction head loss | ft |
| Minor head loss | ft |
| Minimum operating pressure | psi |
| Maximum operating pressure | psi |
| Pump efficiency | % |
| Pump brake horsepower | hp |
| Electrical power | kW |
| Energy consumption | kWh |
| Pump starts | starts/hr |
| Pump operating duration | hr |
| Wet-well operating range | ft |
| Available firm capacity | gpm |

### 14.2 Scenario Comparison Requirements

Each scenario should use:

- Consistent units.
- Documented assumptions.
- Traceable input data.
- Equivalent calculation methods.
- Clearly identified operating conditions.

### 14.3 Governing Scenarios

The governing scenario may differ depending on the design criterion.

Examples include:

- Maximum required head.
- Maximum pump flow.
- Maximum electrical power.
- Minimum available pumping capacity.
- Maximum pressure.
- Maximum pump cycling frequency.

No single operating scenario should automatically be assumed to govern every design requirement.

---

## 15. Scenario Definition and Data Management

### 15.1 Scenario Identification

Each scenario should have a unique identifier.

Example:

`SCN-001`

### 15.2 Scenario Attributes

A future scenario data model may include:

- `scenario_id`
- `scenario_name`
- `description`
- `wet_well_level`
- `downstream_hgl`
- `inflow_rate`
- `active_pumps`
- `pump_speed`
- `force_main_configuration`
- `fluid_properties`
- `analysis_type`

### 15.3 Baseline and Alternatives

HAS should distinguish between:

- Baseline scenario.
- Modified operating scenario.
- Infrastructure alternative.
- Failure scenario.
- Future condition.

### 15.4 Reproducibility

Each scenario should retain the information needed to reproduce the calculation.

This may include:

- Input values.
- Units.
- Model version.
- Calculation method.
- Source references.
- Assumptions.
- Analysis results.

---

## 16. Engineering Verification

### 16.1 Steady-State Verification

Recommended verification cases include:

1. Single-pump operating point.
2. Two identical pumps in parallel.
3. Different pumps operating in parallel.
4. Minimum wet-well elevation.
5. Maximum wet-well elevation.
6. Minimum downstream HGL.
7. Maximum downstream HGL.
8. Increased pipe roughness.
9. Alternative pipe diameter.
10. Reduced pump performance.

### 16.2 Operational Simulation Verification

Recommended cases include:

1. Constant inflow and constant pump capacity.
2. Pump cycling between fixed elevations.
3. Inflow exceeding pump capacity.
4. Lead/lag pump operation.
5. Pump failure.
6. Variable inflow.
7. Minimum run-time controls.
8. Minimum off-time controls.
9. Pump alternation.
10. High-level alarm conditions.

### 16.3 Independent Comparison

Verification may include:

- Analytical solutions.
- Hand calculations.
- Spreadsheet models.
- EPANET.
- WaterGEMS.
- SewerGEMS.
- Manufacturer performance data.

### 16.4 Verification Documentation

Each verification case should document:

- Scenario identifier.
- Input data.
- Boundary conditions.
- Assumptions.
- Expected results.
- HAS results.
- Numerical differences.
- Acceptance criteria.
- Verification status.

---

## 17. HAS Software Architecture

### 17.1 Scenario Model

A future `OperatingScenario` class may define the conditions under which an analysis is performed.

### 17.2 Hydraulic Components

The scenario model may interact with:

**Pipe**

Calculates pipe geometry, velocity, friction factor, and major losses.

**Fitting**

Calculates minor hydraulic losses.

**ForceMain**

Calculates system hydraulic requirements and system head curves.

**Pump**

Represents pump performance curves and operating limitations.

**WetWell**

Represents wet-well geometry, storage, and operating elevations.

**PumpControl**

Defines pump start/stop logic and sequencing.

**LiftStation**

Integrates hydraulic components and operating controls.

### 17.3 Scenario Analysis Engine

A future scenario analysis engine may:

1. Load scenario inputs.
2. Validate hydraulic conditions.
3. Establish available pumps.
4. Construct the applicable system head curve.
5. Determine pump operating points.
6. Evaluate hydraulic performance.
7. Calculate applicable operating metrics.
8. Generate scenario results.
9. Compare alternatives.

### 17.4 Future Analytics Integration

Potential future capabilities include:

- Sensitivity analysis.
- Monte Carlo simulation.
- Lifecycle cost analysis.
- Capital improvement optimization.
- Reliability analysis.
- Decision-support dashboards.

These capabilities are outside the initial HAS MVP unless separately incorporated into the development roadmap.

---

## 18. Limitations and Engineering Judgment

Operating scenario analysis is dependent on the quality and applicability of the input data.

Important limitations include:

- Uncertainty in pump performance curves.
- Uncertainty in pipe roughness.
- Changes in wastewater characteristics.
- Variable downstream hydraulic conditions.
- Incomplete field operating data.
- Simplified control assumptions.
- Differences between modeled and actual system behavior.

Steady-state analysis does not independently evaluate hydraulic transients.

Operational simulation using steady-state hydraulic calculations at successive time steps should not be interpreted as a water-hammer model.

Final engineering decisions must consider applicable design criteria, manufacturer requirements, and professional engineering judgment.

---

## 19. Engineering References

Relevant references include:

1. Hydraulic Institute standards and technical guidelines.
2. Water Environment Federation wastewater pumping station design publications.
3. American Water Works Association manuals and applicable standards.
4. U.S. EPA wastewater pumping station technical guidance.
5. Florida Department of Environmental Protection regulations applicable to wastewater collection and transmission facilities.
6. Pump manufacturer performance and installation manuals.
7. Recognized hydraulic engineering and fluid mechanics textbooks.
8. EPANET documentation and technical references.

Current editions and applicability should be confirmed before use in formal engineering design.

---

## 20. Summary

Operating scenario analysis provides a structured methodology for evaluating lift station performance under changing hydraulic and operational conditions.

The primary engineering requirements are:

1. Define hydraulic boundary conditions.
2. Identify available and operating pumps.
3. Evaluate pump and system curve interactions.
4. Assess single-pump and multiple-pump operation.
5. Evaluate minimum and maximum hydraulic conditions.
6. Assess pump failure and reduced-capacity scenarios.
7. Compare existing and future system configurations.
8. Distinguish steady-state scenarios from time-dependent simulations.
9. Evaluate relevant energy and operating metrics.
10. Independently verify calculations.
11. Document assumptions, limitations, and scenario results.

These principles establish the foundation for future operating scenario analysis and integrated lift station performance modeling within HAS.
