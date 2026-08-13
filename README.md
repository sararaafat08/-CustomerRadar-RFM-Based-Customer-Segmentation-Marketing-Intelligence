# 🎯 CustomerRadar — RFM-Based Customer Segmentation & Marketing Intelligence

A **customer segmentation project** using the RFM (Recency · Frequency · Monetary)
framework to classify customers into meaningful behavioral segments — then translate
each segment into a concrete, actionable **marketing strategy**.

This goes beyond just data analysis: it bridges the gap between raw transaction
data and real business decisions that marketing and CRM teams can act on immediately.

---

## 📁 Repository Structure

```
📦 CustomerRadar-RFM
 ┣ 📓 RFM.ipynb                   ← Full analysis notebook
 ┣ 📂 online_retail.csv           ← Source transaction dataset
 ┣ 📁 screenshots/
 ┃ ┣ 🖼️ segment_distribution.png
 ┃ ┣ 🖼️ avg_spending_by_segment.png
 ┃ ┣ 🖼️ monetary_distribution.png
 ┃ ┗ 🖼️ correlation_heatmap.png
 ┗ 📄 README.md
```

---

## 🔍 What is RFM Analysis?

RFM is a proven marketing framework that scores every customer on three dimensions:

| Dimension | Question It Answers | Higher = Better? |
|---|---|---|
| **R — Recency** | How recently did the customer buy? | ✅ Yes — recent buyers are more likely to buy again |
| **F — Frequency** | How often do they buy? | ✅ Yes — loyal buyers are more valuable |
| **M — Monetary** | How much do they spend? | ✅ Yes — high spenders drive revenue |

Each customer gets a score of **1–5** on each dimension (via quantile-based scoring),
combined into a **3-digit RFM Score** (e.g., `555` = perfect customer) that drives
segment classification.

---

## 📊 Project Workflow

### 1. 📥 Data Loading & Exploration
- Loaded `online_retail.csv` using Pandas
- Initial profiling: `.head()`, `.info()`, `.describe()`, `.isnull().sum()`

---

### 2. 🧹 Data Cleaning

| Step | Action | Reason |
|---|---|---|
| Drop nulls | `dropna(subset=['CustomerID'])` | Can't do customer analysis without a customer ID |
| Fix dates | `pd.to_datetime(df['InvoiceDate'])` | Required for Recency calculation |
| Remove cancellations | Filter out `InvoiceNo` starting with `'C'` | Cancelled orders shouldn't count as purchases |
| Remove returns | Keep only `Quantity > 0` | Negative quantities are returns, not sales |
| Create revenue column | `TotalPrice = Quantity × UnitPrice` | Base metric for Monetary score |

---

### 3. 📐 RFM Table Construction

```python
snapshot_date = df['InvoiceDate'].max() + pd.Timedelta(days=1)

rfm = df.groupby('CustomerID').agg({
    'InvoiceDate': lambda x: (snapshot_date - x.max()).days,  # Recency
    'InvoiceNo':   'nunique',                                  # Frequency
    'TotalPRice':  'sum'                                       # Monetary
})
rfm.columns = ['Recency', 'Frequency', 'Monetary']
```

---

### 4. 🎯 RFM Scoring (1–5 Quantile-Based)

| Score | Method | Logic |
|---|---|---|
| `R_score` | `pd.qcut(..., labels=[5,4,3,2,1])` | Lower recency (days) = higher score |
| `F_score` | `pd.qcut(rank(method='first'), labels=[1,2,3,4,5])` | Higher frequency = higher score (rank used to break ties) |
| `M_score` | `pd.qcut(..., labels=[1,2,3,4,5])` | Higher spending = higher score |
| `RFM_Score` | R + F + M concatenated as string | e.g., `'555'`, `'312'`, `'111'` |

---

### 5. 🏷️ Customer Segmentation

Six segments defined by custom business logic:

```python
def segment_customer(row):
    if row['RFM_Score'] == '555':           return 'Champions'
    elif row['R_score'] >= 4 and row['F_score'] >= 4:  return 'Loyal Customers'
    elif row['R_score'] >= 4 and row['F_score'] <= 2:  return 'New Customers'
    elif row['R_score'] <= 2 and row['F_score'] >= 4:  return 'At Risk'
    elif row['R_score'] == 1 and row['F_score'] == 1:  return 'Lost Customer'
    else:                                   return 'Potential Loyalists'
```

---

### 6. 📈 Visualizations

| Chart | Type | Question It Answers |
|---|---|---|
| Customer Segment Distribution | Count Plot | Which segment has the most customers? |
| Average Spending by Segment | Bar Plot | Which segment spends the most? |
| Distribution of Customer Spending | Histogram | Where does most spending cluster? |
| RFM Correlation Matrix | Heatmap | Is there a link between frequency, recency & spending? |

---

## 📣 Marketing Strategy Recommendations

The final output is a **ready-to-use action table** for the marketing team:

| Segment | Definition | Recommended Strategy |
|---|---|---|
| 🏆 **Champions** | RFM Score = 555 — bought recently, often, and spent the most | VIP rewards, exclusive discounts, early access to new products |
| 💛 **Loyal Customers** | High recency & frequency | Loyalty programs, personalized product recommendations |
| 🆕 **New Customers** | Recent but low frequency | Welcome offers, encourage repeat purchases |
| 🌱 **Potential Loyalists** | Middle scores — could go either way | Personalized promotions to increase engagement |
| ⚠️ **At Risk** | Used to buy often but haven't recently | Reactivation campaigns, limited-time discounts |
| ❌ **Lost Customers** | Low recency AND low frequency | Win-back campaigns, surveys, attractive comeback offers |

---

## 💡 Key Business Questions This Project Answers

- Who are our most valuable customers right now?
- Which customers are about to churn and need reactivation?
- Where should the marketing budget be focused for maximum ROI?
- Which customers just joined and need nurturing?
- What is the spending distribution across our entire customer base?

---

## 🛠️ Tools & Libraries

| Tool | Purpose |
|---|---|
| **Python 3** | Core programming language |
| **Pandas** | Data cleaning, aggregation, RFM table construction |
| **Matplotlib** | Histogram, figure layout |
| **Seaborn** | Count plot, bar plot, heatmap |
| **Jupyter Notebook** | Interactive analysis environment |

---

## ⚙️ Requirements

```bash
pip install pandas matplotlib seaborn notebook
```

## 🚀 Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/sararaafat08/-CustomerRadar-RFM-Based-Customer-Segmentation-Marketing-Intelligence.git

# 2. Navigate to the project folder
cd CustomerRadar-RFM

# 3. Launch Jupyter Notebook
jupyter notebook RFM.ipynb
```

---

## 🧠 Skills Demonstrated

`Python` · `Pandas` · `Matplotlib` · `Seaborn` ·
`Data Cleaning` · `Feature Engineering` · `Quantile Scoring` ·
`RFM Analysis` · `Customer Segmentation` · `Business Strategy` ·
`Exploratory Data Analysis (EDA)` · `CRM Analytics` · `Marketing Analytics`

---

## 📌 About This Project

This project is part of my data analytics portfolio built during my studies in
Mathematics & Computer Science at **Faculty of Science, Helwan University**.

RFM segmentation is one of the most widely used frameworks in real-world
**CRM, e-commerce, and marketing analytics** — making this project directly
relevant to data analyst and business intelligence roles across retail,
fintech, and digital marketing industries.

---

## 📄 License

Free to use as a learning reference or portfolio template. Attribution appreciated.
