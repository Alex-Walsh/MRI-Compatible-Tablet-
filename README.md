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
