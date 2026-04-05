# cyber-drift-infinite-dda
Why "helping" the player can kill them. A DDA simulation showing how reducing traffic density in Cyber Drift Infinite starves the near-miss orb — the player's sole path to Rage Mode. Naive DDA vs. Coupling-Aware DDA, with triggerable failure case. CSYE 7270 Take-Home Midterm, Category B.

# The Starvation Spiral: Why Standard DDA Breaks in Risk-Reward Games

**CSYE 7270 — Virtual Environments and Real-Time 3D**
**Take-Home Midterm: The AI Game Dev Mandate**

**Author:** Dhyeykumar Rabadiya
**Category:** B — AI in Game Mechanics
**Game:** Cyber Drift Infinite

---

## Topic Claim

After reading this piece, a practitioner will understand how AI-driven difficulty scaling interacts with risk-reward mechanics like Cyber Drift Infinite's near-miss orb system well enough to design a DDA system that adapts challenge without disrupting the player's resource economy — without making the mistake of treating traffic density as a single-purpose difficulty knob when it simultaneously serves as the player's only path to their power-up.

---

## Repository Structure

```
├── README.md                    This file
├── notebook/
│   └── dda_starvation_spiral.ipynb   Runnable simulation & failure case
├── essay/
│   └── starvation_spiral_essay.pdf   Technical essay (1,500–2,500 words)
├── authors_note/
│   └── authors_note.pdf              3-page reflection
├── gdd/
│   └── cyber_drift_infinite_gdd.pdf  Game Design Document
└── VIDEO_LINK.md                     Link to 10-minute video
```

---

## Quick Start — Run the Notebook

### Option A: Google Colab (Recommended — No Installation)
1. Open `notebook/dda_starvation_spiral.ipynb`
2. Click the **"Open in Colab"** badge or upload to [colab.research.google.com](https://colab.research.google.com/drive/1K2TzR-Eisyt-KARFmCqRY1eEJNXjxOFe?usp=sharing)
3. Click **Runtime → Run All**
4. All graphs render in ~10 seconds

### Option B: Local Machine
```bash
git clone https://github.com/[your-username]/cyber-drift-infinite-dda.git
cd cyber-drift-infinite-dda/notebook
pip install numpy matplotlib
jupyter notebook dda_starvation_spiral.ipynb
```

### Dependencies
- Python 3.8+
- `numpy`
- `matplotlib`

No other packages required. No API keys. No LLMs. No GPU.

---

## Triggering the Failure Case (< 5 Minutes)

### The Starvation Spiral (Naive DDA)
1. Open the notebook and run all cells
2. Scroll to **Section 6: The Starvation Spiral**
3. Observe the telemetry signature: **distance at death rises while orb at death falls**
4. The player is surviving longer on an emptier road but arriving at Stage 3 without Rage Mode

### Compare with Coupling-Aware DDA
1. **Section 7** runs both controllers side by side with identical seeds
2. Naive DDA: orb charge collapses, formation deaths increase
3. Coupling-Aware DDA: orb charge stays healthy, player reaches Rage Mode

### The Exercise (Section 8)
1. Modify `oncoming_ratio_shift` (try values 0.05 through 0.20)
2. Re-run the cell
3. Find the threshold (~30–40% reduction) where the spiral re-emerges through the partially coupled parameter

---

## Human Decision Node

**Location:** `notebook/dda_starvation_spiral.ipynb`, Section 4, inside the `CouplingAwareDDA` class

**Summary:**
- **AI proposed:** A single DDA controller that uniformly reduces all difficulty parameters, including traffic density
- **I rejected because:** Traffic density is not a single-purpose difficulty knob — it simultaneously feeds the orb's charge pipeline. Reducing it severs the player's only path to Rage Mode.
- **My decision:** Built a coupling-aware controller that classifies parameters by coupling risk (safe / partially coupled / fully coupled) and never reduces traffic density below baseline

---

## The Master Claim

> *AI is a pipeline decision, not a magic layer. Where you put the DDA tap — and which downstream consumers it starves or feeds — determines whether adaptive difficulty saves the player or builds the trap they cannot escape.*

---

## Video

📹 [Watch the 10-minute walkthrough](https://drive.google.com/file/d/1qtNCuq7mn2lXmOHV7stPCJdvsSKsgyrh/view?usp=sharing)