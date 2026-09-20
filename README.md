# Equalizer APO Config — High-Passed Stereo Enhancer, BS2B Crossfeed & IEM Target EQ

A streamlined, high-fidelity [Equalizer APO](https://sourceforge.net/projects/equalizerapo/) configuration providing wide ambience, punchy bass, fatigue-free VST crossfeed, and IEM frequency response correction.

The signal chain in [config.txt](config.txt) runs in this order:

```text
Preamp: -6 dB             (Safe digital headroom across all processing)
├─ stereo-enhancer.txt    (High-passed Mid/Side widening: bit-perfect mono, punchy sub-150Hz bass, +2.5 dB ambience)
├─ Crossfeed\             (BS2BR VST crossfeed, with experimental-crossfeed-2.txt as backup)
└─ Device EQ              (Headphone/IEM profile: Ikuba91 Target v2)
```

---

## 1. High-Passed Stereo Enhancer (`stereo-enhancer.txt`)

Standard stereo widening often ruins low-frequency impact by diffusing mono bass. [stereo-enhancer.txt](stereo-enhancer.txt) solves this with a **high-passed Mid/Side (M/S) differential boost**:

$$L_{out} = L + 0.17 \cdot \text{HP}_{150}(L - R)$$
$$R_{out} = R - 0.17 \cdot \text{HP}_{150}(L - R)$$

- **Center & Mono ($L = R$)**: When a signal is centered (vocals, kick drums), $(L - R) = 0$. Centered content passes with **bit-perfect unity gain ($0\text{ dB}$), zero latency, and zero phase rotation**.
- **Bass Below 150 Hz ($f < 150\text{ Hz}$)**: The high-pass filter attenuates the side-differential in the low end. Bass passes as direct, untouched stereo on the through path with full dynamic punch.
- **Ambience & Micro-Details Above 150 Hz ($f > 150\text{ Hz}$)**: Stereo differential cues, room reverb tails, and panned instruments receive a smooth **$+2.54\text{ dB}$ widening lift**.

---

## 2. Crossfeed (`Crossfeed/`)

Headphones provide 100% channel isolation between left and right ears, which causes listening fatigue on hard-panned mixes. Crossfeed introduces acoustic head-shadowing and interaural time delay to simulate natural loudspeaker listening.

In [Crossfeed\Crossfeed.txt](Crossfeed/Crossfeed.txt):
- **Active ([Crossfeed/plugin-crossfed.txt](Crossfeed/plugin-crossfed.txt))**: The 64-bit **BS2BR VST** plugin set to the **Jan Meier preset** (`Feed 0.607143` = 9.5 dB, `FCut 0.205882` = 650 Hz / 280 $\mu$s delay) with $+1.1\text{ dB}$ unity gain compensation.
- **Backup ([Crossfeed/experimental-crossfeed-2.txt](Crossfeed/experimental-crossfeed-2.txt))**: Native low-band crossfeed (<400 Hz) using LR4 biquad pairs and cross-channel summing.

![BS2BR VST plugin in Equalizer APO Configuration Editor](images/bs2b-plugin.png)

---

## 3. Safe Gain Staging & Headroom

In [config.txt](config.txt), the global preamp is set to:

```text
Preamp: -6 dB
```

This ensures adequate headroom so that the $+2.54\text{ dB}$ side boost, crossfeed summing ($+1.5\text{ dB}$), and your IEM EQ filters do not cause digital clipping on full-scale tracks.

---

## 4. IEM Target EQ & JM-1 Adapters

### Target Curve: `Ikuba91 Target v2.txt`
- Measured on Brüel & Kjær Type 5128 (ITU-T P.57 Type 4.3).
- Clean sub-bass roll-off (<35 Hz), punchy mid-bass bump (50–80 Hz), lower-mid scoop (300 Hz), standard 3 kHz ear gain, and extended upper treble sparkle (7–15 kHz).

### JM-1 Tilt Adapters
If your IEM was AutoEQ'd on another database to **PopAvg-DF (JM-1) with -1 dB/octave tilt**:
- **[JM-1 Tilt -1dB to Ikuba91 Target v2 - 1k.txt](JM-1%20Tilt%20-1dB%20to%20Ikuba91%20Target%20v2%20-%201k.txt)**: Adapts the low end (<1 kHz) while leaving treble untouched to avoid cross-coupler resonance variations.
- **[JM-1 Tilt -1dB to Ikuba91 Target v2 - Full.txt](JM-1%20Tilt%20-1dB%20to%20Ikuba91%20Target%20v2%20-%20Full.txt)**: Full-spectrum adapter including the $+2.3\text{ dB}$ peak at 7.8 kHz and $+2.0\text{ dB}$ air shelf at 14 kHz.

![Ikuba91 Target v2 vs JM-1 with -1 dB/oct tilt](images/ikuba91-target-v2-vs-jm1-tilt.png)

---

## 5. Directory Contents

```text
config.txt                                      Main Equalizer APO entry point
stereo-enhancer.txt                             Native High-Passed Mid/Side stereo widener
Crossfeed/
  Crossfeed.txt                                 Crossfeed selector
  plugin-crossfed.txt                           BS2BR VST plugin (Jan Meier preset: 650 Hz / 9.5 dB)
  plugin-crossfed-masked.txt                    BS2BR VST plugin masked below 400 Hz
  experimental-crossfeed-2.txt                  Backup native crossfeed (<400 Hz)
Ikuba91 Target v2.txt                           B&K 5128 IEM target curve
JM-1 Tilt -1dB to Ikuba91 Target v2 - 1k.txt    Bass/lower-mid translation adapter (<1 kHz)
JM-1 Tilt -1dB to Ikuba91 Target v2 - Full.txt  Full-spectrum translation adapter
```
