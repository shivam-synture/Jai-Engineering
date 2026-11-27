# Product, Component & Technical Specification Master Catalog

> **Document ID:** `DOC-02`  
> **Source Coverage:** 184 Excel workbooks (`KSB DETAILS`, `JSONS PO `, `OVERALL COMMON INWARD REGISTER`, 63 First Piece sheets, 96 PDI workbooks, Hydro/LPT certificates) representing **168+ unique `(Component Category, Pump Size/Model)` combinations**.

---

## 1. Component Categories & Pump Family Taxonomy

### 1.1 Master Component Category Overview (18 Core Categories)
Every machined job-work item belongs to one primary `component_category`:

| # | Canonical Category | Excel Variants / Legacy Aliases | Customer Flow(s) | Typical Process Operations |
| :---: | :--- | :--- | :--- | :--- |
| 1 | **Volute Casing** | `VOLUTE CASING`, `Voulte Casing` | KSB Shirwal | Turning, Facing, Bore (`H7`/`G7`), Flange Drilling, Connection Drilling (`A`/`B`/`D`/`E`/`IV`), WR & Throttle Bush Fitment, Hydro, PDI |
| 2 | **Suction Casing** | `SUCTION CASING`, `SUCTION CASING ` | Shirwal, Chinchwad Direct, JSONS | Bore/Step Turning, Flange Machining (`ASME`/`DIN`), PCD Drilling, Threading (`BSP`/`Metric`), Hydro, LPT (`CL2B`), PDI |
| 3 | **Discharge Casing** | `DISCHARGE CASING`, `DISCHARGE CASING HG3` | Shirwal, JSONS, Maind Flow | Heavy Turning, High-Pressure Bore/Groove (`335H6`, `240H7`), `EXE-06` Drg Overlay (HG3), Flange Serration/RJ, Hydro (up to 163.5 kg/cm²), LPT, PDI |
| 4 | **Stage Casing** | `STAGE CASING`, `STAGE CASING DRILL EXE`, `STAGE CASING TAPER EXE`, `STAGE CASING BLEED OFF`, `STAGE CASING TURN+DRILL`, `STAGE CASING REMACHINING` | Shirwal | Taper Turning (`0.06`–`0.21`), Angled/Offset Drilling (`1` or `2` holes, `Drill Dist 28/30/35`), Bleed-off Execution, Re-machining |
| 5 | **Bearing Housing** | `BEARING HOUSING`, `BEARING HOUSING ` | Shirwal, Chinchwad Direct | Precision Bearing Bores (`H7`/`J6`), Oil-seal Grooves, Circlip Grooves, Grease Nipple Tapping, PDI |
| 6 | **Bearing Bracket** | `BEARING BRACKET`, `BEARING BRACKET 360`, `470`, `620`, `720` | Shirwal | Heavy Bracket Bore, Flange Turning, PCD Holes, Keyway/Slot Machining, PDI |
| 7 | **Bearing End Cover** | `BEARING END COVER`, `BEARING END COVER (DE/NDE)` | Shirwal | Precision OD/ID Turning (`g6`/`H7`), PCD Drilling, Oil-Seal Bore, PDI |
| 8 | **Thrust Bearing Housing** | `THRUST BEARING HOUSING` | Shirwal | Precision Axial Thrust Faces, Multi-Step Bores, Oil Grooves, PDI |
| 9 | **Casing Cover** | `CASING COVER`, `CASING COVER RPHB/RPHD` | Shirwal | Turning, Stuffing Box Bore, WR/Throttle Fitment, Stud PCD Drilling, LPT/Hydro, PDI |
| 10 | **Diffuser Plate** | `DIFFUSER PLATE`, `DIFFUSER PLATE RPHB` | Shirwal | Precision OD/ID (`g6`/`j6`/`H7`), Multi-Step Depth, CSK/Counter-Bore Drills, `M8`/`M10` Tapping, PDI (`JAI/IR/056`) |
| 11 | **Guide Casing** | `GUIDE CASING` | Shirwal | Thickness Facing, Precision Milling Slots (`25±0.1`, `18.0`), Pitch & Hole Drilling, PDI |
| 12 | **Stuffing Box** | `STUFFING BOX`, `STUFFING BOX HOUSING` | Shirwal | High-tolerance Seal Chamber Bore, Gland Stud Drilling/Tapping, PDI |
| 13 | **Pump Foot / Foot** | `FOOT`, `PUMP FOOT` | Shirwal | Flat Surface Milling, Mounting Pad Facing, Slotted/Drilled Bolt Holes, PDI |
| 14 | **Discharge Cover** | `DISCHARGE COVER` | Shirwal | Facing, Seal Groove Turning, Stud Drilling, Hydro/PDI |
| 15 | **Pump Bracket** | `PUMP BRACKET` | Shirwal | Bracket Facing, Concentric Spigot & Flange Drilling, PDI |
| 16 | **Locking Ring / Nut** | `LOCKING RING` | Shirwal | Threading, OD/ID Turning, Spanner Slots |
| 17 | **Adapter / Lantern Ring** | `ADAPTER RING`, `LANTERN` (sub-variants) | Shirwal | Custom Turning & Radial Drilling |
| 18 | **Nuclear / Special Casings** | `KNPMS`, `RHMRI`, `SYT`, `HPH` Components | Shirwal | Special Alloy Machining (`GR 2A`/`CD4MCUN`), Strict RT & LPT Documentation |

---

## 2. Pump Series & Size Matrix

The ERP `pump_series_masters` table must capture the distinct engineering families observed across the 168+ variants:

| Pump Series | Primary Customer Plant | Component Types Manufactured | Observed Size Designations |
| :--- | :--- | :--- | :--- |
| **HDA** | Shirwal, Chinchwad, JSONS | Suction Casing, Discharge Casing, Stage Casing, Bearing Housing | `HDA 40`, `HDA 50`, `HDA 65`, `HDA 80`, `HDA 80 ENL` (Enlarged), `HDA 100`, `HDA 125`, `HDA 150` |
| **HDB** | Shirwal, Chinchwad | Suction Casing, Discharge Casing, Stage Casing, Bearing Housing | `HDB 65`, `HDB 80`, `HDB 100`, `HDB 125`, `HDB 150` |
| **HG / HG1-2 / HG2 / HG3** | Shirwal, Chinchwad | Suction Casing, Discharge Casing, Bearing Housing | `HG 1-2`, `HG 2` (`HG2`), `HG 3` (`HG3 DN80`, `900#`, `1500#`) |
| **WKT / WKTR / WKB / WL** | Shirwal, Maind Flow | Suction Casing, Discharge Casing, Stage Casing | `WKT 50`, `WKT 65`, `WKT 80`, `WKT 100`, `WKT 125`, `WKT 150`, `WKTR 100`, `WKTR 125`, `WKTR 150`, `WKB 65`, `WKB 80`, `WKB 100`, `WKB 125`, `WL 80` |
| **RPH / RPHB / RPHV / RPHD** | Shirwal | Volute Casing, Casing Cover, Diffuser Plate, Bearing Bracket | `50-200`, `50-250`, `50-315`, `80-230`, `80-315/2`, `80-330`, `80-351/2`, `80-360/2`, `100-230`, `100-315`, `150-315`, `150-455`, `200-400`, `250-400`, etc. |
| **CPK** | Shirwal | Volute Casing, Casing Cover, Bearing Bracket | `CPK 50-200`, `65-200`, `80-250`, `100-250`, `150-315` (and standard ISO metric frame sizes) |
| **MC DVMX / CHTR4** | Chinchwad | Bearing Housing | `MC DVMX`, `MC DVMX DE`, `MC DVMX NDE`, `CHTR4` |
| **KNPMS / RHMRI / QG / SYT** | Shirwal | Nuclear / API / Process Casings & Brackets | Frame-specific drawings (`IP...`, `UG...`, `B0CP...`, `P...`) |
| **Bearing Bracket Frame Sizes** | Shirwal | Bearing Bracket, Bearing End Cover, Thrust Bearing Housing | `360`, `470`, `620`, `720` (plus `DE`/`NDE` pairs) |

---

## 3. Material Grades & Quality Classification

### 3.1 Supported Material Grades
All materials observed in `KSB DETAILS`, `JSONS PO `, Hydro, LPT, and PDI reports:

| Material Code | Alloy Family | Quality Class | Hydro / LPT Behavior |
| :--- | :--- | :--- | :--- |
| **`FG260`** | Grey Cast Iron (IS 210 FG260) | `CL3` (or blank) | Lower Hydro Pressure (`25.5 kg/cm²`); rarely LPT unless specified; non-weld-repaired |
| **`WCB`** | Carbon Steel Cast (ASTM A216 Gr. WCB) | `CL2B` | Medium Hydro (`36` / `117.3` / `132.5 kg/cm²`); Mandatory LPT on `CL2B`; Weld-repairable |
| **`LCB`** | Low-Temp Carbon Steel (ASTM A352 Gr. LCB) | `CL2B` | Treated on par with `WCB` pressure matrix (`36` / `117.3` / `132.5 kg/cm²`); Mandatory LPT |
| **`LC3`** | Nickel Alloy Steel (ASTM A352 Gr. LC3) | `CL2B` | Cryogenic/low-temp alloy steel; Mandatory LPT & RT traceability |
| **`CA6NM`** | Martensitic Stainless Steel (ASTM A743 / A487) | `CL2B` | High Hydro Pressure (`46` / `163.5 kg/cm²`); Mandatory LPT; Strict PWHT / repair-weld control |
| **`F6NM`** | Forged / Rolled Martensitic SS | `CL2B` | High-pressure trim / covers |
| **`CF8M` / `CF3M` / `CF3`** | Austenitic Stainless Steels (316 / 316L / 304L Cast) | `CL2B` | Process pumps (`RPHB`, `WKB`, Diffuser Plates); Chloride/dye check sensitive |
| **`GR 1B` / `GR 2A` / `GR 5A` / `GR 6A`** | Duplex / Super Duplex Stainless Steels (ASTM A890 / A995) | `CL2B` | High-alloy (`RPHD 150-455 GR6A`, `115 kg/cm²` for `GR2A` HDA 65 Discharge); zero wrong-alloy tolerance |
| **`CD4MCUN` / `UNS`** | Precipitation Hardened / Duplex SS (`CD4MCuN`, `UNS S32750`) | `CL2B` | Special chemical/nuclear pumps |
| **`CW6MC`** | Nickel-Base Alloy (Inconel 625 Cast) | `CL2B` | Severe service special castings |

---

## 4. Flange Machining Standards & Surface Finish

Observed flange codes on KSB & JSONS POs:

| Standard Family | Observed Flange Values | Key Inspection Dimensions |
| :--- | :--- | :--- |
| **ASME B16.5 Class 150 RF** | `ASME B16.5 CLASS 150 RF`, `150#` | Suction Casings (`HDA 50/65/80/100`), Raised Face OD, Height (`1.6–2.0 mm`), Flange OD, Thickness, PCD, Bolt Holes |
| **ASME B16.5 Class 300 RF** | `ASME B16.5 CLASS 300 RF` | `Volute Casing` (`RPH 100-230`), Medium-Pressure Discharge |
| **ASME B16.5 Class 600 RF** | `ASME B16.5 CLASS 600 RF`, `600#` | Discharge Casings (`HDA 50/65/80`, `WCB`/`CA6NM`) |
| **ASME B16.5 Class 900 RF** | `ASME B16.5 CLASS 900 RF`, `900#` | High-Pressure `HG2`, `HG3 DN80` (`IP14899-00`), `HDA 65/80` (`CA6NM`), Race Face OD `138.0` / Height `6.35 mm` |
| **ASME B16.5 Class 1500 RF / RJ** | `ASME B16.5 CLASS 1500 RF`, `ASME B16.5 CLASS 1500 RJ` | `HG3` (`IP22396-06`, `IP16179-06`), 12 holes × `Ø36.0`, Ring Joint (`RJ`) groove vs Raised Face (`RF`) |
| **DIN / EN Flange Standards** | `DIN 2533 PN 16`, `PN 25`, `PN 40`, `EN 1092-1` | KSB Chinchwad & European-spec `HDA` / `RPH` Suction Casings |
| **Hold / Deferred Spec** | `WILL BE INFORM LATER`, `1 NOS DIN2533 PN16, REMAINING TO CONFIRM` | Triggers `FLANGE_HOLD` state at piece level until customer amendment/email confirmation |

---

## 5. Execution Codes, Options & Machining Parameters

The ERP models **Component Execution Attributes** cleanly by category rather than polluting generic columns:

### 5.1 Stage Casing Execution Parameters (`JAI/DE/004–007`, `JAI/TE/001–004`)
- **Execution Variants**:
  1. `DRILL_EXE`: Execution drilling on stage casing rim/face. Captures `NO OF HOLES` (`1` or `2`), `DRILL DIST` (`28`, `30`, `35` mm), `DRILL DIA`, and execution drawing (e.g., `P123346D PAGE2`).
  2. `TAPER_EXE`: Taper outer/step execution (`TAPER SIZE`: observed `0.06` up to `0.21`) + execution drawing (e.g., `IP16409-34D`, `IP16409-36D`, `IP16409-37D`, `IP16410-31D`, `IP16410-34D`).
  3. `BLEED_OFF`: Bleed-off port execution.
  4. `TURN_AND_DRILL`: Combined final turning + execution drilling.
  5. `REMACHINING`: Re-work / salvage step execution (`JAI/TE/*`).

### 5.2 Volute Casing Parameters (`JAI/IR/*` + `OVERALL COMMON INWARD REGISTER`)
- **Connection Drilling Code**: Port drilling/tapping pattern on volute casing bosses (`A`, `A+B`, `A+D`, `A+E`, `A+B+D`, `A+B+E`, `A+B+C+D`, `IV`, `NIL`).
- **Wear Ring (WR) Fitment**: `wr_required` (`Boolean`), `wr_size` (e.g. `255.1 / 255.2`, `115`, `248.3`), `wr_in_code`, `wr_available_qty`.
- **Throttle Bush Fitment**: `throttle_required` (`Boolean`), `throttle_in_code`, `throttle_available` (`YES`/`NO`).

### 5.3 HG2 / HG3 Execution Parameters
- `no_of_holes_hg`, `bore_dia_hg`, and secondary `execution_drawing_no` (e.g., `EXE-06 DRG NO P089185C` on `Discharge Casing HG3` with `ID 335H6`, `240H7`, `120H7`, `30 G7`).

---

## 6. Hydrostatic Test Pressure Matrix (`JAI/HT/001`)

Verified directly from `HYDRO TEST REPORT 2026-27.xlsx` (`QN 10439 & IT 02176 C REV. 4`, 30-minute hold):

| Component Type | Pump Series & Size | Material Grade | Required Test Pressure (`kg/cm²`) | Validated Gauge ID & Range |
| :--- | :--- | :--- | :---: | :--- |
| **Suction Casing** | `HDA 50 / 65 / 80 / 100` | **`FG260`** | **`25.5`** | `JAI/PRE/08` (`0–100 kg/cm²`) |
| **Suction Casing** | `HDA 50 / 65 / 80 / 100` | **`WCB` / `LCB`** | **`36.0`** | `JAI/PRE/08` (`0–100 kg/cm²`) |
| **Suction Casing** | `HDA 50 / 65 / 80 / 100` | **`CA6NM`** | **`46.0`** | `JAI/PRE/08` (`0–100 kg/cm²`) |
| **Suction Casing** | `HDA` Series | **`GR 2A`** | **`10.0`** | `JAI/PRE/08` (`0–100 kg/cm²`) |
| **Discharge Casing** | `HDA 50` | **`WCB` / `LCB`** | **`117.3`** | `JAI/PRE/12` (`0–420 kg/cm²`) |
| **Discharge Casing** | `HDA 65` | **`GR 2A`** | **`115.0`** | `JAI/PRE/12` (`0–420 kg/cm²`) |
| **Discharge Casing** | `HDA 65 / 80 / 100` | **`WCB` / `LCB`** | **`132.5`** | `JAI/PRE/12` (`0–420 kg/cm²`) |
| **Discharge Casing** | `HDA 50 / 65 / 80 / 100` | **`CA6NM`** | **`163.5`** | `JAI/PRE/12` (`0–420 kg/cm²`) |

> **System Rule:** The ERP stores `(component_category, pump_series, size_code, material_grade) -> test_pressure_kgcm2` as a lookup rule so the QA engineer gets an **auto-populated, tamper-evident Test Pressure** and automatic selection of a `0–100` vs `0–420` calibrated pressure gauge.

---

## 7. Drawing, Execution Drawing & Item Traceability Rules

1. **Base Drawing + Revision**: e.g., `P31487-00 REV-07`, `UG1504769-00 REV-08`, `IP22396-06 REV-00`, `B0CP10288-00 REV-05`.
   - When a customer issues a new revision (`REV-08` -> `REV-09`), the ERP versions the inspection template while preserving historical templates for past POs.
2. **Traceability Identifiers per Piece / Lot**:
   - **Customer Finished Code (`IN Code`)**: Starts with `IN...` (e.g. `IN072138`, `IN180899`) or `O5...` / `05...` (e.g. `O5438632`, `O5626668`).
   - **Raw Casting Code (`Raw Code`)**: Identifies rough casting blank received from KSB/Foundry.
   - **Foundry Pattern Code (`P.C.`)**: Printed on Delivery Challans for Foundry customers (e.g. `P.C. - L00082`, `R33821-00`).
   - **Heat Code / Heat No**: Mandatory per-piece melt identifier (`E 13972`, `F 11646`, `R5662`, `O1D25`, `H16129`).
   - **KSB Order / Batch Ref (`997...`)**: Optional KSB routing reference observed on `HG3` PDI (`9975944825/100`).
   - **Jai Serial No (`SR. NO. JAI/001`, `Sr.No.001`)**: Sequential internal/certificate piece numbering punched or tagged on each finished component.
