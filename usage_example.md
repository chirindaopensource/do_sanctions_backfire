## **Usage Example**

This guide uses the current `orchestrate_study_pipeline` interface for *Do Sanctions Backfire? New Evidence on the Macroeconomic Effects of Supporting Ukraine* (Vicente Rios, Izaskun Barba and Lisa Gianmoena, 2026). Python implementation author: **CS Chirinda**.

The example creates all ten raw DataFrames, reads the existing `config.yaml` into the plain dictionary `config`, and calls the actual study interface. It preserves the notebook's mathematical specification, configuration settings and provenance gates. The supplied production configuration presently fails preflight on three unresolved source-evidence groups; the executable example reports that rejection. No estimator stage, publication figure or empirical replication is claimed for that blocked call.

**Assumptions and execution requirements.**

1. Every research callable and helper already lives in the **single executed notebook** `do_sanctions_backfire_draft.ipynb`. Execute its 36 code cells, in order, before appending the example cells. **No Python task-module folder is imported or required.**
2. The working directory is that notebook's folder. `config.yaml` already exists there. The example reads it; it does not recreate or modify it.
3. The code uses Python 3.10 or newer syntax and the notebook's installed dependencies. The verification interpreter was Python 3.13. The example-specific imports are NumPy, pandas and **ruamel.yaml**. NumPy's private, seeded PCG64 data-generating model is the permitted alternative to Faker; country identities are fixed rather than randomly invented.
4. Copy the Python fences into new cells **in their displayed order**. The complete source tables and helper definitions are embedded below. Configured output destinations remain `results` and `replication_archive`; a genuinely ready invocation may write there. The verified preflight rejection writes no study outputs.
5. Synthetic monetary values, macroeconomic paths, missingness masks, query flags and policy documents are explicitly artificial. Matching a manuscript count demonstrates an input contract; it does not authenticate a data vintage, policy instrument, causal effect or publication-cell authority.

### **The study interface: `orchestrate_study_pipeline`**

Its actual signature has **one required argument and no default arguments**:

```text
orchestrate_study_pipeline(data: Mapping[str, Any]) -> Dict[str, Any]
```

Do not call it with `config=`, `sources=`, `output_dir=`, `stages=` or `fail_fast=` keyword arguments. Place supported inputs inside `data`. This notebook's interface binds runtime settings and automatically threads its **31 registered stages**; it does not use the unrelated climate example's per-stage execution API. The 33 final research orchestrators include engines reused by those stages.

| Input in `data` | Contract and genuine default behaviour |
| --- | --- |
| `study_config` | Required mapping: the entire dictionary loaded from `config.yaml`, including the 19 study sections and the operative execution/binding/provenance sections. No file path is accepted in this slot. |
| `sources` | Required mapping containing **exactly** the ten nonempty DataFrames named in Step 1. Raw sources are ingested together; independently supplied computed artifacts are refused. |
| `seeds` | Automatically copied from `config.runtime_inputs.seeds` when absent. Present configuration: `seed_set_id='primary_reproduction_v1'`, master `20261002`, and child spawn keys `country_history=[0]`, `region_block=[1]`, `placebo=[2]`. |
| `stability_seeds` | Automatically copied from the analogous runtime entry: `seed_set_id='stability_reproduction_v1'`, master `20261003`, same three child spawn keys. These are documented reproduction choices, not recovered author seeds. |
| `output_dir` | Automatically bound to the configured string `'results'`. It is a configured input, not a callable default. |
| `replication_archive_root` | Automatically bound to `'replication_archive'`. A complete study requires verified persisted archive contents. |
| `overlay_pair` | Automatically bound to `['strict_sanctions', 'log_CPI']`. This selects the overlay policy/outcome; Figure 4 still requires both configured level-outcome paths. |
| `archive_version_tag` | Automatically bound to `'v1.1.0-frozen'`. Archive release identity is distinct from source vintage and the configuration fingerprint. |

The binder copies these six runtime entries into a new outer `data` mapping. An explicitly supplied duplicate must equal the configured declaration; conflicting paths, seeds, overlays or release tags raise `IngestError`. Moving a run directory therefore requires a deliberate, documented configuration change; passing a conflicting path silently is not supported. The example makes no such change. Both primary and stability inference retain **500** valid country-history draws, **500** region-block draws and **500** size-matched placebo assignments per requested contract; the example does not substitute three-draw smoke settings.

The following eight **external scientific inputs** cannot be generated truthfully from configuration parameters. They have no automatic scientific defaults:

| External key | Exact object and downstream use |
| --- | --- |
| `income_groups` | Nonempty country-code-to-classification mapping covering the operative countries, obtained from a versioned income classification. Values such as `'high_income'` are used by donor-pool variants; these must not be fabricated from a desired effect. Needed at the baseline/variant handoff. |
| `country_geometry` | Nonempty pandas-compatible geometry frame, normally a GeoDataFrame with `country_code` and `geometry`. Its attrs need nonblank `source_id`, `version_id`, `geographic_scope='Europe'`, and an exact unique `country_codes_in_scope` inventory. Supplies the European choropleths. |
| `world_geometry` | Separate geometry frame with the same identity requirements and `geographic_scope='world'`. Supplies Figure B1; European polygons cannot satisfy this slot. |
| `table_sources` | Mapping for exactly `table_3`–`table_10`, `table_A1`–`table_A8`, `table_B1`–`table_B7`, plus `__cell_inventory__`. Each table is a cell-level DataFrame with `table_id`, `row_identifier`, `column_identifier`, `unit`, `display_precision`, `procedure`, `value_unrounded`, `status`, `source_id`, `gap_id`. The inventory uses the first six fields and covers exactly 418 unique publication cells. Values must resolve from live stage artifacts through independently authenticated selectors. Tables 1 and 2 are not keys in this producer's 23-table registry. |
| `table_binding_authority` | Separately obtained mapping with `authority_id`, `version_id`, `external_verification_reference`, `inventory_sha256`, `binding_registry_sha256`, `expected_cell_counts`, `expected_cell_bindings`, `expected_unresolved_gaps`. A computed selector names `artifact_key`, `artifact_sha256`, `attribute_path`, `source_id`, and the applicable Series/DataFrame `index`/`column`. Copying table attrs or hashing candidate output cannot create an authority pin. |
| `manuscript_universe` | Independently controlled DataFrame covering the complete 418-cell reported-value universe, with its required comparison metadata and authenticated `authority_id`/`version_id` attrs. It supplies reported values and identities, not estimates substituted for pipeline output. |
| `declared_facts` | Mapping of provenance facts to concrete versioned source/evidence records. Required fact identities include `wdi_vintage`, `comtrade_hs_codes`, `random_seeds`, `wgi_release_year`, `b10_b13_reconstruction`, `region_block_definition`. The audit must verify the actual value, source artifact and version. |
| `layer_chains` | Mapping keyed by `(table_id, row_identifier, column_identifier)`. Each checkpoint chain uses ordered `data`, `transformation`, `model` layers; checkpoint fields include `quantity_id`, `unit`, `comparison_key`, `expected`, `observed`, `comparison_type`, `display_precision`, and optional `allowance`. Comparison types are `integer_count`, `point_estimate` or `standard_error`. These localize discrepancies on an identical manuscript quantity. |

Independent registration is declared separately inside `config.independent_authorities.registration_manifest`: `path`, externally verified `sha256`, `external_verification_reference`, `verified_by`. The file must contain all four registries: `table_binding`, `manuscript_universe`, `reconciliation_scenarios`, `archive_vintages`. Its hash must be pinned before loading. The example leaves the current null registration fields intact.

For completeness, the adapters also recognise these **consistency inputs**. Omit them unless you have the exact matching upstream identity:

- `iso3_registry`: must equal the metadata-derived country-code roster.
- `metadata_frame` and `treatment_ledger`: must agree in content with their ingested authoritative counterparts.
- `comtrade_cell_status_raw`: if duplicated at `data` level, must agree with the ingested source; normally it belongs only inside `sources`.
- `region_map`: must agree with the versioned metadata-derived direct-arming map and, when supplied, its provenance.
- `exposure_windows`: must match `w2010_2013=(2010,2011,2012,2013)` and `w2018_2021=(2018,2019,2020,2021)`; configuration remains authoritative.
- `archive_artefacts`: declarations must equal the archive declarations reconstructed from live stage state. This is not a route for nominating arbitrary files.

The original API does not accept disconnected `reproduced_values`, `matched_paths`, `placebo_draws`, `observed_preferred_att`, `production_gaps`, `exposure_numerator`, `exposure_totals`, `exposure_primitives`, `robustness_frame`, `counterfactual_callable`, `imputation_callables`, `eligibility_callable`, `reconciliation_probe_registry` or `comtrade_retry_records` as independent replacements for its live graph.

**Return contract.** A call that reaches the scheduler returns one mapping per stage name plus `pipeline_summary`, `pipeline_state`, `pipeline_digest`, `pipeline_notes`, `pipeline_status`, `pipeline_blockers`. A complete result requires the exact ordered 31-stage execution, all declared output identities/digests, authenticated coverage of 418 cells, all three interface invariants, and the self-contained ten-family archive. Stage failures and skips are recorded; mandatory failures prevent completion. A preflight exception returns **no pipeline result**. The final wrapper below preserves that distinction explicitly.

### **Step 1: Synthetic Data Generation (the ten raw DataFrames)**

**Methodology.** The generator fixes 131 country candidates: the manuscript's 129 descriptive countries plus Syria and Sudan. It also places `WLD` and `HIC` aggregate rows in both WDI representations, so cleansing must use country-registry membership. Every synthetic country has all 29 raw calendar years, 1996–2024. The retained analytical window remains 2000–2024; outcomes and controls stay in raw source units.

The generator compounds annual percentage rates into positive levels:

$$
C_{it}=C_{i,t-1}\left(1+\frac{\pi_{it}}{100}\right),\qquad
Q_{it}=Q_{i,t-1}\left(1+\frac{r_{it}}{100}\right).
$$

These are **synthetic data-generation identities**, not additional manuscript estimators. CPI is rebased to a synthetic 2010 value of 100. Constant-price GDP per capita is kept distinct from current-dollar aggregate GDP. The generated nominal denominator uses the artificial population and price scale; it is not an accounting recovery of any country's actual GDP deflator. Governance levels are generated inside a bounded range and enter unchanged; no observed WGI series is winsorised. Price and output paths contain declared common/regional shocks, not a fitted paper effect.

**Required fidelity corrections to the request.**

- `[A-Z]{3}` also matches `WLD`, `HIC` and other aggregate codes. Pattern matching alone cannot exclude them. The notebook's authoritative country registry supplies the membership test; the example includes two such rows deliberately.
- The raw WDI table has **12**, not 10, primitive indicator codes: four outcomes, six direct controls, fuel imports and fuel exports. Rule of law is supplied separately by WGI. The eight model controls are the six direct controls, rule of law and the **completed-primitives fuel-share balance**.
- Table A6 retains **Russia and Belarus inside the 129-country descriptive sample** while barring their estimation roles. Ukraine is also retained but excluded from the main sanctions ATT and every donor pool. Only Syria and Sudan receive zero retained flags in this 131-country fixture.
- Table A4 contains **Croatia** in the 29-country direct-arming roster. Moldova enters sanctions in 2023; BIH, Korea, New Zealand and Singapore enter in 2022. The four pre-2022 arming countries are Czechia, Poland, Turkiye and the United States. Mexico is neither sanctions-coded nor a North America block member under the seven-region fixture convention.
- The policy source primary key is `(country_code, policy, document_title)` in the actual notebook/configuration. `source_document` is not a declared input column. Albania uses an `alignment_notice`; no EU-member regulation is fabricated for it.
- The wide DataFrame is the synthetic tabular equivalent of the requested wide extract. Its values are generated once, and `wdi_raw_long` is its exact raw-value counterpart. Missing cells are explicit NaNs, not absent rows assigned false zeros.

The resulting source contracts are:

| DataFrame | Generated rows × columns | Key and transformation boundary |
| --- | --- | --- |
| `wdi_raw_wide` | 1,596 × 33 | `(country_code, indicator_code)`; four labels and 29 floating year columns, including aggregate rows. |
| `wdi_raw_long` | 46,284 × 6 | `(country_code, indicator_code, year)`; exact wide/long closure; four outcomes and eight WDI control primitives. |
| `wgi_raw_long` | 3,799 × 5 | `(country_code, year, source_release)`; one TEST_ONLY release, 129 retained-grid missing levels. |
| `comtrade_block_a_raw` | 3,096 × 9 | `(reporter_code, year, product_description)`; coal/crude/refined imports only, selected HS codes, no non-gas mirror recovery. |
| `comtrade_block_b_raw` | 2,053 × 9 | `(reporter_code, partner_code, year, product_description, trade_flow)`; importer LNG/pipeline rows plus five positive Russian LNG mirror rows. No Russian positive pipeline mirror is invented. |
| `comtrade_block_c_raw` | 1,032 × 9 | `(reporter_code, year)`; world gas totals dominate positive Russian flows; missing totals remain missing. No unsupported exporter-sum substitution is asserted. |
| `comtrade_cell_status_raw` | 3,096 × 9 | `(reporter_code, year, series)`; independent gas/world/non-gas statuses and routing, with explicit synthetic retry and HS-report confirmations. |
| `nominal_gdp_raw` | 1,032 × 6 | `(country_code, year)`; `NY.GDP.MKTP.CD`, finite positive current-dollar denominators throughout both windows. |
| `policy_ledger_raw` | 72 × 10 | `(country_code, policy, document_title)`; 43 sanctions-coded and 29 arming records, Table A4 entry years, all documents explicitly TEST_ONLY. Its `estimation_exclusions` attrs declare `('BLR', 'RUS')` independently of policy coding. |
| `country_metadata_raw` | 131 × 5 | `country_code`; seven separate region labels, 129 retained flags and the five special geopolitical/coverage roles. |

Float source fields use `float64`; years use `int64`; retry/coverage flags and retained flags use `int8`; documentary priority uses `int8`. String fields remain strings; ingest applies its own categorical canonicalisation. Missing trade values are NaNs; verified zeros are 0.0 with both confirmations set to 1. The four protected pipeline-gas countries remain missing in their Russian gas windows and are routed to `case_exclusion`. These are fixture declarations, not claims that a Comtrade query was performed.

Two input metadata requirements matter operationally. The WGI `source_release` is `TEST_ONLY synthetic 2024 WGI release`: 2024 is an explicitly artificial parser-compatible fixture year, not a recovered manuscript release. The policy ledger preserves `frame.attrs['estimation_exclusions']=('BLR','RUS')` through ingest/cleansing. Together with Ukraine's `excluded_from_target` decision, this makes the coding summary use the 126-country estimation universe and report 83 neither-policy countries. Omitting those attrs would leave Russia and Belarus in that coding-summary category and fail its count assertion. Preserve this metadata when serializing/reloading authentic inputs; ordinary CSV alone cannot retain DataFrame attrs.

#### **Step 1.1: Imports**

Execute this cell after the notebook's research cells. It imports only the example's direct dependencies.

```python
# Import annotations from __future__.
from __future__ import annotations

# Import Mapping from collections.abc.
from collections.abc import Mapping
# Import Path from pathlib.
from pathlib import Path
# Import Any from typing.
from typing import Any

# Import numpy for the example.
import numpy as np
# Import pandas for the example.
import pandas as pd
# Import YAML from ruamel.yaml.
from ruamel.yaml import YAML
```

#### **Step 1.2: Country names and region convention**

Country labels below are taken from the supplied Table A6; Syria and Sudan are added as the documented eligibility exclusions. The seven-region partition is a declared fixture convention consistent with the configured vocabulary. It is not a newly recovered or current World Bank classification vintage. This preserves readable names such as `Czechia`, `Korea, Rep.`, `Turkiye` and `Russian Federation`.

```python
# Fix supplied manuscript country labels and the region fixture convention.
# Freeze the supplied Table A6 labels plus Syria and Sudan.
USAGE_COUNTRY_NAMES: dict[str, str] = {
    # Record the AGO field in the current mapping.
    "AGO": "Angola",
    # Record the ALB field in the current mapping.
    "ALB": "Albania",
    # Record the ARE field in the current mapping.
    "ARE": "United Arab Emirates",
    # Record the ARM field in the current mapping.
    "ARM": "Armenia",
    # Record the AUS field in the current mapping.
    "AUS": "Australia",
    # Record the AUT field in the current mapping.
    "AUT": "Austria",
    # Record the AZE field in the current mapping.
    "AZE": "Azerbaijan",
    # Record the BEL field in the current mapping.
    "BEL": "Belgium",
    # Record the BEN field in the current mapping.
    "BEN": "Benin",
    # Record the BFA field in the current mapping.
    "BFA": "Burkina Faso",
    # Record the BGD field in the current mapping.
    "BGD": "Bangladesh",
    # Record the BGR field in the current mapping.
    "BGR": "Bulgaria",
    # Record the BHR field in the current mapping.
    "BHR": "Bahrain",
    # Record the BHS field in the current mapping.
    "BHS": "Bahamas, The",
    # Record the BIH field in the current mapping.
    "BIH": "Bosnia and Herzegovina",
    # Record the BLR field in the current mapping.
    "BLR": "Belarus",
    # Record the BLZ field in the current mapping.
    "BLZ": "Belize",
    # Record the BOL field in the current mapping.
    "BOL": "Bolivia",
    # Record the BRA field in the current mapping.
    "BRA": "Brazil",
    # Record the BRN field in the current mapping.
    "BRN": "Brunei Darussalam",
    # Record the BTN field in the current mapping.
    "BTN": "Bhutan",
    # Record the BWA field in the current mapping.
    "BWA": "Botswana",
    # Record the CAF field in the current mapping.
    "CAF": "Central African Republic",
    # Record the CAN field in the current mapping.
    "CAN": "Canada",
    # Record the CHE field in the current mapping.
    "CHE": "Switzerland",
    # Record the CHL field in the current mapping.
    "CHL": "Chile",
    # Record the CHN field in the current mapping.
    "CHN": "China",
    # Record the CIV field in the current mapping.
    "CIV": "Cote d'Ivoire",
    # Record the CMR field in the current mapping.
    "CMR": "Cameroon",
    # Record the COG field in the current mapping.
    "COG": "Congo, Rep.",
    # Record the COL field in the current mapping.
    "COL": "Colombia",
    # Record the COM field in the current mapping.
    "COM": "Comoros",
    # Record the CRI field in the current mapping.
    "CRI": "Costa Rica",
    # Record the CYP field in the current mapping.
    "CYP": "Cyprus",
    # Record the CZE field in the current mapping.
    "CZE": "Czechia",
    # Record the DEU field in the current mapping.
    "DEU": "Germany",
    # Record the DNK field in the current mapping.
    "DNK": "Denmark",
    # Record the DOM field in the current mapping.
    "DOM": "Dominican Republic",
    # Record the DZA field in the current mapping.
    "DZA": "Algeria",
    # Record the ECU field in the current mapping.
    "ECU": "Ecuador",
    # Record the EGY field in the current mapping.
    "EGY": "Egypt, Arab Rep.",
    # Record the ESP field in the current mapping.
    "ESP": "Spain",
    # Record the EST field in the current mapping.
    "EST": "Estonia",
    # Record the FIN field in the current mapping.
    "FIN": "Finland",
    # Record the FJI field in the current mapping.
    "FJI": "Fiji",
    # Record the FRA field in the current mapping.
    "FRA": "France",
    # Record the GAB field in the current mapping.
    "GAB": "Gabon",
    # Record the GBR field in the current mapping.
    "GBR": "United Kingdom",
    # Record the GEO field in the current mapping.
    "GEO": "Georgia",
    # Record the GHA field in the current mapping.
    "GHA": "Ghana",
    # Record the GIN field in the current mapping.
    "GIN": "Guinea",
    # Record the GMB field in the current mapping.
    "GMB": "Gambia, The",
    # Record the GRC field in the current mapping.
    "GRC": "Greece",
    # Record the GTM field in the current mapping.
    "GTM": "Guatemala",
    # Record the HKG field in the current mapping.
    "HKG": "Hong Kong SAR, China",
    # Record the HND field in the current mapping.
    "HND": "Honduras",
    # Record the HRV field in the current mapping.
    "HRV": "Croatia",
    # Record the HUN field in the current mapping.
    "HUN": "Hungary",
    # Record the IDN field in the current mapping.
    "IDN": "Indonesia",
    # Record the IND field in the current mapping.
    "IND": "India",
    # Record the IRL field in the current mapping.
    "IRL": "Ireland",
    # Record the IRN field in the current mapping.
    "IRN": "Iran, Islamic Rep.",
    # Record the ISL field in the current mapping.
    "ISL": "Iceland",
    # Record the ISR field in the current mapping.
    "ISR": "Israel",
    # Record the ITA field in the current mapping.
    "ITA": "Italy",
    # Record the JPN field in the current mapping.
    "JPN": "Japan",
    # Record the KAZ field in the current mapping.
    "KAZ": "Kazakhstan",
    # Record the KEN field in the current mapping.
    "KEN": "Kenya",
    # Record the KGZ field in the current mapping.
    "KGZ": "Kyrgyz Republic",
    # Record the KHM field in the current mapping.
    "KHM": "Cambodia",
    # Record the KIR field in the current mapping.
    "KIR": "Kiribati",
    # Record the KOR field in the current mapping.
    "KOR": "Korea, Rep.",
    # Record the KWT field in the current mapping.
    "KWT": "Kuwait",
    # Record the LKA field in the current mapping.
    "LKA": "Sri Lanka",
    # Record the LTU field in the current mapping.
    "LTU": "Lithuania",
    # Record the LUX field in the current mapping.
    "LUX": "Luxembourg",
    # Record the LVA field in the current mapping.
    "LVA": "Latvia",
    # Record the MAC field in the current mapping.
    "MAC": "Macao SAR, China",
    # Record the MAR field in the current mapping.
    "MAR": "Morocco",
    # Record the MDA field in the current mapping.
    "MDA": "Moldova",
    # Record the MDG field in the current mapping.
    "MDG": "Madagascar",
    # Record the MEX field in the current mapping.
    "MEX": "Mexico",
    # Record the MKD field in the current mapping.
    "MKD": "North Macedonia",
    # Record the MLI field in the current mapping.
    "MLI": "Mali",
    # Record the MLT field in the current mapping.
    "MLT": "Malta",
    # Record the MNG field in the current mapping.
    "MNG": "Mongolia",
    # Record the MRT field in the current mapping.
    "MRT": "Mauritania",
    # Record the MUS field in the current mapping.
    "MUS": "Mauritius",
    # Record the MYS field in the current mapping.
    "MYS": "Malaysia",
    # Record the NAM field in the current mapping.
    "NAM": "Namibia",
    # Record the NER field in the current mapping.
    "NER": "Niger",
    # Record the NIC field in the current mapping.
    "NIC": "Nicaragua",
    # Record the NLD field in the current mapping.
    "NLD": "Netherlands",
    # Record the NOR field in the current mapping.
    "NOR": "Norway",
    # Record the NPL field in the current mapping.
    "NPL": "Nepal",
    # Record the NZL field in the current mapping.
    "NZL": "New Zealand",
    # Record the OMN field in the current mapping.
    "OMN": "Oman",
    # Record the PAK field in the current mapping.
    "PAK": "Pakistan",
    # Record the PAN field in the current mapping.
    "PAN": "Panama",
    # Record the PER field in the current mapping.
    "PER": "Peru",
    # Record the PHL field in the current mapping.
    "PHL": "Philippines",
    # Record the POL field in the current mapping.
    "POL": "Poland",
    # Record the PRT field in the current mapping.
    "PRT": "Portugal",
    # Record the PRY field in the current mapping.
    "PRY": "Paraguay",
    # Record the PSE field in the current mapping.
    "PSE": "West Bank and Gaza",
    # Record the ROU field in the current mapping.
    "ROU": "Romania",
    # Record the RUS field in the current mapping.
    "RUS": "Russian Federation",
    # Record the RWA field in the current mapping.
    "RWA": "Rwanda",
    # Record the SAU field in the current mapping.
    "SAU": "Saudi Arabia",
    # Record the SDN field in the current mapping.
    "SDN": "Sudan",
    # Record the SEN field in the current mapping.
    "SEN": "Senegal",
    # Record the SGP field in the current mapping.
    "SGP": "Singapore",
    # Record the SLB field in the current mapping.
    "SLB": "Solomon Islands",
    # Record the SLV field in the current mapping.
    "SLV": "El Salvador",
    # Record the SVK field in the current mapping.
    "SVK": "Slovak Republic",
    # Record the SVN field in the current mapping.
    "SVN": "Slovenia",
    # Record the SWE field in the current mapping.
    "SWE": "Sweden",
    # Record the SYC field in the current mapping.
    "SYC": "Seychelles",
    # Record the SYR field in the current mapping.
    "SYR": "Syrian Arab Republic",
    # Record the TGO field in the current mapping.
    "TGO": "Togo",
    # Record the THA field in the current mapping.
    "THA": "Thailand",
    # Record the TON field in the current mapping.
    "TON": "Tonga",
    # Record the TUN field in the current mapping.
    "TUN": "Tunisia",
    # Record the TUR field in the current mapping.
    "TUR": "Turkiye",
    # Record the TZA field in the current mapping.
    "TZA": "Tanzania",
    # Record the UGA field in the current mapping.
    "UGA": "Uganda",
    # Record the UKR field in the current mapping.
    "UKR": "Ukraine",
    # Record the URY field in the current mapping.
    "URY": "Uruguay",
    # Record the USA field in the current mapping.
    "USA": "United States",
    # Record the VNM field in the current mapping.
    "VNM": "Viet Nam",
    # Record the ZAF field in the current mapping.
    "ZAF": "South Africa",
# Close the preceding argument, selection or data-structure expression.
}
# Declare the seven-region synthetic fixture convention.
USAGE_REGION_GROUPS: dict[str, str] = {
    # Record the East Asia & Pacific field in the current mapping.
    "East Asia & Pacific": (
        # Supply this component of USAGE_REGION_GROUPS.
        "AUS BRN CHN FJI HKG IDN JPN KHM KIR KOR MAC MNG MYS NZL PHL "
        # Supply this component of USAGE_REGION_GROUPS.
        "SGP SLB THA TON VNM"
    # Close the preceding argument, selection or data-structure expression.
    ),
    # Record the Europe & Central Asia field in the current mapping.
    "Europe & Central Asia": (
        # Supply this component of USAGE_REGION_GROUPS.
        "ALB ARM AUT AZE BEL BGR BIH BLR CHE CYP CZE DEU DNK ESP EST FIN "
        # Supply this component of USAGE_REGION_GROUPS.
        "FRA GBR GEO GRC HRV HUN IRL ISL ITA KAZ KGZ LTU LUX LVA MDA "
        # Supply this component of USAGE_REGION_GROUPS.
        "MKD MLT NLD NOR POL PRT ROU RUS SVK SVN SWE TUR UKR"
    # Close the preceding argument, selection or data-structure expression.
    ),
    # Record the Latin America & Caribbean field in the current mapping.
    "Latin America & Caribbean": (
        # Supply this component of USAGE_REGION_GROUPS.
        "BHS BLZ BOL BRA CHL COL CRI DOM ECU GTM HND MEX NIC PAN PER "
        # Supply this component of USAGE_REGION_GROUPS.
        "PRY SLV URY"
    # Close the preceding argument, selection or data-structure expression.
    ),
    # Record the Middle East & North Africa field in the current mapping.
    "Middle East & North Africa": (
        # Supply this component of USAGE_REGION_GROUPS.
        "ARE BHR DZA EGY IRN ISR KWT MAR OMN PSE SAU SYR TUN"
    # Close the preceding argument, selection or data-structure expression.
    ),
    # Record the North America field in the current mapping.
    "North America": "CAN USA",
    # Record the South Asia field in the current mapping.
    "South Asia": "BGD BTN IND LKA NPL PAK",
    # Record the Sub-Saharan Africa field in the current mapping.
    "Sub-Saharan Africa": (
        # Supply this component of USAGE_REGION_GROUPS.
        "AGO BEN BFA BWA CAF CIV CMR COG COM GAB GHA GIN GMB KEN MDG "
        # Supply this component of USAGE_REGION_GROUPS.
        "MLI MRT MUS NAM NER RWA SDN SEN SYC TGO TZA UGA ZAF"
    # Close the preceding argument, selection or data-structure expression.
    ),
# Close the preceding argument, selection or data-structure expression.
}
```

#### **Step 1.3: Separate trade-coverage plans**

The plan fixes missingness without using policy membership or estimated effects. Thirteen recoverable single-year Russian-gas gaps, thirteen world-gas gaps and eleven non-gas gaps are separate from deliberately unrecoverable windows. The status counts match the configured raw coverage; successful downstream completion must still be established by running its own callable.

```python
# Define plan_trade_statuses with an explicit input and return contract.
def plan_trade_statuses(
    # Declare this input's name and type for plan_trade_statuses.
    config: Mapping[str, Any], retained: tuple[str, ...]
# Finish the typed return signature of plan_trade_statuses.
) -> dict[tuple[str, int, str], str]:
    """Allocate a reproducible, internally consistent trade-coverage fixture.

    Inputs are the loaded configuration and its 129-country descriptive
    roster.
    The process separates the two four-year windows, fixes the four
    unresolved
    pipeline-gas countries and Russia as gas-coverage failures, then places
    recoverable single gaps and unrecoverable multi-year gaps independently
    of
    policy membership. It allocates exactly the configured
    positive/zero/missing
    totals and keeps every positive Russian gas flow inside a positive world
    denominator. Outputs are statuses keyed by country, year and primitive.
    Raises ValueError on an incompatible roster or impossible count
    allocation.
    No query or documentary verification is claimed by these artificial
    flags.
    """
    # Set windows from the expression below.
    windows = config["series_code_registry"]["temporal_universe_by_layer"]
    # Set bounds from the expression below.
    bounds = windows["exposure_windows"]
    # Set years from the expression below.
    years = tuple(
        # Supply this component of years.
        year for first, last in bounds for year in range(first, last + 1)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set cells from the expression below.
    cells = tuple((code, year) for code in retained for year in years)
    # Set targets from the expression below.
    targets = config["execution_parameters"]["comtrade_status_counts"]
    # Set protected from the expression below.
    protected = set(
        # Supply this component of protected.
        config["algorithm_parameters"]["D2_mirror_flow_recovery"][
            # Supply this component of protected.
            "unresolved_pipeline_countries"
        # Close the preceding argument, selection or data-structure expression.
        ]
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set eligible from the expression below.
    eligible = tuple(
        # Supply this component of eligible.
        code for code in retained if code not in protected | {"RUS"}
    # Close the preceding argument, selection or data-structure expression.
    )
    # Reject inputs when this condition implies: The trade fixture requires the
    # declared 129-country grid.
    if len(cells) != 1032 or len(eligible) < 32:
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "The trade fixture requires the declared 129-country grid."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set gas_missing from the expression below.
    gas_missing = {
        # Supply this component of gas_missing.
        (code, year) for code in protected | {"RUS"} for year in years
    # Close the preceding argument, selection or data-structure expression.
    }
    # Supply or execute this component of gas_missing.update.
    gas_missing.update((code, 2011) for code in eligible[:13])
    # Supply or execute this component of gas_missing.update.
    gas_missing.update(
        # Supply or execute this component of gas_missing.update.
        (code, year) for code in eligible[13:15] for year in range(2010, 2014)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Supply or execute this component of gas_missing.update.
    gas_missing.update(
        # Supply or execute this component of gas_missing.update.
        (code, year) for code in eligible[15:18] for year in range(2010, 2013)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set world_missing from the expression below.
    world_missing = {(code, 2011) for code in eligible[:13]}
    # Supply or execute this component of world_missing.update.
    world_missing.update(("RUS", year) for year in years)
    # Supply or execute this component of world_missing.update.
    world_missing.update(
        # Supply or execute this component of world_missing.update.
        (code, year) for code in ("AUT", "DEU") for year in range(2010, 2014)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Supply or execute this component of world_missing.update.
    world_missing.update(
        # Supply or execute this component of world_missing.update.
        (code, year) for code in ("AUT", "DEU") for year in range(2018, 2021)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Supply or execute this component of world_missing.update.
    world_missing.update(("POL", year) for year in range(2010, 2013))
    # Set non_gas_missing from the expression below.
    non_gas_missing = {(code, 2011) for code in eligible[:11]}
    # Supply or execute this component of non_gas_missing.update.
    non_gas_missing.update(
        # Supply or execute this component of non_gas_missing.update.
        (code, year) for code in eligible[18:22] for year in range(2010, 2014)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Supply or execute this component of non_gas_missing.update.
    non_gas_missing.update(
        # Supply or execute this component of non_gas_missing.update.
        (code, year) for code in eligible[22:26] for year in range(2010, 2013)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Keep independent missing-cell sets for each primitive.
    missing_sets = {
        # Record the russian_gas field in the current mapping.
        "russian_gas": gas_missing,
        # Record the world_gas field in the current mapping.
        "world_gas": world_missing,
        # Record the russian_non_gas field in the current mapping.
        "russian_non_gas": non_gas_missing,
    # Close the preceding argument, selection or data-structure expression.
    }
    # Set result from the expression below.
    result: dict[tuple[str, int, str], str] = {}
    # Set gas_available from the expression below.
    gas_available = tuple(
        # Supply this component of gas_available.
        cell for cell in cells if cell not in gas_missing | world_missing
    # Close the preceding argument, selection or data-structure expression.
    )
    # Reserve Russian gas positives inside usable world totals.
    gas_positive = set(gas_available[: targets["russian_gas"]["positive"]])
    # Iterate (series, missing) over missing_sets.items().
    for series, missing in missing_sets.items():
        # Validate len(missing) != targets[series]['missing'] and report the
        # named failure.
        if len(missing) != targets[series]["missing"]:
            # Raise the stated exception with the concrete validation failure.
            raise ValueError(
                # Supply the concrete diagnostic message for ValueError.
                f"The {series} coverage plan disagrees with config.yaml."
            # Close the preceding argument, selection or data-structure
            # expression.
            )
        # Set available from the expression below.
        available = tuple(cell for cell in cells if cell not in missing)
        # Check series == 'russian_gas' before entering this branch.
        if series == "russian_gas":
            # Set positive from the expression below.
            positive = gas_positive
        # Check series == 'world_gas' before entering this branch.
        elif series == "world_gas":
            # Reject external keys that this example does not support.
            extra = tuple(
                # Supply this component of extra.
                cell for cell in available if cell not in gas_positive
            # Close the preceding argument, selection or data-structure
            # expression.
            )
            # Set positive from the expression below.
            positive = gas_positive | set(
                # Supply this component of positive.
                extra[: targets[series]["positive"] - len(gas_positive)]
            # Close the preceding argument, selection or data-structure
            # expression.
            )
        # Apply this alternative only when the preceding condition did not
        # hold.
        else:
            # Set foreign_cells from the expression below.
            foreign_cells = tuple(
                # Supply this component of foreign_cells.
                cell for cell in available if cell[0] != "RUS"
            # Close the preceding argument, selection or data-structure
            # expression.
            )
            # Set positive from the expression below.
            positive = set(foreign_cells[: targets[series]["positive"]])
        # Validate len(positive) != targets[series]['positive'] and report the
        # named failure.
        if len(positive) != targets[series]["positive"]:
            # Raise the stated exception with the concrete validation failure.
            raise ValueError(
                # Supply the concrete diagnostic message for ValueError.
                f"Insufficient available cells for {series} positives."
            # Close the preceding argument, selection or data-structure
            # expression.
            )
        # Iterate (code, year) over cells.
        for code, year in cells:
            # Set status from the expression below.
            status = (
                # Supply this component of status.
                "missing"
                # Apply this additional condition to the current selection or
                # validation.
                if (code, year) in missing
                # Supply this component of status.
                else (
                    # Supply this component of status.
                    "positive" if (code, year) in positive else "verified_zero"
                # Close the preceding argument, selection or data-structure
                # expression.
                )
            # Close the preceding argument, selection or data-structure
            # expression.
            )
            # Set result[code, year, series] from the expression below.
            result[(code, year, series)] = status
    # Return the declared outputs and their audit evidence.
    return result
```

#### **Step 1.4: Generate the ten sources**

The raw missingness mask is applied only after the synthetic underlying histories exist. It does not interpolate a ratio, construct a counterfactual, lag controls, or change eligibility. Early CPI/inflation gaps extend into the 1996–1999 anchor years so they are genuinely leading gaps in the full raw history. Within the retained 2000–2024 grid they yield **55 leading outcome gaps plus the two BIH 2024 carry-forward cases**. Controls yield 1,471 eventual model-control repairs, including the 203 fuel-balance flags inherited from missing fuel-import primitives; the actual notebook completion audit verifies the emitted count separately.

```python
# Define generate_study_sources with an explicit input and return contract.
def generate_study_sources(
    # Declare this input's name and type for generate_study_sources.
    config: Mapping[str, Any], *, seed: int = 20261003
# Finish the typed return signature of generate_study_sources.
) -> dict[str, pd.DataFrame]:
    """Generate all ten raw source frames without altering study parameters.

    Inputs are config.yaml's parsed dictionary and a nonnegative 64-bit
    seed.
    A local NumPy PCG64 generator supplies coherent price/GDP histories,
    annual
    rates, eight WDI control primitives, governance levels, and
    current-dollar
    trade/GDP values. Country names follow the supplied Table A6; the seven
    region labels are fixed fixture conventions. Table A4 entry years are
    retained, but every policy document and archive reference is TEST_ONLY.
    Raw missingness reproduces the prescribed retained-grid counts; no
    outcome
    completion, logarithm, lag, eligibility selection or estimator runs
    here.
    Outputs are exactly the ten named, typed and uniquely keyed DataFrames.
    Raises TypeError for a malformed configuration/seed and ValueError for a
    contradictory roster, registry, temporal declaration or primitive count.
    Synthetic agreement with counts is not recovery of a WDI/WGI vintage.
    """
    # Reject inputs when this condition implies: config must be a mapping
    # parsed from config.yaml.
    if not isinstance(config, Mapping):
        # Raise the stated exception with the concrete validation failure.
        raise TypeError("config must be a mapping parsed from config.yaml.")
    # Reject inputs when this condition implies: seed must be an integer, not a
    # Boolean.
    if isinstance(seed, bool) or not isinstance(seed, (int, np.integer)):
        # Raise the stated exception with the concrete validation failure.
        raise TypeError("seed must be an integer, not a Boolean.")
    # Reject inputs when this condition implies: seed must lie in [0, 2**64).
    if not 0 <= int(seed) < 2**64:
        # Raise the stated exception with the concrete validation failure.
        raise ValueError("seed must lie in [0, 2**64).")
    # Create a private PCG64 stream; leave the global RNG unchanged.
    generator = np.random.Generator(np.random.PCG64(int(seed)))
    # Set rosters from the expression below.
    rosters = config["country_validation_registries"]
    # Build the 129-country descriptive roster from configured roles.
    retained = tuple(
        # Supply this component of retained.
        sorted(
            # Supply this component of retained.
            set(rosters["sanctions_target_42"])
            # Supply this component of retained.
            | set(rosters["sanctions_donors_84"])
            # Supply this component of retained.
            | {"RUS", "BLR", "UKR"}
        # Close the preceding argument, selection or data-structure expression.
        )
    # Close the preceding argument, selection or data-structure expression.
    )
    # Add the two documented coverage exclusions to the raw roster.
    universe = tuple(sorted(set(retained) | {"SYR", "SDN"}))
    # Reject inputs when this condition implies: The configured roster differs
    # from the embedded Table A6 fixture.
    if len(retained) != 129 or set(universe) != set(USAGE_COUNTRY_NAMES):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "The configured roster differs from the embedded Table A6 fixture."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set region_map from the expression below.
    region_map = {
        # Supply this component of region_map.
        code: region
        # Evaluate this comprehension once for each declared item.
        for region, codes in USAGE_REGION_GROUPS.items()
        # Evaluate this comprehension once for each declared item.
        for code in codes.split()
    # Close the preceding argument, selection or data-structure expression.
    }
    # Reject inputs when this condition implies: Every fixture country needs
    # exactly one regional assignment.
    if set(region_map) != set(universe):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "Every fixture country needs exactly one regional assignment."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set layers from the expression below.
    layers = config["series_code_registry"]["temporal_universe_by_layer"]
    # Reject inputs when this condition implies: This example implements the
    # declared 1996-2024 raw window.
    if list(layers["raw_retrieval"]) != [1996, 2024]:
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "This example implements the declared 1996-2024 raw window."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set years from the expression below.
    years = tuple(range(1996, 2025))
    # Set labels from the expression below.
    labels = config["series_code_registry"]
    # Join four WDI outcomes and eight WDI control primitives.
    wdi_registry = {**labels["wdi_outcomes"], **labels["wdi_controls"]}
    # Reject inputs when this condition implies: WDI requires four outcomes and
    # eight control primitives.
    if len(wdi_registry) != 12:
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "WDI requires four outcomes and eight control primitives."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set indicator_labels from the expression below.
    indicator_labels = {
        # Record the CPI field in the current mapping.
        "CPI": "Consumer price index (synthetic 2010 = 100)",
        # Record the RealGDPpc field in the current mapping.
        "RealGDPpc": "GDP per capita (synthetic constant-price US$)",
        # Record the InflationRate field in the current mapping.
        "InflationRate": "Inflation, consumer prices (annual %)",
        # Record the RealGDPpcGrowth field in the current mapping.
        "RealGDPpcGrowth": "GDP per capita growth (annual %)",
        # Record the OilRents field in the current mapping.
        "OilRents": "Oil rents (% of GDP)",
        # Record the TradeOpenness field in the current mapping.
        "TradeOpenness": "Trade (% of GDP)",
        # Record the TermsOfTrade field in the current mapping.
        "TermsOfTrade": "Net barter terms of trade index (synthetic base)",
        # Record the InvestmentRate field in the current mapping.
        "InvestmentRate": "Gross capital formation (% of GDP)",
        # Record the GovConsumption field in the current mapping.
        "GovConsumption": "General government final consumption (% of GDP)",
        # Record the PopulationGrowth field in the current mapping.
        "PopulationGrowth": "Population growth (annual %)",
        # Record the FuelImportShare field in the current mapping.
        "FuelImportShare": "Fuel imports (% of merchandise imports)",
        # Record the FuelExportShare field in the current mapping.
        "FuelExportShare": "Fuel exports (% of merchandise exports)",
    # Close the preceding argument, selection or data-structure expression.
    }
    # Start the explicit raw-WDI missing-cell set; do not impute.
    wdi_missing: set[tuple[str, str, int]] = set()
    # Set targets from the expression below.
    targets = config["raw_data_schemas"]["wdi_raw_long"]["raw_missing_targets"]
    # Iterate variable over ('OilRents', 'TradeOpenness', 'TermsOfTrade',
    # 'InvestmentRate', 'GovConsumption', 'PopulationGrowth').
    for variable in (
        # Include this declared item in the loop over variable.
        "OilRents",
        # Include this declared item in the loop over variable.
        "TradeOpenness",
        # Include this declared item in the loop over variable.
        "TermsOfTrade",
        # Include this declared item in the loop over variable.
        "InvestmentRate",
        # Include this declared item in the loop over variable.
        "GovConsumption",
        # Include this declared item in the loop over variable.
        "PopulationGrowth",
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Set code from the expression below.
        code = wdi_registry[variable]["indicator_code"]
        # Iterate position over range(int(targets[code])).
        for position in range(int(targets[code])):
            # Supply or execute this component of wdi_missing.add.
            wdi_missing.add(
                # Supply or execute this component of wdi_missing.add.
                (retained[position % 129], code, 2001 + position // 129)
            # Close the preceding argument, selection or data-structure
            # expression.
            )
    # Set fuel_code from the expression below.
    fuel_code = wdi_registry["FuelImportShare"]["indicator_code"]
    # Set balance_missing from the expression below.
    balance_missing = int(
        # Supply this component of balance_missing.
        config["execution_parameters"]["completion"][
            # Supply this component of balance_missing.
            "derived_fuel_balance_missing_cells"
        # Close the preceding argument, selection or data-structure expression.
        ]
    # Close the preceding argument, selection or data-structure expression.
    )
    # Iterate position over range(balance_missing).
    for position in range(balance_missing):
        # Supply or execute this component of wdi_missing.add.
        wdi_missing.add(
            # Supply or execute this component of wdi_missing.add.
            (retained[position % 129], fuel_code, 2005 + position // 129)
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set leading from the expression below.
    leading = tuple(code for code in retained if code != "BIH")
    # Iterate (variable, count, double_position) over (('CPI', 23, 22),
    # ('InflationRate', 30, 29)).
    for variable, count, double_position in (
        # Include this declared item in the loop over (variable, count,
        # double_position).
        ("CPI", 23, 22),
        # Include this declared item in the loop over (variable, count,
        # double_position).
        ("InflationRate", 30, 29),
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Set indicator from the expression below.
        indicator = wdi_registry[variable]["indicator_code"]
        # Iterate (position, country) over enumerate(leading[:count]).
        for position, country in enumerate(leading[:count]):
            # Set stop from the expression below.
            stop = 2001 if position == double_position else 2000
            # Supply or execute this component of wdi_missing.update.
            wdi_missing.update(
                # Supply or execute this component of wdi_missing.update.
                (country, indicator, year) for year in range(1996, stop + 1)
            # Close the preceding argument, selection or data-structure
            # expression.
            )
        # Supply or execute this component of wdi_missing.add.
        wdi_missing.add(("BIH", indicator, 2024))
    # Set cpi_code from the expression below.
    cpi_code = wdi_registry["CPI"]["indicator_code"]
    # Supply or execute this component of wdi_missing.update.
    wdi_missing.update(("SYR", cpi_code, year) for year in range(2019, 2024))
    # Supply or execute this component of wdi_missing.update.
    wdi_missing.update(("SDN", cpi_code, year) for year in (2019, 2020))
    # Allocate the rows of the complete raw WDI calendar grid.
    wdi_rows: list[dict[str, Any]] = []
    # Allocate the separate rule-of-law source rows.
    wgi_rows: list[dict[str, Any]] = []
    # Allocate current-dollar GDP denominator observations.
    nominal_rows: list[dict[str, Any]] = []
    # Index positive nominal GDP by country and exposure year.
    nominal_lookup: dict[tuple[str, int], float] = {}
    # Set trade_years from the expression below.
    trade_years = tuple(
        # Supply this component of trade_years.
        year
        # Evaluate this comprehension once for each declared item.
        for first, last in layers["exposure_windows"]
        # Evaluate this comprehension once for each declared item.
        for year in range(first, last + 1)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Create the common, bounded synthetic calendar-year shock.
    common_shock = np.sin(np.arange(len(years), dtype=float) / 3.5)
    # Iterate (position, country) over enumerate(universe).
    for position, country in enumerate(universe):
        # Generate raw annual inflation in percentage-point units.
        price_rates = (
            # Supply this component of price_rates.
            2.4 + 0.7 * common_shock + generator.normal(0.0, 0.25, len(years))
        # Close the preceding argument, selection or data-structure expression.
        )
        # Generate raw annual real-GDP-per-capita growth in percent.
        growth_rates = (
            # Supply this component of growth_rates.
            2.0 + 0.5 * common_shock + generator.normal(0.0, 0.4, len(years))
        # Close the preceding argument, selection or data-structure expression.
        )
        # Set a declared synthetic regional vulnerability parameter.
        exposure = 0.2 + 0.6 * (region_map[country] == "Europe & Central Asia")
        # Add the declared common or exposure-scaled post-2022 price shock.
        price_rates += np.asarray(years) >= 2022
        # Add the declared common or exposure-scaled post-2022 price shock.
        price_rates += exposure * (np.asarray(years) >= 2022)
        # Subtract the exposure-scaled post-2022 synthetic growth shock.
        growth_rates -= 0.6 * exposure * (np.asarray(years) >= 2022)
        # Compound annual inflation to form a positive raw CPI index.
        cpi = 100.0 * np.cumprod(1.0 + price_rates / 100.0)
        # Rebase the synthetic CPI so its 2010 observation equals 100.
        cpi *= 100.0 / cpi[years.index(2010)]
        # Draw an artificial constant-price initial per-capita level.
        base_income = float(
            # Supply this component of base_income.
            np.exp(generator.uniform(np.log(1500.0), np.log(60000.0)))
        # Close the preceding argument, selection or data-structure expression.
        )
        # Compound raw growth into constant-price GDP per capita.
        real_gdp = base_income * np.cumprod(1.0 + growth_rates / 100.0)
        # Keep raw source levels and percentage rates untransformed.
        series_values: dict[str, np.ndarray] = {
            # Record the CPI field in the current mapping.
            "CPI": cpi,
            # Record the RealGDPpc field in the current mapping.
            "RealGDPpc": real_gdp,
            # Record the InflationRate field in the current mapping.
            "InflationRate": price_rates,
            # Record the RealGDPpcGrowth field in the current mapping.
            "RealGDPpcGrowth": growth_rates,
            # Record the OilRents field in the current mapping.
            "OilRents": generator.uniform(0.01, 9.0, len(years)),
            # Record the TradeOpenness field in the current mapping.
            "TradeOpenness": generator.uniform(35.0, 150.0, len(years)),
            # Record the TermsOfTrade field in the current mapping.
            "TermsOfTrade": 100.0
            # Supply this component of series_values.
            + 3.0 * common_shock
            # Supply this component of series_values.
            + generator.normal(0.0, 3.0, len(years)),
            # Record the InvestmentRate field in the current mapping.
            "InvestmentRate": generator.uniform(17.0, 35.0, len(years)),
            # Record the GovConsumption field in the current mapping.
            "GovConsumption": generator.uniform(10.0, 26.0, len(years)),
            # Record the PopulationGrowth field in the current mapping.
            "PopulationGrowth": generator.normal(0.9, 0.4, len(years)),
            # Record the FuelImportShare field in the current mapping.
            "FuelImportShare": generator.uniform(4.0, 24.0, len(years)),
            # Record the FuelExportShare field in the current mapping.
            "FuelExportShare": generator.uniform(0.2, 12.0, len(years)),
        # Close the preceding argument, selection or data-structure expression.
        }
        # Generate bounded synthetic WGI levels without winsorisation.
        governance = float(generator.uniform(-1.8, 1.8)) + 0.15 * common_shock
        # Draw synthetic population for the monetary GDP scale.
        population = float(generator.uniform(0.4e6, 80.0e6))
        # Iterate (year_position, year) over enumerate(years).
        for year_position, year in enumerate(years):
            # Iterate (variable, specification) over wdi_registry.items().
            for variable, specification in wdi_registry.items():
                # Set indicator from the expression below.
                indicator = specification["indicator_code"]
                # Set value from the expression below.
                value = (
                    # Supply this component of value.
                    np.nan
                    # Apply this additional condition to the current selection
                    # or validation.
                    if (country, indicator, year) in wdi_missing
                    # Supply this component of value.
                    else float(series_values[variable][year_position])
                # Close the preceding argument, selection or data-structure
                # expression.
                )
                # Supply or execute this component of wdi_rows.append.
                wdi_rows.append(
                    # Supply or execute this component of wdi_rows.append.
                    {
                        # Record the country_code field in the current mapping.
                        "country_code": country,
                        # Record the country_name field in the current mapping.
                        "country_name": USAGE_COUNTRY_NAMES[country],
                        # Record the indicator_code field in the current
                        # mapping.
                        "indicator_code": indicator,
                        # Record the indicator_name field in the current
                        # mapping.
                        "indicator_name": indicator_labels[variable],
                        # Record the year field in the current mapping.
                        "year": year,
                        # Record the value field in the current mapping.
                        "value": value,
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    }
                # Close the preceding argument, selection or data-structure
                # expression.
                )
            # Supply or execute this component of wgi_rows.append.
            wgi_rows.append(
                # Supply or execute this component of wgi_rows.append.
                {
                    # Record the country_code field in the current mapping.
                    "country_code": country,
                    # Record the country_name field in the current mapping.
                    "country_name": USAGE_COUNTRY_NAMES[country],
                    # Record the year field in the current mapping.
                    "year": year,
                    # Record the rule_of_law field in the current mapping.
                    "rule_of_law": (
                        # Supply or execute this component of wgi_rows.append.
                        np.nan
                        # Apply this additional condition to the current
                        # selection or validation.
                        if country in retained and year == 2024
                        # Supply or execute this component of wgi_rows.append.
                        else float(governance[year_position])
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    ),
                    # Record the source_release field in the current mapping.
                    "source_release": "TEST_ONLY synthetic 2024 WGI release",
                # Close the preceding argument, selection or data-structure
                # expression.
                }
            # Close the preceding argument, selection or data-structure
            # expression.
            )
            # Check country in retained and year in trade_years before entering
            # this branch.
            if country in retained and year in trade_years:
                # Form a positive synthetic current-dollar GDP denominator.
                nominal = float(
                    # Supply this component of nominal.
                    real_gdp[year_position]
                    # Supply this component of nominal.
                    * population
                    # Supply this component of nominal.
                    * cpi[year_position]
                    # Supply this component of nominal.
                    / 100.0
                # Close the preceding argument, selection or data-structure
                # expression.
                )
                # Set nominal_lookup[country, year] from the expression below.
                nominal_lookup[(country, year)] = nominal
                # Supply or execute this component of nominal_rows.append.
                nominal_rows.append(
                    # Supply or execute this component of nominal_rows.append.
                    {
                        # Record the country_code field in the current mapping.
                        "country_code": country,
                        # Record the country_name field in the current mapping.
                        "country_name": USAGE_COUNTRY_NAMES[country],
                        # Record the year field in the current mapping.
                        "year": year,
                        # Record the indicator_code field in the current
                        # mapping.
                        "indicator_code": labels["nominal_gdp"]["NominalGDP"][
                            # Supply or execute this component of
                            # nominal_rows.append.
                            "indicator_code"
                        # Close the preceding argument, selection or
                        # data-structure expression.
                        ],
                        # Record the value_usd field in the current mapping.
                        "value_usd": nominal,
                        # Record the ingest_validation field in the current
                        # mapping.
                        "ingest_validation": "positivity_confirmed",
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    }
                # Close the preceding argument, selection or data-structure
                # expression.
                )
    # Set wdi_long from the expression below.
    wdi_long = pd.DataFrame(wdi_rows)
    # Set aggregate_frames from the expression below.
    aggregate_frames: list[pd.DataFrame] = [wdi_long]
    # Iterate (aggregate_code, aggregate_name) over (('WLD', 'World'), ('HIC',
    # 'High income')).
    for aggregate_code, aggregate_name in (
        # Include this declared item in the loop over (aggregate_code,
        # aggregate_name).
        ("WLD", "World"),
        # Include this declared item in the loop over (aggregate_code,
        # aggregate_name).
        ("HIC", "High income"),
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Set aggregate from the expression below.
        aggregate = wdi_long.groupby(
            # Supply this component of aggregate.
            ["indicator_code", "indicator_name", "year"], as_index=False
        # Supply this component of aggregate.
        )["value"].mean()
        # Set aggregate['country_code'] from the expression below.
        aggregate["country_code"] = aggregate_code
        # Set aggregate['country_name'] from the expression below.
        aggregate["country_name"] = aggregate_name
        # Supply or execute this component of aggregate_frames.append.
        aggregate_frames.append(aggregate)
    # Set wdi_long from the expression below.
    wdi_long = pd.concat(aggregate_frames, ignore_index=True)
    # Set wdi_wide from the expression below.
    wdi_wide = (
        # Supply this component of wdi_wide.
        wdi_long.pivot(
            # Supply this component of wdi_wide.
            index=[
                # Supply this component of wdi_wide.
                "country_name",
                # Supply this component of wdi_wide.
                "country_code",
                # Supply this component of wdi_wide.
                "indicator_name",
                # Supply this component of wdi_wide.
                "indicator_code",
            # Close the preceding argument, selection or data-structure
            # expression.
            ],
            # Supply this component of wdi_wide.
            columns="year",
            # Supply this component of wdi_wide.
            values="value",
        # Close the preceding argument, selection or data-structure expression.
        )
        # Supply this component of wdi_wide.
        .rename(columns=str)
        # Supply this component of wdi_wide.
        .reset_index()
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set wdi_wide.columns.name from the expression below.
    wdi_wide.columns.name = None
    # Allocate the three primitive coverage patterns separately.
    status_plan = plan_trade_statuses(config, retained)
    # Set status_rows from the expression below.
    status_rows: list[dict[str, Any]] = []
    # Set block_a_rows from the expression below.
    block_a_rows: list[dict[str, Any]] = []
    # Set block_b_rows from the expression below.
    block_b_rows: list[dict[str, Any]] = []
    # Set block_c_rows from the expression below.
    block_c_rows: list[dict[str, Any]] = []
    # Set gas_values from the expression below.
    gas_values: dict[tuple[str, int], float] = {}
    # Set protected from the expression below.
    protected = set(
        # Supply this component of protected.
        config["algorithm_parameters"]["D2_mirror_flow_recovery"][
            # Supply this component of protected.
            "unresolved_pipeline_countries"
        # Close the preceding argument, selection or data-structure expression.
        ]
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set hs_codes from the expression below.
    hs_codes = config["reproducibility_gaps"]["comtrade_hs_codes"][
        # Supply this component of hs_codes.
        "selected_implementation"
    # Close the preceding argument, selection or data-structure expression.
    ]
    # Iterate country over retained.
    for country in retained:
        # Set gas_fraction from the expression below.
        gas_fraction = float(generator.uniform(0.0003, 0.015))
        # Set non_gas_fraction from the expression below.
        non_gas_fraction = float(generator.uniform(0.001, 0.025))
        # Iterate year over trade_years.
        for year in trade_years:
            # Set values from the expression below.
            values: dict[str, float] = {}
            # Iterate series over ('russian_gas', 'world_gas',
            # 'russian_non_gas').
            for series in ("russian_gas", "world_gas", "russian_non_gas"):
                # Set status from the expression below.
                status = status_plan[(country, year, series)]
                # Check status == 'missing' before entering this branch.
                if status == "missing":
                    # Set value from the expression below.
                    value = np.nan
                # Check status == 'verified_zero' before entering this branch.
                elif status == "verified_zero":
                    # Set value from the expression below.
                    value = 0.0
                # Check series == 'russian_gas' before entering this branch.
                elif series == "russian_gas":
                    # Set value from the expression below.
                    value = nominal_lookup[(country, year)] * gas_fraction
                # Check series == 'russian_non_gas' before entering this
                # branch.
                elif series == "russian_non_gas":
                    # Set value from the expression below.
                    value = nominal_lookup[(country, year)] * non_gas_fraction
                # Apply this alternative only when the preceding condition did
                # not hold.
                else:
                    # Set russian_value from the expression below.
                    russian_value = values["russian_gas"]
                    # Set value from the expression below.
                    value = nominal_lookup[(country, year)] * 0.04 + (
                        # Supply this component of value.
                        russian_value if np.isfinite(russian_value) else 0.0
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    )
                # Set values[series] from the expression below.
                values[series] = float(value)
                # Set disposition from the expression below.
                disposition = (
                    # Supply this component of disposition.
                    "case_exclusion"
                    # Apply this additional condition to the current selection
                    # or validation.
                    if status == "missing"
                    # Apply this additional condition to the current selection
                    # or validation.
                    and country in protected
                    # Apply this additional condition to the current selection
                    # or validation.
                    and series == "russian_gas"
                    # Supply this component of disposition.
                    else "imputable" if status == "missing" else "usable"
                # Close the preceding argument, selection or data-structure
                # expression.
                )
                # Supply or execute this component of status_rows.append.
                status_rows.append(
                    # Supply or execute this component of status_rows.append.
                    {
                        # Record the reporter_code field in the current
                        # mapping.
                        "reporter_code": country,
                        # Record the year field in the current mapping.
                        "year": year,
                        # Record the series field in the current mapping.
                        "series": series,
                        # Record the request_type field in the current mapping.
                        "request_type": (
                            # Supply or execute this component of
                            # status_rows.append.
                            "individual"
                            # Apply this additional condition to the current
                            # selection or validation.
                            if status != "positive"
                            # Supply or execute this component of
                            # status_rows.append.
                            else "multi_country"
                        # Close the preceding argument, selection or
                        # data-structure expression.
                        ),
                        # Record the retry_succeeded field in the current
                        # mapping.
                        "retry_succeeded": int(status == "verified_zero"),
                        # Record the hs_report_confirmed field in the current
                        # mapping.
                        "hs_report_confirmed": int(status != "missing"),
                        # Record the value_usd field in the current mapping.
                        "value_usd": float(value),
                        # Record the final_status field in the current mapping.
                        "final_status": status,
                        # Record the downstream_disposition field in the
                        # current mapping.
                        "downstream_disposition": disposition,
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    }
                # Close the preceding argument, selection or data-structure
                # expression.
                )
            # Iterate (product, weight) over (('Coal', 0.15), ('Crude oil',
            # 0.45), ('Refined petroleum', 0.4)).
            for product, weight in (
                # Include this declared item in the loop over (product,
                # weight).
                ("Coal", 0.15),
                # Include this declared item in the loop over (product,
                # weight).
                ("Crude oil", 0.45),
                # Include this declared item in the loop over (product,
                # weight).
                ("Refined petroleum", 0.4),
            # Close the preceding argument, selection or data-structure
            # expression.
            ):
                # Supply or execute this component of block_a_rows.append.
                block_a_rows.append(
                    # Supply or execute this component of block_a_rows.append.
                    {
                        # Record the reporter_code field in the current
                        # mapping.
                        "reporter_code": country,
                        # Record the reporter_name field in the current
                        # mapping.
                        "reporter_name": USAGE_COUNTRY_NAMES[country],
                        # Record the partner_code field in the current mapping.
                        "partner_code": "RUS",
                        # Record the partner_name field in the current mapping.
                        "partner_name": USAGE_COUNTRY_NAMES["RUS"],
                        # Record the year field in the current mapping.
                        "year": year,
                        # Record the product_description field in the current
                        # mapping.
                        "product_description": product,
                        # Record the product_code field in the current mapping.
                        "product_code": hs_codes[product][0],
                        # Record the trade_flow field in the current mapping.
                        "trade_flow": "import",
                        # Record the value_usd field in the current mapping.
                        "value_usd": float(values["russian_non_gas"] * weight),
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    }
                # Close the preceding argument, selection or data-structure
                # expression.
                )
            # Set gas_values[country, year] from the expression below.
            gas_values[(country, year)] = values["russian_gas"]
            # Check country != 'RUS' before entering this branch.
            if country != "RUS":
                # Iterate (product, weight) over (('LNG', 0.35), ('Pipeline
                # gas', 0.65)).
                for product, weight in (("LNG", 0.35), ("Pipeline gas", 0.65)):
                    # Set gas_value from the expression below.
                    gas_value = float(values["russian_gas"] * weight)
                    # Set flag from the expression below.
                    flag = (
                        # Supply this component of flag.
                        "exporter_mirror_pipeline_absent"
                        # Apply this additional condition to the current
                        # selection or validation.
                        if product == "Pipeline gas" and country in protected
                        # Supply this component of flag.
                        else (
                            # Supply this component of flag.
                            "no_mirror_available"
                            # Apply this additional condition to the current
                            # selection or validation.
                            if not np.isfinite(gas_value)
                            # Supply this component of flag.
                            else "importer_reported"
                        # Close the preceding argument, selection or
                        # data-structure expression.
                        )
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    )
                    # Supply or execute this component of block_b_rows.append.
                    block_b_rows.append(
                        # Supply or execute this component of
                        # block_b_rows.append.
                        {
                            # Record the reporter_code field in the current
                            # mapping.
                            "reporter_code": country,
                            # Record the reporter_name field in the current
                            # mapping.
                            "reporter_name": USAGE_COUNTRY_NAMES[country],
                            # Record the partner_code field in the current
                            # mapping.
                            "partner_code": "RUS",
                            # Record the partner_name field in the current
                            # mapping.
                            "partner_name": USAGE_COUNTRY_NAMES["RUS"],
                            # Record the year field in the current mapping.
                            "year": year,
                            # Record the product_description field in the
                            # current mapping.
                            "product_description": product,
                            # Record the trade_flow field in the current
                            # mapping.
                            "trade_flow": "import",
                            # Record the value_usd field in the current
                            # mapping.
                            "value_usd": gas_value,
                            # Record the mirror_availability field in the
                            # current mapping.
                            "mirror_availability": flag,
                        # Close the preceding argument, selection or
                        # data-structure expression.
                        }
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    )
            # Supply or execute this component of block_c_rows.append.
            block_c_rows.append(
                # Supply or execute this component of block_c_rows.append.
                {
                    # Record the reporter_code field in the current mapping.
                    "reporter_code": country,
                    # Record the reporter_name field in the current mapping.
                    "reporter_name": USAGE_COUNTRY_NAMES[country],
                    # Record the partner_code field in the current mapping.
                    "partner_code": "WLD",
                    # Record the partner_name field in the current mapping.
                    "partner_name": "World",
                    # Record the year field in the current mapping.
                    "year": year,
                    # Record the product_description field in the current
                    # mapping.
                    "product_description": "Total gas",
                    # Record the trade_flow field in the current mapping.
                    "trade_flow": "import",
                    # Record the value_usd field in the current mapping.
                    "value_usd": values["world_gas"],
                    # Record the recovered_via field in the current mapping.
                    "recovered_via": (
                        # Supply or execute this component of
                        # block_c_rows.append.
                        None
                        # Apply this additional condition to the current
                        # selection or validation.
                        if not np.isfinite(values["world_gas"])
                        # Supply or execute this component of
                        # block_c_rows.append.
                        else "importer_reported"
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    ),
                # Close the preceding argument, selection or data-structure
                # expression.
                }
            # Close the preceding argument, selection or data-structure
            # expression.
            )
    # Set mirror_candidates from the expression below.
    mirror_candidates = tuple(
        # Supply this component of mirror_candidates.
        cell
        # Evaluate this comprehension once for each declared item.
        for cell, value in gas_values.items()
        # Apply this additional condition to the current selection or
        # validation.
        if cell[0] != "RUS" and np.isfinite(value) and value > 0.0
    # Close the preceding argument, selection or data-structure expression.
    )
    # Iterate (country, year) over mirror_candidates[:5].
    for country, year in mirror_candidates[:5]:
        # Supply or execute this component of block_b_rows.append.
        block_b_rows.append(
            # Supply or execute this component of block_b_rows.append.
            {
                # Record the reporter_code field in the current mapping.
                "reporter_code": "RUS",
                # Record the reporter_name field in the current mapping.
                "reporter_name": USAGE_COUNTRY_NAMES["RUS"],
                # Record the partner_code field in the current mapping.
                "partner_code": country,
                # Record the partner_name field in the current mapping.
                "partner_name": USAGE_COUNTRY_NAMES[country],
                # Record the year field in the current mapping.
                "year": year,
                # Record the product_description field in the current mapping.
                "product_description": "LNG",
                # Record the trade_flow field in the current mapping.
                "trade_flow": "export",
                # Record the value_usd field in the current mapping.
                "value_usd": float(gas_values[(country, year)] * 0.35 * 1.05),
                # Record the mirror_availability field in the current mapping.
                "mirror_availability": "exporter_mirror_lng_positive",
            # Close the preceding argument, selection or data-structure
            # expression.
            }
        # Close the preceding argument, selection or data-structure expression.
        )
    # Allocate fictitious, explicitly labelled documentary rows.
    policy_rows: list[dict[str, Any]] = []
    # Retain the late sanctions-entry exceptions of Table A4.
    late_sanctions = {
        # Record the BIH field in the current mapping.
        "BIH": 2022,
        # Record the KOR field in the current mapping.
        "KOR": 2022,
        # Record the NZL field in the current mapping.
        "NZL": 2022,
        # Record the SGP field in the current mapping.
        "SGP": 2022,
        # Record the MDA field in the current mapping.
        "MDA": 2023,
    # Close the preceding argument, selection or data-structure expression.
    }
    # Retain the four pre-2022 direct-arming entry years.
    early_arming = {"CZE": 2018, "POL": 2018, "TUR": 2019, "USA": 2018}
    # Set alignment_countries from the expression below.
    alignment_countries = {"ALB", "BIH", "ISL", "MDA", "MKD", "NOR"}
    # Iterate (policy, countries) over (('sanctions',
    # tuple(rosters['sanctions_target_42']) + ('UKR',)), ('direct_arming',
    # tuple(rosters['direct_arming_target_29']))).
    for policy, countries in (
        # Include this declared item in the loop over (policy, countries).
        ("sanctions", tuple(rosters["sanctions_target_42"]) + ("UKR",)),
        # Include this declared item in the loop over (policy, countries).
        ("direct_arming", tuple(rosters["direct_arming_target_29"])),
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Iterate country over countries.
        for country in countries:
            # Set year from the expression below.
            year = (
                # Supply this component of year.
                late_sanctions.get(country, 2014)
                # Apply this additional condition to the current selection or
                # validation.
                if policy == "sanctions"
                # Supply this component of year.
                else early_arming.get(country, 2022)
            # Close the preceding argument, selection or data-structure
            # expression.
            )
            # Set policy_type from the expression below.
            policy_type = (
                # Supply this component of policy_type.
                "alignment_notice"
                # Apply this additional condition to the current selection or
                # validation.
                if policy == "sanctions" and country in alignment_countries
                # Supply this component of policy_type.
                else (
                    # Supply this component of policy_type.
                    "legal_instrument"
                    # Apply this additional condition to the current selection
                    # or validation.
                    if policy == "sanctions"
                    # Supply this component of policy_type.
                    else "lethal_commitment"
                # Close the preceding argument, selection or data-structure
                # expression.
                )
            # Close the preceding argument, selection or data-structure
            # expression.
            )
            # Supply or execute this component of policy_rows.append.
            policy_rows.append(
                # Supply or execute this component of policy_rows.append.
                {
                    # Record the country_code field in the current mapping.
                    "country_code": country,
                    # Record the policy field in the current mapping.
                    "policy": policy,
                    # Record the issuing_authority field in the current
                    # mapping.
                    "issuing_authority": "TEST_ONLY simulated authority",
                    # Record the document_title field in the current mapping.
                    "document_title": (
                        # Supply or execute this component of
                        # policy_rows.append.
                        f"TEST_ONLY simulated {policy_type}: "
                        # Supply or execute this component of
                        # policy_rows.append.
                        f"{country} {year}"
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    ),
                    # Record the publication_date field in the current mapping.
                    "publication_date": f"{year}-01-15",
                    # Record the archived_source field in the current mapping.
                    "archived_source": (
                        # Supply or execute this component of
                        # policy_rows.append.
                        "TEST_ONLY/generated/" f"{country}/{policy}/{year}"
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    ),
                    # Record the policy_type field in the current mapping.
                    "policy_type": policy_type,
                    # Record the entry_year field in the current mapping.
                    "entry_year": year,
                    # Record the coding_decision field in the current mapping.
                    "coding_decision": (
                        # Supply or execute this component of
                        # policy_rows.append.
                        "excluded_from_target"
                        # Apply this additional condition to the current
                        # selection or validation.
                        if country == "UKR"
                        # Supply or execute this component of
                        # policy_rows.append.
                        else "accepted"
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    ),
                    # Record the priority_rank field in the current mapping.
                    "priority_rank": (
                        # Supply or execute this component of
                        # policy_rows.append.
                        2
                        # Apply this additional condition to the current
                        # selection or validation.
                        if policy_type == "alignment_notice"
                        # Supply or execute this component of
                        # policy_rows.append.
                        else 1 if policy_type == "legal_instrument" else 3
                    # Close the preceding argument, selection or data-structure
                    # expression.
                    ),
                # Close the preceding argument, selection or data-structure
                # expression.
                }
            # Close the preceding argument, selection or data-structure
            # expression.
            )
    # Represent geopolitical estimation bans and coverage exclusions.
    roles = {
        # Record the RUS field in the current mapping.
        "RUS": "target_and_donor_ban",
        # Record the BLR field in the current mapping.
        "BLR": "donor_ban",
        # Record the UKR field in the current mapping.
        "UKR": "target_exclusion_from_ATT",
        # Record the SYR field in the current mapping.
        "SYR": "critical_window_exclusion",
        # Record the SDN field in the current mapping.
        "SDN": "critical_window_exclusion",
    # Close the preceding argument, selection or data-structure expression.
    }
    # Create one geographic and estimation-role record per country.
    metadata_rows = [
        # Supply this component of metadata_rows.
        {
            # Record the country_code field in the current mapping.
            "country_code": code,
            # Record the country_name field in the current mapping.
            "country_name": USAGE_COUNTRY_NAMES[code],
            # Record the world_bank_region field in the current mapping.
            "world_bank_region": region_map[code],
            # Record the geopolitical_role field in the current mapping.
            "geopolitical_role": roles.get(code, "none"),
            # Record the retained_in_final_panel field in the current mapping.
            "retained_in_final_panel": int(code in retained),
        # Close the preceding argument, selection or data-structure expression.
        }
        # Evaluate this comprehension once for each declared item.
        for code in universe
    # Close the preceding argument, selection or data-structure expression.
    ]
    # Set sources from the expression below.
    sources = {
        # Record the wdi_raw_wide field in the current mapping.
        "wdi_raw_wide": wdi_wide,
        # Record the wdi_raw_long field in the current mapping.
        "wdi_raw_long": wdi_long,
        # Record the wgi_raw_long field in the current mapping.
        "wgi_raw_long": pd.DataFrame(wgi_rows),
        # Record the comtrade_block_a_raw field in the current mapping.
        "comtrade_block_a_raw": pd.DataFrame(block_a_rows),
        # Record the comtrade_block_b_raw field in the current mapping.
        "comtrade_block_b_raw": pd.DataFrame(block_b_rows),
        # Record the comtrade_block_c_raw field in the current mapping.
        "comtrade_block_c_raw": pd.DataFrame(block_c_rows),
        # Record the comtrade_cell_status_raw field in the current mapping.
        "comtrade_cell_status_raw": pd.DataFrame(status_rows),
        # Record the nominal_gdp_raw field in the current mapping.
        "nominal_gdp_raw": pd.DataFrame(nominal_rows),
        # Record the policy_ledger_raw field in the current mapping.
        "policy_ledger_raw": pd.DataFrame(policy_rows),
        # Record the country_metadata_raw field in the current mapping.
        "country_metadata_raw": pd.DataFrame(metadata_rows),
    # Close the preceding argument, selection or data-structure expression.
    }
    # Iterate (name, frame) over sources.items().
    for name, frame in sources.items():
        # Set schema from the expression below.
        schema = config["raw_data_schemas"][name]
        # Set frame from the expression below.
        frame = frame.loc[:, schema["columns"]].copy()
        # Iterate column over schema['int_columns'].
        for column in schema["int_columns"]:
            # Set frame[column] from the expression below.
            frame[column] = frame[column].astype("int64")
        # Iterate column over schema['float_columns'].
        for column in schema["float_columns"]:
            # Set frame[column] from the expression below.
            frame[column] = frame[column].astype("float64")
        # Check name == 'comtrade_cell_status_raw' before entering this branch.
        if name == "comtrade_cell_status_raw":
            # Set frame[['retry_succeeded', 'hs_report_confirmed']] from the
            # expression below.
            frame[["retry_succeeded", "hs_report_confirmed"]] = frame[
                # Supply this component of frame[['retry_succeeded',
                # 'hs_report_confirmed']].
                ["retry_succeeded", "hs_report_confirmed"]
            # Supply this component of frame[['retry_succeeded',
            # 'hs_report_confirmed']].
            ].astype("int8")
        # Check name == 'country_metadata_raw' before entering this branch.
        if name == "country_metadata_raw":
            # Set frame['retained_in_final_panel'] from the expression below.
            frame["retained_in_final_panel"] = frame[
                # Supply this component of frame['retained_in_final_panel'].
                "retained_in_final_panel"
            # Supply this component of frame['retained_in_final_panel'].
            ].astype("int8")
        # Check name == 'policy_ledger_raw' before entering this branch.
        if name == "policy_ledger_raw":
            # Set frame['priority_rank'] from the expression below.
            frame["priority_rank"] = frame["priority_rank"].astype("int8")
            # Set exclusions from the expression below.
            exclusions = rosters["geopolitical_exclusions"]
            # Set frame.attrs['estimation_exclusions'] from the expression
            # below.
            frame.attrs["estimation_exclusions"] = tuple(
                # Supply this component of
                # frame.attrs['estimation_exclusions'].
                sorted(
                    # Supply this component of
                    # frame.attrs['estimation_exclusions'].
                    set(exclusions["target_and_donor_ban"])
                    # Supply this component of
                    # frame.attrs['estimation_exclusions'].
                    | set(exclusions["donor_ban"])
                # Close the preceding argument, selection or data-structure
                # expression.
                )
            # Close the preceding argument, selection or data-structure
            # expression.
            )
        # Supply or execute this component of frame.attrs.update.
        frame.attrs.update(
            # Supply or execute this component of frame.attrs.update.
            {
                # Record the synthetic field in the current mapping.
                "synthetic": True,
                # Record the fixture_seed field in the current mapping.
                "fixture_seed": int(seed),
                # Record the source_id field in the current mapping.
                "source_id": f"TEST_ONLY/{name}/v1",
            # Close the preceding argument, selection or data-structure
            # expression.
            }
        # Close the preceding argument, selection or data-structure expression.
        )
        # Set sources[name] from the expression below.
        sources[name] = frame
    # Return the declared outputs and their audit evidence.
    return sources
```

### **Step 2: Loading the Configuration (`config.yaml`)**

**Methodology.** Parse the existing file with a safe **YAML 1.2** loader and reject duplicate keys. This preserves unquoted scientific numeric literals such as `1e-10` as numbers. Generic YAML 1.1 parsing can interpret this literal differently and is not the project's validated parsing path. The loader returns the plain dictionary `config` and checks its section and operative binding contracts. It deliberately leaves arrays as arrays; the notebook handles each declared window and tuple boundary. It does not call unknown provenance resolved.

```python
# Define load_usage_config with an explicit input and return contract.
def load_usage_config(path: str | Path = "config.yaml") -> dict[str, Any]:
    """Read the existing YAML 1.2 configuration without weakening readiness.

    Input is a filename in the notebook's working directory or an explicit
    path.
    The process rejects duplicate YAML keys, safely parses the mapping, and
    uses
    the notebook's section/binding validators to detect contradictory
    execution
    declarations. Output is the plain dict named config by the calling cell.
    This loader deliberately does not resolve evidence gaps, change seeds,
    move
    runtime paths, recreate the file, or convert every YAML list into a
    tuple.
    Raises FileNotFoundError, TypeError, parser errors and the notebook's
    binding
    validation exceptions; these are configuration errors, not study
    results.
    """
    # Reject inputs when this condition implies: path must be a nonempty string
    # or pathlib.Path.
    if not isinstance(path, (str, Path)) or not str(path).strip():
        # Raise the stated exception with the concrete validation failure.
        raise TypeError("path must be a nonempty string or pathlib.Path.")
    # Resolve the existing YAML file relative to the notebook folder.
    config_path = Path(path).expanduser().resolve()
    # Validate not config_path.is_file() and report the named failure.
    if not config_path.is_file():
        # Raise the stated exception with the concrete validation failure.
        raise FileNotFoundError(
            # Supply the concrete diagnostic message for FileNotFoundError.
            f"Required configuration is absent: {config_path}"
        # Close the preceding argument, selection or data-structure expression.
        )
    # Create the project-compatible safe YAML 1.2 loader.
    parser = YAML(typ="safe")
    # Set parser.version from the expression below.
    parser.version = (1, 2)
    # Set parser.allow_duplicate_keys from the expression below.
    parser.allow_duplicate_keys = False
    # Read the existing file with explicit UTF-8 decoding and scoped cleanup.
    with config_path.open("r", encoding="utf-8") as stream:
        # Set config from the expression below.
        config = parser.load(stream)
    # Reject inputs when this condition implies: config.yaml must have a
    # mapping at its root.
    if not isinstance(config, dict):
        # Raise the stated exception with the concrete validation failure.
        raise TypeError("config.yaml must have a mapping at its root.")
    # Supply or execute this component of validate_study_config_sections.
    validate_study_config_sections(config)
    # Supply or execute this component of validate_configuration_bindings.
    validate_configuration_bindings(config)
    # Return the declared outputs and their audit evidence.
    return config
```

Read the configuration, then materialise every requested variable from the single consistent synthetic world. The fixture seed below is independent of the configured inference registries.

```python
# Set config from the expression below.
config = load_usage_config("config.yaml")
# Set synthetic_sources from the expression below.
synthetic_sources = generate_study_sources(config, seed=20261003)
# Set wdi_raw_wide from the expression below.
wdi_raw_wide = synthetic_sources["wdi_raw_wide"]
# Set wdi_raw_long from the expression below.
wdi_raw_long = synthetic_sources["wdi_raw_long"]
# Set wgi_raw_long from the expression below.
wgi_raw_long = synthetic_sources["wgi_raw_long"]
# Set comtrade_block_a_raw from the expression below.
comtrade_block_a_raw = synthetic_sources["comtrade_block_a_raw"]
# Set comtrade_block_b_raw from the expression below.
comtrade_block_b_raw = synthetic_sources["comtrade_block_b_raw"]
# Set comtrade_block_c_raw from the expression below.
comtrade_block_c_raw = synthetic_sources["comtrade_block_c_raw"]
# Set comtrade_cell_status_raw from the expression below.
comtrade_cell_status_raw = synthetic_sources["comtrade_cell_status_raw"]
# Set nominal_gdp_raw from the expression below.
nominal_gdp_raw = synthetic_sources["nominal_gdp_raw"]
# Set policy_ledger_raw from the expression below.
policy_ledger_raw = synthetic_sources["policy_ledger_raw"]
# Set country_metadata_raw from the expression below.
country_metadata_raw = synthetic_sources["country_metadata_raw"]
```

### **Step 3: Validate Inputs and Prepare the Interface Mapping**

**Methodology.** Call the actual notebook ingest layer for an isolated input-contract audit. Verify exact wide/long equality, unique keys, raw retained-grid missingness, nominal-GDP positivity and trade numerator/denominator closure. This audit is not a successful stage in a top-level run: the top-level call still performs its own readiness preflight and ingestion.

```python
# Define audit_usage_sources with an explicit input and return contract.
def audit_usage_sources(
    # Declare this input's name and type for audit_usage_sources.
    sources: Mapping[str, pd.DataFrame], config: Mapping[str, Any]
# Specify this typed argument or field of the current expression.
) -> dict[str, Any]:
    """Validate raw-fixture contracts using the actual notebook ingest layer.

    Inputs are the ten generated DataFrames and the unchanged parsed config.
    The process calls Task 1, audits unique keys and wide/long equality,
    verifies
    retained-grid raw missingness, positive GDP, documentary entry-year
    counts,
    trade statuses and positive-numerator/denominator consistency. Outputs
    are
    human-readable shape/coverage tables plus configuration/source
    fingerprints.
    This is an isolated input-contract audit, not the top-level stage run or
    certification of source authenticity. It cannot supply readiness tokens.
    Raises TypeError/ValueError and the actual ingest exceptions on
    violations.
    """
    # Reject inputs when this condition implies: sources must contain exactly
    # the ten declared names.
    if not isinstance(sources, Mapping) or set(sources) != set(
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        config["raw_data_schemas"]
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "sources must contain exactly the ten declared names."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Run the actual notebook Task 1 ingest and schema contract.
    bundle = orchestrate_raw_data_ingest(sources, config)
    # Record raw table sizes and distinguish structural missing values.
    shapes = pd.DataFrame(
        # Supply this component of shapes.
        [
            # Supply this component of shapes.
            {
                # Record the source field in the current mapping.
                "source": name,
                # Record the rows field in the current mapping.
                "rows": len(frame),
                # Record the columns field in the current mapping.
                "columns": len(frame.columns),
                # Record the missing_values field in the current mapping.
                "missing_values": int(frame.isna().sum().sum()),
            # Close the preceding argument, selection or data-structure
            # expression.
            }
            # Evaluate this comprehension once for each declared item.
            for name, frame in sources.items()
        # Close the preceding argument, selection or data-structure expression.
        ]
    # Supply this component of shapes.
    ).set_index("source")
    # Iterate (name, frame) over sources.items().
    for name, frame in sources.items():
        # Validate frame.duplicated(config['raw_data_schemas'][name]['primary_k
        # ey']).any() and report the named failure.
        if frame.duplicated(
            # Evaluate this specific condition or chained comparison for the
            # input gate.
            config["raw_data_schemas"][name]["primary_key"]
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        ).any():
            # Raise the stated exception with the concrete validation failure.
            raise ValueError(f"Duplicate primary key in {name}.")
    # Set wide from the expression below.
    wide = sources["wdi_raw_wide"]
    # Reverse the wide table for an independent raw-value closure.
    melted = wide.melt(
        # Supply this component of melted.
        id_vars=[
            # Supply this component of melted.
            "country_code",
            # Supply this component of melted.
            "country_name",
            # Supply this component of melted.
            "indicator_code",
            # Supply this component of melted.
            "indicator_name",
        # Close the preceding argument, selection or data-structure expression.
        ],
        # Supply this component of melted.
        value_vars=config["raw_data_schemas"]["wdi_raw_wide"]["year_columns"],
        # Supply this component of melted.
        var_name="year",
        # Supply this component of melted.
        value_name="value",
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set melted['year'] from the expression below.
    melted["year"] = melted["year"].astype("int64")
    # Set keys from the expression below.
    keys = ["country_code", "indicator_code", "year"]
    # Set long_values from the expression below.
    long_values = sources["wdi_raw_long"].set_index(keys)["value"].sort_index()
    # Set wide_values from the expression below.
    wide_values = melted.set_index(keys)["value"].sort_index()
    # Supply or execute this component of pd.testing.assert_series_equal.
    pd.testing.assert_series_equal(long_values, wide_values, check_names=False)
    # Build the 129-country descriptive roster from configured roles.
    retained = set(
        # Supply this component of retained.
        sources["country_metadata_raw"].loc[
            # Supply this component of retained.
            sources["country_metadata_raw"]["retained_in_final_panel"].eq(1),
            # Supply this component of retained.
            "country_code",
        # Close the preceding argument, selection or data-structure expression.
        ]
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set raw from the expression below.
    raw = sources["wdi_raw_long"]
    # Set retained_raw from the expression below.
    retained_raw = raw.loc[
        # Supply this component of retained_raw.
        raw["country_code"].isin(retained) & raw["year"].between(2000, 2024)
    # Close the preceding argument, selection or data-structure expression.
    ]
    # Build the retained 2000-2024 grid before counting missing cells.
    expected_grid = pd.MultiIndex.from_product(
        # Supply this component of expected_grid.
        [
            # Supply this component of expected_grid.
            sorted(retained),
            # Supply this component of expected_grid.
            sorted(raw["indicator_code"].unique()),
            # Supply this component of expected_grid.
            range(2000, 2025),
        # Close the preceding argument, selection or data-structure expression.
        ],
        # Supply this component of expected_grid.
        names=keys,
    # Close the preceding argument, selection or data-structure expression.
    )
    # Count absent rows as missing on the fixed raw-value grid.
    retained_values = retained_raw.set_index(keys)["value"].reindex(
        # Supply this component of retained_values.
        expected_grid
    # Close the preceding argument, selection or data-structure expression.
    )
    # Set missing from the expression below.
    missing = (
        # Supply this component of missing.
        retained_values.isna()
        # Supply this component of missing.
        .groupby(level="indicator_code")
        # Supply this component of missing.
        .sum()
        # Supply this component of missing.
        .astype(int)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Iterate (indicator, count) over config['raw_data_schemas']['wdi_raw_long'
    # ]['raw_missing_targets'].items().
    for indicator, count in config["raw_data_schemas"]["wdi_raw_long"][
        # Include this declared item in the loop over (indicator, count).
        "raw_missing_targets"
    # Include this declared item in the loop over (indicator, count).
    ].items():
        # Validate int(missing.loc[indicator]) != int(count) and report the
        # named failure.
        if int(missing.loc[indicator]) != int(count):
            # Raise the stated exception with the concrete validation failure.
            raise ValueError(
                # Supply the concrete diagnostic message for ValueError.
                f"Raw retained-grid missingness differs for {indicator}."
            # Close the preceding argument, selection or data-structure
            # expression.
            )
    # Generate bounded synthetic WGI levels without winsorisation.
    governance = sources["wgi_raw_long"]
    # Generate bounded synthetic WGI levels without winsorisation.
    governance = governance.loc[
        # Supply this component of governance.
        governance["country_code"].isin(retained)
        # Supply this component of governance.
        & governance["year"].between(2000, 2024)
    # Close the preceding argument, selection or data-structure expression.
    ]
    # Reject inputs when this condition implies: The synthetic WGI grid or
    # missing count is incorrect.
    if (
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        len(governance) != 3225
        # Apply this additional condition to the current selection or
        # validation.
        or int(governance["rule_of_law"].isna().sum()) != 129
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "The synthetic WGI grid or missing count is incorrect."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Form a positive synthetic current-dollar GDP denominator.
    nominal = sources["nominal_gdp_raw"]
    # Reject inputs when this condition implies: Every one of the 1,032
    # nominal-GDP cells must be positive.
    if (
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        len(nominal) != 1032
        # Apply this additional condition to the current selection or
        # validation.
        or not np.isfinite(nominal["value_usd"]).all()
        # Apply this additional condition to the current selection or
        # validation.
        or not nominal["value_usd"].gt(0.0).all()
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "Every one of the 1,032 nominal-GDP cells must be positive."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set ledger from the expression below.
    ledger = sources["comtrade_cell_status_raw"]
    # Tabulate positive, verified-zero and missing cells separately.
    trade_counts = pd.crosstab(ledger["series"], ledger["final_status"])
    # Iterate (series, counts) over
    # config['execution_parameters']['comtrade_status_counts'].items().
    for series, counts in config["execution_parameters"][
        # Include this declared item in the loop over (series, counts).
        "comtrade_status_counts"
    # Include this declared item in the loop over (series, counts).
    ].items():
        # Iterate (status, count) over counts.items().
        for status, count in counts.items():
            # Validate int(trade_counts.loc[series, status]) != int(count) and
            # report the named failure.
            if int(trade_counts.loc[series, status]) != int(count):
                # Raise the stated exception with the concrete validation
                # failure.
                raise ValueError(
                    # Supply the concrete diagnostic message for ValueError.
                    f"The {series}/{status} count differs from config."
                # Close the preceding argument, selection or data-structure
                # expression.
                )
    # Set zero from the expression below.
    zero = ledger.loc[ledger["final_status"].eq("verified_zero")]
    # Reject inputs when this condition implies: A synthetic verified zero
    # lacks its two explicit confirmations.
    if (
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        not zero["value_usd"].eq(0.0).all()
        # Apply this additional condition to the current selection or
        # validation.
        or not zero[["retry_succeeded", "hs_report_confirmed"]]
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        .eq(1)
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        .all()
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        .all()
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "A synthetic verified zero lacks its two explicit confirmations."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set trade_values from the expression below.
    trade_values = ledger.pivot(
        # Supply this component of trade_values.
        index=["reporter_code", "year"], columns="series", values="value_usd"
    # Close the preceding argument, selection or data-structure expression.
    )
    # Select observed positive Russian-gas numerators for closure.
    positive_gas = trade_values["russian_gas"].gt(0.0)
    # Reject inputs when this condition implies: A positive Russian gas
    # numerator exceeds its world total.
    if (
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        not trade_values.loc[positive_gas, "world_gas"]
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        .ge(trade_values.loc[positive_gas, "russian_gas"])
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        .all()
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "A positive Russian gas numerator exceeds its world total."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Set policy from the expression below.
    policy = sources["policy_ledger_raw"]
    # Set sanctions_count from the expression below.
    sanctions_count = int(policy["policy"].eq("sanctions").sum())
    # Set arming from the expression below.
    arming = policy.loc[policy["policy"].eq("direct_arming")]
    # Reject inputs when this condition implies: The Table A4 synthetic policy
    # timing fixture is inconsistent.
    if (
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        sanctions_count != 43
        # Apply this additional condition to the current selection or
        # validation.
        or len(arming) != 29
        # Apply this additional condition to the current selection or
        # validation.
        or set(arming.loc[arming["entry_year"].lt(2022), "country_code"])
        # Evaluate this specific condition or chained comparison for the input
        # gate.
        != {"CZE", "POL", "TUR", "USA"}
    # Close the preceding argument, selection or data-structure expression.
    ):
        # Raise the stated exception with the concrete validation failure.
        raise ValueError(
            # Supply the concrete diagnostic message for ValueError.
            "The Table A4 synthetic policy timing fixture is inconsistent."
        # Close the preceding argument, selection or data-structure expression.
        )
    # Return the declared outputs and their audit evidence.
    return {
        # Record the shapes field in the current mapping.
        "shapes": shapes,
        # Record the wdi_missing field in the current mapping.
        "wdi_missing": missing,
        # Record the trade_counts field in the current mapping.
        "trade_counts": trade_counts,
        # Record the config_fingerprint field in the current mapping.
        "config_fingerprint": bundle.config_fingerprint,
        # Record the source_fingerprints field in the current mapping.
        "source_fingerprints": {
            # Supply the displayed literal or argument to the enclosing
            # expression.
            name: record.fingerprint_sha256
            # Evaluate this comprehension once for each declared item.
            for name, record in bundle.records.items()
        # Close the preceding argument, selection or data-structure expression.
        },
        # Record the synthetic field in the current mapping.
        "synthetic": all(
            # Supply the displayed literal or argument to the enclosing
            # expression.
            bool(frame.attrs.get("synthetic", False))
            # Evaluate this comprehension once for each declared item.
            for frame in sources.values()
        # Close the preceding argument, selection or data-structure expression.
        ),
    # Close the preceding argument, selection or data-structure expression.
    }
```

Inspect the concrete source shapes and coverage, and prepare the mandatory two-entry `data` mapping. The full configuration is supplied under `study_config`; the variable holding it is still called `config` as requested.

```python
# Set source_audit from the expression below.
source_audit = audit_usage_sources(synthetic_sources, config)
# Display the actual computed audit or execution field below.
print(source_audit["shapes"].to_string())
# Display the actual computed audit or execution field below.
print(source_audit["wdi_missing"].to_string())
# Display the actual computed audit or execution field below.
print(source_audit["trade_counts"].to_string())
# Assemble the single data mapping accepted by the interface.
data: dict[str, Any] = {"study_config": config, "sources": synthetic_sources}
```

The raw retained-grid counts are CPI 25, inflation 32, real GDP per capita 0, real-GDP growth 0, oil rents 403, trade openness 10, terms of trade 645, investment 46, government consumption 34 and population growth 1. In this fixture fuel imports have 203 missing primitives and fuel exports have zero; **203 is not an independently retrieved FuelShareBalance series**. Rule-of-law missingness is 129 in its separate WGI source. Country eligibility is fixed before these cells are completed.

The synthetic trade ledger has:

| Primitive | Positive | Verified zero | Missing | Total |
| --- | ---: | ---: | ---: | ---: |
| Russian gas | 229 | 733 | 70 | 1,032 |
| World gas | 809 | 185 | 38 | 1,032 |
| Russian non-gas | 769 | 224 | 39 | 1,032 |

**Supplying genuine external inputs.** The final wrapper accepts `external_inputs` as a mapping. Add only the actual versioned objects described in the interface table. Passing an empty mapping or `None` leaves them unavailable and cannot certify a study. The wrapper rejects unsupported keys; the notebook rejects contradictory duplicates and unauthenticated authority. Once you possess authentic raw data and external objects, pass those raw DataFrames through `raw_sources` instead of the synthetic sources. Synthetic data are not a substitute for the provenance evidence, the publication means, or the matched/exposure sample assertions; resolving the current provenance gaps alone does not make this synthetic fixture reproduce all manuscript outputs.

#### **What the 31-stage scheduler does after a successful preflight**

The following order is derived from the actual draft registry. Stage numbers differ from task numbers after the resampling engine handoff; do not infer an API from task labels.

| Stage | Registry name | Live operation/output |
| ---: | --- | --- |
| 1 | `raw_data_ingest` | Ingest and fingerprint the ten raw frames. |
| 2 | `study_config_validation` | Emit the digest-bound configuration readiness report. |
| 3 | `data_cleansing` | Exclude aggregates by registry membership; verify the cleansed handoff. |
| 4 | `eligibility_screening` | Freeze raw-data eligibility, 131 candidates → 129 retained. |
| 5 | `control_completion` | Complete primitives within country; construct fuel balance with union flags. |
| 6 | `outcome_repair` | Repair permitted leading gaps and the two BIH cells with method tags. |
| 7 | `panel_assembly` | Build log levels, decimal ATT rates and one-year-lagged controls. |
| 8 | `treatment_coding` | Resolve policy coding from the ingested documentary ledger. |
| 9 | `partition_construction` | Derive all eight policy/outcome partitions and audit rosters/means. |
| 10 | `donor_factor` | Construct own-outcome never-treated factors independently per pair. |
| 11 | `pretreatment_fit` | Fit pooled controls and unit loadings from 2001–2013 only. |
| 12 | `counterfactual_projection` | Simulate post-2013 untreated paths and retain distinct fitted-pre residuals. |
| 13 | `gap_and_att` | Calculate country-year gaps, level windows and incremental contrasts. |
| 14 | `country_dispersion_inference` | Apply country-dispersion Student-t inference. |
| 15 | `baseline_pipeline` | Reconcile baseline calculations and freeze the shared variant input mapping. |
| 16 | `resampling_inference` | Call `execute_inference` with full-design bootstrap/placebo refits and seed stability. |
| 17 | `sweep_factory` | Call `execute_sweeps`; retain baseline and variant identities separately. |
| 18 | `robustness_sweeps` | Estimate registered donor/factor/reference diagnostics with conditioning flags. |
| 19 | `secondary_sweeps` | Execute shortened-window and coalition-boundary checks. |
| 20 | `influence_diagnostics` | Refit donor deletion and recompute treated deletion/distribution diagnostics. |
| 21 | `influence_tables` | Produce influence/distribution tables from live diagnostic results. |
| 22 | `matching_designs` | Run high-income and frequency-weighted M=5 designs with lagged controls. |
| 23 | `matching_report` | Audit/report matching balance and design-specific paths. |
| 24 | `cell_ledger` | Classify raw trade primitives using retry/HS-report evidence. |
| 25 | `exposure_primitives` | Recover eligible gas mirrors, validate totals, complete within each window. |
| 26 | `exposure_ratios` | Recompute component ratios; enforce strict/extended coverage and standardisation. |
| 27 | `heterogeneity_suite` | Run the declared exposure-gradient specifications. |
| 28 | `heterogeneity_regressions` | Produce four-outcome interaction estimates and covariance-correct slope sums. |
| 29 | `direct_arming_pipeline` | Execute the 29-target level-outcome wrapper and its inference contracts. |
| 30 | `production` | Render live figures and verified publication tables with separate geometry scopes. |
| 31 | `reconciliation` | Compare 418 cells, execute invariant probes and verify the self-contained archive. |

The actual `AnalysisPanel.lagged` object retains all 3,225 country-year rows and appends the eight `*_lag1` columns. Those columns are NaN in 2000; the **usable** lagged-control grid has 129 × 24 observations for 2001–2024. The pre-treatment matrices select 2001–2013 explicitly. Do not interpret the presence of a 2000 row as an available lag or fitted counterfactual residual.

The research specification remains:

$$
\widehat f_t=\begin{pmatrix}1\\\overline Y_{\mathcal C_Y,t}\end{pmatrix},\qquad
M_{\widehat F}=I-\widehat F\widehat F^{+},\qquad
\widehat\beta=
\left(\sum_{i\in\mathcal S_Y}\widetilde X_i'\widetilde X_i\right)^{+}
\sum_{i\in\mathcal S_Y}\widetilde X_i'\widetilde Y_i,
\quad \widetilde X_i=M_{\widehat F}X_i,\quad
\widetilde Y_i=M_{\widehat F}Y_i.
$$

Here $X_i$ contains the **one-year-lagged** controls paired with 2001–2013 outcomes. Using the Appendix B computational orientation, $\widehat\Lambda_i=\widehat F^{+}X_i$ is $2\times8$, and $\widehat\gamma_i=\widehat F^{+}(Y_i-X_i\widehat\beta)$ is $2\times1$. For $t\ge2014$:

$$
\widehat X_{i,t-1}(\infty)=\widehat\Lambda_i'\widehat f_t,\qquad
\widehat Y_{it}(\infty)=
\widehat X_{i,t-1}(\infty)'\widehat\beta+
\widehat\gamma_i'\widehat f_t,\qquad
\widehat g_{it}=Y_{it}-\widehat Y_{it}(\infty).
$$

For the preferred reference $R=\{2019,2020,2021\}$ and $W_2=\{2022,2023,2024\}$:

$$
d_i=\frac1{|W_2|}\sum_{t\in W_2}\widehat g_{it}
-\frac1{|R|}\sum_{t\in R}\widehat g_{it},\qquad
\widehat{ATT}^{\mathrm{Inc}}_2=\frac1{N_T}\sum_i d_i,\qquad
\widehat{SE}_{\mathrm{CD}}=\frac{\mathrm{sd}(d_i)}{\sqrt{N_T}},\quad
\mathrm{df}=N_T-1.
$$

The exact percentage interpretation of a log-point estimate is $100[\exp(\widehat\theta)-1]$. Annual rate outcomes are divided by 100 once for ATT estimation; mechanism regressions use percentage-point units once. Exposure ratios are computed from separately completed current-dollar components. None of these transformations occur inside the synthetic raw-data generator. Counterfactual construction excludes target post-2013 realizations; eligibility/completion exclude treatment information; robustness never replaces baseline. Fitted dynamic gaps start in **2001**, normalize at 2013, and cannot acquire a fabricated year-2000 residual.

### **Step 4: A Single Function for the Complete Interface Usage**

The following function fuses configuration loading, synthetic generation or explicit raw-source reuse, raw-contract auditing, external-input assembly, the actual interface invocation, and honest output handling. It uses the five helper definitions above, all in the same notebook namespace. Its defaults belong to this example wrapper; they are not invented defaults of `orchestrate_study_pipeline`.

```python
# Define run_study_usage_example with an explicit input and return contract.
def run_study_usage_example(
    # Specify this typed argument or field of the current expression.
    config_path: str | Path = "config.yaml",
    # Require the following parameters to be supplied by keyword.
    *,
    # Specify this typed argument or field of the current expression.
    seed: int = 20261003,
    # Specify this typed argument or field of the current expression.
    raw_sources: Mapping[str, pd.DataFrame] | None = None,
    # Specify this typed argument or field of the current expression.
    external_inputs: Mapping[str, Any] | None = None,
# Specify this typed argument or field of the current expression.
) -> dict[str, Any]:
    """Execute the verified notebook interface and retain honest run status.

    Parameters
    ----------
    config_path
        Existing YAML 1.2 configuration, read from the current notebook
        folder.
        Default: 'config.yaml'. The file and its study parameters are
        unchanged.
    seed
        Local PCG64 fixture entropy, default 20261003. This controls
        synthetic
        inputs only; the configured inference seed registries remain
        untouched.
    raw_sources
        Optional mapping containing exactly the ten source DataFrames. None
        generates the documented synthetic world. Passing synthetic_sources
        reuses the earlier cells. Authentic raw inputs may be passed
        explicitly.
    external_inputs
        Optional caller data keyed by income_groups, country_geometry,
        world_geometry, table_sources, table_binding_authority,
        manuscript_universe, declared_facts and layer_chains. Further
        supported
        consistency keys are enumerated below. Missing authentic evidence is
        left missing. No computed artifact or trust pin is manufactured
        here.

    Process
    -------
    Load config; generate or validate sources; assemble data.study_config
    and
    data.sources; audit registered evidence states; invoke the
    single-argument
    orchestrate_study_pipeline exactly once. Its runtime binder promotes six
    configured keys; its 31-stage scheduler owns all estimator dependencies.
    Preserve a known configuration-readiness rejection as preflight_blocked.
    Propagate unexpected schema, binding and programming failures.

    Returns
    -------
    dict[str, Any]
        config, sources, input_audit, data, gap_audit, execution_status,
        exception_type, exception_message and pipeline_result. A blocked
        call
        has pipeline_result=None because the scheduler was never entered.
        A returned pipeline result retains its own status, blockers and
        digest.

    Raises
    ------
    TypeError, ValueError, FileNotFoundError
        Malformed example arguments, unsupported external keys or missing
        YAML.
    ConfigValidationError and notebook-specific exceptions
        Unexpected configuration/implementation failures are propagated;
        only
        an explicitly identified evidence-readiness rejection is retained.

    Notes
    -----
    All research callables must already be defined in this notebook
    namespace.
    There are no imports from a Python task folder. Synthetic results cannot
    certify manuscript estimates or a recovered source vintage. This wrapper
    does not reduce B=M=500 or bypass the independent
    418-cell/three-invariant
    and ten-family archive requirements. Implementation author: CS Chirinda.
    """
    # Set config from the expression below.
    config = load_usage_config(config_path)
    # Reject inputs when this condition implies: raw_sources must be a mapping
    # of DataFrames or None.
    if raw_sources is not None and not isinstance(raw_sources, Mapping):
        # Raise the stated exception with the concrete validation failure.
        raise TypeError("raw_sources must be a mapping of DataFrames or None.")
    # Set sources from the expression below.
    sources = (
        # Supply this component of sources.
        generate_study_sources(config, seed=seed)
        # Apply this additional condition to the current selection or
        # validation.
        if raw_sources is None
        # Supply this component of sources.
        else dict(raw_sources)
    # Close the preceding argument, selection or data-structure expression.
    )
    # Validate the ten input frames before invoking the study interface.
    input_audit = audit_usage_sources(sources, config)
    # Assemble the single data mapping accepted by the interface.
    data: dict[str, Any] = {"study_config": config, "sources": sources}
    # Check external_inputs is not None before entering this branch.
    if external_inputs is not None:
        # Reject inputs when this condition implies: external_inputs must be a
        # mapping or None.
        if not isinstance(external_inputs, Mapping):
            # Raise the stated exception with the concrete validation failure.
            raise TypeError("external_inputs must be a mapping or None.")
        # Whitelist external evidence and supported consistency inputs.
        permitted = {
            # Supply this component of permitted.
            "income_groups",
            # Supply this component of permitted.
            "country_geometry",
            # Supply this component of permitted.
            "world_geometry",
            # Supply this component of permitted.
            "table_sources",
            # Supply this component of permitted.
            "table_binding_authority",
            # Supply this component of permitted.
            "manuscript_universe",
            # Supply this component of permitted.
            "declared_facts",
            # Supply this component of permitted.
            "layer_chains",
            # Supply this component of permitted.
            "iso3_registry",
            # Supply this component of permitted.
            "metadata_frame",
            # Supply this component of permitted.
            "treatment_ledger",
            # Supply this component of permitted.
            "comtrade_cell_status_raw",
            # Supply this component of permitted.
            "region_map",
            # Supply this component of permitted.
            "exposure_windows",
            # Supply this component of permitted.
            "archive_artefacts",
            # Supply this component of permitted.
            "seeds",
            # Supply this component of permitted.
            "stability_seeds",
            # Supply this component of permitted.
            "output_dir",
            # Supply this component of permitted.
            "replication_archive_root",
            # Supply this component of permitted.
            "overlay_pair",
            # Supply this component of permitted.
            "archive_version_tag",
        # Close the preceding argument, selection or data-structure expression.
        }
        # Reject external keys that this example does not support.
        extra = set(external_inputs) - permitted
        # Validate extra and report the named failure.
        if extra:
            # Raise the stated exception with the concrete validation failure.
            raise ValueError(
                # Supply the concrete diagnostic message for ValueError.
                f"Unsupported external input key(s): {sorted(extra)}"
            # Close the preceding argument, selection or data-structure
            # expression.
            )
        # Supply or execute this component of data.update.
        data.update(external_inputs)
    # Read the actual notebook's evidence-readiness assessment.
    gap_audit = validate_reproducibility_gaps(config)
    # Identify unresolved evidence without changing any gap state.
    unresolved = tuple(
        # Supply this component of unresolved.
        gap_audit.index[~gap_audit["execution_ready"].astype(bool)]
    # Close the preceding argument, selection or data-structure expression.
    )
    # Freeze the caller configuration for a mutation check.
    config_fingerprint = compute_config_fingerprint(config)
    # Invoke the interface while preserving a known readiness rejection.
    try:
        # Set pipeline_result from the expression below.
        pipeline_result = orchestrate_study_pipeline(data)
    # Handle only the declared configuration exception; inspect its cause.
    except ConfigValidationError as exc:
        # Check not unresolved or 'Reproducibility gaps are not
        # execution-ready' not in str(exc) before entering this branch.
        if (
            # Evaluate this specific condition or chained comparison for the
            # input gate.
            not unresolved
            # Apply this additional condition to the current selection or
            # validation.
            or "Reproducibility gaps are not execution-ready" not in str(exc)
        # Close the preceding argument, selection or data-structure expression.
        ):
            # Raise the stated exception with the concrete validation failure.
            raise
        # Record the real outcome without asserting scientific completion.
        execution_status = "preflight_blocked"
        # Preserve the actual readiness exception's class name.
        exception_type = type(exc).__name__
        # Preserve the exact rejection reason for review.
        exception_message = str(exc)
        # Set pipeline_result from the expression below.
        pipeline_result = None
    # Apply this alternative only when the preceding condition did not hold.
    else:
        # Record the real outcome without asserting scientific completion.
        execution_status = str(pipeline_result["pipeline_status"])
        # Preserve the actual readiness exception's class name.
        exception_type = None
        # Preserve the exact rejection reason for review.
        exception_message = None
    # Reject inputs when this condition implies: The interface changed the
    # caller's configuration.
    if compute_config_fingerprint(config) != config_fingerprint:
        # Raise the stated exception with the concrete validation failure.
        raise RuntimeError("The interface changed the caller's configuration.")
    # Return the declared outputs and their audit evidence.
    return {
        # Record the config field in the current mapping.
        "config": config,
        # Record the sources field in the current mapping.
        "sources": sources,
        # Record the input_audit field in the current mapping.
        "input_audit": input_audit,
        # Record the data field in the current mapping.
        "data": data,
        # Record the gap_audit field in the current mapping.
        "gap_audit": gap_audit,
        # Record the execution_status field in the current mapping.
        "execution_status": execution_status,
        # Record the exception_type field in the current mapping.
        "exception_type": exception_type,
        # Record the exception_message field in the current mapping.
        "exception_message": exception_message,
        # Record the pipeline_result field in the current mapping.
        "pipeline_result": pipeline_result,
    # Close the preceding argument, selection or data-structure expression.
    }
```

Execute the wrapper with the previously generated DataFrames. The interface is called exactly once inside the wrapper. `ConfigValidationError` is retained only when it is the actual unresolved-evidence readiness rejection; unrelated configuration errors propagate.

```python
# Set example from the expression below.
example = run_study_usage_example(raw_sources=synthetic_sources)
# Display the actual computed audit or execution field below.
print("Example execution status:", example["execution_status"])
# Display the actual computed audit or execution field below.
print("Configuration readiness:")
# Display the actual computed audit or execution field below.
print(example["gap_audit"][["state", "execution_ready"]].to_string())
# Check example['pipeline_result'] is None before entering this branch.
if example["pipeline_result"] is None:
    # Display the actual computed audit or execution field below.
    print(example["exception_type"] + ": " + example["exception_message"])
# Apply this alternative only when the preceding condition did not hold.
else:
    # Set study_result from the expression below.
    study_result = example["pipeline_result"]
    # Display the actual computed audit or execution field below.
    print(study_result["pipeline_summary"].to_string(index=False))
    # Display the actual computed audit or execution field below.
    print("Study status:", study_result["pipeline_status"])
    # Display the actual computed audit or execution field below.
    print("Study blockers:", study_result["pipeline_blockers"])
    # Display the actual computed audit or execution field below.
    print("Run digest:", study_result["pipeline_digest"])
```

**Observed result with the present unchanged configuration:**

```text
Example execution status: preflight_blocked
Unresolved evidence: wdi_vintage, WGI_release_year,
                    policy_source_archive_paths
Exception class: ConfigValidationError
Pipeline result: None
Scheduler stages executed by this top-level call: 0
```

The exact exception message displayed by the code includes every unresolved field path. The separate authority registration is also pending; it becomes a granular study blocker after configuration readiness is resolved. Missing external objects, independently authenticated table/invariant/vintage authorities and archive files cannot be converted into a complete result by passing dummy objects. The synthetic policy ledger's `TEST_ONLY` archive strings do not resolve documentary provenance.

### **Summary of the Execution Flow**

1. **Notebook namespace.** Execute the existing draft's research cells first; append the example cells below them. No research function is imported from a `.py` folder.
2. **Raw inputs.** A private PCG64 synthetic model creates all ten named DataFrames, preserving source codes, fixed country labels, calendars, units, zeros versus missing values, and documentary/region distinctions.
3. **Configuration.** Read `config.yaml` safely as YAML 1.2 into `config`; keep its study, seed, runtime, numerical and provenance parameters unchanged.
4. **Input audit.** Verify the actual ingest contract, raw 129-country coverage counts, all primary keys, exact wide/long closure, positive nominal GDP and coherent primitive trade statuses. This is source-interface validation, not empirical reproduction.
5. **Interface call.** Supply `data={'study_config': config, 'sources': synthetic_sources}` through the final wrapper. The actual interface promotes six configured runtime entries and, when ready, owns the ordered 31-stage dependency graph.
6. **Current outcome.** The supplied production configuration raises `ConfigValidationError` on its three unresolved evidence groups before the scheduler runs. The wrapper records `preflight_blocked`, the exact error and `pipeline_result=None`; it does not report successful stages.
7. **Ready computational path.** A genuine run proceeds through raw eligibility/completion, one-year-lagged assembly, pair-specific donor factors, pre-2014 estimation, untouched target counterfactuals, level/incremental effects and country-dispersion inference. Bootstrap/placebo diagnostics retain their configured full-design 500-draw contracts.
8. **Design diagnostics.** Donor/factor/reference, influence, matched and coalition-boundary branches retain baseline isolation. Exposure construction uses independent components and separate windows; gradient regressions remain conditional associations, not mediation.
9. **Reporting and certification.** Supply authentic income/geometry/table/inventory/provenance/checkpoint inputs and separately verified authority registries. Completion additionally requires the 418-cell comparison, three invariant probes and ten-family persisted archive. Synthetic counts do not satisfy those evidentiary requirements.
10. **Interpretation.** Price and output gaps describe macroeconomic divergences under maintained counterfactual conditions. This pipeline supplies evidence relevant to coalition burden; the example does not implement a compensation allocator, procurement optimiser, household-incidence model or sanctions-success verdict.
11. **Verification.** Execute the embedded cells and focused tests, preserve the original notebook/configuration digests, and read the machine evidence for the precise scope of what passed. Treat the documented preflight rejection as the correct current operating outcome.
