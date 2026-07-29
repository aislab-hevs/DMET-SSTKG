# DMET-SSTKG

**Data-Minimizing Decentralized Motif Evidence Transfer over Surgical Spatio-Temporal Knowledge Graphs**

This repository contains the derived Surgical Spatio-Temporal Knowledge Graph
(S-STKG) data and Jupyter notebooks used to reproduce the DMET-SSTKG study:
node-detector preparation, S-STKG construction, source phase learning,
phase-initialized motif extraction, calibrated hub construction, remote motif
consultation, objective ablation, decentralized comparison, and external
source-counterpart stress tests.

Two reproducibility routes are supported:

1. **Released-data route:** start from the derived S-STKG folders and reproduce
   the modeling and evaluation stages from notebook 08 onward.
2. **Reconstruction route:** start from provider-authorized surgical data and
   rebuild the S-STKG folders with notebooks 01–07 before continuing from
   notebook 08.

The original surgical videos are **not redistributed** in this repository.
`S-STKG_Data/` contains derived, annotation-defined, phase-aligned graph
fragments only.

---

## Repository structure

```text
DMET-SSTKG/
├── README.md
├── Codes/
│   ├── Node_Detector/
│   ├── S-STKG_Construction/
│   ├── Phase_Classification/
│   ├── Motif_Extraction/
│   ├── Hub_Construction/
│   ├── Remote_Update_Cholecystectomy/
│   ├── External_Stress_Tests/
│   ├── Ablation_Objectives/
│   └── Comparative_Analysis/
└── S-STKG_Data/
    ├── SurgicalSTKG_Local_Cholec80_M2CAI/
    ├── SurgicalSTKG_Remote_CHOLEC80_M2CAI/
    ├── CholecTrack20_STKG/
    ├── CholecT50_STKG/
    ├── MultiBypass140_STKG/
    └── AutoLaparo_STKG/
```

The folders group notebooks by pipeline stage, whereas notebook numbers indicate
the overall execution order. Generated caches, checkpoints, tables, and figures
are written to local output directories configured inside the notebooks and are
not included in the structure above.

---

## Quick start

### 1. Create an environment

The notebooks were prepared with Python 3.9, PyTorch 2.4.1, PyTorch Geometric,
and Ultralytics 8.4.18. Install the PyTorch build appropriate for the local CPU
or CUDA environment first, then install the remaining packages.

```bash
python -m venv .venv
source .venv/bin/activate

python -m pip install torch==2.4.1
python -m pip install torch-geometric ultralytics==8.4.18 opencv-python \
    pandas numpy matplotlib scikit-learn tqdm jupyter nbformat pyyaml
```

### 2. Start Jupyter from the repository root

```bash
cd DMET-SSTKG
jupyter lab
```

Launching Jupyter from the repository root keeps repository-relative paths
consistent across notebooks.

### 3. Select a reproducibility route

Readers using the released S-STKG data can start at notebook 08. Point the
repository-path cell in each notebook to the required folder under
`S-STKG_Data/`.

For example:

```python
from pathlib import Path

SSTKG_ROOT = (
    Path("S-STKG_Data")
    / "SurgicalSTKG_Local_Cholec80_M2CAI"
)
```

Output and cache paths can remain under `outputs/`, as configured in the
notebooks.

Readers rebuilding the graph data must first obtain the original datasets from
their official providers and complete the reconstruction prerequisites described
under **Original datasets and reconstruction inputs**. The construction
notebooks do not download provider data.

---

## Released S-STKG cohorts

The table below is the single reference for released folder names, cohort roles,
sizes, label spaces, and downstream notebooks. Fragment counts refer to the
phase-aligned S-STKG fragments included in the release.

| Released folder | Cohort composition and role | Procedures | Fragments | Stored phase labels | Used by |
|---|---|---:|---:|---|---|
| `SurgicalSTKG_Local_Cholec80_M2CAI` | Source development: 10 Cholec80 [[1]](#dataset-ref-1) + 10 M2CAI16 workflow [[1]](#dataset-ref-1), [[2]](#dataset-ref-2) procedures | 20 | 147 | Source P1–P8, stored as `1–8` | 08–11 and 18 |
| `SurgicalSTKG_Remote_CHOLEC80_M2CAI` | Same-ontology remote evaluation: 5 Cholec80 [[1]](#dataset-ref-1) + 5 M2CAI16 workflow [[1]](#dataset-ref-1), [[2]](#dataset-ref-2) procedures | 10 | 75 | Source P1–P8, stored as `1–8` | 12, 17, and 18 |
| `CholecTrack20_STKG` | Cholecystectomy-family cross-dataset remote evaluation [[3]](#dataset-ref-3) | 10 | 62 | Native CholecTrack20 IDs `0–6` | 13 |
| `CholecT50_STKG` | Cholecystectomy-family cross-dataset remote evaluation [[4]](#dataset-ref-4) | 10 | 67 | Native CholecT50 IDs `0–6` | 14 |
| `MultiBypass140_STKG` | External source-counterpart stress test [[5]](#dataset-ref-5) | 10 | 189 | Retained native IDs `0–11` | 15 |
| `AutoLaparo_STKG` | External source-counterpart stress test [[6]](#dataset-ref-6) | 10 | 82 | Native IDs `1–7` | 16 |

Naming and identifier notes:

- `SurgicalSTKG_Local_Cholec80_M2CAI` is the source-development cohort.
  `Local` is a historical folder name and does not denote a separate evaluation
  protocol.
- MultiBypass140 case-folder identifiers are intentionally non-contiguous and
  reflect the retained study allocation.
- Case-folder numbers are local construction identifiers rather than globally
  meaningful clinical identifiers. Model loaders zero or exclude the node-level
  `case_id` field before predictive use.

---

## Original datasets and reconstruction inputs

Original videos and annotations remain governed by their providers. They must be
obtained independently and used under the current access, license, citation, and
redistribution conditions stated on the official pages below.

The release names in this table are intentional. In particular,
`m2cai16-workflow` and `m2cai16-tool-locations` are different resources:
the former supplies workflow videos and phase annotations for S-STKG
construction, whereas the latter supplies spatial tool annotations for
Stage-1 node-detector initialization.

| Provider release used | Reconstruction dependency | Official access page | Required dataset citation |
|---|---|---|---|
| **Cholec80** | Authorized videos; phase annotations for S-STKG construction; locally prepared detector annotations for notebook 01 | [CAMMA datasets](https://camma.unistra.fr/datasets/) | [[1]](#dataset-ref-1) |
| **m2cai16-workflow** | Authorized workflow videos and phase annotations used in the joint source and same-ontology S-STKG cohorts | [CAMMA datasets](https://camma.unistra.fr/datasets/) | [[1]](#dataset-ref-1), [[2]](#dataset-ref-2) |
| **m2cai16-tool-locations** | Spatial tool-localization annotations used for Stage-1 node-detector initialization | [Official Stanford release](https://ai.stanford.edu/~syyeung/tooldetection.html) | [[1]](#dataset-ref-1), [[7]](#dataset-ref-7) |
| **CholecTrack20** | Retained video frames and matching annotation JSON files | [Official CholecTrack20 repository](https://github.com/CAMMA-public/cholectrack20) | [[3]](#dataset-ref-3) |
| **CholecT50** | Retained video frames and per-video annotation JSON files containing native phase labels | [Official CholecT50 repository](https://github.com/CAMMA-public/cholect50) | [[4]](#dataset-ref-4) |
| **MultiBypass140** | Retained full videos and matching phase JSON files | [Official MultiBypass140 repository](https://github.com/CAMMA-public/MultiBypass140) | [[5]](#dataset-ref-5) |
| **AutoLaparo** | Retained full videos and workflow phase-label files | [Official AutoLaparo website](https://autolaparo.github.io/) | [[6]](#dataset-ref-6) |

The official provider pages are the authoritative source for current access and
license conditions. When a provider does not publish a semantic version number,
record the exact release or variant name, archive filename, download date, and
SHA-256 checksum in the local experiment record rather than inventing a version
label.

For CholecTrack20 and CholecT50, the DMET-SSTKG cohort is determined by the
retained originating video identities listed in the overlap-control section
below; provider train/validation/test split files do not define the released
10-procedure DMET cohorts.

### Node-detector input boundary

Notebook 01 uses three user-configured paths:

```python
M2CAI_TOOL_YAML
COMBINED_DETECTOR_DATASET
OUTPUT_ROOT
```

`M2CAI_TOOL_YAML` must point to a local YOLO-format
`m2cai16-tool-locations` dataset configuration.

`COMBINED_DETECTOR_DATASET` is a **locally prepared input**, not a separately
downloadable provider release. Notebook 01 does not download raw datasets or
perform the original raw-data merge. It expects an existing YOLO dataset with
the following structure:

```text
COMBINED_DETECTOR_DATASET/
├── images/
│   ├── train/
│   ├── val/
│   └── test/
└── labels/
    ├── train/
    ├── val/
    └── test/
```

The source detector labels use nine class IDs: seven instrument classes plus
gallbladder and liver. Notebook 01 copies this prepared dataset, maps those
classes to the eight Role8 functional–anatomical classes, performs
M2CAI-initialized detector training, and produces the final node-detector
checkpoint.

Because the original videos and provider-governed annotations are not
redistributed, users rebuilding notebook 01 must construct this combined
YOLO-format input from their own authorized data. Users interested only in the
DMET-SSTKG graph, motif, hub, and remote-evaluation pipeline should use the
released S-STKG route and begin at notebook 08.

---

## Cholecystectomy-family overlap control

The retained cohorts were selected at the level of originating surgical
procedures rather than repository-local case-folder numbers. This record is
provided for reconstruction and leakage auditing; it is not required when using
the released S-STKG folders.

| Dataset | Retained originating case/video identities |
|---|---|
| **Cholec80** [[1]](#dataset-ref-1) | 1, 5, 6, 8, 9, 11, 12, 15, 16, 25, 28, 30, 34, 40, 45 |
| **CholecTrack20** [[3]](#dataset-ref-3) | 2, 4, 13, 17, 23, 31, 37, 96, 103, 110 |
| **CholecT50** [[4]](#dataset-ref-4) | 10, 27, 32, 48, 50, 56, 62, 68, 70, 80 |

The 15 Cholec80 identities cover all retained Cholec80 procedures across the
source-development and same-ontology remote cohorts. CholecTrack20 and CholecT50
values denote originating video identities rather than the local sequential
numbers assigned to the released DMET subsets.

M2CAI16 procedures were also kept case-disjoint across source-development and
same-ontology remote allocations, but they are not represented by the
Cholec80-origin index scheme above.

Case and original-video identifiers are used only for cohort construction,
leakage auditing, and reproducibility. They are not predictive inputs, hub
fields, or remote-consultation payload fields.

---

## S-STKG file format

Each phase-aligned fragment is represented by four matching text files:

```text
<fragment>_nodes.txt
<fragment>_edges.txt
<fragment>_edge_features.txt
<fragment>_labels.txt
```

### Node file

```text
x,y,w,h,vx,vy,class,case_id,g_ratio,p_ratio,isV
```

| Field | Meaning |
|---|---|
| `x, y, w, h` | Normalized bounding-box geometry |
| `vx, vy` | Instantaneous centroid displacement |
| `class` | Functional–anatomical node class |
| `case_id` | Local bookkeeping identifier, zeroed before model input |
| `g_ratio` | Global procedure-progress feature |
| `p_ratio` | Phase-relative progress feature |
| `isV` | Virtual-node indicator |

Physical detector outputs use Role8 class IDs `0–7`:

| ID | Meaning |
|---:|---|
| 0 | energy / coagulation |
| 1 | clipping / control |
| 2 | manipulation / traction |
| 3 | dissection / energy |
| 4 | irrigation / cleaning |
| 5 | cutting |
| 6 | specimen handling |
| 7 | anatomy / context |

Class `8` is reserved for the programmatic virtual-context node
(`isV=1`). It is not a detector prediction. Each sampled frame contains one
virtual-context node connected bidirectionally to the physical nodes.

### Edge file

```text
g_ratio,p_ratio,src,dst,type
```

`type` is either `physical` or `virtual`.

### Edge-feature file

```text
g_ratio,p_ratio,src,dst,dist,rel_dist,iou
```

Physical edges store centroid distance, object-scale-normalized distance, and
bounding-box intersection-over-union. Virtual edges use neutral zero-valued
geometric descriptors.

### Label file

```text
frame_idx,phase_id
```

Cross-dataset and external cohorts retain their native phase IDs during S-STKG
construction. Phase-space restriction and external source-counterpart projection
are applied only in the relevant evaluation notebook; released labels are not
rewritten into the source phase space.

---

## Notebook guide

### Node detector

| No. | Notebook | Purpose | Main input | Main output |
|---:|---|---|---|---|
| 01 | `01_visual_to_graph_node_extraction_audit.ipynb` | Maps the prepared detector dataset to Role8, performs `m2cai16-tool-locations` initialization and Role8 fine-tuning, evaluates the final detector, and reproduces the node-extraction audit | `m2cai16-tool-locations`, prepared combined YOLO detector data, writable output root | Final Role8 node-detector checkpoint and detector-audit outputs |

Notebook 01 is stored separately under `Codes/Node_Detector/` because it trains
the visual node extractor used by the S-STKG construction notebooks.

### S-STKG construction

| No. | Notebook | Purpose | Main input | Main output |
|---:|---|---|---|---|
| 02 | `02_sstkg_construction_source_cholec80_m2cai.ipynb` | Builds source-development S-STKGs and performs the source edge-threshold audit | Authorized Cholec80/M2CAI16 workflow videos, phase annotations, node detector | `SurgicalSTKG_Local_Cholec80_M2CAI`-style data |
| 03 | `03_sstkg_construction_same_ontology_cholec80_m2cai_remote.ipynb` | Builds the same-ontology remote Cholec80/M2CAI16 workflow cohort | Retained videos, phase annotations, node detector | `SurgicalSTKG_Remote_CHOLEC80_M2CAI`-style data |
| 04 | `04_sstkg_construction_cholectrack20_remote.ipynb` | Builds CholecTrack20 S-STKGs while retaining native phase IDs | Retained frame directories and matching JSON annotations | `CholecTrack20_STKG`-style data |
| 05 | `05_sstkg_construction_cholect50_remote.ipynb` | Builds CholecT50 S-STKGs with the study frame-stride configuration | Retained frame directories and matching annotation JSON files | `CholecT50_STKG`-style data |
| 06 | `06_sstkg_construction_multibypass140_external.ipynb` | Builds MultiBypass140 S-STKGs for external source-counterpart analysis | Retained full videos and phase JSON files | `MultiBypass140_STKG`-style data |
| 07 | `07_sstkg_construction_autolaparo_external.ipynb` | Builds AutoLaparo S-STKGs while retaining native transitions | Retained full videos and phase-label files | `AutoLaparo_STKG`-style data |

Notebooks 02–07 are needed only when rebuilding derived graphs from authorized
provider data. Users of the released `S-STKG_Data/` folders can start at
notebook 08.

### Source representation and hub construction

| No. | Notebook | Purpose | Required input | Important output |
|---:|---|---|---|---|
| 08 | `08_source_sstkg_phase_classifier.ipynb` | Trains the source S-STKG phase classifier with the saved case-disjoint 14/3/3 split | Source S-STKG folder | `best_sstkg_phase_classifier.pt`, `data_split.npz`, `split_cases.json` |
| 09 | `09_phase_initialized_sstkg_motif_extractor.ipynb` | Initializes the motif extractor from the phase checkpoint and trains the final 128-D motif encoder | Source S-STKG, phase checkpoint, saved split | `best_phase_initialized_motif_extractor.pt`, copied `data_split.npz` |
| 10 | `10_source_validation_motif_loss_sensitivity_audit.ipynb` | Reproduces the source-validation one-factor-at-a-time audit for the retained motif-loss coefficients | Source S-STKG, phase checkpoint, saved split | Trial tables, selected settings, and sensitivity curves |
| 11 | `11_source_motif_memory_dirichlet_conformal_hub.ipynb` | Builds stable anchors, phase prototypes, Dirichlet calibration, and the split-conformal APS threshold | Source S-STKG, motif checkpoint, saved split | `Shareable_Motif_Hub_DIRICHLET_CONFORMAL_PROTOTYPE.pt` and hub-audit outputs |

### Remote, external, ablation, and comparative evaluation

| No. | Notebook | Question | Required input | Main output |
|---:|---|---|---|---|
| 12 | `12_same_ontology_cholec80_m2cai_remote_update.ipynb` | Same-ontology uncertainty-selected motif consultation and leave-one-case-out remote update | Same-ontology remote S-STKG, motif checkpoint, hub | Remote-update and direct-consultation summaries |
| 13 | `13_cholectrack20_cross_dataset_remote_update_revised.ipynb` | CholecTrack20 cross-dataset remote update | CholecTrack20 S-STKG, motif checkpoint, hub | `CholecTrack20_RemoteUpdateSummary.csv` |
| 14 | `14_cholect50_cross_dataset_remote_update.ipynb` | CholecT50 cross-dataset remote update | CholecT50 S-STKG, motif checkpoint, hub | `CholecT50_RemoteUpdateSummary.csv` |
| 15 | `15_multibypass140_external_source_counterpart_evaluation.ipynb` | MultiBypass140 external source-counterpart stress test | MultiBypass140 S-STKG, motif checkpoint, hub | `MultiBypass140_ExternalStressTestSummary.csv` |
| 16 | `16_autolaparo_external_source_counterpart_evaluation.ipynb` | AutoLaparo external source-counterpart stress test | AutoLaparo S-STKG, motif checkpoint, hub | `AutoLaparo_ExternalStressTestSummary.csv` |
| 17 | `17_cholec80_m2cai_objective_ablation.ipynb` | Isolated 2×2 hub-target / update-objective ablation | Same-ontology remote S-STKG, motif checkpoint, hub | `Cholec80_M2CAI_RemoteAblation.csv` with paired surgical-case cluster-bootstrap intervals |
| 18 | `18_cholec80_m2cai_decentralized_comparative_analysis.ipynb` | Comparison with decentralized reference methods on the same-ontology remote cohort | Source and remote caches, motif-run split and checkpoint, hub | Comparative performance, communication payload, and seed-sensitivity outputs |

---

## Recommended execution order

### Route A — released S-STKG data

This is the shortest route for reproducing the modeling and evaluation pipeline:

```text
08 Source phase classifier
   ├── 10 Motif-loss sensitivity audit
   │      (supporting audit; optional when retained coefficients are used)
   │
   └── 09 Phase-initialized motif extractor
           └── 11 Source motif memory and calibrated hub
                   ├── 12 Same-ontology remote update
                   │       ├── 17 Remote-update ablation
                   │       └── 18 Decentralized comparative analysis
                   ├── 13 CholecTrack20 remote update
                   ├── 14 CholecT50 remote update
                   ├── 15 MultiBypass140 external evaluation
                   └── 16 AutoLaparo external evaluation
```

For the reported final configuration, notebook 09 can run directly after
notebook 08 with the retained coefficients. Run notebook 10 between notebooks
08 and 09 only when repeating coefficient selection itself.

### Route B — reconstruction from authorized data

```text
Provider-authorized datasets
   ├── prepared node-detector inputs
   │      └── 01 Role8 node detector and audit
   │
   └── dataset-specific videos and phase annotations
          └── 02–07 S-STKG construction
                 └── continue with Route A from notebook 08
```

Route B is not a provider-data downloader. Notebook 01 requires the prepared
combined YOLO detector input described above, and notebooks 02–07 require the
authorized video/annotation layouts documented in their path cells.

---

## Artifact handoff between notebooks

Use artifacts from the same compatible run chain.

| Producing stage | Artifact | Consuming notebooks |
|---|---|---|
| Notebook 08 | `best_sstkg_phase_classifier.pt` | 09 and 10 |
| Notebook 08 | `data_split.npz`, `split_cases.json` | 09 and 10 |
| Notebook 09 | `best_phase_initialized_motif_extractor.pt` | 11–18 |
| Notebook 09 | copied `data_split.npz` | 11 and 18 |
| Notebook 11 | `Shareable_Motif_Hub_DIRICHLET_CONFORMAL_PROTOTYPE.pt` | 12–18 |
| Notebook 12 cache stage | same-ontology remote binary cache | 12, 17, and 18 |

A typical selected phase run contains:

```text
outputs/phase_classifier_runs/<selected_phase_run>/
├── best_sstkg_phase_classifier.pt
├── data_split.npz
└── split_cases.json
```

A typical selected motif run contains:

```text
outputs/motif_extractor_runs/<selected_motif_run>/
├── best_phase_initialized_motif_extractor.pt
└── data_split.npz
```

For notebooks 11 and 18, select the split and motif checkpoint from the **same
notebook-09 run directory**. The copied split must remain aligned with the
source cache and fragment ordering inherited from notebook 08.

Remote notebooks may point directly to the selected motif run and hub output.
Alternatively, local copies can be placed under:

```text
artifacts/
├── best_phase_initialized_motif_extractor.pt
└── Shareable_Motif_Hub_DIRICHLET_CONFORMAL_PROTOTYPE.pt
```

---

## Cache and split consistency

1. Reuse the same source cache across notebooks 08–11 and the source side of
   notebook 18.
2. Do not add, remove, rename, or reorder source S-STKG fragments after
   `data_split.npz` has been created.
3. Use the `data_split.npz` copied into the same notebook-09 run directory as
   the selected motif checkpoint.
4. Set `FORCE_REPROCESS_CACHE=True` only when the underlying S-STKG files or
   feature interface have changed.
5. When rebuilding a changed source cache, rerun notebook 08 and regenerate the
   split before continuing.
6. Use a separate cache directory for each remote or external cohort.

These rules prevent a saved index split from being applied to a different
fragment ordering.

---

## Reproducibility and interpretation boundaries

- The repository distributes derived S-STKG fragments, not original surgical
  videos.
- The released fragments are annotation-defined and phase-aligned. The study
  evaluates fragment-level phase evidence and remote consultation; it does not
  evaluate online phase-boundary detection in unsegmented full videos.
- Source training, validation, and source-test procedures are case-disjoint.
  Remote cohorts are retained separately from source development.
- Source-test and remote evaluation labels are not used to fit source
  prototypes, Dirichlet calibration, or the conformal APS threshold.
- Dataset-native phase IDs are retained during graph construction. Cross-dataset
  phase restriction and external source-counterpart projection occur only in
  the relevant evaluation notebook.
- External source-counterpart mappings are fixed functional evaluation
  projections. They do not imply anatomical identity or clinically validated
  phase equivalence.
- DMET-SSTKG direct consultation exchanges a 128-dimensional float32 motif query
  (512 bytes) and an eight-class float16 hub response (16 bytes), giving a
  logical payload of 528 bytes per query. Transport, encryption, latency, and
  systems overhead are excluded. The one-time compact hub artifact is reported
  separately from query–response payload.
- Notebook 18 applies the same logical accounting boundary to each reference
  method using the protocol-specific objects exchanged by its implementation.
- The framework is data-minimizing: direct consultation does not exchange raw
  videos, model weights, gradients, logits, checkpoints, or proxy models. This
  design does not constitute a formal differential-privacy or cryptographic
  guarantee.

---

## Common problems

### `FileNotFoundError` at notebook start

Launch Jupyter from the repository root and update the notebook's path cell.
Released graph folders are located under `S-STKG_Data/`; original provider data
are not downloaded automatically.

### Notebook 01 cannot find detector data

Check all three configured paths:

```text
M2CAI_TOOL_YAML
COMBINED_DETECTOR_DATASET
OUTPUT_ROOT
```

`COMBINED_DETECTOR_DATASET` must already contain YOLO `images/` and `labels/`
directories for `train`, `val`, and `test`.

### Saved split is incompatible with the source cache

The cache contents or ordering changed after the split was created. Rebuild the
affected source cache, rerun notebook 08, and use the newly generated phase and
motif run chain.

### A remote notebook cannot find the motif checkpoint or hub

Run notebooks 09 and 11 first, then update `MOTIF_CHECKPOINT` and `HUB_FILE`, or
place local copies under `artifacts/`.

### Notebook 18 cannot find a required artifact

Confirm that the following are available and mutually compatible:

```text
source binary cache
same-ontology remote binary cache
motif-run data_split.npz
motif-run checkpoint
Dirichlet-conformal hub
```

### PyTorch Geometric installation fails

Install the PyTorch build for the local CPU/CUDA environment first, then install
the corresponding PyTorch Geometric packages. Platform-specific wheels may be
required.

---

## Dataset references

<a id="dataset-ref-1"></a>**[1]** Twinanda, A. P., Shehata, S., Mutter, D.,
Marescaux, J., de Mathelin, M., and Padoy, N. EndoNet: A deep architecture
for recognition tasks on laparoscopic videos. *IEEE Transactions on Medical
Imaging* **36**(1), 86–97 (2017).
https://doi.org/10.1109/TMI.2016.2593957

<a id="dataset-ref-2"></a>**[2]** Stauder, R., Ostler, D., Kranzfelder, M.,
Koller, S., Feußner, H., and Navab, N. The TUM LapChole dataset for the
M2CAI 2016 workflow challenge. *CoRR* abs/1610.09278 (2016).
https://doi.org/10.48550/arXiv.1610.09278

<a id="dataset-ref-3"></a>**[3]** Nwoye, C. I., Elgohary, K., Srinivas, A.,
Zaid, F., Lavanchy, J. L., and Padoy, N. CholecTrack20: A multi-perspective
tracking dataset for surgical tools. In: *Proceedings of the IEEE/CVF
Conference on Computer Vision and Pattern Recognition (CVPR)*, 8942–8952
(2025).

<a id="dataset-ref-4"></a>**[4]** Nwoye, C. I., Yu, T., Gonzalez, C.,
Seeliger, B., Mascagni, P., Mutter, D., Marescaux, J., and Padoy, N.
Rendezvous: Attention mechanisms for the recognition of surgical action
triplets in endoscopic videos. *Medical Image Analysis* **78**, 102433
(2022). https://doi.org/10.1016/j.media.2022.102433

<a id="dataset-ref-5"></a>**[5]** Lavanchy, J. L., Ramesh, S., Dall'Alba,
D., Gonzalez, C., Fiorini, P., Müller-Stich, B. P., Nett, P. C.,
Marescaux, J., Mutter, D., and Padoy, N. Challenges in multi-centric
generalization: Phase and step recognition in Roux-en-Y gastric bypass
surgery. *International Journal of Computer Assisted Radiology and Surgery*
**19**, 2249–2257 (2024).
https://doi.org/10.1007/s11548-024-03166-3

<a id="dataset-ref-6"></a>**[6]** Wang, Z., Lu, B., Long, Y., Zhong, F.,
Cheung, T.-H., Dou, Q., and Liu, Y. AutoLaparo: A new dataset of integrated
multi-tasks for image-guided surgical automation in laparoscopic hysterectomy.
In: *Medical Image Computing and Computer Assisted Intervention – MICCAI
2022*, 486–496 (2022).
https://doi.org/10.1007/978-3-031-16449-1_46

<a id="dataset-ref-7"></a>**[7]** Jin, A., Yeung, S., Jopling, J.,
Krause, J., Azagury, D., Milstein, A., and Fei-Fei, L. Tool detection and
operative skill assessment in surgical videos using region-based convolutional
neural networks. In: *2018 IEEE Winter Conference on Applications of Computer
Vision (WACV)*, 691–699 (2018).
https://doi.org/10.1109/WACV.2018.00081
