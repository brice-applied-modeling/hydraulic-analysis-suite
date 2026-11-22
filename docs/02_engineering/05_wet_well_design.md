# Wet-Well Design and Pump Cycling Analysis

## 1. Purpose and Scope

This document establishes the engineering methodology, design assumptions, governing equations, and verification requirements for wet-well analysis within the Hydraulic Analysis Suite (HAS).

The purpose of the wet-well module is to support preliminary sizing and evaluation of municipal wastewater lift station wet wells, including:

- Wet-well geometry and storage capacity.
- Pump operating elevations.
- Pump cycling frequency.
- Minimum pump run time.
- Wastewater inflow conditions.
- Pump drawdown and refill time.
- Pump submergence.
- Emergency storage considerations.
- Integration with force-main and pump performance models.

This document establishes the engineering basis for future software development. It does not imply that the wet-well analysis module has been implemented.

---

## 2. Wet-Well Design Fundamentals

### 2.1 General

A wastewater wet well provides temporary storage of incoming wastewater and establishes the hydraulic conditions at the pump intake.

The wet well must accommodate variations in wastewater inflow while supporting reliable pump operation.

The design must balance competing objectives:

- Limit excessive pump starts.
- Maintain adequate pump run times.
- Minimize wastewater detention.
- Maintain adequate pump submergence.
- Avoid vortex formation and air entrainment.
- Provide appropriate operational flexibility.
- Accommodate peak wastewater inflows.

### 2.2 Wet-Well Functions

The primary functions of a wet well include:

1. Receiving wastewater from the collection system.
2. Providing hydraulic storage between pump cycles.
3. Establishing pump operating water levels.
4. Maintaining adequate pump intake conditions.
5. Supporting lead/lag pump operation.
6. Providing temporary storage during changing inflow conditions.

Emergency storage may be provided within the wet well or through other approved facilities, depending on applicable design requirements.

### 2.3 Wet-Well Design Types

Common configurations include:

- Circular wet wells.
- Rectangular wet wells.
- Submersible pump wet wells.
- Wet wells associated with dry-pit pumping stations.
- Duplex and triplex pumping arrangements.

The initial HAS methodology should support simple geometric configurations before expanding to complex wet-well shapes.

---

## 3. Design Inputs

### 3.1 Hydraulic Inputs

| Parameter | Symbol | Typical Unit |
|---|---|---|
| Average wastewater inflow | $Q_{avg}$ | gpm |
| Peak wastewater inflow | $Q_{peak}$ | gpm |
| Minimum wastewater inflow | $Q_{min}$ | gpm |
| Pump discharge capacity | $Q_p$ | gpm |
| Number of pumps | $N$ | count |
| Maximum allowable starts | $N_{max}$ | starts/hr |
| Minimum pump run time | $t_{run,min}$ | min |
| Minimum pump off time | $t_{off,min}$ | min |

### 3.2 Geometric Inputs

| Parameter | Symbol | Typical Unit |
|---|---|---|
| Wet-well diameter | $D$ | ft |
| Wet-well length | $L$ | ft |
| Wet-well width | $W$ | ft |
| Wet-well bottom elevation | $EL_{bottom}$ | ft |
| Pump-off elevation | $EL_{off}$ | ft |
| Pump-on elevation | $EL_{on}$ | ft |
| High-level alarm elevation | $EL_{alarm}$ | ft |
| Incoming sewer invert | $EL_{invert}$ | ft |

### 3.3 Manufacturer and Owner Requirements

Additional inputs may include:

- Minimum pump submergence.
- Pump intake clearance.
- Maximum starts per hour.
- Minimum run time.
- Minimum off time.
- Required pump separation.
- Wet-well geometry requirements.
- Owner-specific operating criteria.

These requirements must be established using applicable owner criteria and manufacturer information.

---

## 4. Wet-Well Geometry and Storage Volume

### 4.1 Circular Wet Well

For a circular wet well with a constant internal diameter:

$$
A = \frac{\pi D^2}{4}
$$

Where:

- $A$ = wet-well plan area (ft²).
- $D$ = internal wet-well diameter (ft).

The storage volume between two elevations is:

$$
V = A(h_2-h_1)
$$

Where:

- $V$ = storage volume (ft³).
- $h_2$ = upper water surface elevation (ft).
- $h_1$ = lower water surface elevation (ft).

Converting to gallons:

$$
V_{gal} = 7.48052A(h_2-h_1)
$$

### 4.2 Rectangular Wet Well

For a rectangular wet well:

$$
A = LW
$$

Therefore:

$$
V_{gal} = 7.48052LW(h_2-h_1)
$$

### 4.3 Variable Geometry

For wet wells with changing cross-sectional area:

$$
V = \int_{h_1}^{h_2} A(h)\,dh
$$

Where $A(h)$ represents the wetted horizontal area at elevation $h$.

Future HAS functionality may support elevation-storage tables for nonuniform wet-well geometries.

### 4.4 Effective Storage Volume

The effective operating storage is the usable liquid volume between the applicable pump-on and pump-off elevations.

For a constant-area wet well:

$$
V_{effective} = A(EL_{on}-EL_{off})
$$

The equation produces cubic feet when area is expressed in square feet and elevations in feet.

Pump and piping displacement should be considered where material to the calculated storage volume.

---

## 5. Pump Operating Levels

### 5.1 General

Pump operating elevations establish the control sequence and usable wet-well storage.

Typical levels include:

1. Wet-well bottom.
2. Minimum allowable operating level.
3. Lead pump off.
4. Lead pump on.
5. Lag pump on.
6. High-level alarm.
7. Emergency overflow or other limiting elevation.

The sequence depends on the pump control strategy and station configuration.

### 5.2 Pump-Off Elevation

The pump-off elevation should maintain adequate:

- Pump submergence.
- Intake clearance.
- Cooling requirements, where applicable.
- Protection against vortex formation.
- Manufacturer operating requirements.

The pump-off elevation must not be established solely from the desired cycling volume.

### 5.3 Pump-On Elevation

The pump-on elevation establishes the upper limit of the lead pump operating storage.

The difference between the pump-on and pump-off elevations determines the lead pump cycling volume.

### 5.4 Lag Pump Elevation

For multiple-pump stations, the lag pump may start at a higher wet-well elevation when the lead pump cannot maintain the required water level.

The lag pump start elevation should be evaluated in conjunction with:

- Peak inflow.
- Combined pump capacity.
- Available storage.
- Control sequence.
- Pump operating limitations.

---

## 6. Pump Cycling Analysis

### 6.1 General

Pump cycling is governed by the relationship between wastewater inflow, pump discharge capacity, and available operating storage.

For a simple constant-speed pump operating with fixed on/off elevations, cycling consists of:

- Wet-well filling while the pump is off.
- Wet-well drawdown while the pump is operating.

### 6.2 Wet-Well Filling Time

When the pump is off:

$$
t_{fill} = \frac{V}{Q_{in}}
$$

Where:

- $t_{fill}$ = filling time (min).
- $V$ = operating volume (gal).
- $Q_{in}$ = wastewater inflow (gpm).

This equation assumes constant inflow and no other wet-well outflow.

### 6.3 Wet-Well Drawdown Time

When the pump is operating:

$$
t_{drawdown} = \frac{V}{Q_p-Q_{in}}
$$

Where:

- $t_{drawdown}$ = drawdown time (min).
- $Q_p$ = pump discharge capacity (gpm).
- $Q_{in}$ = wastewater inflow (gpm).

This equation requires:

$$
Q_p > Q_{in}
$$

If pump capacity does not exceed inflow, the wet well cannot draw down under the assumed operating condition.

### 6.4 Total Pump Cycle Time

$$
t_{cycle} = t_{fill}+t_{drawdown}
$$

Substituting:

$$
t_{cycle}
=
\frac{V}{Q_{in}}
+
\frac{V}{Q_p-Q_{in}}
$$

### 6.5 Pump Starts per Hour

For repeated identical cycles:

$$
N = \frac{60}{t_{cycle}}
$$

Where:

- $N$ = pump starts per hour.
- $t_{cycle}$ = cycle time (min).

### 6.6 Maximum Cycling Frequency

For a constant-speed pump with constant capacity and inflow, maximum cycling frequency occurs when:

$$
Q_{in} = \frac{Q_p}{2}
$$

Under this condition:

$$
t_{cycle,min} = \frac{4V}{Q_p}
$$

Therefore:

$$
N_{max,actual} = \frac{15Q_p}{V}
$$

Where:

- $Q_p$ = pump capacity (gpm).
- $V$ = operating storage (gal).
- $N_{max,actual}$ = maximum calculated starts per hour.

This relationship applies to the simplified single-pump cycling model and should not be generalized to all multi-pump or variable-speed operating conditions.

---

## 7. Minimum Operating Storage

### 7.1 Storage Based on Allowable Starts

Rearranging the maximum cycling relationship:

$$
V_{required} = \frac{15Q_p}{N_{allowable}}
$$

Where:

- $V_{required}$ = required operating storage (gal).
- $Q_p$ = pump capacity (gpm).
- $N_{allowable}$ = allowable starts per hour.

This equation is applicable to the simplified constant-speed, single-pump cycling case.

### 7.2 Storage Based on Minimum Run Time

For constant inflow and pump capacity:

$$
V_{required,run}
=
t_{run,min}(Q_p-Q_{in})
$$

Where:

- $t_{run,min}$ = minimum pump run time (min).

### 7.3 Storage Based on Minimum Off Time

$$
V_{required,off}
=
t_{off,min}Q_{in}
$$

Where:

- $t_{off,min}$ = minimum pump off time (min).

### 7.4 Governing Storage Requirement

The operating storage must satisfy all applicable criteria.

For a particular evaluated scenario:

$$
V_{required}
=
\max
\left(
V_{cycling},
V_{run},
V_{off}
\right)
$$

The governing requirement should be determined across the relevant range of inflow conditions and pump operating scenarios.

---

## 8. Example Wet-Well Sizing Calculation

### 8.1 Design Conditions

Consider a hypothetical duplex wastewater lift station with the following conditions:

| Parameter | Value |
|---|---:|
| Lead pump capacity | 500 gpm |
| Allowable starts per hour | 6 |
| Wet-well internal diameter | 8 ft |
| Wet-well geometry | Circular |
| Pump configuration | Duplex |
| Operating method | Constant-speed on/off |

Assume that the simplified single-pump cycling relationship is appropriate for the preliminary example.

### 8.2 Required Operating Storage

$$
V_{required} = \frac{15Q_p}{N_{allowable}}
$$

Substituting:

$$
V_{required} = \frac{15(500)}{6}
$$

$$
V_{required} = 1250 \ \mathrm{gal}
$$

### 8.3 Wet-Well Area

$$
A = \frac{\pi D^2}{4}
$$

$$
A = \frac{\pi(8)^2}{4}
$$

$$
A = 50.27 \ \mathrm{ft^2}
$$

### 8.4 Required Operating Depth

Convert gallons to cubic feet:

$$
V_{ft^3} = \frac{1250}{7.48052}
$$

$$
V_{ft^3} \approx 167.1 \ \mathrm{ft^3}
$$

Calculate operating depth:

$$
h = \frac{V}{A}
$$

$$
h = \frac{167.1}{50.27}
$$

$$
h \approx 3.32 \ \mathrm{ft}
$$

### 8.5 Preliminary Result

The simplified calculation indicates approximately:

- Required operating storage: **1,250 gallons**.
- Wet-well plan area: **50.27 ft²**.
- Required operating depth: **3.32 ft**.

This result addresses only the specified pump cycling criterion.

Final wet-well dimensions must also satisfy pump submergence, control elevations, incoming sewer geometry, storage requirements, and applicable owner criteria.

---

## 9. Pump Submergence and Intake Conditions

### 9.1 Minimum Submergence

Adequate submergence is necessary to reduce the risk of:

- Surface vortex formation.
- Air entrainment.
- Unstable pump operation.
- Inadequate motor cooling, where applicable.

### 9.2 Manufacturer Requirements

Minimum submergence should be established using manufacturer information and applicable intake design guidance.

It should not be determined using a universal fixed depth without consideration of pump geometry and operating conditions.

### 9.3 Intake Geometry

Wet-well intake design should consider:

- Pump spacing.
- Wall clearance.
- Floor clearance.
- Approach flow.
- Potential dead zones.
- Solids accumulation.
- Vortex formation.

Detailed intake hydraulic design may require specialized analysis.

---

## 10. Wastewater Detention Time

### 10.1 General

Excessive wastewater detention can contribute to:

- Septicity.
- Odor generation.
- Hydrogen sulfide formation.
- Solids deposition.
- Operational maintenance concerns.

### 10.2 Simplified Detention Time

For a defined storage volume and constant inflow:

$$
t_{detention} = \frac{V}{Q_{in}}
$$

Where:

- $t_{detention}$ = nominal detention time (min).
- $V$ = evaluated volume (gal).
- $Q_{in}$ = wastewater inflow (gpm).

This is a simplified volume-to-flow calculation.

Actual wastewater residence time depends on mixing, pump cycling, inflow variability, and wet-well geometry.

### 10.3 Design Considerations

Detention time should be evaluated under relevant low-flow conditions.

Increasing wet-well volume to reduce pump starts may increase wastewater detention time.

The design must balance these competing objectives.

---

## 11. Emergency Storage

### 11.1 General

Emergency storage is intended to provide additional response time during conditions such as:

- Pump failure.
- Power failure.
- Control system malfunction.
- Temporary loss of pumping capacity.

### 11.2 Simplified Storage Calculation

For constant inflow and no pumping:

$$
V_{emergency} = Q_{in}t_{emergency}
$$

Where:

- $V_{emergency}$ = required emergency storage (gal).
- $Q_{in}$ = evaluated inflow (gpm).
- $t_{emergency}$ = required storage duration (min).

### 11.3 Available Emergency Storage

For a constant-area wet well:

$$
V_{available}
=
7.48052A(EL_{limit}-EL_{initial})
$$

Where:

- $EL_{limit}$ = limiting water surface elevation (ft).
- $EL_{initial}$ = starting water surface elevation (ft).

The limiting elevation must be established using applicable design criteria and the collection system configuration.

### 11.4 Storage Limitations

Emergency storage calculations should consider:

- Incoming sewer elevations.
- Upstream collection system surcharge.
- Available upstream storage.
- High-level alarms.
- Emergency power.
- Portable pumping provisions.
- Applicable owner and regulatory requirements.

Emergency storage should not automatically be treated as equivalent to normal operating storage.

---

## 12. Multiple-Pump Operating Conditions

### 12.1 Duplex Stations

A duplex station commonly includes two pumps with lead/lag controls.

Depending on design requirements, the second pump may provide:

- Standby capacity.
- Supplemental peak-flow capacity.
- Alternating lead operation.

### 12.2 Lead/Lag Operation

The wet-well control sequence may include:

1. Wet-well level rises.
2. Lead pump starts.
3. If inflow exceeds lead pump capacity, wet-well level continues rising.
4. Lag pump starts at its designated elevation.
5. Combined pumping reduces the wet-well level when total capacity exceeds inflow.
6. Pumps stop according to the established control sequence.

### 12.3 Combined Pump Capacity

For pumps operating in parallel, total station flow must be determined using the combined pump performance curve and the system head curve.

The combined station flow should not automatically be assumed equal to the sum of each pump's independent single-pump operating flow.

### 12.4 Cycling Analysis

Multiple-pump cycling analysis may require time-step simulation or event-based simulation.

The simplified single-pump cycling equations should not be applied without evaluating the applicable control sequence.

---

## 13. Integration with Force-Main and Pump Models

### 13.1 Wet-Well Elevation and Static Head

Wet-well water surface elevation establishes the upstream hydraulic boundary.

For simplified reservoir-to-reservoir conditions:

$$
H_{static}
=
HGL_{downstream}-WSEL_{wetwell}
$$

As the wet-well level changes, the system head curve changes.

### 13.2 Variable Pump Capacity

Actual pump discharge may vary with wet-well elevation.

Therefore:

$$
Q_p = f(H_{system},H_{pump})
$$

For a more detailed simulation, the pump flow should be determined from the intersection of the pump and system curves at the applicable wet-well elevation.

### 13.3 Dynamic Wet-Well Volume Balance

The governing storage equation is:

$$
\frac{dV}{dt}
=
Q_{in}(t)-Q_{out}(t)
$$

Where:

- $V$ = wet-well liquid volume.
- $Q_{in}$ = incoming wastewater flow.
- $Q_{out}$ = pumped wastewater flow.

For a constant-area wet well:

$$
A\frac{dh}{dt}
=
Q_{in}(t)-Q_{out}(t)
$$

All terms must use consistent units.

This equation provides the foundation for future time-dependent wet-well simulation.

### 13.4 Simulation Applications

Future HAS functionality may evaluate:

- Variable inflow.
- Changing wet-well elevations.
- Pump cycling.
- Lead/lag control.
- Pump alternation.
- Multiple-pump operation.
- Variable-speed pumping.

---

## 14. Engineering Verification

### 14.1 Analytical Verification

HAS calculations should be independently verified for:

- Wet-well area.
- Storage volume.
- Operating depth.
- Fill time.
- Drawdown time.
- Pump cycle time.
- Starts per hour.
- Minimum operating storage.
- Emergency storage.

### 14.2 Reference Calculations

Verification may include:

- Independent hand calculations.
- Spreadsheet calculations.
- Manufacturer technical guidance.
- Recognized engineering references.
- Comparison with established pump station design methods.

### 14.3 Verification Cases

Recommended cases include:

1. Circular wet well with constant inflow.
2. Rectangular wet well with constant inflow.
3. Maximum cycling frequency condition.
4. Minimum pump run time.
5. Minimum pump off time.
6. Inflow equal to pump capacity.
7. Inflow exceeding pump capacity.
8. Emergency storage with pumps unavailable.
9. Variable inflow simulation.
10. Multiple-pump operating scenarios.

### 14.4 Numerical Acceptance Criteria

Verification tolerances should be defined in the HAS Verification Plan.

---

## 15. HAS Software Implementation Considerations

### 15.1 Wet-Well Data Model

A future `WetWell` class may include:

- Geometry.
- Internal dimensions.
- Bottom elevation.
- Operating elevations.
- Storage volume.
- Elevation-volume relationships.

### 15.2 Pump Control Model

A separate pump control component may define:

- Lead pump start.
- Lead pump stop.
- Lag pump start.
- Lag pump stop.
- Pump alternation.
- High-level alarm.
- Minimum run time.
- Minimum off time.

### 15.3 Calculation Functions

Potential functions include:

- `calculate_wet_well_area()`
- `calculate_storage_volume()`
- `calculate_operating_depth()`
- `calculate_fill_time()`
- `calculate_drawdown_time()`
- `calculate_cycle_time()`
- `calculate_starts_per_hour()`
- `calculate_required_storage()`
- `calculate_emergency_storage()`

These names are conceptual and should be reconciled with the existing HAS architecture.

### 15.4 Separation of Responsibilities

**WetWell**

Represents geometry, elevations, and storage.

**Pump**

Represents pump hydraulic performance.

**ForceMain**

Represents hydraulic system requirements.

**PumpControl**

Represents operating logic and pump sequencing.

**LiftStation**

Integrates the wet well, pumps, controls, and force-main system.

This architecture supports modular development and independent verification.

---

## 16. Limitations and Engineering Judgment

The initial methodology is intended for preliminary engineering calculations.

It does not independently evaluate:

- Detailed three-dimensional intake hydraulics.
- Vortex formation.
- Air entrainment.
- Complex wet-well mixing.
- Biological wastewater processes.
- Detailed solids deposition.
- Electrical control system design.
- Complete collection system surcharge behavior.
- Hydraulic transients.

Final wet-well design must consider applicable regulations, owner criteria, manufacturer requirements, and professional engineering judgment.

---

## 17. Engineering References

Relevant references include:

1. Hydraulic Institute, applicable pump intake and pumping system design standards.
2. Water Environment Federation, wastewater pumping station design publications.
3. U.S. EPA, wastewater pumping station technical guidance.
4. Florida Department of Environmental Protection, applicable wastewater collection and transmission system requirements.
5. Pump manufacturer installation and operating manuals.
6. Recognized wastewater engineering and pump station design textbooks.

Applicable editions and requirements should be verified before use in formal engineering design.

---

## 18. Summary

Wet-well design requires integration of hydraulic storage, wastewater inflow, pump performance, and operating controls.

The principal engineering requirements are:

1. Establish wastewater inflow conditions.
2. Define wet-well geometry.
3. Establish pump operating elevations.
4. Calculate usable operating storage.
5. Evaluate pump cycling frequency.
6. Verify minimum pump run and off times.
7. Evaluate pump submergence.
8. Assess wastewater detention.
9. Evaluate emergency storage separately.
10. Integrate wet-well calculations with pump and force-main models.
11. Independently verify engineering calculations.

These principles establish the engineering foundation for wet-well analysis within HAS and support future development of integrated lift station modeling and operational simulation capabilities.
