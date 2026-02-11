# **EEGBCI Sensorimotor Dynamics**  
**Within-Subject ERP and ERD Analysis of Voluntary Hand Movement**

## **Overview**
This project presents a within-subject, within-run EEG analysis of sensorimotor dynamics during voluntary hand movement, using data from the MNE-EEGBCI Motor Movement/Imagery Dataset. The analysis focuses on event-related spectral desynchronization (ERD) and event-related potentials (ERP) associated with left and right fist movements, with attention to preprocessing rationale, signal interpretation, and methodological limitations.

The goal of this project is to demonstrate a complete, reproducible EEG analysis pipeline from raw data to interpretable neurophysiological results, than to make population-level claims or inferences.

---

## **Dataset**
- **Dataset:** EEG Motor Movement/Imagery Dataset (EEGBCI)
- **Source:** [PhysioNet](https://physionet.org/content/eegmmidb/1.0.0/)
- **Participants:** 109 healthy subjects
- **Runs per subject:** 14
- **Selection:**
  - Subject: 3
  - Run: 3
  - Task: Open and close left or right fist (real movement)
- **EEG montage:** 64-channel EEG (10–20 system)

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
- Multi-subject analysis with mixed-effects models
- Comparison of real vs. imagined movements
- Cluster-based permutation statistics
- Source-space localization
- Decoding (classification of left vs. right movement)

---

## **License**
MIT License