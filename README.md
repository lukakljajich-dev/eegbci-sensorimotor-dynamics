# **EEGBCI Sensorimotor Dynamics**  

## **Repository Overview**
This repository contains two related EEG analyses of sensorimotor dynamics during voluntary hand movement, using data from the MNE-EEGBCI Motor Movement/Imagery Dataset:
- **Single-subject demonstration** (Subject 3, run 3)
- **Multi-subject group analysis** (Subjects 3-10, run 3, N=8)

## **Dataset information:**
- **Dataset name:** EEG Motor Movement/Imagery Dataset (EEGBCI)
- **Source:** [PhysioNet](https://physionet.org/content/eegmmidb/1.0.0/)
- **EEG montage:** 64-channel EEG (10–20 system)
- **Participants:** 109 healthy subjects
- **Runs per subject:** 14

---

## **Reproducibility**
To run this project:

```bash
pip install -r requirements.txt
```
Open and execute the notebook from top to bottom. Raw EEG data is downloaded automatically via MNE-Python.

---

## **Future Extensions**
Possible extensions include:
- Comparison of real vs. imagined movements
- Cluster-based permutation statistics
- Source-space localization
- Decoding (classification of left vs. right movement)

---

## **License**
MIT License


---


## Multi-Subject Voluntary Hand Movement Group Analysis N=8 (Main) - February 18, 2026

## **Overview**
This analysis extends the original single-subject demonstration to a group-level analysis (N=8 subjects, 3-10, run 3). The same  methodology is preserved.

The pipeline is modularized for scalability, enabling statistical inference and validation of the trends observed in the single-subject analysis.

---

- **Selection:**
  - Subject: 3-10 (N=8)
  - Run: 3
  - Task: Open and close left or right fist (real movement)

---

## **Methods**

Multi-subject adaptations from the original single-subject pipeline are noted below:

### **1-3. Loading, QC, Preprocessing**
- Functions created for reusability (load_subject, preprocess_raw)
- QC plots (PSD) limited to first 2 subjects

### **4. ICA & Artifact Removal**
- Automated EOG detection via correlation with Fp1/Fp2
- Only the strongest EOG component removed 
- Overwriting raw after ICA instead of keeping raw + raw_post_ica

### **5. Epoching**
- Identical parameters to single-subject (baseline: -0.2–0 s)
- Baseline contamination acknowledged as shared limitation

### **6. ERD Analysis**
- TFR computation wrapped in compute_power() function
- Values stored per subject for group comparison

### **7. ERP Analysis**
- Averaging trials to 1 value per subject instead of retaining all trials
- Combining both hands into single contra/ipsi values instead of separate left/right tests

### **8. Master Loop & Data Validation**
- process_one_subject() orchestrates pipeline
- Results stored in dictionary, converted to DataFrame
- ERP values converted from volts to microvolts

### **9. Plotting & H1 testing**
- Paired t-test across 8 subjects instead of no statistical test
- Group bar plot with SEM instead of individual time-frequency plots

### **10. Plotting & H2 testing**
- Single paired t-test across subjects instead of two per-subject t-tests
- Group bar plot with SEM instead of individual ERP waveforms/topomaps

---

## **Results**
- **H1 – Mu/Beta ERD (8–30 Hz):** Movement elicited significantly stronger desynchronization compared to rest, t(7) = -2.601, p = 0.035. The observed t-value exceeds the critical value (±2.365) under α = 0.05, with df=7.
- **H2 – ERP Lateralization (300–600 ms):** Contralateral electrodes showed significantly reduced amplitudes relative to ipsilateral electrodes, t(7) = -3.193, p = 0.015, Cohen's d = -1.13 (large effect).

---

## **Limitations**
- Sample size (N=8) modest, though sufficient for effect detection
- Baseline contamination from rapid task design persists
- Findings should be considered preliminary


---


## Within-Subject Voluntary Hand Movement Analysis (Original Demonstration) - February 11, 2026

## **Overview**
This project presents a within-subject, within-run EEG analysis of sensorimotor dynamics during voluntary hand movement, using data from the MNE-EEGBCI Motor Movement/Imagery Dataset. The analysis focuses on event-related spectral desynchronization (ERD) and event-related potentials (ERP) associated with left and right fist movements, with attention to preprocessing rationale, signal interpretation, and methodological limitations.

The goal of this project is to demonstrate a complete, reproducible EEG analysis pipeline from raw data to interpretable neurophysiological results, than to make population-level claims or inferences.

---

- **Selection:**
  - Subject: 3
  - Run: 3
  - Task: Open and close left or right fist (real movement)

---

## **Research Questions & Hypotheses**
Two hypotheses guided the analysis:

### **H1 – Movement vs. Rest (ERD)**
Real hand movement (T1/T2) should produce stronger desynchronization in sensorimotor rhythms (mu/beta, 8–30 Hz) compared to rest (T0).

**H1 Test:** Baseline-corrected time-frequency power (ERSP) was compared between movement and rest epochs over sensorimotor electrodes (C3, C4).

### **H2 – Motor ERP Lateralization**
Motor-related ERPs should peak approximately 200–500 ms after movement onset and show greater amplitude over the contralateral motor cortex.

**H2 Test:** ERP peak amplitude and latency were quantified at electrodes C3 and C4, and contralateral vs. ipsilateral responses were compared using paired, within-trial statistics.

---

## **Methods**

### **1. Data Loading / setup**
- Importing  libraries (MNE, NumPy, Matplotlib, SciPy)
- Raw EDF files loaded using MNE-Python

### **2. Visualization & Quality Control**
- Visual inspection of raw signals
- Power spectral density (PSD) plots used for frequency-domain QC
- Event annotations (T0 = rest, T1 = left hand, T2 = right hand)

### **3. Preprocessing**
- Notch filter at 60 Hz (line noise)
- Downsampling to 120 Hz

### **4. Independent Component Analysis (ICA)**
- Assigning standard 10–20 montage
- Data high-pass filtered at 1 Hz for ICA fitting
- 20 components FastICA algorithm
- Components evaluated using time series, scalp topographies, and power spectral density
- One ocular artifact component identified and removed

### **5. Epoching**
- Epochs extracted from −0.2 s to 1.0 s around events (T0-T2)
- Baseline correction: −0.2 to 0 s
- Automatic rejection of bad epochs based on annotations

> **Important note on baseline:** Due to the rapid task design, the pre-event baseline may contain residual activity from the previous trial. All ERP and ERD results should therefore be interpreted as relative to the immediate pre-event brain state, not a true neutral rest condition.

### **6. Time-Frequency Analysis (ERSP / ERD)**
- Morlet wavelet transform (4–40 Hz, logarithmic spacing)
- Focus on sensorimotor rhythm (8–30 Hz)
- Baseline normalization using log-ratio
- ERD quantified by averaging power within:
  - Time window: 0.2–1.0 s
  - Frequency band: 8–30 Hz
  - Electrodes: C3, C4
  - Statistically combining and comparing movement vs. rest amplitude difference

### **7. Event-Related Potentials (ERP)**
- ERPs plotted separately for T0, T1, and T2
- Topographic maps at 300 ms and 500 ms for each condition
- H2 Quantitative testing:
  - Trial-wise mean amplitudes extracted (0.2–0.6 s)
  - Paired t-tests compared contralateral vs. ipsilateral electrodes
  - Effect sizes (Cohen’s d) computed

---

## **Results**
- **ERD:** Movement trials exhibited only a negligible difference in mu/beta suppression compared to rest (-0.075 dB), with unexpected low-frequency power increases rather than the expected desynchronization pattern.
- **ERP:** Waveforms showed a clear polarity asymmetry. Left-hand responses produced a positive peak, right-hand a negative peak—while topographic maps displayed the expected contralateral organization.
- **Statistics:** Paired tests found no significant lateralization (all p > 0.05); effect sizes were small and inconsistent; limited power prevented reliable significance testing
- **Interpretation:** Observed patterns align qualitatively with established sensorimotor physiology, but statistical reliability cannot be claimed given the single-subject, limited‑trial design.

---

## **Limitations**
- Single subject, single run
- Small number of trials per condition
- No group-level statistics
- Baseline contamination due to rapid task structure

This project is intended as a methodological demonstration, not definitive claims.
