# Pump Selection and Hydraulic Performance

## 1. Purpose and Scope

This document establishes a systematic engineering methodology for selecting and evaluating pumps for municipal water and wastewater applications, with particular emphasis on wastewater lift stations and force-main systems.

The methodology addresses:

- Establishing hydraulic design requirements.
- Developing system head curves.
- Evaluating manufacturer pump performance curves.
- Determining pump operating points.
- Evaluating best efficiency point (BEP) and acceptable operating regions.
- Assessing net positive suction head (NPSH) and cavitation risk.
- Evaluating single-pump and multiple-pump operation.
- Assessing variable-speed operation.
- Evaluating pump efficiency, motor sizing, and energy consumption.
- Documenting and independently verifying pump selection.

**Application to the Hydraulic Analysis Suite (HAS):** This document serves as an engineering reference for the development, testing, and verification of future pump modeling and selection functionality.

It is not a substitute for manufacturer recommendations, applicable regulatory requirements, or project-specific engineering judgment.

---

## 2. Pump Selection Methodology

Pump selection should begin with the hydraulic requirements of the system rather than a specific pump model.

### Step 1. Establish Design Requirements

Identify the following:

- Average, peak, and minimum design flows.
- Required firm pumping capacity.
- Number of duty and standby pumps.
- Wet-well operating elevations.
- Discharge hydraulic grade line (HGL).
- Force-main diameter, length, material, and roughness.
- Pipe fittings, valves, and appurtenances.
- Anticipated future system expansion.
- Operating philosophy and control strategy.
- Applicable owner criteria and regulatory requirements.

For wastewater lift stations, evaluate the range of anticipated inflows and the resulting pump operating conditions.

### Step 2. Develop System Head Curves

Calculate the total dynamic head over a range of flow rates and relevant operating conditions.

Develop separate curves where appropriate for:

- Minimum wet-well level.
- Normal wet-well operating level.
- Maximum wet-well level.
- Minimum and maximum downstream HGL.
- Existing and future force-main conditions.
- Single-pump and multiple-pump operation.

### Step 3. Identify Candidate Pumps

Select candidate pump types and configurations based on:

- Required capacity and head.
- Wastewater characteristics.
- Solids-handling requirements.
- Installation configuration.
- Efficiency and operating range.
- Maintenance and reliability.
- Manufacturer support.

### Step 4. Evaluate Manufacturer Pump Curves

Obtain manufacturer-certified or published performance information, including:

- Head versus flow.
- Efficiency versus flow.
- Brake horsepower versus flow.
- NPSH required versus flow, where applicable.
- Impeller diameter and rotational speed.
- Minimum and maximum allowable operating flows.

### Step 5. Determine Operating Points

Overlay the pump and system curves.

The operating point occurs where the pump develops the head required by the system:

**H_pump(Q) = H_system(Q)**

Evaluate all applicable system operating conditions.

### Step 6. Evaluate Operating Regions

Compare the resulting operating points against:

- Best efficiency point (BEP).
- Preferred operating region (POR).
- Allowable operating region (AOR).
- Minimum continuous stable flow.
- Maximum allowable flow.
- Manufacturer limitations.

### Step 7. Evaluate NPSH and Submergence

Verify adequate suction conditions throughout the anticipated operating range.

For submersible wastewater pumps, also verify manufacturer-required minimum submergence, vortex prevention, and wet-well geometry.

### Step 8. Evaluate Power and Motor Requirements

Determine the brake horsepower required over the operating range.

Verify that the selected motor is suitable for the maximum anticipated load and operating conditions.

### Step 9. Evaluate Multiple-Pump and Variable-Speed Operation

Where applicable, evaluate:

- One pump operating.
- Two or more pumps operating simultaneously.
- Duty and standby configurations.
- Variable-frequency drive (VFD) operation.
- Minimum and maximum operating speeds.
- Pump sequencing and controls.

### Step 10. Complete Selection and Verification

Compare candidate pumps based on hydraulic performance, reliability, energy consumption, maintenance, lifecycle cost, and project requirements.

Document the selected equipment and supporting engineering calculations.

---

## 3. System Head Curves

### 3.1 General Equation

The total dynamic head required by a pumping system can be expressed as:

$$
TDH(Q)=H_{static}+h_f(Q)+h_m(Q)
$$

Where:

- \(TDH\) = total dynamic head (ft)
- \(H_{static}\) = static head difference between hydraulic boundaries (ft)
- \(h_f\) = major friction losses (ft)
- \(h_m\) = minor losses (ft)
- \(Q\) = flow rate

This simplified formulation assumes steady-state conditions, appropriate boundary definitions, and no additional pressure-head or velocity-head terms requiring separate treatment.

For a lift station discharging into a pressurized system, the downstream HGL must be considered rather than relying solely on physical pipe elevation.

### 3.2 Static Head

For a wet-well pumping system:

$$
H_{static}=HGL_{discharge}-WSEL_{wetwell}
$$

Where:

- \(HGL_{discharge}\) = downstream hydraulic grade elevation (ft)
- \(WSEL_{wetwell}\) = wet-well water surface elevation (ft)

The controlling static head depends on the combination of wet-well water level and downstream hydraulic conditions.

### 3.3 Major Friction Losses

Using the Darcy-Weisbach equation:

$$
h_f=f\frac{L}{D}\frac{V^2}{2g}
$$

Where:

- \(f\) = Darcy friction factor
- \(L\) = pipe length (ft)
- \(D\) = internal pipe diameter (ft)
- \(V\) = average fluid velocity (ft/s)
- \(g\) = gravitational acceleration (ft/s²)

For water and wastewater force mains, the Hazen-Williams equation may also be used where appropriate and permitted by project criteria.

The selected friction-loss method should be documented and consistently applied.

### 3.4 Minor Losses

Minor losses associated with fittings, valves, and other appurtenances may be calculated using:

$$
h_m=\sum K_i\frac{V_i^2}{2g}
$$

Where:

- \(K_i\) = loss coefficient for an individual fitting or appurtenance
- \(V_i\) = velocity in the applicable pipe section

Loss coefficients should be obtained from traceable engineering references or manufacturer data.

### 3.5 Developing the System Curve

1. Establish the design flow range.
2. Select the applicable static head condition.
3. Calculate major friction losses at each flow rate.
4. Calculate minor losses at each flow rate.
5. Sum the static and dynamic components.
6. Plot TDH against flow.
7. Repeat for other relevant operating conditions.
8. Compare the curves with manufacturer pump performance data.

**Important:** A system curve does not establish the actual pump operating point by itself. The operating point must be determined from its intersection with the applicable pump performance curve.

---

## 4. Pump Performance Curves

### 4.1 Head-Capacity Curve

The head-capacity curve describes the relationship between flow rate and the head developed by a pump at a specified speed and impeller diameter.

For many centrifugal pumps, developed head decreases as flow increases.

The manufacturer curve should be used for final equipment selection.

### 4.2 Pump Efficiency Curve

Pump efficiency represents the relationship between hydraulic power delivered to the fluid and mechanical power supplied to the pump shaft.

$$
\eta_p=\frac{P_{hydraulic}}{P_{shaft}}
$$

The point of maximum pump efficiency is the best efficiency point (BEP).

### 4.3 Brake Horsepower

For water in U.S. customary units:

$$
BHP=\frac{Q \times H \times SG}{3960 \times \eta_p}
$$

Where:

- \(BHP\) = brake horsepower (hp)
- \(Q\) = flow rate (gpm)
- \(H\) = pump head (ft)
- \(SG\) = fluid specific gravity
- \(\eta_p\) = pump efficiency expressed as a decimal

Motor efficiency is not included in this equation. Electrical input power must be calculated separately.

### 4.4 Affinity Laws

For a given pump under conditions where the affinity-law approximations are applicable:

$$
\frac{Q_2}{Q_1}\approx\frac{N_2}{N_1}
$$

$$
\frac{H_2}{H_1}\approx\left(\frac{N_2}{N_1}\right)^2
$$

$$
\frac{P_2}{P_1}\approx\left(\frac{N_2}{N_1}\right)^3
$$

Where \(N\) represents rotational speed.

These relationships are useful for preliminary variable-speed analysis but do not replace manufacturer performance data.

**Important:** When speed changes, the pump operating point must be recalculated against the system curve. Flow does not necessarily change in direct proportion to speed when significant static head is present.

---

## 5. Best Efficiency Point and Operating Regions

### 5.1 Best Efficiency Point

The BEP is the operating condition at which the pump achieves its maximum hydraulic efficiency for a particular speed and impeller configuration.

Operation near BEP generally supports improved hydraulic efficiency and can reduce mechanical loading associated with operation away from the pump's intended duty.

### 5.2 Preferred Operating Region

The preferred operating region (POR) is the range of operation recommended for favorable pump performance and reliability.

The applicable range depends on pump design, manufacturer recommendations, and relevant industry standards.

### 5.3 Allowable Operating Region

The allowable operating region (AOR) represents the broader range over which the pump may operate subject to defined limitations.

Operating within the AOR does not necessarily mean the pump is suitable for continuous operation at every point in that range.

### 5.4 Operating Limits

Pump selection should consider:

- Minimum continuous stable flow.
- Minimum thermal flow.
- Recirculation and vibration.
- Cavitation risk.
- Motor loading.
- Maximum allowable flow.
- Manufacturer-specific operating restrictions.

Do not assume a universal percentage of BEP is acceptable for all pump types.

---

## 6. Net Positive Suction Head

### 6.1 General Considerations

Net positive suction head is used to evaluate whether adequate suction pressure exists to limit vapor formation within the pump.

Two quantities must be distinguished:

**NPSH Available (NPSHA):** Determined by the hydraulic conditions of the suction system.

**NPSH Required (NPSHR):** Obtained from manufacturer performance information, commonly based on an established test criterion such as a 3% reduction in developed head.

NPSHR should not be interpreted as a guarantee that cavitation is completely absent.

### 6.2 NPSHA Equation

For an open wet well supplying a pump:

$$
NPSHA=H_{atm}+H_{static,suction}-h_{f,suction}-H_{vapor}
$$

Where:

- \(H_{atm}\) = atmospheric pressure head (ft)
- \(H_{static,suction}\) = liquid surface elevation relative to the pump suction datum (ft)
- \(h_{f,suction}\) = suction-side hydraulic losses (ft)
- \(H_{vapor}\) = liquid vapor pressure head (ft)

All terms must use consistent units and an appropriate datum.

### 6.3 NPSH Margin

The basic selection condition is:

$$
NPSHA>NPSHR
$$

However, an appropriate NPSH margin must be established using manufacturer recommendations, applicable standards, and project operating conditions.

### 6.4 Wastewater Lift Station Considerations

For submersible pumps, evaluate:

- Minimum wet-well operating level.
- Minimum pump submergence.
- Air entrainment.
- Vortex formation.
- Wet-well geometry.
- Solids accumulation.
- Pump suction approach conditions.

Adequate NPSH alone does not ensure satisfactory wet-well hydraulic performance.

---

## 7. Multiple-Pump Operation

### 7.1 Pumps Operating in Parallel

Parallel pump operation is commonly used in municipal lift stations.

For pumps operating in parallel, flow contributions are added at a common developed head, subject to the actual hydraulic arrangement.

For identical pumps:

$$
Q_{total}(H)=nQ_{single}(H)
$$

Where:

- \(n\) = number of operating pumps
- \(Q_{single}(H)\) = flow delivered by one pump at the specified head

This relationship assumes identical pump curves and appropriate common hydraulic boundaries.

The resulting combined pump curve must be intersected with the system curve.

**Two pumps operating in parallel do not necessarily deliver twice the flow of one pump operating alone.**

As total flow increases, force-main friction losses also increase.

### 7.2 Duty and Standby Pumps

For municipal wastewater lift stations, evaluate the required firm capacity with the largest pumping unit unavailable, where required by applicable criteria.

Pump configurations may include:

- One duty and one standby.
- Two duty and one standby.
- Multiple duty pumps with standby capacity.

### 7.3 Pump Sequencing

Evaluate the hydraulic effects of:

- Lead pump startup.
- Lag pump startup.
- Pump shutdown.
- Alternating lead pumps.
- Simultaneous pump operation.

Steady-state analysis establishes operating points but does not evaluate transient pressure events during pump startup or shutdown.

---

## 8. Variable-Speed Pump Operation

Variable-frequency drives allow pump rotational speed to be adjusted in response to changing system requirements.

Potential benefits include:

- Improved flow control.
- Reduced unnecessary throttling.
- Improved energy efficiency under suitable conditions.
- Reduced cycling under certain operating strategies.

However, variable-speed operation introduces additional considerations:

- Minimum operating speed.
- Static head limitations.
- Minimum force-main velocity.
- Motor cooling requirements.
- Pump efficiency at reduced speed.
- Solids transport and deposition.
- Control stability.

For wastewater applications, reduced flow velocities must be evaluated carefully to avoid adverse solids-handling conditions.

---

## 9. Energy Consumption and Lifecycle Cost

### 9.1 Electrical Power

Electrical input power can be estimated using:

$$
P_{input}=\frac{QHSG}{5310\eta_p\eta_m}
$$

Where:

- \(P_{input}\) = electrical input power (kW)
- \(Q\) = flow rate (gpm)
- \(H\) = pump head (ft)
- \(SG\) = specific gravity
- \(\eta_p\) = pump efficiency
- \(\eta_m\) = motor efficiency

This expression excludes additional losses, such as VFD losses, unless incorporated into the overall efficiency.

### 9.2 Annual Energy Consumption

For approximately constant operating power:

$$
E_{annual}=P_{input}\times t_{annual}
$$

Where:

- \(E_{annual}\) = annual energy consumption (kWh)
- \(t_{annual}\) = annual operating hours

For systems with variable operating conditions, calculate energy consumption across the anticipated duty cycle.

### 9.3 Lifecycle Cost

Pump selection should consider more than initial purchase price.

Relevant costs include:

- Equipment purchase.
- Installation.
- Electrical and controls.
- Energy consumption.
- Routine maintenance.
- Repair and replacement.
- Downtime and reliability.
- Equipment service life.

For HAS, lifecycle cost analysis could eventually be integrated with uncertainty and Monte Carlo modeling to compare alternatives.

---

## 10. Pump Selection Example

### 10.1 Hypothetical Design Conditions

Consider a wastewater lift station with the following preliminary design requirements.

| Parameter | Value |
|---|---|
| Required design flow | 500 gpm |
| Estimated TDH at design flow | 100 ft |
| Fluid | Municipal wastewater |
| Pump configuration | One duty, one standby |
| Installation | Submersible |
| Operating mode | Constant speed |
| Assumed pump efficiency | 70% |
| Assumed motor efficiency | 90% |
| Specific gravity | 1.0 |

These values are hypothetical and are not a complete pump selection design basis.

### 10.2 Hydraulic Power

Calculate brake horsepower:

$$
BHP=\frac{500(100)(1.0)}{3960(0.70)}
$$

$$
BHP\approx18.0\ hp
$$

### 10.3 Electrical Power

$$
P_{input}=\frac{500(100)(1.0)}{5310(0.70)(0.90)}
$$

$$
P_{input}\approx14.9\ kW
$$

### 10.4 Preliminary Selection

Identify manufacturer pump models capable of operating near:

**500 gpm at 100 ft TDH**

For each candidate:

1. Obtain the manufacturer head-capacity curve.
2. Develop the applicable system head curves.
3. Determine actual operating points.
4. Evaluate BEP and operating regions.
5. Verify solids-handling capability.
6. Verify NPSH and submergence.
7. Evaluate power requirements across the operating range.
8. Compare equipment and lifecycle costs.

### 10.5 Selection Limitations

The example establishes a preliminary hydraulic duty point and estimated power requirement.

It does not establish a final pump model, impeller diameter, motor rating, or acceptable operating region.

Those decisions require actual manufacturer performance data and a complete system hydraulic analysis.

---

## 11. Engineering Verification

### 11.1 Required Verification

The pump selection process should include independent checks of:

- Unit conversions.
- Static head calculations.
- Major and minor losses.
- System curve generation.
- Pump curve interpolation.
- Operating point determination.
- Pump efficiency.
- Brake horsepower.
- Motor sizing.
- NPSH margin.
- Multiple-pump operation.

### 11.2 Manufacturer Verification

Compare analytical results with manufacturer pump selection software and published performance curves.

Where discrepancies occur, investigate:

- Differences in hydraulic assumptions.
- Curve interpolation.
- Impeller diameter.
- Pump speed.
- Fluid properties.
- System boundary conditions.

### 11.3 Design Limitations

Steady-state pump selection does not replace:

- Hydraulic transient analysis.
- Wet-well hydraulic design.
- Surge protection evaluation.
- Electrical system design.
- Detailed equipment specifications.
- Manufacturer review.

These analyses should be completed where required by the application.

---

## 12. HAS Software Implementation Considerations

The pump selection methodology provides a foundation for future HAS development.

### 12.1 Pump Data Model

A future Pump class may include:

- Manufacturer and model.
- Pump type.
- Impeller diameter.
- Rated speed.
- Head-capacity data.
- Efficiency data.
- Power data.
- NPSHR data.
- BEP.
- Operating limits.

### 12.2 Hydraulic Calculations

Future functionality may include:

- System curve generation.
- Pump curve interpolation.
- Operating point calculations.
- Single-pump analysis.
- Parallel pump analysis.
- Variable-speed analysis.
- Power and energy calculations.

### 12.3 Engineering Validation

Each calculation should have:

- Defined input units.
- Documented assumptions.
- Input validation.
- Numerical tolerances.
- Independent verification cases.
- Traceable reference data.

### 12.4 Future Analytical Capabilities

Potential future HAS capabilities include:

- Automated pump comparison.
- Pump scheduling analysis.
- Energy cost optimization.
- Lifecycle cost analysis.
- Sensitivity analysis.
- Monte Carlo evaluation of uncertain operating conditions.
- Capital improvement alternative comparisons.

These capabilities represent potential future development, not existing HAS functionality.

---

## 13. Engineering References

The following references should be consulted as applicable:

1. **Hydraulic Institute:** ANSI/HI 9.6.3, Rotodynamic Pumps: Guideline for Operating Regions.
2. **Hydraulic Institute:** ANSI/HI 9.6.1, Rotodynamic Pumps: Guideline for NPSH Margin.
3. **Hydraulic Institute:** ANSI/HI 9.8, Rotodynamic Pumps for Pump Intake Design.
4. **Hydraulic Institute:** ANSI/HI 14.6, Rotodynamic Pumps for Hydraulic Performance Acceptance Tests.
5. **U.S. EPA:** Wastewater Technology Fact Sheet: In-Plant Pump Stations (EPA 832-F-00-069).
6. **Crane:** Technical Paper No. 410, Flow of Fluids Through Valves, Fittings, and Pipe.
7. **Pump manufacturer:** Published pump curves, technical manuals, and installation requirements.
8. **Applicable Florida regulatory requirements:** Florida Administrative Code, Chapter 62-604, and applicable utility owner standards.

Use current applicable editions and verify the scope of each reference before applying its requirements.

---

## 14. Summary

Pump selection is an iterative engineering process that integrates system hydraulics, manufacturer performance data, operational requirements, reliability, and lifecycle cost.

The primary engineering objective is not simply to identify a pump capable of delivering a specified flow and head. It is to select a pumping system that operates reliably and efficiently throughout its anticipated range of conditions.

For the Hydraulic Analysis Suite, this methodology establishes a technical foundation for developing pump performance calculations, operating point analysis, and future pump selection capabilities.

All analytical methods should be supported by documented assumptions, traceable references, and independent verification before use in engineering design decisions.
