# AI Ethics Assignment: COMPAS Racial Bias Analysis

## Overview
This project analyzes racial bias in the COMPAS (Correctional Offender Management Profiling for Alternative Sanctions) algorithm using ProPublica's Broward County criminal records dataset. The analysis employs IBM's AI Fairness 360 (AIF360) toolkit to quantify algorithmic bias and develop remediation strategies.

## Problem Statement
The COMPAS algorithm is widely used in the U.S. criminal justice system to assess recidivism risk. ProPublica's investigation found evidence of racial bias, with African-American defendants being significantly more likely to be falsely flagged as high-risk. This project rigorously quantifies this bias and proposes solutions.

## Key Findings

### Critical Racial Bias Detected
- **2.39× False Positive Rate Disparity**: African-Americans are 2.39 times more likely to be falsely classified as high-risk
- **False Positive Rate (African-American)**: 34.32% (1 in 3 innocent defendants wrongly flagged)
- **False Positive Rate (Other Races)**: 14.35% (1 in 7 innocent defendants wrongly flagged)
- **Disparity Magnitude**: 19.97 percentage point gap (exceeds 1.25× fairness threshold by ~2×)

### Additional Findings
- **Gender Bias**: Males have higher false positive rates (23.84%) than females (21.85%)
- **Model Miscalibration**: COMPAS overestimates risk for African-Americans (predicted 48.63% vs. actual 39.54%) and underestimates for others
- **Disparate Impact Ratio**: 0.788 (critically below 0.80 fairness threshold)
- **Equal Opportunity Gap**: 19.97 percentage points between racial groups

## Remediation Strategies

### 1. Group-Aware Threshold Adjustment
Adjust classification thresholds by race to achieve fairness-accuracy balance:
- African-American threshold: 4
- Other races threshold: 5
- **Result**: 42.9% reduction in bias disparity (from 2.19× to 1.25×)

### 2. Model Recalibration
Retrain the algorithm with:
- Explicit fairness constraints using AIF360
- Balanced training data across demographic groups
- Regular calibration checks to ensure prediction alignment

### 3. Human Oversight
- Mandatory judicial review for all high-risk classifications
- Enhanced scrutiny for African-American defendant classifications
- Appeal mechanisms for potentially biased decisions

### 4. Continuous Monitoring
- Real-time fairness dashboards tracking FPR, disparate impact, and equal opportunity metrics
- Quarterly bias audits by independent third parties
- Alert mechanisms for emerging bias patterns

### 5. Data Enhancement
- Collect bias-corrected supplementary data
- Analyze misclassified cases to identify systematic patterns
- Improve future training datasets with better representation

### 6. Transparency & Accountability
- Publish bias audit results publicly
- Mandate impact assessments before deploying similar algorithms
- Establish algorithmic accountability frameworks

## Project Structure

```
compas-analysis/
├── README.md                              # This file
├── REMEDIATION_REPORT.md                 # Executive summary with findings & solutions
├── PROJECT_SUMMARY.txt                   # Detailed project overview
├── BIAS_ANALYSIS_RESULTS.txt             # Technical analysis results
├── FAIRNESS_QUICK_REFERENCE.txt          # Quick reference guide to metrics
│
├── Notebooks/
│   ├── AIF360_Bias_Analysis.ipynb        # AI Fairness 360 toolkit analysis
│   ├── Fairness_Visualizations_FPR_Analysis.ipynb
│   └── Cox with interaction term and independent variables.ipynb
│
├── Visualizations/
│   ├── 01_FPR_Disparity_Analysis.png     # 4-panel FPR analysis
│   ├── 02_Confusion_Matrices_by_Race.png # Side-by-side confusion matrices
│   ├── 03_AIF360_Fairness_Metrics.png    # 8-metric fairness dashboard
│   ├── 04_Comprehensive_Demographic_Analysis.png
│   ├── 05_Fairness_Accuracy_Tradeoff.png
│   ├── confusion_matrices_by_race.png
│   ├── error_rates_by_race.png
│   ├── fairness_metrics_heatmap.png
│   ├── high_risk_prediction_rate.png
│   └── mitigation_comparison.png
│
├── Data/
│   ├── compas-scores.csv                 # Original COMPAS risk scores
│   ├── compas-scores-raw.csv             # Raw data
│   ├── compas-scores-two-years.csv       # 2-year recidivism follow-up
│   ├── compas-scores-two-years-violent.csv
│   ├── cox-parsed.csv                    # Parsed Cox regression data
│   └── cox-violent-parsed.csv
│
└── Documentation/
    ├── FAIRNESS_QUICK_REFERENCE.txt
    ├── README_BIAS_ANALYSIS.md
    ├── FAIRNESS_VISUALIZATIONS_REPORT.md
    └── FILE_INDEX.txt
```

## Fairness Metrics Explained

| Metric | Definition | Threshold | Status |
|--------|-----------|-----------|--------|
| **False Positive Rate (FPR)** | Innocent people classified as high-risk | <0.10 per group | ❌ CRITICAL |
| **False Negative Rate (FNR)** | Actual offenders classified as low-risk | <0.10 per group | ⚠️ MODERATE |
| **Disparate Impact Ratio** | Ratio of positive outcomes between groups | ≥0.80 | ❌ CRITICAL |
| **Equal Opportunity Difference** | TPR gap between demographic groups | ±0.05 | ❌ CRITICAL |
| **Average Odds Difference** | Average classification error gap | ±0.05 | ❌ CRITICAL |

## How to Use This Repository

### 1. Review the Analysis
- Start with `REMEDIATION_REPORT.md` for a 300-word summary
- Read `FAIRNESS_QUICK_REFERENCE.txt` for key metrics
- Examine visualizations in the `Visualizations/` folder

### 2. Reproduce the Analysis
```bash
# Install dependencies
pip install aif360 pandas numpy matplotlib seaborn scikit-learn jupyter

# Run analysis notebooks
jupyter notebook AIF360_Bias_Analysis.ipynb
jupyter notebook Fairness_Visualizations_FPR_Analysis.ipynb
```

### 3. Implement Remediation
- Review mitigation strategies in `REMEDIATION_REPORT.md`
- Apply group-aware thresholds to production models
- Deploy monitoring dashboards using visualization scripts

## Technical Stack
- **Python 3.13.7**
- **IBM AI Fairness 360 (AIF360 0.6.1)**: Bias detection and mitigation
- **Pandas**: Data manipulation
- **Matplotlib/Seaborn**: Visualization
- **Scikit-learn**: Machine learning baseline
- **Jupyter**: Interactive analysis

## Dataset Information
- **Source**: ProPublica COMPAS Analysis
- **Location**: Broward County, Florida
- **Total Records**: 11,038 defendants
- **Demographics**: 49.8% African-American, 50.2% Other races
- **Outcome**: 2-year recidivism tracking
- **Time Period**: Covering multiple years of criminal records

## Key Insights

### Why This Matters
Algorithmic bias in criminal justice has real-world consequences:
- Innocent people face longer sentences based on biased risk assessments
- Racial discrimination perpetuated through "objective" algorithms
- Systematic inequality embedded in automated decision-making systems

### Fairness vs. Accuracy Trade-off
- Increasing fairness may reduce overall accuracy
- Different fairness criteria (equal opportunity, demographic parity, calibration) may conflict
- Decision-makers must explicitly choose fairness priorities aligned with societal values

### Mitigation Feasibility
- 42.9% bias reduction achievable through threshold adjustment alone
- Further improvements possible with model retraining and human oversight
- Requires deliberate design choices prioritizing fairness

## References
- ProPublica: [Machine Bias](https://www.propublica.org/article/machine-bias-there-is-software-used-across-the-country-to-predict-future-criminals)
- IBM AI Fairness 360: [GitHub Repository](https://github.com/Trusted-AI/AIF360)
- Dressel & Farid (2018): "The accuracy, fairness, and limits of predicting recidivism"
- Corbett-Davies et al. (2016): "Algorithmic Decision Making and the Cost of Fairness"

## Author
Created for AI Ethics Assignment - Week 7
Analysis conducted: November 16, 2025

## License
This project uses ProPublica's COMPAS dataset and analysis methodology. See original sources for licensing details.

## Contributing
For questions or suggestions about this analysis, please review the documentation files or contact the project maintainer.

---

**Disclaimer**: This analysis is for educational purposes. It highlights algorithmic bias concerns but does not constitute legal or policy advice. Actual remediation requires domain expertise, legal consultation, and stakeholder engagement.
