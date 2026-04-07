# 🌍 Global Development Clustering Dashboard

> **Can Machine Learning group every country in the world by development level — without any labels?**
> This project answers that question using unsupervised learning on real-world socio-economic data.

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://globalclusteringdashboard-nnmmvegwyphy52cefdwh3a.streamlit.app)

---

## 📌 Project Overview

Every country in the world sits somewhere on the development spectrum — but where exactly? And what features define that position?

This end-to-end machine learning project analyzes **2,700+ socio-economic records across 190+ countries** — covering features like GDP, birth rate, CO2 emissions, life expectancy, computer usage, health expenditure, and 15+ more indicators — to discover natural groupings in global development **without any predefined labels**.

The result? Three distinct clusters that aligned perfectly with real-world development tiers — discovered entirely by the data itself.

---

## 🔗 Live Dashboard

👉 **[Explore the Dashboard](https://globalclusteringdashboard-nnmmvegwyphy52cefdwh3a.streamlit.app)**

The interactive Streamlit dashboard lets you:
- Visualize all 190+ countries on a color-coded world map
- Explore cluster assignments on interactive scatter plots
- Drill into any specific country and see its development profile
- Compare countries across features within and across clusters

---

## 🏆 Results

The K-Means model discovered **3 meaningful development clusters**:

| Cluster | Name | Characteristics |
|---|---|---|
| 🟠 Cluster 0 | **Rising Nations** | High birth rates, lower GDP, developing infrastructure |
| 🔵 Cluster 1 | **Advanced Nations** | Moderate indicators, growing economies |
| 🟢 Cluster 2 | **Global Leaders** | High GDP, low birth rates, high life expectancy |

**No labels were given. The model figured this out from the numbers alone.**

---

## 📊 Model Evaluation

Five clustering algorithms were tested and evaluated on **two datasets** — raw cleaned data and averaged country-level data:

| Model | Silhouette ↑ | DBI ↓ | Notes |
|---|---|---|---|
| **K-Means** | **0.364** | **0.896** | ✅ Best overall — selected for deployment |
| Agglomerative | 0.360 | 0.901 | Very close to K-Means, slightly weaker DBI |
| Gaussian Mixture | ~0.35 | ~0.95 | Soft boundaries not ideal for segmentation |
| Bisecting K-Means | ~0.35 | ~0.95 | Similar to K-Means, less interpretable |
| DBSCAN | 0.636* | 0.500* | *Excluded 157+ countries as noise — unusable |

> 💡 **Key Insight:** DBSCAN achieved the highest silhouette score on valid clusters — but classified 157+ countries as noise, making it completely unusable for a country-level dashboard. This taught me that metrics don't tell the full story. Practical usability matters just as much as mathematical performance.

---

## 🔍 Key Challenges & Solutions

### Challenge 1 — Messy Money Columns
GDP, Health Expenditure, and Tourism columns were stored with `$` symbols and commas (e.g., `$1,234,567`). When loaded into Pandas, these read as strings — breaking all numeric operations.

**Solution:** Built a custom cleaning function to strip `$`, remove commas, handle whitespace, and convert to numeric.

### Challenge 2 — Multiple Entries Per Country
The dataset had multiple rows per country — likely from different years. Using all rows would give some countries more statistical weight than others, distorting the clustering.

**Solution:** Engineered an averaged dataset — grouped by unique country name and computed the mean of all features across entries. This gave one clean, representative row per country.

### Challenge 3 — Feature Scale Differences
Features like GDP range in the millions while Birth Rate ranges 0–50. K-Means is distance-based, so unscaled features would cause high-range features to dominate.

**Solution:** Applied `StandardScaler` before clustering to normalize all features to mean=0 and std=1.

---

## ⚙️ Technical Pipeline

```
Raw Excel Data
      ↓
Data Loading & Inspection
      ↓
Cleaning (money columns, data types, null handling)
      ↓
Dropping high-null columns (e.g., Ease of Business — 93% null)
      ↓
Median Imputation for remaining nulls
      ↓
Outlier Analysis (IQR method)
      ↓
Exploratory Data Analysis (distributions, correlations, bivariate analysis)
      ↓
Feature Engineering (country-level averaging)
      ↓
StandardScaler normalization
      ↓
Elbow Method → K=3 selected
      ↓
5 Models × 2 Datasets → Silhouette + DBI evaluation
      ↓
K-Means on avg_countries selected as final model
      ↓
Streamlit Dashboard → Deployed
```

---

## 📁 Repository Structure

```
Global_Clustering_Dashboard/
│
├── 📁 notebooks/
│     ├── Global_Clustering_EDA.ipynb          # Exploratory Data Analysis
│     └── Global_Clustering_Modeling.ipynb     # Model building & evaluation
│
├── 📁 dashboard/
│     └── dashboard_global_clustering.py       # Streamlit app
│
├── 📁 data/
│     ├── World_development_mesurement.xlsx    # Raw dataset
│     └── cleaned_Global_clustering.csv        # Cleaned & processed data
│
├── 📁 docs/
│     └── Global_Development_Cluster_Analysis.pptx  # Project presentation
│
└── README.md
```

---

## 🛠 Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3.x |
| Data Processing | Pandas, NumPy |
| Machine Learning | Scikit-learn (KMeans, DBSCAN, AgglomerativeClustering, GaussianMixture, BisectingKMeans) |
| Evaluation | Silhouette Score, Davies-Bouldin Index |
| Visualization | Matplotlib, Seaborn, Plotly |
| Deployment | Streamlit |
| Environment | Google Colab, VS Code |

---

## 📈 Dataset

- **Source:** World Development Measurement Dataset
- **Records:** 2,705+ rows
- **Features:** 20+ socio-economic indicators
- **Coverage:** 190+ countries
- **Key Features:** GDP, Birth Rate, CO2 Emissions, Life Expectancy (Male & Female), Health Expenditure per Capita, Computer Usage, Internet Usage, Tourism Inbound/Outbound, and more

---

## 💡 What I Learned

1. **Real-world data is messy** — dollar signs in numeric columns, inconsistent formats, and missing values are the norm, not the exception.
2. **Metrics don't tell the full story** — DBSCAN had a better silhouette score, but excluded 157+ countries as noise. Always evaluate models for practical usability, not just numbers.
3. **Feature engineering matters** — averaging multi-year country entries improved clustering quality across all models.
4. **Scale before clustering** — K-Means is distance-based. Without StandardScaler, high-range features dominate and distort results completely.

---

## 🚀 How to Run Locally

```bash
# Clone the repository
git clone https://github.com/venkystech/Global_Clustering_Dashboard.git

# Navigate to project
cd Global_Clustering_Dashboard

# Install dependencies
pip install -r requirements.txt

# Run the dashboard
streamlit run dashboard/dashboard_global_clustering.py
```

---

## 👨‍💻 Author

**Avuluri Venkatesh**
Data Science Intern @ AiVariant | MCA Student

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://linkedin.com/in/your-profile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black)](https://github.com/venkystech)

---

*Built during my internship at AiVariant. Every design choice — from scaling to model selection — impacted the outcome.*
