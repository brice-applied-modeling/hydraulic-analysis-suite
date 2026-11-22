# Hydraulic Analysis Suite — Design Decisions

**Status:** Phase 0 decision record  
**Scope:** Major architecture and engineering-software tradeoffs

## How to use this record

Each decision has an identifier, status, rationale, consequences, and conditions for reconsideration. **Accepted** means the design direction is agreed, not that the current implementation already complies. Update the record when a decision changes; do not silently rewrite the project's history.

## DD-001 — Python 3.12 with a `src/` package layout

**Status:** Accepted

**Decision:** Develop HAS using Python 3.12 and the existing `src/` package structure, with automated tests separate from source code.

**Alternatives:** Flat script repository; other languages.

**Rationale:** Supports maintainability, testing, numerical libraries, and the project's Python learning objectives without sacrificing engineering clarity.

**Tradeoff:** Packaging and import configuration require more discipline than ad hoc scripts.

**Revisit when:** Deployment constraints or required integrations make the current layout impractical.

## DD-002 — U.S. customary internal calculation conventions

**Status:** Accepted; implementation audit pending

**Decision:** Use a consistent **ft–s–lbf** basis internally, with mass expressed in slugs where required. Convert common external units such as gpm, inches, psi, and hp at well-defined boundaries. SI support may be added through a conversion layer.

**Alternatives:** SI-only internal calculations; mixing convenient units throughout equations.

**Rationale:** Aligns with intended U.S. municipal engineering workflows and simplifies review against familiar hydraulic calculations.

**Tradeoff:** Requires careful distinction between lbm and lbf, and deliberate unit conversions. SI-based third-party libraries will require adapters.

**Rules:** Pressure internally in lbf/ft²; pressure head in ft; specific weight in lbf/ft³; mass density in slug/ft³. Gauge and absolute pressure must be identified where relevant. `HGL = elevation + pressure head` only when datum and pressure basis are consistent. Do not hard-code 2.31 ft/psi as universally valid.

**Revisit when:** An external integration justifies a different internal convention; such a change requires a documented migration and regression verification.

## DD-003 — Explicit units and structured results

**Status:** Accepted; detailed contract to be implemented

**Decision:** Public calculation boundaries must document units; results must include numerical values, units, status, warnings, assumptions, and references where applicable. Favor typed models/dataclasses once interfaces stabilize, without prematurely requiring a validation framework.

**Alternatives:** Bare floats and ad hoc dictionaries; mandatory Pydantic models throughout.

**Rationale:** Reduces ambiguity and makes reports, comparisons, and verification reproducible.

**Tradeoff:** Adds schema design and serialization work.

**Revisit when:** Result consumers demand a stable external API or schema versioning.

## DD-004 — Separate invalid inputs, numerical failures, and engineering warnings

**Status:** Accepted; implementation pending

**Decision:** Reject invalid inputs with descriptive exceptions. Identify numerical convergence/domain failures separately. Return mathematically valid results with design warnings when criteria are exceeded, rather than treating every warning as an exception.

**Examples:** Negative diameter = input error; missing required fitting coefficient = missing-data error; high velocity = engineering warning; failed numerical solver = calculation failure.

**Alternatives:** Silent defaults; generic errors; treating every criterion exceedance as fatal.

**Rationale:** Engineering review requires both transparency and access to computed values.

**Tradeoff:** More explicit error taxonomy and test cases.

**Revisit when:** A safety-critical or regulated workflow requires hard-stop criteria for specific conditions.

## DD-005 — Engineering equations independent of interfaces and file formats

**Status:** Accepted; implementation audit pending

**Decision:** Keep calculations and domain objects independent of CLI formatting, file I/O, and report generation. Use orchestration code to assemble scenarios and invoke calculations.

**Alternatives:** Monolithic command-line scripts; direct file reads inside domain classes.

**Rationale:** Makes the same calculation usable in CLI workflows, automated tests, and future applications.

**Tradeoff:** Introduces additional interfaces and module boundaries.

**Revisit when:** A boundary creates complexity without improving testability or reuse.

## DD-006 — Traceable reference data and explicit applicability

**Status:** Accepted; audit pending

**Decision:** Track sources, units, assumptions, and applicable ranges for constants, pipe dimensions, roughness data, and fitting coefficients. Do not silently replace missing coefficients with zero or treat nominal pipe size as actual inside diameter.

**Alternatives:** Unannotated hard-coded lookup tables; defaulting missing values.

**Rationale:** Hydraulic outputs are only as defensible as the underlying reference data.

**Tradeoff:** More metadata and maintenance effort.

**Revisit when:** A validated external engineering-data library can provide equal or better traceability.

## DD-007 — Verification is independent of implementation

**Status:** Accepted

**Decision:** Verify core equations using hand calculations, independently constructed spreadsheets, established benchmarks, and/or suitable external models. Store documented expected results and justified tolerances. A calculation generated by HAS is not independent verification of HAS.

**Alternatives:** Rely only on tests that reproduce implementation logic or compare outputs to prior HAS runs.

**Rationale:** Engineering credibility requires evidence beyond code execution.

**Tradeoff:** Verification cases take meaningful time to prepare and review.

**Revisit when:** New calculation families require different benchmark methods or specialist review.

## DD-008 — LS-001 is wholly fictional and reproducible

**Status:** Accepted; case buildout pending

**Decision:** Use LS-001 as an illustrative municipal wastewater lift station replacement case, with clearly labeled hypothetical inputs, boundary conditions, and verification results. Keep client/employer data and proprietary drawings out of the public example.

**Alternatives:** Real-project case study; arbitrary isolated test values without a system context.

**Rationale:** Supports an end-to-end demonstration without confidentiality concerns and provides repeatable engineering benchmarks.

**Tradeoff:** Hypothetical assumptions may not represent a particular utility's design criteria and must not be presented as such.

**Revisit when:** A separate case is explicitly authorized for public use and fully reviewed for disclosure.

## DD-009 — Incremental steady-state scope before advanced analyses

**Status:** Accepted

**Decision:** Prioritize pipe/fitting losses, boundary conditions, system curves, pump operating points, and wet-well calculations before expanding into transient simulation, uncertainty analysis, or digital-twin functionality. Document screening limitations instead of implying that steady-state calculations validate surge behavior.

**Alternatives:** Build a broad modeling platform immediately; integrate advanced solvers before core verification.

**Rationale:** Reduces technical risk and creates a defensible engineering foundation.

**Tradeoff:** Advanced features arrive later.

**Revisit when:** A verified use case requires a different sequence and appropriate specialized methods are available.

## DD-010 — Minimal dependencies and no premature framework commitment

**Status:** Accepted

**Decision:** Prefer Python standard-library facilities for basic models, exceptions, and conversions initially. Add external dependencies when their benefits are demonstrated through an actual requirement.

**Alternatives:** Adopt a full validation/API framework at the outset.

**Rationale:** Keeps the core transparent and easier to audit.

**Tradeoff:** Some validation and serialization utilities may initially be custom-built.

**Revisit when:** Complex schema validation, dimensional analysis, or integration requirements justify a maintained library.

## Open implementation questions

1. Exact public result model: dataclass, typed dictionary, or a validation library?
2. Final exception hierarchy and warning codes?
3. How should external units be expressed in Python APIs: explicit suffixes, unit arguments, or typed quantities?
4. Which existing class methods already conform to the internal unit convention?
5. What independent benchmark values and tolerances will be adopted for LS-001?

These are **implementation decisions still to be resolved**, not reasons to delay documenting the accepted architecture.

## Related documents

- `docs/01_architecture/00_project_structure.md`
- `docs/01_architecture/01_architecture_overview.md`
- `docs/01_architecture/03_numerical_conventions.md`
- `docs/02_engineering/09_verification_plan.md`
- `docs/02_engineering/10_ls001_design_basis.md`

## Change log

| Date | Change |
|---|---|
| 2026-11-22 | Initial Phase 0 design decisions recorded. |
