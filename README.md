# Multipath Fading Characterization of FM Signals Using RTL-SDR

Characterizes small-scale multipath fading of a commercial FM broadcast signal (93.5 MHz) captured with an RTL-SDR, by walking a fixed indoor corridor path while logging signal power.

## Method

**Capture (`FM_Multipath.grc` — GNU Radio):**
1. RTL-SDR captures complex IQ at 2.4 MSPS
2. Decimate 100× → 24 kSPS
3. Compute instantaneous power `|x[n]|²`
4. Moving-average smoothing to isolate the slow fading envelope
5. Log to raw `float32` `.bin` file

5 runs (~70–80 s each) were recorded walking ~20–30 m at a steady pace, with fixed gain/frequency across runs.

**Analysis (`FM_Multipath.ipynb`):**
- Envelope extraction (`sqrt(power)`, normalized)
- Rayleigh / Rician distribution fitting (KS test)
- Fade depth, Level Crossing Rate (LCR), Average Fade Duration (AFD)
- Coherence length, Doppler spread

## Key Results

- Rician fits collapse to Rayleigh — no dominant LOS component, confirming a richly-scattering NLOS environment
- Fade depth: 18.5–35.3 dB across walks
- Coherence length: 0.41–0.92 m, well below the ~3.2 m wavelength at 93.5 MHz — fast spatial decorrelation
- Doppler spread: 0.058–0.131 Hz

**Conclusion:** the indoor corridor path is NLOS and well-modeled by Rayleigh fading; results are physically consistent with expected multipath behavior.

*Note: walking speed was estimated (pacing/timing), not instrument-measured — the main source of uncertainty in coherence length and Doppler spread.*

## Requirements

Hardware:
- RTL-SDR dongle (RTL2832U-based)
- Antenna (tuned/suitable for FM broadcast band)

Software:
- GNU Radio (for capture, FM_Multipath.grc)
- Python: numpy, matplotlib, scipy
