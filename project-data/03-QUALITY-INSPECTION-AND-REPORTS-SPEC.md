# Quality Inspection, NDT, Calibration & Reporting Specifications

> **Document ID:** `DOC-03`  
> **Scope:** Exact field specifications, numbering schemes, calculation formulas, format codes (`JAI/*`), tolerance rules, and print layouts for all ERP documents and registers.

---

## 1. Master Registry of Client Format Codes (`JAI/*`)

All controlled documents at Jai Engineering bear an ISO-style Format Number in the top-right header:

| Format Code Family | Document Title | Issued When | Certificate / Report Numbering Pattern |
| :--- | :--- | :--- | :--- |
| **`JAI/IR/001` – `JAI/IR/060`** | Pre-Delivery Inspection (PDI) Report | Before dispatch of finished batch | `YYYY-YY/NNN` (e.g. `2026-27/293`) |
| **`JAI/DE/004` – `JAI/DE/007`** | PDI Report – Stage Casing Drill Execution | Before dispatch of Drill Exe batches | `YYYY-YY/NNN` |
| **`JAI/TE/001` – `JAI/TE/004`** | PDI Report – Stage Casing Taper Exe / Remachining | Before dispatch of Taper Exe batches | `YYYY-YY/NNN` |
| **`JAI/INPIR/001` – `JAI/INPIR/050`** | First Piece Inspection Report (In-Process) | On setup of 1st piece for any new batch/drg | `JAI/INPIR/NNN` (incl. suffix e.g. `028A`) |
| **`JAI/HT/001`** | Hydrostatic Test Certificate | Pressure testing of Suction / Discharge / Volute Casings | `JAI/HYD/YYYY-YY/NNN` (e.g. `JAI/HYD/2026-27/001`) |
| **`JAI/DP/001`** | Liquid Penetrant (Dye Penetrant) Test Certificate | NDT surface inspection (`CL2B` / specified items / post-weld) | `JAI/LPT/YYYY-YY/NNN` (e.g. `JAI/LPT/2026-27/394`) |
| **`JAI/CAL/001`** | Instrument & Gauge Calibration Record | Master register (annual/semi-annual NABL sync) | Internal ID `JAI/<TYPE>/NN` + NABL `ULR` |
| **`JAI/ST/008`** | KSB Direct PO & Stock Balance Register (`KSB DETAILS`) | Live operational & financial ledger | Continuous FY ledger |
| **`JAI/ST/008A`** | Foundry-Mediated (`JSONS PO `) Dual-Balance Ledger | Live 3-party PO & receipt ledger | Continuous FY ledger |
| **Delivery Challan (Unnumbered Format)** | Commercial Job-Work / Returnable Delivery Challan | At physical dispatch gate | `YY/NNN` (`26/315`) or `NB-YY/NNN` (Non-Billable) |

---

## 2. First Piece Inspection Report (`JAI/INPIR/*`)

### 2.1 Purpose & Master Registry (`INPROCESS INSPECTION REPORT NUMBERING.xlsx`)
Tracks 51+ controlled master first-piece inspection templates (`JAI/INPIR/001` to `JAI/INPIR/050`, including variants like `028A`). Every time a CNC/VMC machine sets up for a Drawing + Material + Execution variant, the **1st machined piece** must be measured and signed off before batch production continues.

### 2.2 Header Fields
- **Header Block**: Company Name (`M/s JAI ENGINEERING AND REPAIRING WORKS`), Document Title (`1ST PIECE INSPECTION REPORT`), Format Code (`JAI/INPIR/NNN`).
- **Metadata**: `Customer Name`, `Item Description`, `Drawing No & Rev`, `Execution Drawing No` (if applicable e.g. `EXE-06 DRG NO P089185C`), `Total Qty`, `Material Grade`, `Operation Start Date`.

### 2.3 Observation Table Columns
| Column | Data Type & Rule |
| :--- | :--- |
| `Sr. No` | Auto sequence `1..N` |
| `Parameter Description & Spec` | Text + parsed nominal & limits (e.g., `OD 405g6 (-0.02/-0.06)`, `ID 335H6 (0/+0.036)`, `Hole 10H7`, `Tapping 1/2" BSP`, `Flange Machining ASME B 16.5 CL. 900 RF`, `Lug Width 40.0 (-0.1)`) |
| `Inst.` (Instrument Type) | Canonical code: `VC` (Vernier Caliper), `OM` (Outside Micrometer), `BG` (Bore Gauge), `DV` (Depth Vernier), `IM` (Inside Micrometer), `MM` (Micrometer), `HG` (Height Gauge), `GAUGE` (Plug/Thread/Snap Gauge), `VISUAL` |
| `Observation` | Actual measured numeric value OR `OK` / `NOT OK` (for `GAUGE`/`VISUAL`) |
| `Machine No` | CNC / VMC asset ID (e.g., `CNC-01`, `VMC-02`) |
| `Operator` | Logged operator name / employee ID |

---

## 3. Pre-Delivery Inspection (PDI) Report (`JAI/IR/*`, `JAI/DE/*`, `JAI/TE/*`)

### 3.1 Format Code Sub-Series Mapping
- **`JAI/IR/001..060`**: Standard component PDI formats (e.g. `JAI/IR/041` Discharge Casing `HG3`, `JAI/IR/056` Diffuser Plate `RPHB`).
- **`JAI/DE/004..007`**: Stage Casing Drill Execution PDI formats.
- **`JAI/TE/001..004`**: Stage Casing Taper Execution & Re-machining PDI formats.

### 3.2 Dynamic Multi-Piece Matrix Structure
Unlike single-row tables, Jai Engineering's PDI reports use a **dynamic horizontal multi-piece grid** where **one column represents one physical casting**:
- **Top Metadata**: `Inspection Report No` (`YYYY-YY/NNN`), `Challan No` (`YY/NNN`), `Date`, `Customer Name` (`K.S.B. PUMPS LTD` or `JSONS FOUNDRY PVT LTD`), `Item Description`, `Drawing No & Rev`, `Total Qty` (`NN Nos.`), `Material Grade`.
- **Per-Piece Traceability Sub-Header (4 Metadata Rows across Columns `1..Qty`)**:
  1. `IN Code / Material Code` (e.g., `O5548890`, `IN107965`)
  2. `Heat Code` (e.g., `I1694`, `R5927`, `R5926`, `R6266`)
  3. `PO No / Line / Seq` (e.g., `4507887303/20/1`, `/2`, `/3`) — plus optional KSB reservation ref (`9975944825/100`)
  4. `Serial No` (`Sr.No.001`, `Sr.No.002`, ...)
- **Body Rows (`Parameter × Piece`)**:
  - Each parameter row displays `Description + Nominal + Tolerance`, `Inst.`, and `Piece 1..K Observations`.
  - **Auto-Validation Logic in ERP**: When `Tolerance` is numeric (`405g6 (-0.02/-0.06)` -> `[404.94, 404.98]`), entering `404.96` highlights **GREEN (PASS)**; entering out-of-tolerance highlights **RED (FAIL)** and requires deviation/rework disposition before approval!
- **Footer Block**:
  - `Visual Inspection`: `OK` per piece or Defect tag (e.g., `BH`)
  - `Deviation / Defect Remarks`: Explicit per-serial text when applicable (observed on `DIFFUSER PLATE RPHB 80-360/2`: `"Sr. NO. 007 - Blow hole at face"`, `"Sr. NO. 008 - Blow hole at face, 380 ID"`).
  - `Inspected By` signature block.

---

## 4. Hydrostatic Test Certificate (`JAI/HT/001`)

### 4.1 Fields & Rules (`HYDRO TEST REPORT 2026-27.xlsx`)
- **Certificate No**: `JAI/HYD/YYYY-YY/NNN` (e.g., `JAI/HYD/2025-26/209`, `JAI/HYD/2026-27/001`).
- **Standard Header**: Client Name & Plant Address (`M/s K.S.B. LIMITED, ENERGY PROJECTS DIVISION, SHIRWAL` or `M/s JSONS FOUNDRY PVT LTD, KUPWAD BLOCK, SANGLI`).
- **PO & Challan Cross-References (1-to-Many!)**:
  - `P.O. Sequence No`: Supports multiple KSB POs or multiple JSONS Challans (e.g., `25/19794/6`, `25/22352/22`, `25/23452/12,13`).
  - `KSB PO NO`: Shown explicitly when Client is a Foundry (`4507807362/20`, `4507754627/60`, `4507865575/50`).
  - `P.O. Qty`, `Our Ch. No.` (e.g. `25/972`), `Date of Test`.
- **Fixed Procedure Header**: `Hydrostatic Test Plan : QN 10439 & IT 02176 C REV. 4`, Ambient Water, Holding Time `30 Minutes`, Result `No Leakage / No Pressure Drop / Satisfactory`.
- **Line Items**: `Description`, `Drawing No & Rev`, `Material`, `Item Code`, `Serial Nos` (`SR. NO. JAI/094 TO 097`), `Heat Code(s)` (per-piece/lot), `Qty`, `Test Pressure in KG/CM²` (auto-derived from Section 6 of `DOC-02`).
- **Calibrated Pressure Gauge Block (Auto-Fetched & Locked!)**:
  - `Pressure Gauge Range` (`0 - 100 KG / CM²` or `0 - 420 KG / CM²`)
  - `Pressure Gauge Identification No` (`JAI/PRE/08`, `JAI/PRE/12`, etc. — **REJECTED/EXPIRED gauges hard-blocked**)
  - `Calibration Date` & `Calibration Due Date` (auto-filled from `JAI/CAL/001`).

---

## 5. Liquid Penetrant (LPT / DP) Test Certificate (`JAI/DP/001`)

### 5.1 Fields & Rules (`LPT TEST REPORT 2026-27.xlsx`)
- **Certificate No**: `JAI/LPT/YYYY-YY/NNN` (e.g., `JAI/LPT/2026-27/394`).
- **Standard Header**: Supports multi-line Foundry PO/Challan + multi-line `KSB PO NO` + `Our Ch. No.` + `Date` + `Total Qty`.
- **Fixed & Consumable Parameters**:
  - **Procedure**: `QN 1110005` (`Visible Solvent Removable`)
  - **Consumables (3 Rows: Cleaner, Penetrant, Developer)**: `Make` (`P-Met Flawcheck`), `Batch No` (e.g. `OX-05-53643`), `Mfg. Date`, `Expiry Date` (**ERP blocks certificate issuance if `Test Date > Consumable Expiry Date`!**)
  - **Process Conditions**: Penetration Time (`10 - 15 Minutes`), Bleed Out Time (`10 Minutes Minimum`), Surface Temperature (`Ambient 30°C`), Acceptance Standard (`SS`).
  - **Certifier Authority**: NDT Level II Engineer / Certification Ref (`SVTECPL/NDT/01115`).
- **Per-Piece Result Table**:
  - Lists each Serial No (`JAI/...`) + Heat No + Examined Area (`Machined Surface / Flange / Bore / Weld Zone`) + Result (`OK` or defect code: `RI` Rounded Indication e.g. `RI 5-8MM / 3 NOS FACE`, `LI` Linear Indication e.g. `LI 1-50 MM`, `REJECTED`).
  - Any non-`OK` piece automatically creates/links a Rework or Rejection record and **blocks dispatch of that failed piece**.

---

## 6. Instrument & Gauge Calibration Register (`JAI/CAL/001`)

Verified from `CALIBRATION RECORD FOR INSPECTION INSTRUMENT.xlsx` (46 total instruments across 11 instrument classes):

| Prefix Code | Instrument Category | Count | Active Range Examples | Special Status Handling |
| :--- | :--- | :---: | :--- | :--- |
| **`JAI/OM/*`** | Outside Micrometer | 7 | `0–25`, `25–50`, `50–75`, `75–100`, `100–125`, `125–150`, `150–175` mm | Active |
| **`JAI/BG/*`** | Bore Gauge | 4 | `18–35`, `35–60`, `50–160`, `160–450` mm | Active |
| **`JAI/D/*`** | Dial Indicator / Plunger Dial | 11 | `0.01` / `0.001` mm L.C. | Active |
| **`JAI/VC/*`** | Vernier Caliper | 7 | `0–150`, `0–200`, `0–300`, `0–600`, `0–1000` mm | Active |
| **`JAI/DV/*`** | Depth Vernier / Micrometer | 2 | `0–200`, `0–300` mm | Active |
| **`JAI/HG/*`** | Height Gauge | 1 | `0–300` mm | Active |
| **`JAI/DEVP/*`** | Depth / Plug / Bore Special | 4 | — | Active |
| **`JAI/PRE/*`** | Hydraulic Pressure Gauge | 8 | `0–100`, `0–420 kg/cm²` | **`JAI/PRE/03` & `JAI/PRE/06` = `REJECT`** |
| **`JAI/WG/*`** | Weld Gauge / Fillet Gauge | 1 | — | Active |
| **`JAI/SP/*`** | Surface Plate / Straight Edge | 1 | — | Active |
| **`JAI/IM/*`** | Inside Micrometer | 2 | Extension set | Active |

**Tracked Fields per Instrument**: `Sr. No`, `Instrument Description`, `Make`, `Range`, `Least Count (L.C.)`, `Identification No` (`JAI/...`), `Calibration Date`, `Calibration Due Date`, `Certificate / ULR No`, `Calibration Agency`, `Status` (`ACTIVE` | `DUE_SOON` | `EXPIRED` | `REJECT` | `UNDER_REPAIR`).

---

## 7. Delivery Challan (`YY/NNN` & `NB-YY/NNN`)

### 7.1 Verified Print Layout (`DELIVERY CHALLAN JAI ENGG.xlsx`)
- **Supplier Letterhead**: `JAI ENGINEERING & REPAIRING WORKS`, `Gat No 505, A/P -Palashi, Tal. Khandala, Dist - Satara - 412801`, `Email`, `Mob`, **`GST - 27AAPFJ7138K1ZQ`**.
- **Document Meta**:
  - `CH. NO.` (`26/315`, or `NB-26/NNN` for Non-Billable Return) & `Date` (`DD-MM-YYYY`)
  - `TO: M/s <Customer / Foundry Legal Name & Address>` (e.g., `JSONS FOUNDRY PVT LTD, G12/1, G13, G2/B, Kupwad Block, Sangli 416436`)
  - `P.O. NO.` & `P.O. Date` (For Foundry: lists Foundry Challan/PO Batch `26/6727/8 TO 11` Date `10-07-2026`; for Direct KSB: KSB PO No & Date)
- **Line Item Body**:
  - `Sr. No.`
  - `Description`: Component Name + Size + `(Material)`, Drawing No + Rev (`P31487-00 REV-07`), `(IN Code) Raw/Item Code` (`(IN072138) R33821-00`), `P.C.` (`L00082`)
  - `KSB PO` (multi-line support: e.g. `4508008257/70,80,90` + `4508036075/10`)
  - `Foundry PO` (when applicable: `Jsons PO - 26/6727/8 TO 11`)
  - **Per-Heat Quantity Breakdown**: e.g. `Heat No - E 13972 - 1 NOS`, `E 13974 - 1 NOS`, `F 11646 - 2 NOS`, `F 11767 - 1 NOS`
  - `Qty.` (`5 NOS`) & `Total Qty` (`5 NOS`)
- **Footer**: `Receiver's Signature`, `For Jai Engineering & Repairing Works — Authorised Signature`.

---

## 8. Management, Stock & Financial Registers

### 8.1 KSB Direct Ledger (`JAI/ST/008` — `KSB DETAILS`)
Computed formulas replicated identically in ERP SQL views/backend:
- $\text{BAL QTY} = \text{QTY RECEIVED} - \text{QTY DISPATCHED} - \text{QTY RETURNED (NB)}$
- $\text{AMOUNT RECEIVED} = \text{QTY RECEIVED} \times \text{RATE}$
- $\text{AMOUNT DISPATCHED} = \text{QTY DISPATCHED} \times \text{RATE}$
- $\text{BALANCE AMOUNT (WIP Value)} = \text{BAL QTY} \times \text{RATE}$
- **Dashboard Totals**: Filtered `SUM` aggregations matching Excel's `SUBTOTAL(9, ...)` so filtering by `SHIRWAL`, `CHINCHWAD`, `Month`, or `Material` immediately updates WIP Qty & WIP ₹ Value.

### 8.2 Foundry Dual-Balance Ledger (`JAI/ST/008A` — `JSONS PO `)
- $\text{QTY RECEIVED FROM FOUNDRY} = \sum_{i=1}^{N} \text{Batch}_i.\text{qty}$
- $\text{BAL QTY AT JAI} = \text{QTY RECEIVED FROM FOUNDRY} - \text{QTY DISPATCHED} - \text{QTY RETURNED}$
- $\text{QTY BAL FROM FOUNDRY} = \text{KSB LINE ITEM QTY} - \text{QTY RECEIVED FROM FOUNDRY}$
- $\text{FY DISPATCHED QTY} = \text{Cumulative Dispatched} - \text{Prior FY Dispatched Carryover}$
- $\text{COMBINED WIP VALUATION} = \text{WIP Value (KSB Direct)} + \text{WIP Value (Foundry)}$

### 8.3 Yearly Rework Analytics (`YEARLY REWORK PERCENTAGE`)
- $\text{Rework \%} = \frac{\text{Distinct Reworked Pieces in Period}}{\text{Total Supplied / Dispatched Pieces in Period}} \times 100$
- Breakdowns by: Financial Year, Month, Pump Size (`HDA 50/65/80/100`), Material (`WCB` vs `CA6NM`), Defect Category (`BH`, `Crack`, `Hydro Leak`, `Less Material`), and Repair Cycle (`1st Repair` vs `Twice Repaired`).

### 8.4 Monthly Delivery Schedule Commitment Matrix (`Sheet1` — `MONTH WISE BALANCE QTY`)
- Pivot view across `Rows = (Component Type, Size, Material, Flange Rating)` × `Columns = Commitment Months (Apr..Mar)` showing exact pending castings due to KSB Chinchwad/Shirwal, highlighting overdue months in red.
