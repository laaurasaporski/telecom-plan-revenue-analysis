# 📱 Telecom Plan Revenue Analysis — Megaline

Exploratory data analysis and statistical hypothesis testing to determine which prepaid plan generates more revenue, supporting advertising budget allocation decisions.

---

## 📌 Overview

Megaline, a telecom company, offers two prepaid plans: **Surf** and **Ultimate**. This project analyzes usage behavior and monthly revenue across 500 customers throughout 2018 to identify which plan is more profitable and whether regional differences in revenue exist.

**Business questions:**
- Which plan generates higher average revenue per user?
- Do users in the NY-NJ area generate significantly different revenue than other regions?

---

## 📊 Datasets

| File | Description | Records |
|---|---|---|
| `megaline_users.csv` | Customer profiles (plan, city, registration date) | 500 users |
| `megaline_plans.csv` | Plan details (limits, prices per extra unit) | 2 plans |
| `megaline_calls.csv` | Call logs with duration | 110,901 records |
| `megaline_messages.csv` | SMS logs | 76,051 records |
| `megaline_internet.csv` | Data session logs (MB used) | 104,825 records |

---

## 🔧 Methodology

### Data Preprocessing
- Parsed datetime columns across all tables
- Rounded call durations up (`np.ceil`) — billing rounds up to the nearest minute
- Removed zero-duration calls (no charge applied)
- Enriched user data with `is_active` flag and `days_active` from registration to churn date

### Feature Engineering
- Aggregated usage per user per month: total minutes, messages, and MB used
- Calculated overage for each category beyond plan limits
- Computed monthly revenue: `base_fee + extra_minutes_cost + extra_messages_cost + extra_data_cost`
- Converted MB to GB (ceiling applied) for data overage billing

### Statistical Testing
- **Levene's test** to check variance equality before applying t-test
- **Welch's t-test** (independent samples) for plan revenue comparison
- **Student's t-test** for regional revenue comparison (NY-NJ vs. others)
- Significance level: α = 0.05

---

## 📈 Results

**Plan Comparison**

| Plan | Avg. Monthly Revenue | Variance | Monthly Fee |
|---|---|---|---|
| Surf | $60.71 | 3,067.84 | $20 |
| Ultimate | $72.31 | 129.85 | $70 |

**Hypothesis Tests**

| Test | H₀ | p-value | Result |
|---|---|---|---|
| Plan revenue difference | Surf = Ultimate avg. revenue | < 0.0001 | ✅ Rejected |
| Regional revenue difference | NY-NJ = Other regions avg. revenue | 0.0436 | ✅ Rejected |

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-lightgrey)
![SciPy](https://img.shields.io/badge/SciPy-Statistics-blue)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange)
![Seaborn](https://img.shields.io/badge/Seaborn-Visualization-teal)

- **Python** — Pandas, NumPy
- **Statistics** — SciPy (Levene, t-test)
- **Visualization** — Matplotlib, Seaborn

---

## 📁 Project Structure

```
telecom-plan-revenue-analysis/
│
├── S7.ipynb             # Full analysis and hypothesis testing
├── README.md
└── .gitignore
```

---

## 🚀 How to Run

```bash
# Clone the repository
git clone https://github.com/laaurasaporski/telecom-plan-revenue-analysis.git

# Install dependencies
pip install pandas numpy scipy matplotlib seaborn

# Open the notebook
jupyter notebook S7.ipynb
```

---

## 💡 Key Insights

- **Ultimate generates 19% more revenue per user** on average, driven by a higher base fee
- **Surf revenue is highly variable** (variance = 3,067) due to unpredictable overages, while Ultimate is stable (variance = 129) — users rarely exceed limits
- **NY-NJ users generate slightly less revenue** than other regions ($59.92 vs $65.22), a statistically significant difference despite the small effect size
- **Surf attracts high-volume, cost-conscious users** — many exceed data limits and pay substantial overages
- **Strategic recommendation:** Allocate advertising budget toward Ultimate for revenue maximization; use Surf campaigns for customer acquisition and volume growth
