# Battery Maintenance

*This section covers the maintenance, monitoring, and safety procedures for the high-energy lithium polymer battery packs used in the Aetherwings eVTOL fleet.*

---

## 10.1 Battery System Overview

*[Placeholder — chemistry, voltage, capacity, cycle life specifications for Aether X1 battery packs]*

---

## 10.2 Daily Pre-Flight Inspections

*[Placeholder — visual checks, state-of-charge verification, connector integrity]*

---

## 10.3 Charging Procedures

*[Placeholder — standard charge vs. fast charge protocols, charge rates, termination criteria]*

---

## 10.4 Storage Requirements

*[Placeholder — temperature, humidity, state-of-charge for short-term and long-term storage, calendar aging considerations]*

---

## 10.5 In-Flight Monitoring

*[Placeholder — real-time telemetry, cell voltages, temperatures, state-of-health indicators, alert thresholds]*

---

## 10.6 Scheduled Maintenance

### 10.6.1 Overview

Scheduled battery maintenance is the backbone of the Aetherwings reliability programme. The Aether X1 pack — a 12S4P lithium polymer assembly with a nominal voltage of 44.4 V and a rated capacity of 6.800 mAh per cell — requires structured periodic intervention to sustain state-of-health (SoH), maximise cycle life, and prevent premature degradation. All maintenance intervals are tracked against **flight hours** and **calendar days**, whichever comes first.

> **Reference Pack:** Aetherwings P/N AX1-BAT-6800, Rev. C
> **Chemistry:** LiPo 4S (lithium polymer, hard-case)
> **Nominal Pack Voltage:** 44.4 V (12S4P)
> **Rated Capacity:** 6.800 mAh per cell | 27.200 mAh pack total
> **Max Continuous Discharge:** 120 A (4.4C)
> **Max Charge Current:** 13.6 A (0.5C standard / 1.0C fast)
> **Design Cycle Life:** ≥ 500 cycles to 80% SoH
> **Operating Temperature Range:** −10 °C to +55 °C
> **Weight:** 14.2 kg (including housing and BMS)

---

### 10.6.2 Maintenance Intervals

| Interval | Trigger | Action |
|---|---|---|
| **A-Turn** | Every 50 flight hours | Visual inspection + SoH scan |
| **B-Turn** | Every 200 flight hours OR 6 calendar months | Cell balance + capacity verification |
| **C-Turn** | Every 500 flight hours OR 18 calendar months | Full deep-cycle conditioning + BMS calibration |
| **Conditioning Discharge** | Every 30 calendar days (if pack stored > 5 days) | Controlled deep discharge to 20% DoD |

---

### 10.6.3 A-Turn Procedure (50 Flight Hours)

**Tools Required:**
- Aetherwings Battery Diagnostic Wand (P/N AX1-DIAG-WAND, USB-C interface)
- Battery Management System (BMS) readout software v3.14 or later
- Insulated torque wrench (2–10 Nm range)
- Anti-static wrist strap

**Steps:**

1. **Isolate the pack** — disconnect aircraft power bus; wait ≥ 5 minutes for bleed-off resistors to drain residual capacitance.
2. **Remove the pack** — using the two-handle extraction frame (P/N AX1-EXT-FRAME); do not lift by connector wires.
3. **Visual inspection** — check housing for cracks, swelling, gasket integrity, and terminal corrosion. See Figure 10.6-A.
4. **Connect diagnostic wand** — attach to pack JST-SR 12-pin service port; launch BMS readout software.
5. **Run SoH scan** — the wand performs a 60-second internal impedance sweep across all 12 series groups. Export report as `AX1_SoH_[Serial]_[Date].xml`.
6. **Log results** — enter into the Aetherwings Maintenance Log (AML) via the ground operations portal.

```
  ┌──────────────────────────────────────────────────────────┐
  │  FIGURE 10.6-A  —  PACK VISUAL INSPECTION POINTS         │
  │                                                          │
  │  ┌─────────────┐   TOP VIEW (connector end)             │
  │  │ ○ ○ ○ ○ ○ ○ │   ← Terminal block: 6 pairs           │
  │  │ CONNECTOR   │     Check for corrosion, looseness      │
  │  └─────────────┘                                         │
  │                                                          │
  │  ┌─────────────────────────────────────────────────────┐ │
  │  │                                                     │ │
  │  │  HOUSING BODY  (scan all faces for swelling)       │ │
  │  │                                                     │ │
  │  │  [FRONT]        [LEFT]        [RIGHT]       [REAR] │ │
  │  │  - cracks       - bulges      - burns       - cracks│
  │  │  - scorch      - venting     - gasket      - dents │
  │  └─────────────────────────────────────────────────────┘ │
  │                                                          │
  │  ┌─────────────┐                                         │
  │  │  GASKET     │   ← Run finger around full perimeter    │
  │  │  CHECK LINE │     for完整性 (integrity)             │
  │  └─────────────┘                                         │
  │                                                          │
  │  [LABEL: P/N, Serial, Mfg Date, Cycle Count]            │
  └──────────────────────────────────────────────────────────┘
```

**Pass Criteria:**
- No housing deformation > 1 mm per axis
- Terminal resistance < 0.8 mΩ per phase
- SoH ≥ 92% (see Section 10.6.5 for SoH calculation)
- BMS reports zero cell-group faults

**Fail Action:** Quarantine pack, tag with red maintenance flag, raise a B-Turn investigation before any return-to-service.

---

### 10.6.4 B-Turn Procedure (200 Flight Hours / 6 Months)

**Tools Required:**
- Aetherwings Cell Balance Station (P/N AX1-BAL-STATION)
- Precision charger (Skytronic MCT-800, 0.1C resolution)
- Fluke 289 multimeter with iFlex clamp
- Thermal imaging camera (FLIR ONE Pro minimum)

> **⚠ WARNING — Ventilation Required**
> This procedure must be performed in a dedicated LiPo service bay rated for minimum 20 ACH (air changes per hour). No open flames within 3 m. Fire suppression system must be armed. Technician must wear Nomex coveralls, safety glasses, and nitrile gloves.

**Steps:**

1. **Full charge** — restore pack to 100% SoC using standard 0.5C protocol (see Section 10.3). Record start time, initial cell voltages.
2. **Rest period** — allow pack to rest at ambient temperature (20–25 °C) for 120 minutes to reach electrochemical equilibrium.
3. **Open cell-balance mode** — transfer pack to balance station; launch balance software. The station will:
   - Measure each of the 12 series groups individually
   - Discharge any cell group exceeding +10 mV above the minimum group
   - Target: all groups within ±5 mV of each other
4. **Balance cycle duration** — typically 90–180 minutes depending on initial imbalance. Station logs real-time group voltages to `AX1_BAL_[Serial]_[Date].csv`.
5. **Capacity verification** — after balancing, perform a controlled discharge at 0.5C to 20% SoC. Record actual delivered mAh. Compare against rated capacity.
6. **Thermal scan** — image the pack with thermal camera at peak discharge load. Note any cell group exceeding 40 °C above ambient.
7. **BMS firmware verification** — connect BMS service tool; confirm firmware version matches current production release (v2026.03 or later as of this revision). Apply any pending calibration parameters.

```
  ┌─────────────────────────────────────────────────────────────────┐
  │  FIGURE 10.6-B  —  CELL BALANCE STATION WIRING DIAGRAM        │
  │                                                                  │
  │    PACK (12S4P)              BALANCE STATION                    │
  │                                                                  │
  │   ┌────────────┐            ┌──────────────────────────────┐   │
  │   │  Cell  1  │─────────────│ CH1 ─── DISCHG circuit ────  │   │
  │   │  Cell  2  │─────────────│ CH2 ─── DISCHG circuit ────  │   │
  │   │  Cell  3  │─────────────│ CH3 ─── DISCHG circuit ────  │   │
  │   │    ·      │             │   ·                          │   │
  │   │    ·      │             │   ·                          │   │
  │   │  Cell 12  │─────────────│ CH12 ─── DISCHG circuit ───  │   │
  │   └────────────┘             └──────────────────────────────┘   │
  │                                                                  │
  │   Positive Bus ──────────── Main Output + (to charger/discharge) │
  │   Negative Bus ──────────── Main Output −                       │
  │   Service Port ──────────── BMS comms (USB-C)                   │
  │                                                                  │
  │   LED Panel:  green = balanced | amber = balancing | red = fail │
  └─────────────────────────────────────────────────────────────────┘
```

**Pass Criteria:**
- All 12 cell groups within ±5 mV after balance
- Delivered capacity ≥ 95% of rated capacity
- No thermal anomaly > 40 °C above ambient during peak discharge
- BMS firmware: current production release

**Fail Action:** If capacity < 95% but ≥ 80%, escalate to C-Turn. If capacity < 80%, initiate replacement process (see Section 10.8).

---

### 10.6.5 C-Turn Procedure (500 Flight Hours / 18 Months)

> **📋 NOTE — C-Turn is a Factory-Level Procedure**
> C-Turns must be performed at an Aetherwings authorised service centre. The procedure requires specialised equipment including a battery formation rack, precision cycle cycler (Arbin BT-2000 or equivalent), and a cleanroom-rated balance station. This procedure may not be performed field-side.

**Full deep-cycle conditioning steps:**

1. **Formation charge** — three consecutive 0.2C charge/discharge cycles to prime the electrodes. Each cycle: charge to 100% at 0.2C, rest 60 min, discharge to 20% at 0.2C.
2. **Impedance spectroscopy** — measure AC impedance at 1 kHz across each cell group. Log baseline for future trend analysis.
3. **Capacity grading** — cycle at 0.5C until capacity stabilises (change between cycle N and N+1 < 0.5%). Average final three cycles as **graded capacity**.
4. **BMS parameter update** — write graded capacity and impedance baseline into BMS EEPROM. Seal with calibration tag.
5. **HTC seal inspection** — apply hyper-thin ceramic (HTC) coating inspection under 10× magnification. Reapply where coating has chipped.
6. **Final inspection and sealing** — torqued gasket replacement, IP67 post-maintenance verification per IEC 60529.

```
  ┌──────────────────────────────────────────────────────────────────┐
  │  FIGURE 10.6-C  —  C-TURN CAPACITY GRADING CURVE (example)      │
  │                                                                   │
  │  Capacity (mAh)                                                    │
  │  68000 ┤                           ●●●●●●●●●●●●●  ← Stabilisation │
  │  67000 ┤                   ●●●●●●●●●●●●●●●●●●●●●●●●●●●●          │
  │  66000 ┤             ●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●       │
  │  65000 ┤       ●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●    │
  │  64000 ┤ ●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●   │
  │        └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴──→         │
  │              1    2    3    4    5    6    7    8  Cycle No.    │
  │                                                                   │
  │  ● = Individual cycle measurement                                 │
  │  Dashed line = 0.5% stabilisation threshold                      │
  │  Shaded zone = Acceptable final capacity range (≥ 95% rated)    │
  └──────────────────────────────────────────────────────────────────┘
```

---

### 10.6.6 State-of-Health (SoH) Calculation

SoH is expressed as a percentage of the pack's rated capacity. It is computed after a full charge/discharge cycle:

```
SoH (%) = (C_actual / C_rated) × 100

Where:
  C_rated  = 27,200 mAh (AX1-BAT-6800 Rev. C, nominal)
  C_actual = measured discharge capacity at 0.5C, 20 °C ± 2 °C
             ambient, from 100% SoC to 20% SoC
```

**SoH Thresholds:**

| SoH Range | Classification | Action |
|---|---|---|
| 100 – 95% | Excellent | Return to service; log and monitor |
| 94 – 90% | Good | Return to service; increase A-Turn frequency to 25 FH |
| 89 – 85% | Fair | B-Turn within 14 days; monitor trend |
| 84 – 80% | Marginal | C-Turn or replacement evaluation |
| < 80% | End-of-Life | Remove from service; initiate Section 10.8 process |

---

### 10.6.7 Conditioned Discharge (30-Day Storage Packs)

Packs removed from aircraft service for more than 5 days must receive a conditioned discharge every 30 calendar days to prevent lithium plating from a sustained high-state-of-charge condition.

```
  ┌──────────────────────────────────────────────────────────────────┐
  │  30-DAY CONDITIONED DISCHARGE SCHEDULE                          │
  │                                                                   │
  │  Day 0   →  Pack removed from aircraft                          │
  │             If SoC > 40%: discharge to 40% using 0.5C           │
  │             Seal terminals; store at 20 °C ± 5 °C               │
  │                                                                   │
  │  Day 30  →  Retrieve pack                                       │
  │             Discharge to 20% SoC at 0.2C                        │
  │             Immediately recharge to 60% SoC (storage level)       │
  │             Inspect gasket; apply anti-corrosion coating         │
  │             Return to service or extend schedule                 │
  │                                                                   │
  │  Day 60  →  Repeat Day 30 procedure                             │
  │  Day 90  →  B-Turn required if still not in service              │
  └──────────────────────────────────────────────────────────────────┘
```

---

### 10.6.8 Maintenance Log Requirements

Every maintenance action must be recorded in the AML (Aetherwings Maintenance Log). Required fields:

| Field | Description |
|---|---|
| **Pack Serial** | e.g., `AX1-2026-00417` |
| **P/N** | e.g., `AX1-BAT-6800 Rev. C` |
| **Flight Hours at Action** | Total FH logged on pack |
| **Cycle Count** | BMS-reported cumulative cycles |
| **SoH (%)** | Post-action SoH measurement |
| **Procedure Code** | `A-Turn / B-Turn / C-Turn / Cond. Disharge` |
| **Technician ID** | Aetherwings employee number |
| **Tool Calibration Ref** | ID of tool used (balance station, wand, etc.) |
| **Pass/Fail** | Outcome |
| **Notes** | Any anomaly, observation, or follow-up required |

> **📋 Record Retention:** AML entries must be retained for the greater of (a) 5 years after pack disposal, or (b) any active regulatory investigation period. Records are stored on the Aetherwings ground server and replicated to the cloud backup endpoint `ops-archive@aetherwings.eu`.

---

### 10.6.9 Reference Documents

| Document | Aetherwings P/N | Notes |
|---|---|---|
| Battery Pack Specification Sheet | AX1-BAT-SPEC | Rev. C current |
| BMS Interface Specification | AX1-BMS-IF | v3.14 |
| A-Turn Diagnostic Procedure | AX1-PROC-AT | This document |
| B-Turn Balance Procedure | AX1-PROC-BT | Current revision |
| C-Turn Factory Procedure | AX1-PROC-CT | Authorised centres only |
| AML User Guide | AX1-PROC-AML | Ground ops portal |
| EASA SC-VTOL Battery Compliance | EASA-SC-VTOL-BAT | Regulatory ref |
