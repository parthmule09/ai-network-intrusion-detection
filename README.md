# AI-Based Network Intrusion Detection & Cybersecurity Analytics

A complete end-to-end data analytics and machine learning project that builds an
AI-powered binary classifier to distinguish normal network traffic from cyberattacks,
using the UNSW-NB15 benchmark dataset.

---

## Project Overview

Modern networks generate enormous volumes of traffic that cannot be manually inspected
for threats. This project applies machine learning — specifically a Random Forest classifier —
to network connection records, automatically learning which traffic patterns indicate
intrusion attempts. The pipeline covers data loading, quality audit, cleaning, exploratory
analysis, feature engineering, model training, rigorous evaluation, and actionable insights.

---

## Problem Statement

Organisations face a constant stream of network intrusion attempts including DoS attacks,
reconnaissance scans, exploitation, backdoors, and worm propagation. Traditional
signature-based intrusion detection systems miss novel attacks. An AI-based approach that
learns statistical patterns from labelled traffic data can generalise to unseen threats,
reduce analyst workload, and lower mean-time-to-detect (MTTD).

---

## Objectives

- Determine the proportion of normal vs attack traffic in the UNSW-NB15 dataset
- Identify which attack categories occur most frequently
- Discover which protocols and services are associated with malicious activity
- Understand which traffic features differ most between normal and attack connections
- Engineer meaningful derived features from raw connection attributes
- Train a Random Forest classifier for binary intrusion detection
- Evaluate the model using Accuracy, Precision, Recall, F1 Score, and ROC-AUC
- Extract feature importances and interpret them in a cybersecurity context

---

## Dataset

**UNSW-NB15**  
Source: https://www.kaggle.com/datasets/dhoogla/unswnb15/versions/5

The UNSW-NB15 dataset was created at the University of New South Wales using the IXIA
PerfectStorm tool to generate a mix of real normal network activity and nine contemporary
attack families (DoS, Fuzzers, Analysis, Backdoors, Exploits, Reconnaissance, Shellcode,
Worms, Generic). It provides 175,341 training records and a separate test set, each with
36 features covering:

- **Connection metadata**: duration (`dur`), protocol (`proto`), service, state
- **Volume metrics**: source/dest bytes (`sbytes`, `dbytes`), packet counts (`spkts`, `dpkts`)
- **Rate/load metrics**: `rate`, `sload`, `dload`, `sinpkt`, `dinpkt`
- **TCP features**: `swin`, `dwin`, `tcprtt`, `synack`, `ackdat`, `stcpb`, `dtcpb`
- **Statistical metrics**: `sjit`, `djit`, `smean`, `dmean`, `sloss`, `dloss`
- **Application-layer**: `trans_depth`, `response_body_len`, `ct_flw_http_mthd`
- **FTP/service flags**: `is_ftp_login`, `ct_ftp_cmd`, `is_sm_ips_ports`
- **Connection count features**: `ct_src_dport_ltm`, `ct_dst_sport_ltm`
- **Target**: `label` (0 = Normal, 1 = Attack), `attack_cat` (attack category)

The dataset files are provided in Parquet format:
- `UNSW_NB15_training-set.parquet`
- `UNSW_NB15_testing-set.parquet`

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Python 3.9+ | Primary language |
| Pandas | Data loading, wrangling, analysis |
| NumPy | Numerical operations |
| Matplotlib | Chart rendering |
| Seaborn | Statistical visualisations |
| Scikit-learn | Preprocessing pipeline, Random Forest, evaluation metrics |
| PyArrow | Parquet file reading |
| Jupyter Notebook | Interactive development and presentation |

---

## Methodology

1. **Data Loading** — Flexible file-search logic locates the Parquet files across Kaggle,
   VS Code, IBM Bob, and local environments; clear error messages guide the user if files
   are missing.

2. **Data Quality Audit** — Missing values, duplicate rows, data types, unique value counts,
   target distribution, and descriptive statistics are all summarised.

3. **Data Cleaning** — Duplicates removed; infinite values replaced; columns with >80%
   missing data dropped; numeric NaNs filled with column median; categorical NaNs filled
   with `'Unknown'`; string values normalised to lowercase.

4. **Exploratory Data Analysis** — Six visualisations covering traffic distribution, attack
   categories, protocol and service breakdown, feature distributions across label classes,
   and a full correlation heatmap.

5. **Feature Engineering** — Six derived features created:
   `total_bytes`, `total_pkts`, `bytes_per_pkt`, `src_dst_byte_ratio`,
   `load_diff`, `tcp_handshake_complete`.

6. **ML Problem Definition** — Binary classification on `label` (0/1); `attack_cat`
   excluded to prevent target leakage; no raw identifiers in feature set.

7. **Preprocessing Pipeline** — `ColumnTransformer` with separate numeric
   (`SimpleImputer → StandardScaler`) and categorical
   (`SimpleImputer → OneHotEncoder(handle_unknown='ignore')`) pipelines.

8. **Random Forest** — `RandomForestClassifier(n_estimators=200, class_weight='balanced',
   random_state=42, n_jobs=-1)` trained inside a full scikit-learn `Pipeline`.

9. **Evaluation** — Accuracy, Precision, Recall, F1, ROC-AUC, classification report,
   normalised and raw confusion matrices, ROC curve.

10. **Feature Importance** — Top 20 transformed features extracted and visualised as a
    horizontal bar chart with cybersecurity interpretation.

11. **Attack Category Analysis** — Descriptive breakdown of attack types with frequency
    and percentage contribution.

12. **Insights** — Data-driven summary generated programmatically from actual executed
    results.

---

## Machine Learning

| Item | Detail |
|---|---|
| Task | Binary classification |
| Target variable | `label` (0 = Normal, 1 = Attack) |
| Key feature groups | Volume (bytes, packets), rate/load, TCP handshake, jitter, connection counts |
| Algorithm | Random Forest (200 trees, balanced class weights) |
| Evaluation metrics | Accuracy, Precision, Recall, F1 Score, ROC-AUC |

---

## How to Run

### 1. Download the dataset

Visit the Kaggle page and download version 5:  
https://www.kaggle.com/datasets/dhoogla/unswnb15/versions/5

### 2. Place the dataset files

Create a folder `UNSW-NB15` in the same directory as the notebook and place both files
inside it:

```
AI-Based-Network-Intrusion-Detection/
├── Indian_Network_Intrusion_Detection.ipynb
├── requirements.txt
├── README.md
├── Project_Report_Network_Intrusion_Detection.docx
└── UNSW-NB15/
    ├── UNSW_NB15_training-set.parquet
    └── UNSW_NB15_testing-set.parquet
```

Alternatively, place the `.parquet` files in the same folder as the notebook.

### 3. Install requirements

```bash
pip install -r requirements.txt
```

### 4. Open the notebook

```bash
jupyter notebook Indian_Network_Intrusion_Detection.ipynb
```

### 5. Run all cells

Use **Kernel → Restart & Run All** to execute the notebook from top to bottom.  
No manual code edits are required.

---

## Expected Outputs

Running the notebook produces:

- **EDA charts** — traffic distribution, attack category breakdown, protocol and service
  analysis, feature box plots, correlation heatmap
- **Attack distribution** — exact counts and percentages for normal vs attack traffic
- **Confusion matrix** — raw counts and normalised visualisation
- **Classification metrics** — accuracy, precision, recall, F1, ROC-AUC
- **ROC curve** — with AUC score
- **Feature importance** — top 20 features ranked by mean decrease in impurity
- **Sample predictions** — 10 test records with actual label, predicted label, and
  probability score

---

## Important Note

All model metrics, statistics, and visualisations are **generated dynamically** from the
dataset during notebook execution. No results are pre-filled or fabricated.
The notebook must be run with the actual dataset files to produce valid outputs.

---

## Project Files

| File | Description |
|---|---|
| `Indian_Network_Intrusion_Detection.ipynb` | Main Jupyter Notebook (run this) |
| `requirements.txt` | Python library dependencies |
| `README.md` | This documentation file |
| `Project_Report_Network_Intrusion_Detection.docx` | Professional project report |
