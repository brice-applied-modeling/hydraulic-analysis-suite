# Engineering Criteria and Requirements

## 1. Purpose

This document establishes the engineering criteria, regulatory considerations, technical references, and verification requirements that guide development of the Hydraulic Analysis Suite (HAS).

HAS is intended to support the analysis and preliminary design evaluation of municipal water and wastewater infrastructure, with an initial emphasis on force mains, wet wells, and pumping systems.

The document provides a framework for:

- Identifying applicable owner and utility requirements.
- Identifying relevant Florida regulations and engineering practice requirements.
- Establishing the use of manufacturer technical information.
- Maintaining traceability to recognized engineering references.
- Defining independent verification methods.
- Establishing numerical accuracy and acceptance criteria.
- Documenting assumptions, limitations, and engineering judgments.

The criteria described herein establish the basis for software development and verification. They do not constitute a complete design manual or replace project-specific engineering requirements.

---

## 2. Intended Use and Limitations

### 2.1 Intended Use

HAS is being developed as a Python-based engineering analysis tool to support:

1. Force-main hydraulic calculations.
2. Pipe and fitting head-loss calculations.
3. System head curve development.
4. Pump performance and operating point analysis.
5. Multiple-pump operating scenarios.
6. Wet-well storage and operating cycle analysis.
7. Hydraulic alternative comparisons.
8. Engineering calculation documentation.

These capabilities represent the intended development scope and should not be interpreted as functionality already implemented.

### 2.2 Engineering Applications

Potential applications include:

- Preliminary force-main sizing.
- Evaluation of pipe materials and internal diameters.
- Comparison of alternative pipeline configurations.
- Estimation of pumping head requirements.
- Evaluation of pump operating conditions.
- Preliminary lift station design studies.
- Identification of hydraulic constraints.
- Sensitivity analysis of selected design parameters.

### 2.3 Limitations

Unless specifically developed and independently verified, HAS shall not be assumed to provide:

- Hydraulic transient or water-hammer analysis.
- Computational fluid dynamics.
- Detailed wet-well flow pattern analysis.
- Complete collection-system network modeling.
- Structural design.
- Electrical system design.
- Automatic regulatory compliance certification.
- Manufacturer-certified equipment selection.

HAS calculations shall not be relied upon for final engineering design without appropriate verification, review, and professional engineering judgment.

---

## 3. Hierarchy of Engineering Requirements

Engineering requirements may originate from several sources.

For each project, the applicable requirements should be identified and documented before performing design calculations.

The following hierarchy is a general organizational framework, not an absolute legal order of precedence.

| Source | Typical Application |
|---|---|
| Applicable laws and regulations | Mandatory regulatory requirements |
| Permit conditions | Project-specific regulatory obligations |
| Owner and utility standards | Design and operational requirements |
| Adopted engineering codes and standards | Technical design criteria |
| Manufacturer requirements | Equipment-specific operating limitations |
| Recognized engineering references | Calculation methods and recommended practices |
| Project engineering judgment | Selection of assumptions and evaluation of alternatives |

Where requirements conflict, the applicable legal obligations, contract documents, and governing authority must be evaluated. A general hierarchy alone cannot resolve every conflict.

### 3.1 Requirements Traceability

Each significant engineering requirement should be documented with:

- Requirement identifier.
- Description.
- Source document.
- Applicable section or paragraph.
- Source edition or effective date.
- Applicability.
- Verification method.
- Verification status.

This approach provides a traceable relationship between engineering requirements, HAS calculations, and supporting references.

---

## 4. Owner and Utility Criteria

### 4.1 General Requirements

Owner criteria establish project-specific design and operating expectations.

These requirements may differ among municipalities, utilities, and infrastructure owners.

HAS should allow engineering inputs to be configured according to the applicable owner's requirements rather than assuming that one set of design criteria applies universally.

### 4.2 Force-Main Criteria

Typical owner requirements may address:

- Minimum and maximum operating velocities.
- Minimum allowable pipe diameter.
- Approved pipe materials.
- Pipe pressure class.
- Allowable head loss.
- Minimum cover.
- Air release and vacuum protection.
- Valve selection and spacing.
- Corrosion protection.
- Redundancy and reliability.
- Future capacity allowances.

These criteria should be evaluated individually for each project.

**HAS implementation consideration:** Owner-defined velocity limits, pipe material restrictions, and other configurable design criteria should be stored separately from the underlying hydraulic calculation methods.

### 4.3 Pumping System Criteria

Owner requirements may address:

- Design pumping capacity.
- Firm pumping capacity.
- Duty and standby arrangements.
- Acceptable pump types.
- Minimum solids-handling capability.
- Pump operating regions.
- Motor sizing.
- Variable-frequency drive requirements.
- Pump control strategies.
- Emergency power.
- Monitoring and instrumentation.

Pump operating limits should be evaluated using manufacturer data in addition to owner requirements.

### 4.4 Wet-Well Criteria

Wet-well requirements may include:

- Minimum operating volume.
- Maximum allowable pump starts per hour.
- Minimum pump submergence.
- Operating water levels.
- Emergency storage.
- Retention time.
- Odor and septicity considerations.
- Maintenance access.
- Wet-well geometry.
- Inflow characteristics.

Wet-well sizing should account for both hydraulic performance and operational reliability.

### 4.5 Project-Specific Criteria

For each engineering case study, the following should be documented:

| Parameter | Project Value | Source |
|---|---|---|
| Design flow | To be established | Owner/design basis |
| Peak flow | To be established | Flow analysis |
| Force-main velocity limits | To be established | Owner criteria |
| Approved pipe materials | To be established | Utility standards |
| Pumping redundancy | To be established | Owner/regulatory criteria |
| Wet-well operating levels | To be established | Design basis |
| Maximum pump starts | To be established | Manufacturer |
| Downstream HGL | To be established | Hydraulic analysis |
| Required design life | To be established | Owner criteria |

Values should not be assigned without an identified source or documented engineering assumption.

---

## 5. Florida Regulatory and Professional Requirements

### 5.1 General

Engineering projects in Florida may be subject to state regulations, permit conditions, local utility requirements, and professional engineering practice requirements.

Applicability depends on the infrastructure type, ownership, project location, and proposed improvements.

HAS should provide analytical support for evaluating applicable requirements but should not independently determine regulatory compliance.

### 5.2 Florida Department of Environmental Protection

Relevant regulatory frameworks may include:

**Florida Administrative Code, Chapter 62-604**

Collection Systems and Transmission Facilities.

This chapter is particularly relevant to domestic wastewater collection and transmission facilities, including applicable pumping and force-main facilities.

**Florida Administrative Code, Chapter 62-600**

Domestic Wastewater Facilities.

This chapter may be relevant where pumping infrastructure is associated with domestic wastewater treatment facilities.

**Florida Administrative Code, Chapter 62-555**

Permitting, Construction, Operation, and Maintenance of Public Water Systems.

This chapter may apply to public drinking water infrastructure and associated facilities.

**Florida Administrative Code, Chapter 62-610**

Reuse of Reclaimed Water and Land Application.

This chapter may apply to reclaimed-water systems and associated pumping and transmission infrastructure.

These references are starting points for determining applicability. The current rule text, incorporated standards, exemptions, and applicable permit requirements must be reviewed for each project.

### 5.3 Florida Professional Engineering Requirements

Relevant professional practice requirements include:

- Florida Statutes, Chapter 471: Engineering.
- Florida Administrative Code, Chapter 61G15: Rules of the Florida Board of Professional Engineers.

Engineering documents prepared for regulated professional practice must comply with applicable requirements for responsible charge, signing, sealing, and engineering documentation.

Use of HAS does not transfer engineering responsibility from the professional engineer to the software.

### 5.4 Local and Regional Requirements

Additional requirements may originate from:

- Municipal utilities.
- County utilities.
- Regional wastewater authorities.
- Local development standards.
- Utility technical specifications.
- Applicable permit conditions.

HAS should not embed local requirements as universal engineering constants.

---

## 6. Manufacturer Information

### 6.1 General

Manufacturer information provides equipment-specific performance characteristics and operating limitations.

This information is particularly important for pumps, motors, valves, and specialized hydraulic components.

### 6.2 Pump Performance Data

Pump information may include:

- Manufacturer and model.
- Pump type.
- Impeller diameter.
- Rotational speed.
- Head-capacity curve.
- Efficiency curve.
- Brake horsepower curve.
- NPSH required curve.
- Preferred operating region.
- Allowable operating region.
- Minimum continuous flow.
- Maximum allowable flow.
- Solids-handling characteristics.

Manufacturer performance data should be identified by model, configuration, revision, and applicable operating conditions.

### 6.3 Pipe and Fitting Data

Relevant information may include:

- Nominal pipe diameter.
- Actual internal diameter.
- Pipe material.
- Dimension ratio or pressure class.
- Surface roughness.
- Pressure and temperature limitations.
- Fitting loss coefficients.
- Valve flow coefficients.
- Manufacturer installation requirements.

### 6.4 Data Traceability

Each manufacturer-derived value should include, where available:

- Manufacturer.
- Product identification.
- Source document.
- Document revision or publication date.
- Applicable operating conditions.
- Units.
- Data limitations.

Manufacturer values should not be represented as universally applicable where they depend on equipment configuration or operating conditions.

---

## 7. Engineering References and Standards

### 7.1 Hydraulic Calculation References

Relevant references include:

- Crane Technical Paper No. 410, *Flow of Fluids Through Valves, Fittings, and Pipe*.
- Idelchik, *Handbook of Hydraulic Resistance*.
- Standard fluid mechanics textbooks covering conservation of energy, friction losses, and pipe-flow hydraulics.
- Applicable AWWA standards and manuals.

### 7.2 Pumping System References

Relevant references include:

- Hydraulic Institute pump standards and guidelines.
- ANSI/HI 9.6.1, NPSH Margin.
- ANSI/HI 9.6.3, Operating Regions.
- ANSI/HI 9.8, Pump Intake Design.
- ANSI/HI 14.6, Hydraulic Performance Acceptance Tests.
- Manufacturer pump selection and installation manuals.

The current applicable editions and exact titles should be confirmed before referencing them in formal engineering documentation.

### 7.3 Water and Wastewater References

Relevant resources may include:

- Water Environment Federation design manuals.
- AWWA manuals and standards.
- U.S. EPA technical publications.
- FDEP regulations and technical guidance.
- Utility-specific design manuals.

### 7.4 Reference Selection

References should be selected according to the calculation being performed.

For each implemented method, document:

1. Governing equation.
2. Source reference.
3. Applicable assumptions.
4. Input requirements.
5. Units.
6. Limitations.
7. Independent verification method.

---

## 8. Independent Verification Methods

### 8.1 Verification Objectives

Independent verification is intended to establish that HAS calculations are implemented correctly and produce results consistent with established hydraulic principles.

Verification should distinguish among:

- **Software verification:** Whether equations and algorithms are implemented correctly.
- **Model validation:** Whether the selected mathematical representation adequately reflects the physical system.
- **Engineering review:** Whether assumptions and results are appropriate for the intended application.

Agreement with another software package alone does not establish physical validity.

### 8.2 Analytical Verification

Where possible, compare HAS results against independently calculated solutions.

Examples include:

- Continuity equation.
- Velocity calculations.
- Reynolds number.
- Darcy-Weisbach head loss.
- Minor losses.
- Total dynamic head.
- Hydraulic power.

Independent calculations should avoid simply reproducing the same software implementation.

### 8.3 Reference Software Comparison

Where appropriate, compare results with established software such as:

- EPANET.
- Bentley WaterGEMS.
- Bentley SewerGEMS.
- Manufacturer pump selection software.

Comparisons must use consistent boundary conditions, pipe properties, loss coefficients, calculation methods, and units.

Reference software results should not be assumed correct without checking their input assumptions and applicability.

### 8.4 Manufacturer Data Verification

Pump and equipment calculations should be compared against manufacturer performance data.

Examples include:

- Pump curve interpolation.
- Operating point determination.
- Efficiency calculations.
- Brake horsepower.
- NPSH requirements.
- Multiple-pump operating conditions.

### 8.5 Experimental or Field Data

Where reliable field data are available, compare predicted results against measured conditions.

Potential measurements include:

- Flow.
- Pressure.
- Wet-well elevation.
- Pump speed.
- Electrical power.
- Pump operating duration.

Measurement uncertainty, instrument calibration, and operating conditions should be documented.

### 8.6 Automated Software Testing

HAS should incorporate automated tests for:

- Unit conversions.
- Valid input ranges.
- Hydraulic calculations.
- Numerical edge cases.
- Invalid input handling.
- Regression testing.
- Known reference cases.

Tests should be maintained as part of the software development process.

---

## 9. Numerical Tolerances and Engineering Allowances

### 9.1 General

Numerical tolerances and engineering design allowances serve different purposes.

**Numerical tolerances** define acceptable differences between computed and independently verified results.

**Engineering allowances** account for physical uncertainty, operational variability, or design requirements.

They should not be treated interchangeably.

### 9.2 Numerical Verification Tolerances

Numerical tolerances should be established for each calculation or verification case.

For example:

\[
|x_{HAS}-x_{reference}|\leq T
\]

Where:

- \(x_{HAS}\) = calculated HAS result.
- \(x_{reference}\) = independently established reference result.
- \(T\) = applicable absolute tolerance.

A combined absolute and relative tolerance may be used:

\[
|x_{HAS}-x_{reference}|
\leq
T_{abs}+T_{rel}|x_{reference}|
\]

The selected tolerances should reflect numerical precision, calculation method, input data quality, and the intended application.

### 9.3 Proposed Initial Software Verification Targets

The following values are preliminary development targets, not regulatory or industry-mandated acceptance criteria.

| Calculation | Proposed Verification Target |
|---|---|
| Unit conversions | Relative tolerance of 1×10⁻¹⁰, where appropriate |
| Flow velocity | Relative tolerance of 1×10⁻⁶ |
| Reynolds number | Relative tolerance of 1×10⁻⁶ |
| Major head loss | Relative tolerance of 1×10⁻⁴ |
| Minor head loss | Relative tolerance of 1×10⁻⁴ |
| Total dynamic head | Relative tolerance of 1×10⁻⁴ |
| Hydraulic power | Relative tolerance of 1×10⁻⁴ |

These are proposed targets for deterministic analytical tests using consistent inputs and methods.

They should be reviewed and adjusted based on the independent verification method, numerical algorithm, and expected precision.

Near-zero results require appropriate absolute tolerances.

Pump curve interpolation, iterative solutions, and cross-software comparisons require separately established acceptance criteria.

### 9.4 Engineering Allowances

Engineering allowances may address:

- Pipe aging and roughness.
- Manufacturing tolerances.
- Pump performance variability.
- Flow projection uncertainty.
- Downstream hydraulic conditions.
- Future system expansion.
- Operational variability.

These allowances should be established through project-specific design criteria and documented assumptions.

### 9.5 Uncertainty Analysis

Future HAS capabilities may incorporate sensitivity analysis and uncertainty evaluation.

Potential uncertain inputs include:

- Pipe roughness.
- Design flow.
- Downstream HGL.
- Pump efficiency.
- Minor loss coefficients.
- Wet-well operating conditions.

Monte Carlo analysis may eventually be used to quantify the effect of these uncertainties on hydraulic performance.

This functionality is not assumed to exist in the current HAS implementation.

---

## 10. Requirements Management

### 10.1 Requirement Identification

Engineering and software requirements should be assigned unique identifiers.

Suggested categories include:

- HYD: Hydraulic calculations.
- PIP: Pipe and fitting data.
- PMP: Pump performance.
- WET: Wet-well calculations.
- REG: Regulatory considerations.
- VER: Verification requirements.
- DOC: Documentation requirements.

### 10.2 Example Requirements Register

| ID | Requirement | Verification |
|---|---|---|
| HYD-001 | Calculate flow velocity using actual internal pipe diameter | Analytical test |
| HYD-002 | Calculate Darcy-Weisbach friction loss | Independent calculation |
| HYD-003 | Calculate total dynamic head | Reference case |
| PIP-001 | Maintain traceable pipe material data | Data audit |
| PMP-001 | Calculate pump operating point from pump and system curves | Manufacturer/reference case |
| PMP-002 | Calculate hydraulic and brake horsepower | Analytical test |
| VER-001 | Maintain automated verification cases | Test suite |
| DOC-001 | Document assumptions and limitations | Documentation review |
| REG-001 | Identify applicable regulatory criteria for each case study | Engineering review |

These requirements represent intended capabilities and should not be interpreted as completed implementations.

### 10.3 Requirement Status

Requirements should be tracked using clearly defined states, such as:

- Proposed.
- Approved.
- Implemented.
- Verified.
- Deferred.

A requirement should not be considered verified solely because the associated code has been written.

---

## 11. Engineering Documentation Requirements

Each engineering analysis should record:

- Analysis identifier.
- Purpose.
- Date and software version.
- Input data.
- Units.
- Hydraulic assumptions.
- Calculation methods.
- Engineering references.
- Boundary conditions.
- Results.
- Verification status.
- Limitations.
- Reviewer information, where applicable.

The goal is to make calculations reproducible, auditable, and understandable to another engineer.

---

## 12. Summary

The Hydraulic Analysis Suite should be developed using a documented engineering design basis, traceable reference data, and independently verified calculation methods.

The primary requirements are:

1. Apply established hydraulic engineering principles.
2. Distinguish regulatory requirements from owner criteria and recommended practices.
3. Maintain traceability to manufacturer information and engineering references.
4. Separate numerical verification tolerances from engineering design allowances.
5. Establish independent verification methods for each major calculation.
6. Document assumptions, limitations, and applicable operating conditions.
7. Maintain a requirements register linking intended functionality to implementation and verification evidence.

These practices establish a foundation for developing HAS as a technically credible engineering analysis tool while supporting future expansion into pump selection, wet-well analysis, and infrastructure decision analytics.
