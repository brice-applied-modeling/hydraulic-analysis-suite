# Units and Unit Conventions

## 1. Purpose

This document establishes the units, conversion factors, dimensional conventions, and numerical handling requirements used throughout the Hydraulic Analysis Suite (HAS).

Consistent unit management is essential for accurate hydraulic calculations, reproducible engineering results, and reliable software verification.

The objectives are to:

- Establish standard engineering input and output units.
- Define internal calculation unit conventions.
- Document unit conversion factors.
- Prevent dimensional inconsistencies.
- Establish conventions for hydraulic equations.
- Support future expansion to SI units.
- Provide a foundation for automated unit verification.

---

## 2. Unit System

### 2.1 Primary Engineering Unit System

HAS initially uses **U.S. customary engineering units** for user inputs, calculation results, and engineering reports.

These units reflect common practices in municipal water and wastewater engineering in the United States.

The initial development scope does not require a fully configurable SI unit interface.

However, the software architecture should permit future SI support without rewriting the underlying hydraulic calculations.

### 2.2 Standard Engineering Units

| Quantity | Standard Unit | Symbol |
|---|---|---|
| Pipe length | Feet | ft |
| Pipe diameter | Inches | in |
| Flow rate | U.S. gallons per minute | gpm |
| Velocity | Feet per second | ft/s |
| Pressure | Pounds per square inch | psi |
| Hydraulic head | Feet | ft |
| Elevation | Feet | ft |
| Pipe roughness | Feet or inches, explicitly identified | ft, in |
| Pump power | Horsepower | hp |
| Electrical power | Kilowatts | kW |
| Time | Seconds | s |
| Pump operating duration | Hours | hr |
| Volume | U.S. gallons | gal |
| Temperature | Degrees Fahrenheit | °F |
| Fluid density | Slugs per cubic foot | slug/ft³ |
| Dynamic viscosity | Pound-force seconds per square foot | lbf·s/ft² |
| Kinematic viscosity | Square feet per second | ft²/s |
| Gravitational acceleration | Feet per second squared | ft/s² |

The distinction between mass, force, and weight must be maintained when implementing hydraulic equations.

---

## 3. Internal Calculation Conventions

### 3.1 General Approach

HAS should distinguish between:

1. User input units.
2. Internal calculation units.
3. Engineering output units.

The initial implementation should use a consistent U.S. customary calculation convention.

All unit conversions should occur explicitly at defined calculation boundaries.

### 3.2 Proposed Internal Units

| Quantity | Input Unit | Internal Unit | Output Unit |
|---|---|---|---|
| Pipe diameter | in | ft | in |
| Pipe length | ft | ft | ft |
| Flow rate | gpm | ft³/s | gpm |
| Pipe area | Derived | ft² | ft² |
| Velocity | Derived | ft/s | ft/s |
| Head loss | Derived | ft | ft |
| Pressure | psi | lbf/ft² | psi |
| Elevation | ft | ft | ft |
| Hydraulic power | Derived | ft·lbf/s | hp |
| Volume | gal | ft³ | gal |
| Time | s or hr | s | s or hr |
| Kinematic viscosity | ft²/s | ft²/s | ft²/s |

These are proposed conventions and should be reconciled with the existing HAS implementation before adoption.

### 3.3 Internal Consistency

All hydraulic equations must receive values expressed in the units expected by the calculation.

For example, Darcy-Weisbach calculations require pipe diameter and pipe length to use consistent length units.

A diameter entered as 6 inches must be converted to 0.5 feet before calculating cross-sectional area in square feet.

Conversions should not be silently embedded in unrelated engineering calculations.

---

## 4. Unit Conversion Factors

### 4.1 Length

| From | To | Multiply By |
|---|---|---|
| ft | m | 0.3048 |
| in | ft | 0.0833333333333 |
| in | cm | 2.54 |
| ft | in | 12 |
| mile | ft | 5,280 |

### 4.2 Flow Rate

All gallons referenced in HAS are **U.S. liquid gallons**, not Imperial gallons.

| From | To | Multiply By |
|---|---|---|
| gpm | ft³/s | 0.00222800926 |
| gpm | m³/s | 0.0000630901964 |
| gpm | m³/hr | 0.227124707 |
| gpm | L/s | 0.0630901964 |
| gpm | MGD | 0.00144 |
| ft³/s | gpm | 448.831169 |
| MGD | gpm | 694.444444 |

### 4.3 Volume

| From | To | Multiply By |
|---|---|---|
| U.S. gal | ft³ | 0.133680556 |
| ft³ | U.S. gal | 7.48051948 |
| U.S. gal | L | 3.785411784 |
| U.S. gal | m³ | 0.003785411784 |

### 4.4 Pressure

| From | To | Multiply By |
|---|---|---|
| psi | kPa | 6.89475729 |
| psi | lbf/ft² | 144 |
| psi | ft of water | Approximately 2.31 |
| ft of water | psi | Approximately 0.433 |

**Important:** Pressure-to-head conversions depend on fluid specific weight.

The approximate values of 2.31 ft/psi and 0.433 psi/ft are applicable to water near typical ambient temperatures and should not be treated as universal physical constants.

### 4.5 Power

| From | To | Multiply By |
|---|---|---|
| Mechanical hp | W | 745.699872 |
| Mechanical hp | kW | 0.745699872 |
| kW | Mechanical hp | 1.34102209 |
| ft·lbf/s | Mechanical hp | 0.00181818182 |

### 4.6 Temperature

Fahrenheit to Celsius:

\[
T_C=(T_F-32)\frac{5}{9}
\]

Celsius to Fahrenheit:

\[
T_F=T_C\frac{9}{5}+32
\]

Temperature conversions are not simple multiplication operations because the scales have different zero points.

---

## 5. Hydraulic Calculation Unit Conventions

### 5.1 Flow Velocity

The continuity equation is:

\[
Q=AV
\]

Where:

- \(Q\) = flow rate (ft³/s)
- \(A\) = cross-sectional area (ft²)
- \(V\) = velocity (ft/s)

For a circular pipe:

\[
A=\frac{\pi D^2}{4}
\]

Where \(D\) is the actual internal diameter in feet.

The nominal pipe diameter should not automatically be assumed equal to the actual internal diameter.

### 5.2 Reynolds Number

\[
Re=\frac{VD}{\nu}
\]

Where:

- \(Re\) = dimensionless Reynolds number
- \(V\) = velocity (ft/s)
- \(D\) = internal diameter (ft)
- \(\nu\) = kinematic viscosity (ft²/s)

All quantities must use consistent units.

### 5.3 Darcy-Weisbach Equation

\[
h_f=f\frac{L}{D}\frac{V^2}{2g}
\]

Where:

- \(h_f\) = friction head loss (ft)
- \(f\) = dimensionless Darcy friction factor
- \(L\) = pipe length (ft)
- \(D\) = pipe internal diameter (ft)
- \(V\) = velocity (ft/s)
- \(g\) = gravitational acceleration (ft/s²)

HAS must distinguish the Darcy friction factor from the Fanning friction factor.

### 5.4 Minor Losses

\[
h_m=K\frac{V^2}{2g}
\]

Where:

- \(h_m\) = minor head loss (ft)
- \(K\) = dimensionless loss coefficient
- \(V\) = velocity (ft/s)

The applicable velocity must correspond to the pipe section associated with the fitting or appurtenance.

### 5.5 Pressure and Head

Pressure head is calculated using:

\[
h_p=\frac{P}{\gamma}
\]

Where:

- \(h_p\) = pressure head (ft)
- \(P\) = pressure (lbf/ft²)
- \(\gamma\) = fluid specific weight (lbf/ft³)

For pressure supplied in psi:

\[
h_p=\frac{144P_{psi}}{\gamma}
\]

This equation is valid when consistent units are used.

### 5.6 Hydraulic Power

Hydraulic power in horsepower may be calculated as:

\[
HP_{hydraulic}=\frac{QHSG}{3960}
\]

Where:

- \(Q\) = flow rate (gpm)
- \(H\) = hydraulic head (ft)
- \(SG\) = specific gravity relative to the water reference used for the conversion
- \(HP_{hydraulic}\) = hydraulic horsepower

The constant 3960 is a conventional rounded engineering conversion factor for water-based calculations.

Pump brake horsepower is:

\[
BHP=\frac{QHSG}{3960\eta_p}
\]

Where \(\eta_p\) is pump efficiency expressed as a decimal.

---

## 6. Physical Constants and Fluid Properties

### 6.1 Gravitational Acceleration

The standard gravitational acceleration is:

\[
g_0=32.17404856\ \mathrm{ft/s^2}
\]

For preliminary hydraulic calculations, a rounded value of 32.174 ft/s² is commonly appropriate.

HAS should maintain a single documented value for standard gravity rather than independently defining different values across modules.

### 6.2 Fluid Density

Fluid density varies with temperature and fluid composition.

For water near 20°C, a representative density is approximately:

\[
\rho\approx1.936\ \mathrm{slug/ft^3}
\]

This is an approximate reference value rather than a universal constant.

### 6.3 Kinematic Viscosity

Water viscosity varies with temperature.

A representative kinematic viscosity near 20°C is:

\[
\nu\approx1.08\times10^{-5}\ \mathrm{ft^2/s}
\]

Fluid properties should be obtained from documented sources and associated with the applicable temperature and fluid assumptions.

### 6.4 Fluid Property Management

HAS should distinguish between:

- Physical constants.
- Reference fluid properties.
- User-specified fluid properties.
- Temperature-dependent properties.

Physical constants should not be confused with values that change according to operating conditions.

---

## 7. Dimensional Consistency and Error Prevention

### 7.1 Explicit Unit Conversion

Engineering calculation functions should receive values in clearly defined units.

For example, a pipe diameter provided in inches should be converted to feet before calculating cross-sectional area.

### 7.2 Input Validation

HAS should validate:

- Positive pipe diameters.
- Nonnegative physical lengths.
- Valid flow rates for the calculation.
- Valid fluid properties.
- Acceptable pressure and temperature inputs.
- Valid efficiency ranges.
- Consistent units.

Negative values should not automatically be rejected for quantities where negative values have physical meaning, such as signed flow or gauge pressure.

### 7.3 Dimensional Verification

Engineering equations should be reviewed to ensure that their terms are dimensionally consistent.

For example:

\[
\frac{V^2}{2g}
=
\frac{\mathrm{ft^2/s^2}}{\mathrm{ft/s^2}}
=
\mathrm{ft}
\]

Therefore, the Darcy-Weisbach equation produces head loss in feet when consistent units are applied.

### 7.4 Avoiding Implicit Conversions

The software should avoid unexplained numerical conversion factors within calculation methods.

Conversion factors should be:

- Named.
- Documented.
- Traceable.
- Centrally maintained.
- Independently tested.

---

## 8. Software Implementation Requirements

### 8.1 Centralized Conversion Functions

Unit conversions should be implemented through a centralized utility module.

Potential functions include:

- `inches_to_feet()`
- `gpm_to_cfs()`
- `cfs_to_gpm()`
- `psi_to_head()`
- `head_to_psi()`
- `gallons_to_cubic_feet()`
- `horsepower_to_kw()`

Function names should identify the units being converted.

### 8.2 Internal Unit Contracts

Each engineering calculation should document:

- Input quantities.
- Required input units.
- Output quantities.
- Output units.
- Assumptions.
- Applicable limitations.

For example:

**Pipe velocity calculation**

Inputs:

- Flow: ft³/s
- Internal diameter: ft

Output:

- Velocity: ft/s

### 8.3 Output Documentation

Numerical outputs should identify their associated units.

Examples:

- Flow: 500 gpm.
- Velocity: 3.5 ft/s.
- Head loss: 12.4 ft.
- Pressure: 45 psi.
- Pump power: 18.0 hp.

Unit labels should be maintained consistently across reports, tables, plots, and exported datasets.

### 8.4 Future SI Support

Future versions of HAS may support SI inputs and outputs.

The preferred implementation approach is to maintain consistent internal calculation units and perform explicit conversions at the interface and reporting boundaries.

SI support should not require separate implementations of the same hydraulic equations.

---

## 9. Unit Verification and Testing

### 9.1 Conversion Tests

Each conversion function should have automated tests using independently established reference values.

Example:

\[
12\ \mathrm{in}=1\ \mathrm{ft}
\]

\[
448.831169\ \mathrm{gpm}\approx1\ \mathrm{ft^3/s}
\]

### 9.2 Round-Trip Tests

Where appropriate, verify that converting a value to another unit and back reproduces the original value within the established numerical tolerance.

For example:

1. Convert 500 gpm to ft³/s.
2. Convert the resulting ft³/s value back to gpm.
3. Compare the result with 500 gpm.

### 9.3 Dimensional Tests

Tests should verify that hydraulic calculations use consistent units and produce physically reasonable results.

Examples include:

- Pipe velocity.
- Reynolds number.
- Major friction loss.
- Minor head loss.
- Pressure head.
- Hydraulic power.

### 9.4 Tolerances

Numerical acceptance tolerances should be established in the HAS verification plan.

Tests should use appropriate absolute and relative tolerances rather than requiring exact floating-point equality.

---

## 10. Documentation and Traceability

Conversion factors and physical constants should be documented with:

- Quantity name.
- Symbol.
- Numerical value.
- Units.
- Source.
- Applicable assumptions.
- Precision.
- Revision history, where applicable.

The primary source of unit definitions should be maintained in the software's centralized constants and conversion modules.

Engineering documentation should remain consistent with those implementations.

---

## 11. References

The following references may be used to establish unit definitions and conversion factors:

1. National Institute of Standards and Technology (NIST), *Guide for the Use of the International System of Units (SI)*, NIST SP 811.
2. NIST, *The International System of Units (SI): Conversion Factors for General Use*, NIST SP 1038.
3. U.S. Geological Survey, published water measurement and unit conversion references.
4. Recognized fluid mechanics and hydraulic engineering textbooks.
5. Hydraulic Institute engineering publications.

Relevant source editions and numerical values should be confirmed when constants and conversion factors are implemented.

---

## 12. Summary

The Hydraulic Analysis Suite initially adopts U.S. customary engineering units for hydraulic analysis and reporting.

The primary implementation requirements are:

1. Establish standardized internal calculation units.
2. Explicitly convert engineering inputs before calculations.
3. Maintain centralized conversion functions and physical constants.
4. Prevent inconsistent use of inches, feet, gallons, and cubic feet.
5. Distinguish physical constants from temperature-dependent fluid properties.
6. Document units for all calculation inputs and outputs.
7. Independently verify unit conversions and dimensional consistency.
8. Preserve the ability to introduce SI support in future development.

These conventions provide the foundation for reliable hydraulic calculations and consistent engineering documentation throughout HAS.
