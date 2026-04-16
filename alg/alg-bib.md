# Annotated Bibliography — Neural Signal Processing / BCI (Algorithms & DSP Pipeline Focus)

---
---

**Llobet, V., Wyngaard, A., & Barbour, B. (2022). Automatic post-processing and merging of multiple spike-sorting analyses with Lussac.** *bioRxiv.*  [https://doi.org/10.1101/2022.02.08.479192](https://doi.org/10.1101/2022.02.08.479192)

---

This paper directly addresses a fundamental limitation in the spike-sorting step 
of the neural signal processing pipeline: no single algorithm performs optimally 
across all neuron types simultaneously. The authors introduce Lussac, a 
Python-based post-processing tool that runs multiple spike-sorting passes -- 
using tools like Kilosort and MountainSort with different filter bands and 
detection thresholds — then automatically merges the outputs using a 
quality-guided cluster fusion algorithm. The quality metric combines cluster 
contamination (estimated via refractory period violations) and firing frequency,
and merges are only accepted when they increase that score. Key DSP techniques 
include bandpass filtering tailored to spike type (300–6000 Hz for simple spikes;
150–3000 Hz for complex spikes), cross-correlation-based spike alignment to 
reduce detection jitter, and coincident spike deduplication across analyses. 
Tested on cerebellar multielectrode recordings and the synthetic SpikeForest 
dataset, Lussac yielded roughly a 50% increase in detected Purkinje cell 
clusters with both simple and complex spikes compared to either standalone 
sorter. The paper is particularly relevant for understanding why spike sorting 
is not a solved problem and how ensemble-based algorithmic approaches can 
squeeze more signal out of noisy, low-SNR channels.

---
---

**Shevchenko, O., Yeremeieva, S., & Laschowski, B. (2024). Comparative analysis of neural decoding algorithms for brain-machine interfaces.** *bioRxiv.* [https://doi.org/10.1101/2024.12.05.627080](https://doi.org/10.1101/2024.12.05.627080)

---

This paper conducts one of the most comprehensive head-to-head evaluations 
of the full BCI decoding pipeline to date, testing 48 combinations of signal 
processing, feature extraction, and classification across 14 subjects (672 
total experiments). On the signal processing side, it compares artifact subspace 
reconstruction (ASR), surface Laplacian filtering (SLF), and a normalization 
baseline. Feature extraction methods include common spatial patterns (CSP), 
independent component analysis (ICA), and short-time Fourier transform (STFT). 
These are paired with four classifiers: SVM, LDA, CNN (EEGNet architecture), and 
LSTM. The EEG dataset uses a 1–20 Hz bandpass filter and 200 ms rolling windows 
with 160 ms overlap, optimized for decoding gait events from real-world walking. 
Top-line findings: CNNs consistently outperformed all other classifiers (best F1
of ~93% with SLF+ICA); STFT feature extraction significantly degraded 
performance across the board (up to 25% drop); and ASR, despite being widely 
used, often hurt accuracy or increased variance. Critically for embedded DSP 
applications, the paper also benchmarks computational footprint — CNN model size
was 0.03 MB versus 278.5 MB for SVM, and average inference time per segment was 
under 1 ms for most feature extractors. This makes it a valuable reference for 
evaluating algorithm trade-offs in real-time constrained systems.

---
---

**Zhu, B., Shin, U., & Shoaran, M. (2021). Closed-loop neural prostheses with on-chip intelligence: A review and a low-latency machine learning model for brain state detection.** *IEEE Transactions on Biomedical Circuits and Systems, 15* (5), 877–897. [https://doi.org/10.1109/TBCAS.2021.3112756](https://doi.org/10.1109/TBCAS.2021.3112756)

---

This paper is the most hardware-oriented of the three, focusing on how ML-based 
neural decoding algorithms are actually realized in silicon for real-time, 
closed-loop prostheses. The review covers on-chip implementations across 
epilepsy, Parkinson's disease, and other neurological applications, with 
particular attention to application-specific integrated circuits (ASICs) and 
algorithm-hardware co-design. The authors introduce an energy-area (E-A) figure 
of merit to fairly compare across architectures accounting for channel count and
sampling rate. The core contribution is the Depth-Variant Tree Ensemble (DVTE), 
a gradient-boosted decision tree classifier where trees of varying depths (1–4) 
run in parallel, allowing shallow trees to output early predictions while deeper
trees correct errors. On a seizure detection task using intracranial EEG, DVTE 
reduced processing latency by 2.5x compared to a conventional uniform-depth 
ensemble with only a marginal drop in sensitivity (<3%). A cost-aware learning 
framework further penalizes power-hungry and high-latency features (e.g., 
low-frequency bandpower requiring long windows) during training, achieving a 3x 
power reduction and 1.7x latency reduction at the same accuracy operating point.
The final DVTE classifier was implemented in TSMC 65 nm CMOS at $2.8 \microW$ 
and $0.31 mm^2$, achieving 5.6 nJ/class energy efficiency — over 6x better than 
prior state-of-the-art. Directly relevant to the embedded DSP angle of this 
project, as it demonstrates how algorithm design decisions (tree depth, feature 
selection, quantization) have direct hardware consequences and must be 
co-optimized from the start.
