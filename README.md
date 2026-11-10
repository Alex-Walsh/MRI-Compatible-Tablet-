# Capstone – Bench Testing Log

**Course:** ELEC 490/498 Electrical Engineering Project (Group 25)

This log records hardware bring-up and testing done on the capstone project.

---

## Session: November 1, 2026

**Date:** 2026-11-01

### 1. Display bring-up

- Powered up both the **old (small) display** and the **new, larger display**.
- **Result:** Both displays work.
- The new display is driven by an HDMI driver board with a ribbon cable to the panel. It showed the laptop's desktop over HDMI.

![Old and new displays on the bench](images/displays-bench.jpg)

![New display running](images/new-display-running.jpg)

### 2. Fiber optic link

- Established communication over fiber optic using hardware left over from the previous team.

| Role | Hardware |
|------|----------|
| Transmitter | HDMI/KVM fiber extender over IP |
| Receiver | Green PCB with an SFP fiber module |
| Link | Yellow single-strand fiber patch cables |

- **Result:** The link was established and carried **video**.

![Transmitter, receiver board and fiber patch cables](images/fiber-tx-rx-closeup.jpg)

### 3. Power

- A bench DC power supply (OWON ODP3032, dual channel) powered both the transmitter and the receiver, one on each channel.

| Channel | Set | Measured voltage | Measured current | Power |
|---------|-----|------------------|------------------|-------|
| CH1 | 5.000 V / 1.000 A | 5.005 V | 0.538 A | 2.691 W |
| CH2 | 5.000 V / 1.000 A | 5.007 V | 0.581 A | 2.909 W |

![OWON ODP3032 readings](images/power-supply-readings.jpg)

### Full test setup

![Bench setup overview](images/bench-setup-overview.jpg)

---

## Session: November 2, 2026

**Date:** 2026-11-02

### Low-voltage test: MRI-side fiber optic receiver

Lowered the supply voltage to the fiber optic receiver to find the minimum input voltage at which it still works.

| Vin (V) | Current (A) | Power (W) | Working? |
|---------|-------------|-----------|----------|
| 4.75 | 0.55 | 2.61 | Yes |
| 4.5 | 0.56 | 2.52 | Yes |
| 4.3 | 0.58 | 2.49 | Yes |
| 4.1 | 0.59 | 2.42 | Yes |
| 3.9 | 0.60 | 2.34 | Yes |
| 3.8 | n/a | n/a | No |
| 3.7 | n/a | n/a | No |

**Result:** The receiver works down to **3.9 V** and stops working at 3.8 V. Current rises slightly as voltage drops (0.55 A to 0.60 A), while power drops slightly (about 2.6 W to 2.3 W). Power is calculated as Vin × current.

---

## Next steps

Focus: **power**. Everything on the bench is currently running from a DC power supply, and that needs to change.

- [ ] **Move to battery power.** Replace the bench power supply so the system runs without it.
- [ ] **Identify which parts need replacement.** Review the existing hardware (displays, driver board, fiber transmitter and receiver) and decide what has to be swapped out to run on battery power.
- [ ] **Find replacement parts.** Source the parts identified above.

### Fiber optic receiver: magnetic parts to replace

Parts on the receiver board that are magnetic and need to be replaced for MRI compatibility:

| Part | Action |
|------|--------|
| Optical receiver (SFP module) | Replace with a non-magnetic equivalent |
| HDMI shroud | Replace with a non-magnetic equivalent |
| USB shrouds | Desolder and remove. USB is not needed for this project. |
| 5V DC connector | Desolder and hardwire the power leads directly to the board. |

- [ ] Find a non-magnetic optical receiver
- [ ] Find a non-magnetic HDMI connector/shroud
- [ ] Desolder the USB connectors
- [ ] Desolder the 5V DC connector and hardwire power in

### Fiber optic receiver: inductor replacement options

Candidate non-ferrite replacements for the inductors on the receiver board. Measure each original inductor with the LCR meter after removal and check that the replacement's current rating covers the circuit it sits in.

#### L4

Original L4: value and current rating to be measured.

| Option | Part (manufacturer) | Inductance | Core | Current rating | DCR (max) | Package | Price (CAD, qty 1) | Link |
|--------|---------------------|------------|------|----------------|-----------|---------|--------------------|------|
| A | LQW2UAS4R7J00L (Murata) | 4.7 µH ±5% | Air (wirewound), unshielded | 260 mA | 4 Ω | 1008 (2520 metric) | Check on DigiKey | [DigiKey](https://www.digikey.ca/short/81cn75m5) |
| B | ASMPM-0806-4R7M-T (Abracon) | 4.7 µH ±20% | Ceramic (multilayer), unshielded | 1.1 A (Isat 1.3 A) | 360 mΩ | 0806 (2016 metric) | $0.85 | [DigiKey](https://www.digikey.ca/short/9n5dfhpf) |

Notes:
- **Option A** has a true air core, so it is non-magnetic, but it is only rated for 260 mA with up to 4 Ω of resistance. It suits a filter or low-current line, not a switching converter's main inductor. Its self-resonant frequency is 60 MHz.
- **Option B** handles about four times the current with far lower resistance. DigiKey lists its core as ceramic; confirm it is non-magnetic with the bench magnet (T-01) before fitting it.
- Choose between them once L4's original rating is known: if L4 carries more than about 200 mA, Option A is too small.

#### L5

Original L5: value and current rating to be measured.

| Option | Part (manufacturer) | Inductance | Core | Current rating | DCR (max) | Package | Price (CAD, qty 1) | Link |
|--------|---------------------|------------|------|----------------|-----------|---------|--------------------|------|
| A | AISC-1008-6R8J-T (Abracon) | 6.8 µH ±5% | Ceramic (wirewound), unshielded | 200 mA | 7.7 Ω | 1008 (2520 metric) | Check on DigiKey (about $0.33 each at 2,000) | [DigiKey](https://www.digikey.ca/short/wwn94nzb) |

Notes:
- At 200 mA and up to 7.7 Ω, this part suits a filter or low-current line only, not a switching converter's main inductor. Its self-resonant frequency is 40 MHz.
- DigiKey lists the core as ceramic; confirm it is non-magnetic with the bench magnet (T-01) before fitting it.
