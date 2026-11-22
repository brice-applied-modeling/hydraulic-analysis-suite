# Audit Report — `src/hydraulic_analysis_suite/data/`

| Item | Value |
|---|---|
| Audit date | 2026-11-22 |
| Branch / commit | `dev` @ `b1cd425` (working tree with uncommitted docs) |
| Scope | `data/constants.py`, `data/pipe_materials.py`, `data/fittings_data.py`, `data/fitting_coefficients.py`; supporting `utils/fitting_data_helper_calcs.py`; consumers in `models/pipe.py`; related tests |
| Checklist items | Phase 0 → Software Implementation: audit `constants.py` for units, gravity, viscosity, and traceable sources; audit the pipe-material and fitting modules for applicability and provenance |
| Criteria applied | `docs/02_engineering/02_units.md`, `00_design_basis.md`, `03_criteria_and_requirements.md` §9, `09_verification_plan.md` §6, `docs/01_architecture/03_numerical_conventions.md`, `docs/09_references/00_references.md` |
| Status | **Not accepted for verified calculations.** Fine for scaffolding and preliminary or illustrative use once the High findings are fixed. |

---

## 1. Executive Summary

The data layer has a sensible structure: centralized dictionaries, snake_case keys, and per-entry source fields for fittings. The values and provenance do not yet meet the project's own design basis and verification plan. The main problems:

1. **Stored K-factors do not match the stated derivation basis.** `fittings_data.py` and the helper module say K = f_T × (Le/D) at an 8-inch basis. The stored K values are 1.2× to 2.0× higher than that, and the f_T they imply ranges from 0.011 to 0.030 between entries. The `k_factor` and `equivalent_length_ratio` fields give different head losses for the same fitting (F-FIT-01).
2. **Viscosity is missing, and density is mislabeled.** `constants.py` defines no viscosity, which the checklist asks for explicitly. `WATER_DENSITY_LB_PER_FT3 = 62.4` is a specific weight (lbf/ft³) labeled as a density, with no reference temperature (F-CON-01, F-CON-02).
3. **Conversion factors are too coarse for the project's own tolerances.** `GPM_TO_CFS = 0.002228` has a relative error of 4.2 × 10⁻⁶, above the 1 × 10⁻¹⁰ unit-conversion tolerance and the 1 × 10⁻⁶ velocity tolerance in `03_criteria_and_requirements.md` §9. `Pipe.velocity_fps` inherits this error directly (F-CON-03).
4. **Pipe-material values have no per-entry provenance.** Some lining and roughness pairings are also inconsistent (ductile iron uses unlined cast-iron roughness with a lined-pipe C-factor). Nothing captures actual internal diameter, pressure class, or DR, even though the design basis says nominal ID ≠ actual ID (F-MAT-01 to F-MAT-04).
5. **`verify_database_values()` is a tautology.** It compares `FITTINGS_DATA` with a hand-copied list of the same numbers and never with the computed `K_*` values sitting in the same file. That is why 159 tests pass while F-FIT-01 exists (F-FIT-02).
6. **`fitting_coefficients.py` is an empty file**, yet the strategy docs name it as the home of minor-loss data (F-FIT-08).

Finding counts: **High 7 · Medium 12 · Low 10**.

---

## 2. Method

1. Read each module line by line and checked units, naming, docstrings, and source attributions.
2. Recomputed every conversion factor from its exact definition: 1 US gal = 231 in³ exactly; 1 hp = 550 ft·lbf/s = 745.699872 W; g₀ = 32.17405 ft/s².
3. Re-derived the Hazen-Williams and Manning US-unit coefficients.
4. Recomputed Crane f_T = [−2 log₁₀(ε / 3.7D)]⁻² for D = 4–24 in with ε = 0.00015 ft, and compared f_T × Le/D against stored K.
5. Traced how the data is consumed (`models/pipe.py`, `utils/fitting_data_helper_calcs.py`) and reviewed the tests in `tests/data/` and `tests/utils/`.
6. Ran the test suite.

**Limitation:** The auditor did not have physical copies of Crane TP-410, the HI Engineering Data Book, Sanks *Pumping Station Design* (3rd ed.), or the AWWA manuals. Where a finding relies on recalled published values, it is marked **[CONFIRM vs SOURCE]**. Those findings mean the values need checking against a copy. They are not confirmed errors.

### 2.1 Test-suite observation

| Command | Result |
|---|---|
| `uv run pytest -q` | **5 collection errors**: `ModuleNotFoundError: No module named 'hydraulic_analysis_suite'` |
| `PYTHONPATH=src uv run pytest -q` | 159 passed |

`pyproject.toml` has no `[build-system]` table, so `uv` does not install the package into the environment. Either add a build backend (for example, setuptools) so the project installs in editable mode, or add `pythonpath = ["src"]` under `[tool.pytest.ini_options]`. **(F-ENV-01, Medium)**

---

## 3. `constants.py`

### 3.1 Recomputed values

| Constant | Stored | Reference value | Relative error | Verdict |
|---|---|---|---|---|
| `GPM_TO_CFS` | 0.002228 | 231/1728/60 = 0.00222800926 | −4.2 × 10⁻⁶ | Fails 1e-10 conversion tolerance |
| `CFS_TO_GPM` | 448.831 | 448.831169 | −3.8 × 10⁻⁷ | Fails 1e-10; not the reciprocal of `GPM_TO_CFS` (1/0.002228 = 448.8330) |
| `GALLONS_PER_CUBIC_FOOT` | 7.48052 | 1728/231 = 7.48051948 | 6.9 × 10⁻⁸ | Fails 1e-10; derive exactly instead |
| `HP_TO_KW` | 0.746 | 0.745699872 | 4.0 × 10⁻⁴ | Fails 1e-10 and the 1e-4 power tolerance |
| `KW_PER_HP` | 1.341 | hp per kW = 1.34102209 | — | **Name and docstring are inverted** (see F-CON-04) |
| `GRAVITY_FTPS2` | 32.174 | 32.17404856 | −1.5 × 10⁻⁶ | Acceptable if documented as a rounded value of g₀ |
| `HAZEN_WILLIAMS_US_COEFFICIENT` | 4.727 | 4.72733 (from V = 1.318 C R⁰·⁶³ S⁰·⁵⁴) | −7 × 10⁻⁵ | Acceptable; unit form and exponents are not documented |
| `MANNING_US_COEFFICIENT` | 1.49 | 1.486 (= 3.28084^(1/3)) | +2.7 × 10⁻³ | Rounded value; would fail a 1e-4 head-loss benchmark that uses 1.486 |
| `THREE_PHASE_FACTOR` | 1.732 | √3 = 1.7320508 | −2.9 × 10⁻⁵ | Wrong docstring (see F-CON-06) |
| `PI` | 3.141592653589793 | `math.pi` | 0 | Redundant |
| `INCHES_PER_FOOT`, `FEET_PER_MILE`, `SQFT_PER_ACRE`, `SECONDS_PER_DAY` | — | Exact | 0 | OK |

### 3.2 Findings

**F-CON-01 — No viscosity defined. (High)**
The checklist requires viscosity, and the design basis (§6.4–6.5) and units doc (§6.3) depend on it for Reynolds number and Darcy-Weisbach. `constants.py` has no dynamic or kinematic viscosity, and the project has no fluid-property module.
*Action:* Create a separate `fluid_properties.py` (or a `FluidProperties` dataclass) for temperature-dependent values: ρ (slug/ft³), γ (lbf/ft³), μ (lbf·s/ft²), ν (ft²/s), vapor pressure, and reference temperature, each with a cited source and temperature. Keep `constants.py` for true constants and exact conversions. Example reference point to cite and verify: ν ≈ 1.08 × 10⁻⁵ ft²/s at 68 °F, as given in `02_units.md` §6.3. For LS-001 (Florida wastewater), choose and document a design temperature. Ambient sewage there commonly runs 70–85 °F, where ν is roughly 0.9–1.0 × 10⁻⁵ ft²/s **[CONFIRM vs SOURCE]**.

**F-CON-02 — `WATER_DENSITY_LB_PER_FT3` conflates density and specific weight. (High)**
The docstring reads "Density/specific weight … (lb/ft³)". In the project's coherent ft–lbf–s system, density must be in slug/ft³ (`03_numerical_conventions.md` lines 22–31), and 62.4 is a specific weight in lbf/ft³. "lb" does not say whether it means lbm or lbf. No temperature is stated. 62.4 matches water near 50–60 °F, while 62.2–62.3 is typical at 70–85 °F **[CONFIRM vs SOURCE]**.
*Action:* Replace it with `WATER_SPECIFIC_WEIGHT_LBF_PER_FT3` and `WATER_DENSITY_SLUG_PER_FT3` in the fluid-property module, each with a reference temperature. Record which value the 2.31 ft/psi rule assumes.

**F-CON-03 — Truncated conversion factors break the stated tolerances. (High)**
`GPM_TO_CFS`, `CFS_TO_GPM`, `GALLONS_PER_CUBIC_FOOT`, and `HP_TO_KW` are rounded literals. They are not reciprocals of each other, so a gpm → cfs → gpm round trip leaves a residual of about 4.5 × 10⁻⁶. `02_units.md` §9.2 requires round-trip tests, and §9 sets the conversion tolerance at 1e-10. `Pipe.flow_rate_cfs` and `Pipe.velocity_fps` (`models/pipe.py:241`, `:259`) carry the 4.2 × 10⁻⁶ error into velocity, which exceeds the 1e-6 velocity tolerance. Any independent benchmark built on exact conversions will fail.
*Action:* Derive the factors from exact definitions:
```python
CUBIC_INCHES_PER_US_GALLON = 231.0            # exact by definition
CUBIC_INCHES_PER_CUBIC_FOOT = 1728.0          # exact
GALLONS_PER_CUBIC_FOOT = CUBIC_INCHES_PER_CUBIC_FOOT / CUBIC_INCHES_PER_US_GALLON
CFS_TO_GPM = GALLONS_PER_CUBIC_FOOT * 60.0
GPM_TO_CFS = 1.0 / CFS_TO_GPM
FT_LBF_PER_S_PER_HP = 550.0                   # exact (mechanical hp)
W_PER_HP = 745.69987158227022                 # NIST SP 811
```

**F-CON-04 — `KW_PER_HP = 1.341` is inverted. (High)**
1.341 is horsepower per kilowatt, yet the name and docstring say "kilowatts per horsepower". `HP_TO_KW × KW_PER_HP = 1.000386`, not 1. The strategy doc (`06_strategey/02_constants.md`) calls the same constant `KW_TO_HP`, so names have drifted between code and docs. A future power calculation that uses `KW_PER_HP` as its name suggests would be wrong by a factor of 1.8.
*Action:* Rename to `HP_PER_KW` (or `KW_TO_HP`), define it as `1 / HP_TO_KW`, and pick one naming convention (`A_TO_B` or `B_PER_A`) for the whole module.

**F-CON-05 — Sources cannot be traced. (Medium)**
The module header lists "Pumping Station Design, Hydraulic Institute Standards, AWWA Manuals" with no edition, section, or link to any constant. `02_units.md` §10–11 requires each constant to list its quantity, symbol, value, units, source, assumptions, and precision, and names NIST SP 811 and SP 1038 as conversion sources.
*Action:* Give each constant a docstring with units, source (NIST SP 811 for conversions), whether it is exact or rounded, and the precision it carries.

**F-CON-06 — `THREE_PHASE_FACTOR` docstring is wrong. (Medium)**
The docstring says "Three-phase power factor". The value is √3, the line-to-phase voltage ratio, not a power factor. That is easy to confuse with `DEFAULT_POWER_FACTOR`, which sits right below it.
*Action:* Rename to `SQRT_3`, set it to `math.sqrt(3)`, and change the docstring to "√3, used in P = √3·V_LL·I·PF".

**F-CON-07 — Hazen-Williams coefficient has no unit contract. (Medium)**
The value 4.727 is correct only for hf = 4.727 · L · Q^1.852 / (C^1.852 · D^4.87), with Q in cfs, L and D in ft, and hf in ft. Other common forms (10.44 with gpm and inches; 4.52 with gpm, inches, and psi per ft) give wrong results if mixed up. The exponents are not centralized either.
*Action:* Document the equation form, units, and exponents next to the coefficient, or move the coefficient into `equations/hazen_williams.py` with its unit contract. Add a benchmark case.

**F-CON-08 — Manning coefficient is rounded and out of scope. (Low)**
1.49 versus 1.486. Manning applies to open-channel and gravity flow, which v1 excludes (`00_design_basis.md` §2). Keep it only if it is documented as the rounded 1.486, or switch to 1.486.

**F-CON-09 — Redundant or misplaced items. (Low)**
- `PI` duplicates `math.pi`. `pipe.py` already uses `math.pi`, so the constant is unused. Remove it.
- `DEFAULT_POWER_FACTOR = 0.80` is a project assumption, not a constant. Move it to a case- or assumption-level configuration so it appears in outputs, as `03_numerical_conventions.md` requires.
- `SQFT_PER_ACRE` and `SECONDS_PER_DAY` are unused but harmless.

**F-CON-10 — Constants needed for v1 are missing. (Medium)**
These are required by the design basis or numerical conventions but not defined:
- `PSF_PER_PSI = 144` (exact)
- the 550 ft·lbf/s-per-hp definition
- the 3960 water-horsepower constant, labeled as rounded
- `LBM_PER_SLUG = 32.17405`
- standard atmospheric pressure (needed for NPSH)
- `GPM_PER_MGD`

`02_units.md` §8.1 also calls for named conversion functions (`gpm_to_cfs()`, `psi_to_head(γ)`, and others), and none exist yet.

---

## 4. `pipe_materials.py`

### 4.1 Findings

**F-MAT-01 — No per-entry provenance. (High)**
None of the 33 entries has a `source` or `source_reference` field, although `fittings_data.py` sets that pattern. The module header lists five sources, two of which (Crane TP-410 and "Manning and Hazen-Williams reference tables") are not specific publications. The provenance checklist item cannot be signed off.
*Action:* Add `source`, `source_reference` (table or page), `condition_basis`, and `notes` fields to each entry, and extend `tests/data/test_pipe_materials.py` to require them, matching what `test_fittings_data.py` does.

**F-MAT-02 — Ductile-iron entries mix lined and unlined properties. (High)**
`dip_excellent` pairs C = 140, a value typical of cement-mortar-lined pipe, with ε = 0.00085 ft, which is the classic Moody value for unlined cast iron (0.26 mm) and identical to `cast_iron_unlined_excellent`. DIP in water and wastewater service is almost always cement-lined; ceramic-epoxy and polyethylene linings are common in sewer service. The lining determines roughness, and no entry identifies it. The fair and poor Manning n values of 0.020 and 0.030 are far above typical pipe-flow ranges; 0.030 is in natural-channel territory **[CONFIRM vs SOURCE]**.
*Action:* Key DIP entries by lining (`dip_cement_lined`, `dip_ceramic_epoxy_lined`) instead of condition, cite AWWA M41 or DIPRA for C and ε, and explain the n values or remove them.

**F-MAT-03 — C, ε, and n are presented as interchangeable even though they are independent. (Medium)**
Each record holds a Hazen-Williams C, a Darcy ε, and a Manning n as though they describe one physical state. They come from different empirical traditions and are not mutually consistent. For example, `dip_good` has C = 130 and n = 0.015, while `cast_iron_unlined_good` has C = 120 and n = 0.013. A reader comparing a Hazen-Williams result with a Darcy-Weisbach result for the same "material" may see differences that come from the data, not the equations. `09_verification_plan.md` requires such differences to be explained.
*Action:* Document each parameter's source separately, and state that cross-method comparisons need care.

**F-MAT-04 — No actual ID, pressure class, or DR. (High for force-main work)**
`02_units.md` §5.1 and the design basis say nominal diameter must not be treated as internal diameter. `Pipe` takes `diameter_in` and uses it directly for area and velocity. For PVC C900 DR18 at 6-inch nominal, the ID is about 6.09 in (DIPS OD of 6.90 in), roughly a 3% difference in area and velocity **[CONFIRM vs SOURCE, AWWA C900]**. LS-001 lists PVC with "pressure class, roughness TBD".
*Action:* Out of scope for this dictionary, but add a pipe-dimension table (standard, nominal size, DR or pressure class, OD, ID) or require an explicit `inside_diameter_in` input. Track this under checklist item 1.1, "Confirm internal vs nominal diameter".

**F-MAT-05 — Single fixed C values are not suitable for force-main design. (Medium)**
Pump selection needs both ends of the system curve: a low C (aged or slimed, giving maximum TDH) and a high C (new pipe, giving minimum TDH or runout). Many owners and design guides specify a design C for wastewater force mains of about 120–140, whatever the material, because of slime and grease buildup **[CONFIRM vs owner criteria / Ten States Standards]**. `PVC = 150` used as a design default would underestimate TDH.
*Action:* Keep the material table as reference data, and take design C, ε, and n ranges from the case or criteria inputs (`03_criteria_and_requirements.md`). Show which value was used in outputs.

**F-MAT-06 — Low Manning n for PVC and HDPE. (Medium)**
n = 0.009 is at the very low end. Design practice and many agencies use 0.010–0.013 for plastic gravity pipe **[CONFIRM vs SOURCE]**. This matters less in v1 (no gravity scope), but the value should be flagged.

**F-MAT-07 — Questionable or unverified values. (Medium, all [CONFIRM vs SOURCE])**
- `corrugated_metal_pipe`: ε = 0.008 ft is low for typical corrugation profiles, and C = 60 has no source.
- `steel`: C = 120 with ε = 0.00015 ft mixes an aged C with a new-pipe ε.
- `aluminum` C = 130, `brass` and `bronze` C = 130, and `stainless_steel` ε = 7 × 10⁻⁶ ft each need a citation.
- `brick_sewer_old` ε = 0.010 ft and `concrete_sewer_aged` values need a citation.

**F-MAT-08 — Duplicate entries. (Low)**
`rcp`, `concrete`, and `concrete_sewer` are identical (C 120, ε 0.0010, n 0.013). Merge them, or document what distinguishes each.

**F-MAT-09 — Typo and broken reference. (Low)**
- Category `"abestos_cement"` should be `"asbestos_cement"` (lines 118, 127, 136).
- The comment at line 206 cites `docs/02_strategy/01_pipe_materials.md`. The actual path is `docs/06_strategey/01_pipe_materials.md`, and the directory name `strategey` is itself misspelled.
- The CIP condition and age table in that doc has no source, and DIP conditions are not defined anywhere.

**F-MAT-10 — The condition model is age-only. (Low)**
The module docstring correctly says age is a proxy. Still, `excellent/good/fair/poor` have no defined C or ε ranges or sources. Consider replacing the condition keys with an explicit `c_factor_range` and a cited "new" value.

**F-MAT-11 — Plausibility tests are wide. (Low)**
`test_c_factor_in_engineering_range` accepts 50–165. That catches typos but not wrong values. Once sources are attached, add per-entry expected-value tests.

---

## 5. `fittings_data.py` and `utils/fitting_data_helper_calcs.py`

### 5.1 Stored K versus the stated Crane basis

The module docstring and the helper say K = f_T × (Le/D) with f_T taken at an 8-inch basis and ε = 0.00015 ft. Recomputed f_T values:

| D (in) | 4 | 6 | **8** | 12 | 16 | 24 |
|---|---|---|---|---|---|---|
| f_T | 0.0163 | 0.0149 | **0.0141** | 0.0130 | 0.0123 | 0.0114 |

| Entry | Le/D | Stored K | f_T(8") × Le/D | Stored ÷ computed | Implied f_T = K ÷ (Le/D) |
|---|---|---|---|---|---|
| 11.25° std | 4 | 0.10 | 0.056 | 1.78 | 0.025 |
| 22.5° std | 8 | 0.20 | 0.113 | 1.78 | 0.025 |
| 45° std | 16 | 0.40 | 0.225 | 1.78 | 0.025 |
| 90° std | 30 | 0.75 | 0.422 | 1.78 | 0.025 |
| 11.25° LR | 2 | 0.06 | 0.028 | 2.13 | 0.030 |
| 22.5° LR | 5 | 0.12 | 0.070 | 1.70 | 0.024 |
| 45° LR | 10 | 0.20 | 0.141 | 1.42 | 0.020 |
| 90° LR | 16 | 0.45 | 0.225 | 2.00 | 0.028 |
| Tee run | 20 | 0.60 | 0.281 | 2.13 | 0.030 |
| Tee branch | 60 | 1.80 | 0.844 | 2.13 | 0.030 |
| Gate valve | 8 | 0.10 | 0.113 | 0.89 | 0.0125 |
| Plug valve | 18 | 0.20 | 0.253 | 0.79 | 0.011 |
| Swing check | 135 | 2.30 | 1.899 | 1.21 | 0.017 |
| Tee combining* | 80 | 2.00 | — | — | 0.025 |
| Wye standard* | 30 | 0.50 | — | — | 0.017 |
| Wye combination* | 50 | 0.75 | — | — | 0.015 |
| Butterfly* | 45 | 0.60 | — | — | 0.013 |
| Ball check* | 150 | 4.00 | — | — | 0.027 |

\* Not Crane-derived according to the helper.

No single pipe size from 4 to 24 inches reproduces the stored K values. Most bends and tees imply f_T ≈ 0.025–0.030, which corresponds to about a 1-inch pipe. The gate and plug valves imply about 0.011–0.0125, which corresponds to pipe larger than 24 inches.

**Worked impact (LS-001-like case):** 450 gpm in a 6.00-inch ID pipe gives V = 5.11 ft/s and V²/2g = 0.405 ft. For one 90° standard elbow:
- Stored K (0.75): h = **0.304 ft**
- Crane f_T(6") × 30 = 0.448: h = **0.181 ft**
- Le/D × actual f (smooth PVC, Re ≈ 2.4 × 10⁵, f ≈ 0.015): h = **0.182 ft**

The `k_factor` and `equivalent_length_ratio` fields of the same record differ by about 67%.

**F-FIT-01 — K and Le/D are inconsistent with each other and with the documented basis. (High)**
See the tables above. The docstrings ("K is representative for mid-range pipe diameters; compute K = f_T × (Le/D)") claim a relationship the data does not satisfy. The helper's derivation table labels K as "derived", while `verify_database_values()` calls the same values "fixed-K textbook reference values, not computed from the Crane method". The two statements contradict each other, and neither names the textbook the fixed K values came from.
*Action:* Choose one method and document it:
- **(a) Crane method (recommended):** store only Le/D from Crane, compute K at run time from the actual ID, and drop the stored K. Or store K equal to the computed `K_*` values and record the diameter basis.
- **(b) Fixed-K method:** keep the K values, cite the specific source and table (for example, a Sanks or HI table and page), and drop or relabel Le/D as "Crane Le/D (independent of K)".

Whichever you choose, the calculator must use only one field, and the output must say which.

**F-FIT-02 — `verify_database_values()` and its tests are circular. (High)**
`_CRANE_DERIVED_CHECKS` hand-copies the same K and Le/D values that are in `FITTINGS_DATA` and compares them with exact `==`. It never compares stored K against `K_45_DEG_STANDARD` and the other computed constants in the same module. Those constants are computed and then never used. The check can only detect edits, not errors, so 159 passing tests give false assurance on F-FIT-01. Exact float equality also goes against `02_units.md` §9.4.
*Action:* Assert `FITTINGS_DATA[k]["k_factor"] ≈ F_T_REPRESENTATIVE × Le/D` (with `pytest.approx` at a documented rounding tolerance) for every Crane-sourced entry. Under option (b) of F-FIT-01, add an independent fixed-K benchmark instead.

**F-FIT-03 — Partial-angle Le/D derivation does not match its description. (Medium)**
The fittings docstrings say "interpolated from 45-degree and 90-degree … values". The helper actually scales linearly from (0°, 0) to (45°, Le/D₄₅) and never uses the 90° point (its comment at lines 200–204 acknowledges this). Bend loss is not linear in angle. Crane's multi-bend formula and published 22.5° and 11.25° data show a non-zero intercept, so straight proportional scaling can understate loss at small angles **[CONFIRM vs SOURCE]**. In addition, `LE_D_11_25_LONG = round(2.5)` gives **2** because Python rounds half to even, and that 20% truncation is not documented.
*Action:* Correct the docstrings to "proportional scaling from 0°–45°, auditor-derived, not Crane". Use a published source for 11.25° and 22.5° bends if one exists (manufacturer or AWWA data), and keep the unrounded values or round deliberately.

**F-FIT-04 — Bend geometry labels may not match Crane categories. (Medium, [CONFIRM vs SOURCE])**
From the auditor's recollection of TP-410 (A-27 to A-29 series):
- **30 f_T** and **16 f_T** belong to standard threaded elbows, not to r/D = 1.0 bends.
- Flanged or butt-welded 90° bends are listed by r/D: about 20 f_T at r/D = 1 and about 14 f_T at r/D = 1.5.
- Gate valve 8 f_T, plug valve straight-way 18 f_T, and tee run 20 f_T / branch 60 f_T agree with recollection.
- **Swing check:** recollection is 100 f_T (50 f_T for clearway) in current editions. Le/D = 135 appears to come from older or alternative tables.
- **Butterfly:** Crane gives about 45 f_T for 2–8 in, 35 f_T for 10–14 in, and 25 f_T for 16–24 in. The stored Le/D = 45 matches the small-size band but is attributed to HI.

Pump-station piping is mostly flanged or MJ ductile iron, so "standard radius = threaded elbow" values are likely conservative but are mislabeled.
*Action:* Check each Le/D against the specific TP-410 printing in use, record the printing year (see `00_references.md`), and record the table and row in `source_reference`. Add a size-band field where Crane values vary with size, as for butterfly valves.

**F-FIT-05 — Reducers and expansions use fixed K and a meaningless Le/D. (Medium)**
K depends strongly on β = d₁/d₂ and cone angle θ. Crane-type formulas, cited from recollection **[CONFIRM vs SOURCE]**: contraction (θ ≤ 45°) K₁ ≈ 0.8 sin(θ/2)(1 − β²); enlargement K₁ ≈ 2.6 sin(θ/2)(1 − β²)². For β = 0.5 and θ = 45° these give about 0.23 and 0.56, against stored values of 0.05 to 0.08 and 0.10, which would understate the loss. Le/D for a reducer is ambiguous because it does not say which diameter it refers to. The notes correctly identify the reference velocity, but the `Fitting` model has only one `diameter_in`.
*Action:* Replace the fixed values with a geometry-based calculation that takes D₁, D₂, and θ, or mark these entries `screening_only` and emit a warning. Remove their Le/D values.

**F-FIT-06 — Meter entries contradict their notes. (Medium)**
- The magnetic flow meter is described as full bore with "no flow obstructions", yet K = 0.50. A full-bore mag meter's loss is normally close to that of the same length of pipe. Reduced-bore meters are different. K = 0.5 is conservative, but the note and value disagree.
- The venturi's permanent loss depends on β and on the measured differential. K = 0.10 referenced to pipe velocity has no stated basis.
- The turbine meter value of 1.0 should come from the manufacturer.
*Action:* Mark meter entries as manufacturer-dependent placeholders that require a project-specific override, or remove them from v1.

**F-FIT-07 — Non-Crane sources are not specific enough to trace. (Medium)**
Entries cite "Hydraulic Institute Engineering Data Book — Minor Loss Coefficients", "Pumping Station Design, Third Edition — Minor Loss Data", and similar, with no table, page, or edition. `00_references.md` marks HI and Cameron as **[VERIFY EDITION]**. Cameron Hydraulic Data and AWWA M11/M23/M41 are listed in the header but no entry uses them. The module's own rule, "No coefficient shall exist in this database without a traceable engineering source", is not met for 10 of 25 entries.
*Action:* Add table and page references, or set `source_status: "unverified"` and have calculators raise a structured warning, per `03_numerical_conventions.md` ("unverified … fitting coefficients" warning).

**F-FIT-08 — `fitting_coefficients.py` is an empty file that overlaps `fittings_data.py`. (Medium)**
The file is 0 bytes. `docs/06_strategey/02_constants.md` and `03_pipe_fittings_coef.md` name it as the home of minor-loss coefficients, while the real data is in `fittings_data.py`. The checklist item names both files.
*Action:* Delete `fitting_coefficients.py` and update the strategy docs and checklist to point to `fittings_data.py`, or give the empty file a clear purpose and document it.

**F-FIT-09 — Check-valve applicability is not captured. (Low)**
Swing and ball check K values hold only at or above full-disc-open velocity. Below that, K rises sharply. Wastewater stations often run near that threshold at low-flow scenarios.
*Action:* Add a `min_velocity_full_open` note or field, citing the Crane minimum-velocity relationship or manufacturer data, and a warning hook.

**F-FIT-10 — Helper code quality. (Low)**
- The `K_*` module constants are computed but never used or tested against the data (see F-FIT-02).
- The helper imports from `data/`, a utils → data dependency. That is acceptable, but `verify_database_values()` prints to stdout. A pure function that returns structured results, with printing done by the caller, would fit `03_numerical_conventions.md` better.
- The f_T formula has no guard for ε/D ≤ 0.

**F-FIT-11 — Category naming is inconsistent. (Low)**
`fitting_category` values are singular (`bend`, `valve`), whereas `pipe_materials` uses `application: "both"`. Neither set of allowed values is defined in code, as an enum or constant. They exist only in the tests.

---

## 6. Cross-Cutting Findings

**F-X-01 — The data dictionaries have no defined schema or version. (Medium)**
The allowed keys and values exist only in tests. Neither dictionary carries a data version or revision history, which `02_units.md` §10 requires.
*Action:* Define a `TypedDict` or frozen dataclass per record, add `DATA_VERSION` and change-log notes, and consider `types.MappingProxyType` to make the tables read-only.

**F-X-02 — Tables are mutable module-level dicts. (Low)**
A consumer can change `PIPE_MATERIALS["pvc"]["c_factor"]` at run time and affect every later calculation.

**F-X-03 — Applicability is not machine-readable. (Low)**
Temperature range, fluid (clean water versus raw wastewater), flow regime (fully turbulent), and size range appear only in prose. Make them fields so calculators can emit `NOT_EVALUATED` or warnings, as the verification plan expects.

---

## 7. Finding Register

| ID | Module | Severity | Summary |
|---|---|---|---|
| F-CON-01 | constants | High | No viscosity or fluid-property module |
| F-CON-02 | constants | High | 62.4 labeled as density; it is γ in lbf/ft³, with no temperature |
| F-CON-03 | constants | High | Truncated conversions; exceed 1e-10 / 1e-6 tolerances; non-reciprocal |
| F-CON-04 | constants | High | `KW_PER_HP` name and docstring inverted |
| F-CON-05 | constants | Medium | No per-constant source or precision |
| F-CON-06 | constants | Medium | `THREE_PHASE_FACTOR` described as a power factor |
| F-CON-07 | constants | Medium | Hazen-Williams 4.727 has no unit or exponent contract |
| F-CON-08 | constants | Low | Manning 1.49 rounded; out of v1 scope |
| F-CON-09 | constants | Low | `PI` redundant; power factor is an assumption, not a constant |
| F-CON-10 | constants | Medium | Missing 144, 550, 3960, slug/lbm, atmospheric pressure, conversion functions |
| F-MAT-01 | pipe_materials | High | No per-entry source fields |
| F-MAT-02 | pipe_materials | High | DIP mixes lined C with unlined ε; extreme n |
| F-MAT-03 | pipe_materials | Medium | C, ε, and n presented as one consistent state |
| F-MAT-04 | pipe_materials | High | No ID, pressure class, or DR; nominal used as ID |
| F-MAT-05 | pipe_materials | Medium | Single fixed C; force mains need a design C range |
| F-MAT-06 | pipe_materials | Medium | PVC/HDPE n = 0.009 is low |
| F-MAT-07 | pipe_materials | Medium | Several values unverified or questionable |
| F-MAT-08 | pipe_materials | Low | Duplicate concrete entries |
| F-MAT-09 | pipe_materials | Low | `abestos` typo; broken doc path |
| F-MAT-10 | pipe_materials | Low | Condition classes undefined and unsourced |
| F-MAT-11 | pipe_materials | Low | Plausibility tests too broad |
| F-FIT-01 | fittings_data | High | Stored K ≠ f_T × Le/D; K and Le/D disagree by up to 2× |
| F-FIT-02 | helper / tests | High | Verification is circular; computed K never compared |
| F-FIT-03 | helper | Medium | "Interpolation" is actually proportional scaling; banker's rounding |
| F-FIT-04 | fittings_data | Medium | Bend, swing-check, and butterfly labels and values vs Crane need confirmation |
| F-FIT-05 | fittings_data | Medium | Reducer and expansion K fixed; Le/D meaningless; likely unconservative |
| F-FIT-06 | fittings_data | Medium | Meter values contradict their notes |
| F-FIT-07 | fittings_data | Medium | 10 of 25 entries not traceable to a table or page |
| F-FIT-08 | fitting_coefficients | Medium | Empty file; docs point to it |
| F-FIT-09 | fittings_data | Low | Check-valve minimum-velocity applicability missing |
| F-FIT-10 | helper | Low | Unused K constants; prints instead of returning; no input guard |
| F-FIT-11 | fittings_data | Low | Allowed category values not defined in code |
| F-X-01 | all | Medium | No schema or data version |
| F-X-02 | all | Low | Mutable global tables |
| F-X-03 | all | Low | Applicability not machine-readable |
| F-ENV-01 | project | Medium | `pytest` fails without `PYTHONPATH=src` (no build-system) |

---

## 8. Recommended Remediation Order

1. **Fix quickly (mechanical, low risk):** F-CON-03, F-CON-04, F-CON-06, F-CON-09, F-MAT-09, F-ENV-01, F-FIT-08. Add round-trip and reciprocal tests for the conversions.
2. **Create a fluid-property module:** F-CON-01, F-CON-02, F-CON-10, with a cited reference temperature for LS-001.
3. **Decide the fitting-loss method:** F-FIT-01 and F-FIT-02. This is an engineering decision; record it in `docs/01_architecture/02_design_decisions.md`, then fix the data and tests to match.
4. **Pass on provenance:** F-MAT-01, F-FIT-07, F-FIT-04, F-CON-05. Do this with the physical references at hand and close the **[VERIFY EDITION]** items in `00_references.md`.
5. **Model the applicability limits:** F-MAT-04, F-MAT-05, F-FIT-05, F-FIT-06, F-FIT-09, F-X-03. Coordinate with Phase 1 (Pipe/Fitting models) and the design-check warning framework.

---

## 9. Checklist Disposition

| Checklist item | Disposition |
|---|---|
| Audit `constants.py` for units, gravity, viscosity, and traceable sources | **Audit complete; findings open.** Gravity acceptable. Units: F-CON-02, F-CON-03, F-CON-04. Viscosity missing (F-CON-01). Sources not traceable (F-CON-05). |
| Audit `pipe_materials.py`, `fitting_coefficients.py`, `fittings_data.py` for applicability and provenance | **Audit complete; findings open.** Provenance: F-MAT-01, F-FIT-07. Applicability: F-MAT-04, F-MAT-05, F-FIT-05. Data integrity: F-FIT-01, F-FIT-02. |
| Acceptance: "Existing engineering reference data have been audited, with findings documented" | **Can be checked** once this report is committed. Closing the findings is separate follow-up work. |
