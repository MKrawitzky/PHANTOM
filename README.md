<div align="center">

```
                                    👻

              ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
           ░░░                                    ░░░
         ░░░        P H A N T O M                    ░░░
        ░░     Probabilistic High-Accuracy MS           ░░
       ░░                                                ░░
       ░░   particle filter · EKF · GRU · EB-FDR         ░░
       ░░                                                ░░
        ░░       30–40K PSMs @ 1% FDR                   ░░
         ░░░    no spectral library required          ░░░
           ░░░                                    ░░░
              ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

              it was there all along. you just
                 couldn't see it until now.
```

*Five algorithmic layers borrowed from radar, information theory, and Bayesian statistics*

![Python](https://img.shields.io/badge/python-3.9%2B-blue?style=flat-square)
![Platform](https://img.shields.io/badge/timsTOF-native-22d3ee?style=flat-square)
![License](https://img.shields.io/badge/license-Academic-a855f7?style=flat-square)
![ZIGGY](https://img.shields.io/badge/engine%20in-ZIGGY-DAAA00?style=flat-square)

</div>

---

## Who

Michael Krawitzky Â· built within [ZIGGY](https://github.com/MKrawitzky/Ziggy), the proteomics rockstar dashboard.

---

## What

A **deep-recall DIA search engine** that tracks peptide candidates across multiple frames using probabilistic state estimation borrowed from radar target tracking, then scores them with a Gated Recurrent Unit and reports identifications at calibrated Empirical Bayes FDR.

**Five algorithmic layers:**

```
Precursor candidates enter the pipeline as particles →

┌─────────────────────────────────────────────────────┐
│  1. Particle Filter                                 │
│     N particles track each candidate across frames  │
│     Weights updated by fragment match probability   │
│                                                     │
│  2. Extended Kalman Filter (4-state)               │
│     State: (RT, RT', 1/K₀, 1/K₀')                 │
│     Predicts elution position; flags stragglers     │
│                                                     │
│  3. GRU Predictor                                  │
│     Gated Recurrent Unit scores fragment sequence   │
│     context — same architecture as language models  │
│                                                     │
│  4. Empirical Bayes FDR (EB-FDR)                   │
│     Models the null score distribution              │
│     nonparametrically from the data itself          │
│                                                     │
│  5. Co-elution Network                             │
│     Graph of co-eluting precursors; group-level     │
│     confidence boosts borderline PSMs               │
└─────────────────────────────────────────────────────┘

            ↓
    30–40K PSMs @ 1% FDR
    No spectral library required
```

---

## When

Developed 2024–2026 as ZIGGY's maximum-depth DIA engine. PHANTOM exists to push the ceiling: if GauDIA is the baseline and Copperfield is the precision layer, PHANTOM asks *how deep can we go without a library, and without sacrificing statistical rigor?*

---

## Where

Runs locally — instrument PC or analysis workstation. Python 3.9+, Bruker TimsData SDK, a `.d` directory, a FASTA database. No spectral library needed — PHANTOM builds its scoring model entirely from the data it is given. Within [ZIGGY](https://github.com/MKrawitzky/Ziggy), PHANTOM runs in the comparison hub and reports in the Searches tab in real time.

---

## Why

The name captures the experience of looking at a noisy DIA spectrum and knowing a peptide is in there — somewhere — without being able to see it clearly. Chimeric isolation windows are the haunting of proteomics: multiple peptides in one window, their fragments interleaved, the signal ghosted across frames.

PHANTOM treats each peptide candidate as a particle that exists with some probability across many frames. It does not commit to one explanation of the data — it maintains a probabilistic distribution over all explanations and lets the data itself converge on the most likely one. The peptide was there all along. PHANTOM finds it.

The algorithmic choices are intentional borrowings from adjacent fields:
- **Particle filters** track stealth aircraft in radar when the signal is too weak and intermittent for classical tracking
- **Extended Kalman Filters** maintain state estimates across noisy observations in GPS receivers and spacecraft
- **GRU architectures** model sequential context in language — here, the sequence of fragment ions tells a story about peptide identity
- **Empirical Bayes FDR** avoids parametric assumptions about the null distribution — the data defines its own null

> *It was there all along. You just couldn't see it until now.*

---

## Part of ZIGGY

| Engine | PSMs @ 1% FDR | Strategy |
|--------|---------------|----------|
| GauDIA | ~22K | Cosine baseline |
| Copperfield | ~12K | Precision through elimination |
| **PHANTOM** | 30–40K | Depth through probabilistic tracking |

[→ MKrawitzky/Ziggy](https://github.com/MKrawitzky/Ziggy)

---

*Academic License · © 2024–2026 Michael Krawitzky & Brett S. Phinney*
