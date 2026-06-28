# Calibrating the nanoVNA-H and Analyzing an HF Dipole

This guide covers calibrating a nanoVNA-H (v3.6.1_SM_ST, SMA version) and using it to find the resonant frequency and SWR of an HF dipole. It assumes the antenna and feedline are already built and connected — this document does not cover antenna construction. See the [Reference section](#reference-band-plans-and-sweep-ranges) at the end for band-specific frequencies and suggested sweep ranges.

## Equipment

- nanoVNA-H v3.6.1_SM_ST
- SMA calibration kit (OPEN, SHORT, LOAD standards)
- One SMA jumper cable (used for calibration and as a permanent patch between the VNA and feedline)
- Coax feedline to the dipole feedpoint

## Important: Calibrate With the Same Jumper You'll Measure Through

The nanoVNA establishes its "zero reference" — the reference plane — at whatever is connected to its ports during calibration. If you calibrate with the standards screwed directly onto the VNA, but then add a jumper cable before connecting your feedline, that jumper's length and loss become an uncorrected error in every reading.

**The rule:** calibrate through everything that will stay in the signal path when you measure the antenna.

If you always connect your feedline to the VNA through a short SMA jumper, leave that jumper on CH0 for the entire calibration process, and put the OPEN/SHORT/LOAD standards on the *far end* of the jumper. That jumper must be the exact same cable every time this calibration is used — swapping it invalidates the calibration.

## Step 1 — Reset and Set the Sweep Range

1. Power on the nanoVNA.
2. Clear any old calibration:
   ```
   MENU → CAL → RESET
   ```
3. Set the display to show SWR on Trace 0:
   ```
   MENU → DISPLAY → TRACE → TRACE 0 → SWR
   ```
4. Turn off Trace 1 to reduce clutter:
   ```
   MENU → DISPLAY → TRACE → TRACE 1 → OFF
   ```
5. Set the sweep range to cover your target band with margin on both sides. This makes it easier to see the full resonance curve rather than only the portion inside the band. See the [Reference section](#reference-band-plans-and-sweep-ranges) for suggested ranges per band:
   ```
   MENU → STIMULUS → START → [start frequency in Hz]
   MENU → STIMULUS → STOP  → [stop frequency in Hz]
   ```

## Step 2 — Calibrate (SOLT)

Leave the SMA jumper attached to **CH0** for the entire calibration. Do not remove it between steps — only swap what's on the *far end* of the jumper.

1. Open the calibration menu:
   ```
   MENU → CAL → CALIBRATE
   ```
2. **OPEN** — attach the OPEN standard to the far end of the jumper. Wait for the trace to settle, then tap `OPEN`.
3. **SHORT** — remove the OPEN, attach the SHORT standard to the far end of the jumper. Wait, then tap `SHORT`.
4. **LOAD** — remove the SHORT, attach the 50Ω LOAD standard to the far end of the jumper. Wait, then tap `LOAD`.
5. **THRU** — remove the LOAD. Connect the far end of the jumper directly to **CH1**. Wait, then tap `THRU`.
6. Tap `DONE`.
7. Save the calibration to a slot so it can be reloaded later without repeating these steps:
   ```
   CAL → SAVE → SAVE 0
   ```

To reload this calibration in a future session: `MENU → CAL → LOAD → 0`.

> **Re-calibrate if:** the sweep range changes, the jumper cable is replaced or damaged, or any standard is swapped for a different one. A calibration is only valid for the exact frequency range and exact cable it was performed with.

## Step 3 — Connect the Antenna and Sweep

1. Disconnect CH1. Leave the jumper on CH0.
2. Connect the dipole's feedline to the far end of the jumper.
3. Watch the SWR trace across your sweep range. A dipole typically shows one clear, fairly sharp dip — sharper than an end-fed antenna's broader curve.
4. If no dip is visible anywhere on screen, widen the sweep range until one appears, then narrow back down once you've located it.

## Step 4 — Mark the Resonant Point

1. Place a marker on the lowest point of the dip:
   ```
   MENU → MARKER → MARKER 1 → tap the dip on screen
   ```
2. Read the frequency and SWR value shown for that marker.
3. Optionally add markers at specific frequencies of interest within your target operating segment — for example the SSB or digital portion of the band — to check SWR across your intended operating range:
   ```
   MENU → MARKER → MARKER 2 → move to desired frequency
   ```

## Step 5 — Interpret the Result

| SWR at resonance | Assessment |
|---|---|
| < 1.5 | Excellent |
| 1.5 – 2.0 | Good — most radios/tuners handle this with no issue |
| 2.0 – 3.0 | Marginal — tuner likely required, or consider trimming |
| > 3.0 | Needs adjustment before operating |

**If the dip frequency is below your target (resonance too low):** each leg is too long. Trim a small amount — start with 1–2 inches per leg — and re-sweep.

**If the dip frequency is above your target (resonance too high):** each leg is too short. Add wire or temporarily extend with a clip lead to confirm before splicing permanently.

As a rough rule of thumb, each inch trimmed or added per leg shifts resonance by approximately the amounts shown below, but this varies with height, surroundings, and wire gauge — always re-sweep after each adjustment rather than calculating blind.

| Band | Approx. shift per inch per leg |
|---|---|
| 40m | 5 – 10 kHz |
| 20m | 10 – 20 kHz |
| 10m | 40 – 80 kHz |

> Antenna height and nearby objects (trees, structures, ground conductivity) affect resonance. A final sweep should be done with the antenna at its actual installed height, since bench or ground-level measurements will shift once it's raised.

## Step 6 — Confirm at Operating Height

Once the dip lands at or near your target frequency with an acceptable SWR, do a final sweep with the antenna at its permanent installed height to confirm before going on the air.

---

## Reference: Band Plans and Sweep Ranges

### 40m (7.000 – 7.300 MHz)

**Suggested sweep range:** 6.5 – 8.0 MHz
```
START → 6500000   (6.5 MHz)
STOP  → 8000000   (8.0 MHz)
```

| Segment | Frequencies | Typical target |
|---|---|---|
| Digital / FT8 | 7.074 MHz | 7.074 MHz |
| Digital / PSK31 | 7.070 MHz | 7.070 MHz |
| SSB / Phone (General, US) | 7.125 – 7.300 MHz | ~7.200 MHz |

---

### 20m (14.000 – 14.350 MHz)

**Suggested sweep range:** 13.5 – 15.0 MHz
```
START → 13500000   (13.5 MHz)
STOP  → 15000000   (15.0 MHz)
```

| Segment | Frequencies | Typical target |
|---|---|---|
| Digital / FT8 | 14.074 MHz | 14.074 MHz |
| Digital / PSK31 | 14.070 MHz | 14.070 MHz |
| SSB / Phone (General, US) | 14.150 – 14.350 MHz | ~14.225 MHz |

---

### 10m (28.000 – 29.700 MHz)

**Suggested sweep range:** 27.0 – 30.5 MHz
```
START → 27000000   (27.0 MHz)
STOP  → 30500000   (30.5 MHz)
```

| Segment | Frequencies | Typical target |
|---|---|---|
| Digital / FT8 | 28.074 MHz | 28.074 MHz |
| Digital / PSK31 | 28.120 MHz | 28.120 MHz |
| SSB / Phone (General, US) | 28.300 – 28.600 MHz | ~28.400 MHz |

---

> All frequency allocations above are for US General class licensees. Verify current band plans via the ARRL or your national licensing authority before operating.
