# UI/UX Screen Specifications & Implementation Roadmap

> **Document ID:** `DOC-06`  
> **UI Framework:** Next.js 16 App Router · React 19 · Tailwind CSS v4 · Responsive Desktop + Shop-Floor Tablet UX

---

## 1. Navigation Architecture & Screen Map

Sidebar grouped cleanly by shop-floor operational roles:

1. **Executive Overview** (`/dashboard`)
2. **Orders & Inward**
   - Customer PO & Direct Ledger (`/orders/direct` — KSB Shirwal & Chinchwad `JAI/ST/008`)
   - Foundry 3-Party Dual-Balance Ledger (`/orders/foundry` — JSONS / Maind Flow `JAI/ST/008A`)
   - Common Inward & Piece Registry (`/inward`)
   - Monthly Dispatch Plan Matrix (`/planning/monthly-matrix` — `Sheet1`)
3. **Production & Execution**
   - CNC / VMC Job Card Board (`/production/board` — Priority `P1`/`P2`/`P3`/`URGENT`)
   - Kit & Hold Queues (`/production/holds` — `Flange Hold`, `WR / Throttle Pending`)
   - Rework & Repair-Weld Console (`/rework` — `JSONS REWORK DETAILS` + `Yearly Rework %`)
4. **Quality, Testing & Calibration**
   - First Piece Inspection (`/qa/first-piece` — `JAI/INPIR/*`)
   - PDI Report Builder (`/qa/pdi` — `JAI/IR/*`, `DE/*`, `TE/*`)
   - Hydrostatic Test Bench (`/qa/hydro` — `JAI/HT/001`)
   - LPT / Dye Penetrant Lab (`/qa/lpt` — `JAI/DP/001`)
   - Instrument & Gauge Calibration (`/qa/calibration` — `JAI/CAL/001`)
5. **Dispatch & Returns**
   - Delivery Challan Generator (`/dispatch/challans`)
   - Non-Billable / Returnable Gate Pass (`/dispatch/returns` — `NB-YY/NNN`)
6. **Masters & Migration**
   - Component Catalog, Execution Specs & Inspection Templates (`/masters/*`)
   - Legacy Excel Import Wizard (`/admin/import-excel`)

---

## 2. Detailed Screen Specifications

### 2.1 Executive Dashboard (`/dashboard`)
- **Top KPI Strip** (Live SQL metrics matching `KSB LTD 2026-2027.xlsx` summaries):
  - **Combined WIP Valuation (₹)** (`KSB Direct WIP ₹` + `Foundry WIP ₹` — mirrors `=W3+'KSB DETAILS'!T1`)
  - **Physical Castings in Plant** (`KSB Direct Bal Qty` + `Foundry Bal Qty at Jai`)
  - **Pending Foundry Receipts** (`QTY BAL FROM JSONS / Foundries`)
  - **Monthly Rework Rate %** (Current month & Rolling FY vs historical 17.65%–38.91% baseline)
- **Actionable Exception Panels**:
  - Overdue / Due-This-Week KSB Dispatch Lines
  - Castings Blocked on `FLANGE_HOLD` or `WR/THROTTLE` > 3 Days
  - Gauges Expiring in ≤ 15 Days & Consumables Expiring Soon
  - High-Risk **`TWICE REPAIRING`** Castings Awaiting QA Sign-Off

### 2.2 Foundry 3-Party Dual-Balance Workbench (`/orders/foundry`)
- **Master View**: Grouped by `(KSB PO No, Line Item No, Component, Size, Material, Flange)` showing:
  - `KSB Ordered Qty`, `Received from Foundry` (clickable badge expanding unlimited partial challan batches), `Dispatched Qty`, **`Bal Qty at Jai`**, **`Bal Qty from Foundry`**, `WIP ₹`, `KSB Required Month`.
- **Action Drawer (`+ Log Foundry Challan Batch`)**:
  - Enter Foundry Challan No (e.g., `26/6727/8 TO 11`), Date, Batch Qty, and generate serialized pieces (`Heat No`, `P.C.`, `RT No`, per-piece Flange check).
  - Auto-updates both balances immediately on save.

### 2.3 Common Inward & Piece Genealogy Drawer (`/inward`)
- Unified table across Direct & Foundry flows with quick-filter chips:
  - Flow (`SHIRWAL`, `CHINCHWAD`, `JSONS`, `MAIND_FLOW`)
  - Component Special Filters (`Volute – WR Pending`, `Stage – Taper/Drill Exe`, `HG2/HG3`, `Flange Hold`, `Twice Repaired`)
- Clicking any row opens the **Piece Genealogy Timeline**: shows every timestamped event from `Challan Receipt -> Heat/RT -> Kit Fitment -> Machine Ops -> Rework/Weld Cycles -> LPT Cert -> Hydro Cert -> PDI Report -> Delivery Challan`.

### 2.4 Digital Quality Inspection Matrix (`/qa/pdi` & `/qa/first-piece`)
- **Ergonomic Grid Entry**:
  - Select Customer, Component & Drawing Rev -> System loads the controlled `JAI/IR/*` or `JAI/INPIR/*` parameter rows automatically.
  - Add `[1..N]` ready pieces by serial/heat (`Sr.No.001..N`).
  - **Keyboard-Fast Columnar Entry** (Tab / Enter moves smoothly down each piece column):
    - Auto-parses tolerances (`405g6 (-0.02/-0.06)` -> `[404.94, 404.98]`): turns cell **Emerald Green** on pass or **Crimson Red** on out-of-tolerance.
    - Quick buttons `OK` / `NOT OK` / `BH` for `GAUGE` and `VISUAL` rows.
    - Displays active calibrated instrument dropdown per parameter (`JAI/VC/*`, `JAI/OM/*`, `JAI/BG/*`, `JAI/IM/*`).
  - **One-Click Print / Download A4 PDF** reproducing exact `JAI/IR/*` or `JAI/INPIR/*` layout.

### 2.5 Hydro Test Bench (`/qa/hydro`) & LPT Lab (`/qa/lpt`)
- **Hydro Test Screen**:
  - Select Client & Component Lot (supports bundling multiple Foundry Challans / KSB POs of identical size/material/pressure).
  - **Auto-Filled Read-Only Standard**: `Test Pressure` (e.g. `132.5 kg/cm²`), `Test Plan: QN 10439 & IT 02176 C REV. 4`, `Holding Time: 30 Mins`.
  - **Calibrated Gauge Picker**: Lists **only `ACTIVE`, non-expired** `JAI/PRE/*` gauges whose max range exceeds test pressure (`JAI/PRE/03` & `06` `REJECT` cannot be selected). Auto-stamps Calibration Date & Due Date onto the certificate.
- **LPT Test Screen**:
  - Auto-loads active non-expired `P-Met Flawcheck` Cleaner / Penetrant / Developer batches (`Batch No`, `Mfg Date`, `Expiry Date`).
  - Per-piece table supports `OK` or structured indication notation (`RI 5-8MM`, `LI 1-50 MM`, `LEAKAGE`). Selecting any fail/indication auto-prompts to create a `Rework` or `NB Return` case.

### 2.6 Rework & Repair-Weld Console (`/rework`)
- Tracks open defects sorted by aging (`Days Waiting for Weld` / `Days in Re-machining`).
- Filter toggles: `JSONS Foundry Rework`, `In-House (IH) Weld`, **`Twice Repaired Flag`**.
- Embedded **Rework % Analytics Tab**: Charts `Supply Qty vs Rework Qty vs Rework %` monthly & yearly across `WCB` vs `CA6NM` and `HDA 50/65/80/100` to give Jai Engineering negotiation data with foundries.

### 2.7 Dispatch & Returnable Challan Builder (`/dispatch/challans`)
- **Multi-PO / Multi-Heat Consolidation Cart**:
  - Dispatcher selects Consignee (`KSB Shirwal`, `KSB Chinchwad`, `JSONS Foundry`, `Maind Flow`), picks any `PDI_PASSED` pieces (even across multiple KSB POs & Heat Nos).
  - Real-time validation badge verifies Hydro (if pressure part) + LPT (if `CL2B`/repaired) + Flange Confirmed + Kits Fitted.
  - **Live Preview of Print Challan**: Auto-formats Description, Drawing, `(IN Code) Raw Code`, `P.C.`, grouped `KSB PO` lines, `Foundry PO` lines, and exact `Heat No - Qty` breakdown before issuing the atomic `26/NNN` Delivery Challan.
- **Returnable Gate Pass (`/dispatch/returns`)**: Issues `NB-26/NNN` (or `SAME CHALLAN` return) for wrong-material or unrepairable scrap castings at ₹0 billing.

---

## 3. Phased Implementation Roadmap

### Phase 1: Foundation, Masters & Direct Order-to-Dispatch (Weeks 1–3)
- Initialize PostgreSQL + ORM migrations (`parties`, `material_grades`, `flange_standards`, `component_masters`, `po_headers`, `po_line_items`, `inward_batches`, `casting_pieces`, `delivery_challans`).
- Build Auth & RBAC roles + FY Document Numbering Engine (`YY/NNN`, `NB-YY/NNN`).
- Launch Direct KSB (`SHIRWAL` & `CHINCHWAD`) PO Entry, Inward Piece Serialization, and Commercial / NB Delivery Challan PDF generator.
- **Milestone 1 Deliverable**: Parallel-run Delivery Challan & Stock Ledger (`JAI/ST/008`) replacing manual Excel DC typing.

### Phase 2: Foundry 3-Party Flow, Kits & Execution Trackers (Weeks 4–6)
- Implement unlimited `inward_batches` & Dual-Balance Ledger (`JAI/ST/008A` for `JSONS` & `Maind Flow`).
- Add Split Flange Hold controls, Wear Ring / Throttle Bush kit tracking, and category execution fields (`Stage Casing` Taper/Drill, `Volute` Connection Drilling, `HG2/HG3`).
- Build CNC/VMC Shop-Floor Job Card Board & Monthly Dispatch Commitment Matrix (`Sheet1`).
- **Milestone 2 Deliverable**: Full visibility of KSB due-month bottlenecks, foundry casting shortages, and kit waits.

### Phase 3: Quality, NDT, Hydro & Calibration Gate Suite (Weeks 7–10)
- Build Instrument Calibration Master (`JAI/CAL/001`) with hard blocks on `REJECT` / expired gauges and expiring LPT chemicals.
- Implement `hydro_pressure_rules` lookup & `JAI/HT/001` Hydro Certificate PDF generator.
- Implement `JAI/DP/001` LPT Certificate generator with consumable batch validation.
- Build Inspection Template Engine & multi-piece columnar entry for `JAI/INPIR/*` (First Piece) and `JAI/IR/*`, `DE/*`, `TE/*` (PDI Reports).
- **Milestone 3 Deliverable**: Zero-paper shop-floor QC & instant customer compliance pack export (`DC + PDI + Hydro + LPT` ZIP/PDF bundle).

### Phase 4: Rework Analytics, Financial Dashboards & Legacy Migration (Weeks 11–12)
- Implement Defect & Multi-Cycle Repair-Weld Console (`IH` / Foundry Weld / `Twice Repaired` alerts) + `Yearly Rework %` dashboards.
- Build Legacy Excel ETL Importer (`KSB LTD 2026-2027.xlsx`, `OVERALL COMMON INWARD REGISTER.xlsx`, `CALIBRATION`, `INPIR`, and PDI templates) with date-anomaly staging review.
- Add Accounts WIP valuation reconciliation & ExcelJS export parity.

### Phase 5: Shop-Floor UAT, Parallel Run & Cutover (Weeks 13–14)
- **Validation & Parity Checklist**:
  1. Verify live ERP `JAI/ST/008` and `JAI/ST/008A` totals match Excel (`Bal Qty`, `WIP ₹`, `Qty Bal from Foundry`) down to the exact rupee on snapshot cutover date.
  2. Test Calibration hard gate with `JAI/PRE/03` (`REJECT`) and expired consumable batch -> verify system refuses certificate issuance.
  3. Test Multi-PO / Multi-Heat Delivery Challan (`5 NOS` across 2 KSB POs & 4 Heat Nos) -> verify PDF output matches `DELIVERY CHALLAN JAI ENGG.xlsx`.
  4. Test Split Flange PO (`1` confirmed + `4` pending) and Rework -> Re-LPT -> Re-Hydro gating before Dispatch.
  5. Complete 2-week parallel run with QA, Stores, and Accounts before retiring manual spreadsheets.
