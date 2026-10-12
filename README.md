# Hospital Infection Control: Predictive Network Modeling

🏥 **Predicting downstream hospital infection clusters using patient transfer networks, NetworkX, and Scikit-Learn.**

---

## 📌 Project Overview
This repository contains a predictive modeling project designed to evaluate whether patient transfer dynamics within a regional hospital group can help forecast future infection outbreaks. 

Using a directed, weighted graph representation of ward-to-ward movements alongside traditional baseline attributes, this project investigates if network exposure metrics provide an early warning indicator for downstream infection clusters among eligible wards.

---

## 🔬 Methodology & Core Architecture

The project is structured into three main phases:

### 1. Exploratory Data Analysis (EDA)
*   Filters out wards already affected at the starting prediction point.
*   Analyzes the baseline attributes: `occupancy_ratio` and `device_use_share`.
*   Visualizes the distributional overlap using boxplots to determine if baseline attributes alone can cleanly segment the future target class (`new_cluster_later`).

### 2. Network Construction & Graph Feature Engineering
*   **Graph Formulation:** Constructs a directed network (G) using `NetworkX`, where nodes represent individual wards and directed edges signify patient flows.
*   **Edge Weighting:** Incorporates `transfer_count` as link weights representing movement volume over a 14-day observation window.
*   **Feature Extraction:** Evaluates two critical graph-theoretic metrics for every node:
    *   `incoming_volume`: The total volume of incoming transfers received by a ward.
    *   `from_active_volume`: The targeted influx coming exclusively from source wards with active clusters at the time of prediction.

### 3. Pipeline & Predictive Evaluation
*   Implements a standardized machine learning pipeline via `Scikit-Learn`: `StandardScaler` → `LogisticRegression`.
*   Enforces strict data hygiene by applying scaling transformations solely on training rows within a stratified train/test split (70/30).
*   Compares two distinct models:
    *   **Model A (Baseline):** Trained exclusively on original isolated ward attributes.
    *   **Model B (Network Enriched):** Integrates baseline features with `incoming_volume` and `from_active_volume`.
*   Evaluates predictive capacity using **Accuracy**, **F1-Score**, **ROC AUC**, and **Confusion Matrices**.

---

## 📊 Summary of Insights & Interpretations

*   **Baseline Limitations:** EDA shows that baseline attributes (`occupancy_ratio` and `device_use_share`) heavily overlap across future outcomes. They are insufficient on their own to establish a clean decision boundary.
*   **Network Predictive Power:** Incorporating directed patient flow metrics structurally boosts both **F1-Score** and **ROC AUC** metrics. This confirms that topological exposure to active infection sources is highly predictive of future cluster expansion.
*   **Directionality Matters:** Patient transfers are a one-way vector for spreading contagion. Total incoming volume acts as a proxy for ward traffic, whereas targeted influx from infected nodes specifically tracks contagion exposure. Outgoing transfers only affect receiving wards and do not alter the risk profile of the source node itself.

### ⚖️ Ethical & Operational Considerations
Deploying algorithmic flags to prioritize infection-control attention carries profound real-world consequences:
*   **False Negatives (Missed Clusters):** Threaten patient safety by leaving real outbreaks unchecked.
*   **False Positives (False Alarms):** Lead to staff burnout, wasted personal protective equipment (PPE), and unnecessary ward isolation protocols.
*   **Operational Protocol:** Automated models should never operate autonomously. Every flag must be reviewed and audited by an expert **clinical infection-control committee** to balance predictive alerts with medical intuition.

---

## 🛠️ Repository Structure

```text
├── hospital_infection_prediction.ipynb   # Main Jupyter Notebook
├── hospital_wards.csv                    # Synthetic ward-level attribute dataset
├── hospital_transfers.csv                # Directed patient transfer logs
└── README.md                             # Repository documentation
```

---

## 🚀 Getting Started

### Prerequisites
Ensure you have Python 3.8+ installed along with the following core dependencies:
```bash
pip install numpy pandas matplotlib networkx scikit-learn
```

### Running the Notebook
Clone this repository and open the interactive notebook file to execute the pipeline:
```bash
git clone https://github.com
cd hospital-infection-network
jupyter notebook hospital_infection_prediction.ipynb
```

---


