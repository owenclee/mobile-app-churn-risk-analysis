# Mobile App Churn Prediction and Retention Analysis

## Project Overview

This project applies a product analytics workflow to a synthetic Waze user dataset to examine patterns associated with mobile app churn. The analysis uses user-level activity, driving behavior, navigation behavior, account age, and device type to better understand which users appear more likely to stop using the app.

The project begins with data cleaning and exploratory analysis, then moves into feature engineering and churn prediction modeling. I compare logistic regression and random forest models to evaluate whether behavioral features can provide useful churn-risk signals. The final output includes modeled risk segments and a Tableau dashboard designed to summarize the main retention patterns.

Rather than treating the model as a final decision system, I frame it as a prioritization tool. The practical goal is to identify user segments that may benefit from earlier onboarding support, re-engagement, or habit-building interventions.

## Tableau Dashboard

View the interactive dashboard here: [Mobile App Churn & Retention Risk Dashboard](https://public.tableau.com/views/MobileAppChurnRetentionRiskDashboard/ChurnDashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## Business Question

Which mobile app users are most likely to churn, what behaviors are most associated with churn risk, and how could a product or retention team use that information to prioritize outreach?

## Dataset

The dataset contains approximately 15,000 synthetic Waze user records with fields related to:

- User churn status
- Sessions and drives
- Total app sessions
- Account age after onboarding
- Navigation behavior
- Kilometers driven
- Driving duration
- Activity days
- Driving days
- Device type

The raw dataset included some records with missing churn labels. Those records were excluded from the modeling dataset because the churn outcome was unknown.

## Tools Used

- Python
- pandas
- NumPy
- scikit-learn
- matplotlib
- Tableau Public
- Google Colab
- GitHub

## Project Workflow

### 1. Data Cleaning and Exploratory Analysis

The first notebook loads the raw dataset, standardizes column names, checks missing values and duplicates, creates a numeric churn flag, and removes records where the churn label is missing.

The exploratory analysis focuses on how churn differs across account age, activity days, driving days, device type, and overall app usage.

### 2. Feature Engineering

I created behavior-based features to capture usage consistency and intensity:

- Sessions per activity day
- Drives per driving day
- Kilometers per drive
- Minutes per drive
- Total favorite-place navigations
- Activity rate
- Driving rate

These features helped test whether churn was more related to consistent app habits than raw usage volume.

### 3. Churn Prediction Modeling

I compared two classification models:

- Logistic Regression
- Random Forest

Model performance was evaluated using accuracy, precision, recall, F1 score, ROC-AUC, and confusion matrices.

The goal was not to build a perfect prediction system. The goal was to understand whether the data could support useful churn prioritization.

### 4. Risk Segmentation

After modeling, users were assigned predicted churn probabilities and grouped into four risk segments. These segments were used to compare behavior patterns between lower-risk and higher-risk users.

### 5. Tableau Dashboard

I built a Tableau dashboard to summarize the main retention story across:

- Churn rate by predicted risk group
- Activity days by risk group
- Driving days by risk group
- Account age by risk group
- Sessions by risk group

The dashboard highlights the central finding: users with higher churn risk were less consistent and had lower account age, even though they averaged more sessions.

## Key Findings

### Churn was tied more to consistency than raw usage volume

Churned users were not simply inactive users. On average, churned users had more sessions, more drives, more navigation activity, and slightly more driving time than retained users.

The larger difference was consistency. Churned users were active across fewer days and had fewer driving days.

### Newer users were more likely to churn

Users in the newest tenure group had the highest churn rate, while the longest-tenured users had the lowest churn rate.

This suggests that early lifecycle engagement may be a major retention opportunity.

### Activity days were one of the strongest churn signals

Users in the lowest activity group churned at a much higher rate than users in the highest activity group.

This points to a practical retention insight: building consistent app habits may matter more than increasing raw session volume.

### Device type did not meaningfully explain churn

Android and iPhone users had very similar churn rates. Based on this dataset, churn does not appear to be driven by platform differences.

## Model Results

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.6892 | 0.3124 | 0.6262 | 0.4168 | 0.7243 |
| Random Forest | 0.7477 | 0.3445 | 0.4685 | 0.3971 | 0.7289 |

The random forest performed slightly better on accuracy and ROC-AUC, while logistic regression had stronger recall.

For a retention use case, recall matters because it shows how many actual churned users the model catches. A higher-recall model may be more useful when the business wants to identify as many at-risk users as possible, even if the outreach list is noisier.

## Most Important Churn Signals

The random forest model relied most heavily on:

- Activity days
- Driving days
- Account age after onboarding
- Activity rate
- Driving rate
- Sessions per activity day
- Total favorite-place navigations
- Driving duration
- Drives per driving day
- Total sessions

These results support the EDA finding that churn risk is more connected to consistency and account maturity than raw activity volume alone.

## Risk Segment Findings

Users were grouped into four predicted churn-risk segments for dashboard exploration.

| Risk Group | Users | Actual Churn Rate | Avg. Predicted Churn Probability | Avg. Activity Days | Avg. Driving Days | Avg. Tenure Days | Avg. Sessions | Avg. Drives |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Lowest Risk | 3,575 | 1.29% | 8.59% | 24.89 | 20.14 | 2,189.81 | 72.37 | 60.55 |
| Lower-Mid Risk | 3,575 | 2.57% | 20.66% | 19.11 | 15.14 | 1,803.13 | 76.58 | 63.94 |
| Upper-Mid Risk | 3,574 | 14.19% | 38.45% | 12.35 | 9.39 | 1,670.36 | 83.20 | 69.31 |
| Highest Risk | 3,575 | 52.90% | 64.52% | 5.82 | 4.06 | 1,343.97 | 90.36 | 75.23 |

The highest-risk group had the lowest activity days, lowest driving days, and lowest average tenure. Interestingly, this group also had the highest average sessions and drives, reinforcing the idea that churn risk is not just about total usage. It is about whether usage becomes consistent over time.

## Business Recommendations

### 1. Focus on early lifecycle retention

Newer users showed higher churn rates, so the product team should focus on improving early engagement. This could include stronger onboarding prompts, clearer first-use guidance, and early reminders that help users form consistent app habits.

### 2. Re-engage low-consistency users

Users with fewer activity days and fewer driving days were much more likely to churn. Retention efforts should target users who use the app in bursts but do not return consistently.

### 3. Use behavior-based retention segments

Predicted churn probabilities and activity patterns can be used to group users into retention-priority segments. These segments could support targeted nudges, education, or reactivation campaigns.

### 4. Avoid over-focusing on device type

Because Android and iPhone churn rates were nearly identical, the retention strategy should focus more on behavior and lifecycle stage than platform.

## Repository Structure

- `data/raw/waze_dataset.csv`
- `data/processed/waze_churn_cleaned.csv`
- `data/processed/waze_churn_scored.csv`
- `data/processed/model_results.csv`
- `data/processed/feature_importance.csv`
- `data/processed/risk_summary.csv`
- `notebooks/01_data_cleaning_and_eda.ipynb`
- `notebooks/02_churn_prediction_modeling.ipynb`
- `README.md`

## Limitations

This project uses synthetic data, so the findings should be interpreted as a portfolio case study rather than real Waze business performance.

The dataset does not include revenue, geography, acquisition channel, marketing exposure, user demographics, or app feature-level event logs. Those fields would allow a deeper retention analysis.

The models are useful for prioritization, but they should not be treated as automated decision systems. A real retention program would need experimentation, campaign testing, and post-intervention measurement to confirm whether targeting high-risk users improves retention.

## Summary

This project found that churn risk was tied more strongly to activity consistency and account age than to raw usage volume or device type.

The strongest business takeaway is that retention teams should not only look for users with low total usage. They should also identify users who show bursty, inconsistent usage patterns, especially earlier in the user lifecycle.
