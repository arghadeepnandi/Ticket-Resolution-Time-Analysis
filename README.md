Ticket Resolution Time Analysis

⏱️ Support Analytics | Python | Survival Analysis | Data Science
🎯 Predict Ticket Resolution Times with Survival Analysis
View Notebook • Report Bug • Request Feature


🎯 About The Project
A Ticket Resolution Time Analysis System powered by Survival Analysis that predicts support ticket resolution patterns and identifies performance differences across support tiers. This project analyzes historical ticket lifecycle data to provide insights into resolution times, helping support teams optimize their operations and improve SLA compliance.
💡 Project Objective:

"I modeled ticket resolution as a time-to-event problem, where the event is ticket closure, and open tickets are treated as censored data."


Key Analysis Factors:

🎫 Ticket Status - Open vs. Resolved tickets
📊 Support Tiers - Tier 1, Tier 2, and Tier 3 analysis
⏰ Resolution Time - Days from creation to closure
📈 Survival Curves - Probability of tickets remaining open
🎯 SLA Compliance - Service level agreement breach rates
📉 Median Times - 50th percentile resolution times
🔍 Statistical Testing - Log-rank tests for tier comparison

The analysis uses Kaplan-Meier estimation to handle both resolved and open (censored) tickets, enabling accurate prediction of resolution patterns.

✨ Features

📊 Kaplan-Meier Survival Analysis - Industry-standard survival curve estimation
🎯 Multi-tier Comparison - Analyze Tier 1, 2, and 3 support performance
📈 Visual Analytics - Beautiful survival curve visualizations
🔬 Statistical Testing - Log-rank tests for significant differences
⚡ SLA Monitoring - Track service level agreement breaches
📉 Median Resolution Times - 50th percentile estimates per tier
🎨 Interactive Analysis - Jupyter notebook for exploration
📊 Summary Statistics - Comprehensive performance metrics table


🛠️ Built With

Statistical Analysis

Lifelines - Survival analysis library
Kaplan-Meier Fitter - Non-parametric survival estimation
Log-rank Test - Statistical comparison of survival curves

Data Processing

DateTime Handling - Timestamp processing and calculations
Event Encoding - Binary outcome variable creation
Censored Data - Proper handling of open tickets


📊 Live Analysis
Explore the complete analysis in the Jupyter notebook:
👉 Ticket Resolution Time Analysis.ipynb
Analyzed Support Tiers

Tier 1 - First-level support (153 tickets)
Tier 2 - Second-level support (84 tickets)
Tier 3 - Expert-level support (63 tickets)

Key Metrics Tracked

Resolution rates by tier
Median resolution times
SLA breach percentages
Average resolution days
Survival probability curves


💻 Installation
Prerequisites

Python 3.8 or higher
pip package manager
Jupyter Notebook (optional)

Setup Instructions

Clone the repository

bash   git clone https://github.com/arghadeepnandi/Ticket-Resolution-Time-Analysis
   cd ticket-resolution-analysis

Create a virtual environment (Optional but recommended)

bash   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate

Install dependencies

bash   pip install -r requirements.txt

Launch Jupyter Notebook

bash   jupyter notebook "Ticket Resolution Time Analysis.ipynb"

Run all cells
Execute cells sequentially to reproduce the analysis


📖 Usage
Running the Analysis

Open the Jupyter Notebook

bash   jupyter notebook "Ticket Resolution Time Analysis.ipynb"
```

2. **Execute Sequential Sections:**
   - Import Libraries
   - Generate Sample Data
   - Data Understanding & Cleaning
   - Create Event & Duration Columns
   - Calculate Resolution Time
   - Survival Analysis
   - Compare Support Tiers
   - Median Resolution Time
   - Statistical Comparison
   - SLA Breach Analysis
   - Final Summary Table

### Understanding the Output

#### Survival Curves
```
📈 Shows probability a ticket remains open over time
📉 Steeper decline = faster resolution
🔵 Different colors = different support tiers
```

#### Summary Statistics
```
✅ Resolved Percentage: % of tickets closed
⏱️ Avg Resolution Days: Mean time to closure
🚨 SLA Breach %: Tickets exceeding threshold
📊 Total Tickets: Sample size per tier
```

### Example Interpretation
```
Support Tier: Tier 1
Total Tickets: 153
Resolved: 74.5%
Avg Resolution: 187.3 days
SLA Breach: 47.7%

📊 Status: NEEDS IMPROVEMENT
🎯 Action: Review escalation procedures

🧠 Methodology
1. Data Preparation
Goal: Create analysis-ready dataset with proper event encoding
python# Event indicator: 1 = resolved, 0 = censored (open)
df["event_resolved"] = df["closed_at"].notna().astype(int)

# Time calculation: days from creation to resolution/censoring
today = pd.Timestamp.today()
df["resolution_time"] = (
    df["closed_at"].fillna(today) - df["created_at"]
).dt.days
Key Concepts:

✅ Event = 1: Ticket has been resolved (complete observation)
⏳ Event = 0: Ticket still open (censored observation)
📅 Time: Duration in days since ticket creation

2. Kaplan-Meier Survival Analysis
Goal: Estimate survival function accounting for censored data
pythonfrom lifelines import KaplanMeierFitter

kmf = KaplanMeierFitter()
kmf.fit(
    durations=df["resolution_time"],
    event_observed=df["event_resolved"],
    label="All Tickets"
)
What it does:

📊 Estimates probability of ticket remaining open at each time point
🎯 Handles censored observations (open tickets) properly
📈 Creates survival curves for visualization
⏱️ Calculates median survival time (50th percentile)

Why Kaplan-Meier?

✅ Non-parametric (no distribution assumptions)
✅ Handles censored data correctly
✅ Industry standard for time-to-event analysis
✅ Provides confidence intervals

3. Multi-Tier Comparison
Goal: Compare resolution patterns across support tiers
pythonfor tier in df["support_tier"].unique():
    tier_data = df[df["support_tier"] == tier]
    kmf.fit(
        tier_data["resolution_time"],
        tier_data["event_resolved"],
        label=tier
    )
    kmf.plot_survival_function()
Visualization Interpretation:

📉 Steeper curve = Faster resolution
📈 Flatter curve = Slower resolution
🔄 Crossing curves = Performance varies over time
📍 Plateau = Period of few resolutions

4. Statistical Testing
Goal: Determine if tier differences are statistically significant
pythonfrom lifelines.statistics import logrank_test

result = logrank_test(
    tier1["resolution_time"],
    tier3["resolution_time"],
    event_observed_A=tier1["event_resolved"],
    event_observed_B=tier3["event_resolved"]
)
Test Results:

p < 0.05: Significant difference between tiers
p ≥ 0.05: No significant difference (similar performance)

Current Finding:

📊 p = 0.60 (not significant)
💡 Interpretation: Tier 1 and Tier 3 have similar resolution patterns
🎯 Action: Performance standardization across tiers is working

5. SLA Breach Analysis
Goal: Identify tickets exceeding service level agreements
pythonsla_days = 5
df["sla_breached"] = df["resolution_time"] > sla_days
breach_rate = df.groupby("support_tier")["sla_breached"].mean() * 100
```

**Thresholds:**
- 🟢 **< 20%**: Excellent SLA compliance
- 🟡 **20-40%**: Acceptable performance
- 🟠 **40-60%**: Needs improvement
- 🔴 **> 60%**: Critical issues

---

## 📈 Results

### Overall Performance Summary

| Support Tier | Total Tickets | Resolved % | Avg Resolution Days | SLA Breach % | Status |
|--------------|---------------|------------|---------------------|--------------|--------|
| Tier 1       | 153           | 74.5%      | 187.3               | 47.7%        | 🟠 Review Needed |
| Tier 2       | 84            | 75.0%      | 183.7               | 47.6%        | 🟠 Review Needed |
| Tier 3       | 63            | 74.6%      | 186.0               | 39.7%        | 🟡 Acceptable |

### Key Findings

#### ⏱️ Resolution Times
- **Median:** 5.0 days across all tiers
- **Average:** ~185 days (influenced by long-tail tickets)
- **Distribution:** Right-skewed with outliers

#### 📊 Resolution Rates
- **Overall:** ~75% of tickets resolved
- **Consistency:** Similar rates across all tiers
- **Improvement:** 25% of tickets remain open

#### 🎯 SLA Performance
- **Best:** Tier 3 (39.7% breach rate)
- **Worst:** Tier 1 (47.7% breach rate)
- **Target:** < 20% breach rate (industry standard)
- **Gap:** ~28% improvement needed

#### 📈 Statistical Analysis
- **Log-rank test:** p = 0.60
- **Interpretation:** No significant difference between Tier 1 and Tier 3
- **Conclusion:** Consistent performance standardization
- **Implication:** Training programs are working effectively

---

## 📊 Visualizations

### 1. Overall Survival Curve
**Purpose:** Shows aggregate ticket resolution pattern
```
┌─────────────────────────────────────┐
│ 1.0 ┤████████████▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄  │
│     │               ▀▀▀▀▀▀▀▀▀▀▀▀▀▀  │
│ 0.5 ┤                              │
│     │                              │
│ 0.0 ┤──────────────────────────────│
│     0     100    200    300    400 │
│           Days Since Creation      │
└─────────────────────────────────────┘
```

**Interpretation:**
- 📉 Rapid initial decline (many quick resolutions)
- 📈 Plateau around 25% (hard-to-resolve tickets)
- ⏱️ Median at ~5 days (50% resolved by day 5)

### 2. Tier Comparison Curves
**Purpose:** Compare resolution patterns across support levels
```
┌─────────────────────────────────────┐
│ 1.0 ┤─── Tier 1                    │
│     │--- Tier 2                    │
│     │··· Tier 3                    │
│ 0.5 ┤        ▄▄▄                   │
│     │     ▄▄▄   ▀▀▀                │
│ 0.0 ┤▄▄▄▄▄           ▀▀▀▀▀▀▀▀▀▀▀▀▀│
│     0     100    200    300    400 │
└─────────────────────────────────────┘
```

**Insights:**
- 🔵 Similar initial resolution rates
- 🟢 Tier 3 slightly better long-term
- 🔴 All tiers plateau around 25-30%

---

## 📁 Project Structure
```
ticket-resolution-analysis/
│
├── Ticket Resolution Time Analysis.ipynb   # Main analysis notebook
├── ticket_history.csv                      # Generated dataset (300 tickets)
├── requirements.txt                        # Python dependencies
├── README.md                              # This file
│
├── visualizations/                        # (Optional) Saved plots
│   ├── overall_survival_curve.png
│   ├── tier_comparison.png
│   └── sla_breach_analysis.png
│
└── data/                                  # (Optional) Data folder
    ├── raw/                              # Original ticket data
    └── processed/                        # Cleaned datasets

🔧 Dependencies
Core Requirements
txtpandas==2.0.0
numpy==1.24.0
matplotlib==3.7.0
lifelines==0.27.0
Installation
bashpip install pandas numpy matplotlib lifelines
Full requirements.txt
txt# Data Manipulation
pandas>=2.0.0
numpy>=1.24.0

# Visualization
matplotlib>=3.7.0
seaborn>=0.12.0

# Survival Analysis
lifelines>=0.27.0

# Jupyter Notebook
jupyter>=1.0.0
ipython>=8.0.0

# Additional Utilities
python-dateutil>=2.8.0
Install all dependencies:
bashpip install -r requirements.txt
```

---

## 📚 Understanding Survival Analysis

### What is Survival Analysis?

Survival analysis is a branch of statistics that deals with **time-to-event data**, where:
- ⏰ **Time**: Duration until an event occurs
- 🎯 **Event**: The outcome of interest (ticket resolution)
- 📊 **Censoring**: Observations where the event hasn't occurred yet

### Why Use Survival Analysis for Tickets?

#### Traditional Analysis Problem:
```
❌ Ignoring open tickets = biased results
❌ Treating open tickets as "failed" = incorrect
❌ Only analyzing closed tickets = selection bias
```

#### Survival Analysis Solution:
```
✅ Includes all tickets (open and closed)
✅ Properly handles censored data
✅ Provides unbiased time estimates
✅ Accounts for varying observation periods
```

### Key Concepts

#### 1. Survival Function S(t)
**Definition:** Probability that a ticket survives (remains open) beyond time t
```
S(t) = P(T > t)

where:
- T = time to resolution
- t = specific time point
```

**Example:**
- S(5 days) = 0.50 → 50% of tickets still open after 5 days
- S(10 days) = 0.30 → 30% of tickets still open after 10 days

#### 2. Hazard Function h(t)
**Definition:** Instantaneous rate of resolution at time t
```
h(t) = lim[Δt→0] P(t ≤ T < t+Δt | T ≥ t) / Δt
```

**Interpretation:**
- 📈 High hazard = Tickets resolving quickly at time t
- 📉 Low hazard = Tickets resolving slowly at time t

#### 3. Censoring
**Right Censoring:** Most common type (used in this project)
```
Ticket Created → Still Open → Analysis Date
         |__________________________|
              Censored observation
```

**Why it matters:**
- 🎯 We know the ticket has been open for at least X days
- ⏳ We don't know when it will be resolved
- ✅ Survival analysis handles this correctly

---

## 💡 Interpretation Guide

### Reading Survival Curves

#### Y-Axis: Survival Probability
```
1.0 = 100% of tickets still open
0.5 = 50% of tickets still open
0.0 = All tickets resolved
```

#### X-Axis: Time (Days)
```
0 = Ticket creation
50 = 50 days after creation
100 = 100 days after creation
```

#### Curve Patterns

**Steep Decline:**
```
📉 Rapid resolution
🎯 Good: Many tickets closing quickly
```

**Gradual Decline:**
```
📊 Slow resolution
⚠️ Concern: Tickets taking longer
```

**Plateau:**
```
📈 Few additional resolutions
🔴 Problem: Backlog of difficult tickets
```

### Statistical Significance

#### Log-rank Test Results

**p < 0.05:**
```
✅ Significant difference
📊 Tiers perform differently
🎯 Action: Investigate why
```

**p ≥ 0.05:**
```
❌ No significant difference
📊 Tiers perform similarly
🎯 Action: Maintain consistency
```

**Current Project:**
```
p = 0.60 (not significant)
💡 Tier 1 and Tier 3 have similar patterns
✅ Standardization is working well

🚀 Future Enhancements

 Real-time Dashboard - Live ticket monitoring
 Predictive Modeling - Estimate resolution time for new tickets
 Root Cause Analysis - Identify factors causing delays
 Interactive Visualizations - Plotly/Dash integration
 Automated Reporting - Scheduled analysis reports
 API Integration - Connect to ticketing systems (Jira, ServiceNow)
 Customer Segmentation - Analyze by customer priority
 Seasonal Analysis - Detect time-based patterns
 Resource Optimization - Staff allocation recommendations
 Multi-variate Analysis - Cox proportional hazards model
 Mobile Dashboard - On-the-go analytics
 Export Functionality - PDF/Excel reports


🤝 Contributing
Contributions make the open-source community an amazing place to learn and create. Any contributions you make are greatly appreciated!
How to Contribute:

Fork the Project
Create your Feature Branch

bash   git checkout -b feature/AmazingFeature

Commit your Changes

bash   git commit -m 'Add some AmazingFeature'

Push to the Branch

bash   git push origin feature/AmazingFeature

Open a Pull Request

Ideas for Contribution:

🎨 Improve visualizations - More interactive plots
📊 Add new metrics - Additional KPIs and analytics
🔄 Cox regression - Advanced survival modeling
🧪 Unit tests - Ensure code reliability
📈 Feature engineering - New variables for analysis
📝 Documentation - Improve explanations
🚨 Alerting system - Real-time notifications
📱 Mobile support - Responsive design
🌐 API development - RESTful endpoints
🔐 Authentication - Secure data access


📞 Contact
Your Name - Arghadeep Nandi

LinkedIn - https://www.linkedin.com/in/arghadeep-nandi-159523252/

Project Link: https://github.com/arghadeepnandi/Ticket-Resolution-Time-Analysis

📜 License
Distributed under the MIT License. See LICENSE for more information.

🙏 Acknowledgments

Lifelines - Excellent survival analysis library
Kaplan-Meier Method - Statistical foundation
Survival Analysis Resources - Learning materials
Stack Overflow Community - Problem-solving support
Open Source Contributors - Making this possible


📚 Further Reading
Books

Survival Analysis: A Self-Learning Text by David Kleinbaum
Applied Survival Analysis by Hosmer, Lemeshow, and May

Online Resources

Lifelines Documentation
Survival Analysis in Python Tutorial
Khan Academy: Survival Analysis

Papers

Kaplan, E. L., & Meier, P. (1958). "Nonparametric estimation from incomplete observations"
Cox, D. R. (1972). "Regression models and life-tables"


🎓 Learning Path
Beginner

✅ Run the notebook
✅ Understand survival curves
✅ Interpret median times

Intermediate

🎯 Modify SLA thresholds
🎯 Add custom visualizations
🎯 Experiment with groupings

Advanced

🚀 Implement Cox regression
🚀 Build predictive models
🚀 Create interactive dashboards


⭐ Star this repo if you found it helpful! ⭐
🔖 Fork it to start your own analysis! 🔖
📢 Share with your team! 📢

Made with ❤️ and 📊 by [Arghadeep Nandi]
Empowering support teams with data-driven insights
