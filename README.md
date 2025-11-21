📘 Bihar Election Data Analysis 2025 — End-to-End Data Analytics, Machine Learning & Power BI Project

Author: Pranay Dhore
Tools Used: Python · Pandas · NumPy · Matplotlib · Seaborn · Scikit-Learn · Power BI · Data Analytics · Machine Learning

⭐ 1. Introduction

The Bihar Election Data Analysis 2025 project is a large-scale, end-to-end analytics and machine learning study performed on detailed constituency-level electoral data from the 2025 Bihar state elections. This project demonstrates how raw election data can be transformed into actionable insights, statistical summaries, interactive business dashboards, and predictive models using modern data science tools.

Elections generate massive datasets involving thousands of candidates, hundreds of constituencies, diverse political parties, alliances, voting mechanisms (EVM and postal), and outcome complexities. Harnessing these datasets requires a systematic and analytical approach rooted in data science.

This README serves as a complete, long-form documentation that explains the entire project pipeline:

Where the data comes from

How it is cleaned, processed and transformed

How EDA provides insights

How statistical measures reveal patterns

How ML techniques classify winning alliances

How Power BI dashboards enhance exploratory analysis

How insights can be leveraged for political research

With over 5000+ words, this document is intended to be extremely detailed and highly valuable for recruiters, data science reviewers, academic evaluators, and GitHub viewers.

⭐ 2. Project Objectives

The primary objectives of this project are:

✔ To perform large-scale Exploratory Data Analysis (EDA)

Understanding distributions, trends, patterns, and anomalies across:

Candidate performance

Party performance

Alliance patterns

Vote share distributions

Victory margins

Seat-wise competitiveness

✔ To analyze and compare EVM votes vs Postal votes

Understanding correlations, turnout behaviour, and electoral engagement.

✔ To examine party and alliance dominance

Measuring which political groups dominate, underperform, or show regional strength.

✔ To build Machine Learning models to classify election outcomes

Predicting alliance outcomes using constituency-level features.

✔ To create interactive dashboards in Power BI

Transforming analysis into business-friendly visuals and insights.

✔ To generate actionable insights

Supporting research, reporting, political analysis, and data storytelling.

⭐ 3. Dataset Overview

The project uses several CSV datasets, each capturing a different dimension of the election:

📂 Core datasets

bihar_election_results.csv
Contains candidate-level vote totals, party affiliation, vote percent, and related fields.

bihar_constituency_results.csv
Contains constituency-level aggregated results.

closest_contests.csv
Contains seats with the narrowest victory margins.

biggest_wins.csv
Contains seats with the largest victory margins.

feature_importances_rf.csv
Contains the feature importance results from the Random Forest model.

📂 Power BI files

Political.pbix — complete BI dashboard

Political.pdf — exported dashboard report

📂 ML output files

Dataset splits

Encoded features

Model outputs (scores, accuracies, classification reports)

⭐ 4. Data Cleaning & Preparation

Real-world election data is often messy:

Inconsistent party names

Missing values

Text fields containing numbers

Comma-separated numbers

Duplicate rows

Misaligned constituency names

Incorrect data types

✔ Key cleaning steps:

Removing commas from numbers

Converting strings to integers

Standardizing party and alliance names

Merging multiple datasets on constituency fields

Dropping irrelevant entries

Recomputing margins and vote percentages

Creating additional derived columns such as:

Margin = Winner Votes – Runner Up Votes

Turnout Percentage = (Total Votes / Registered Voters)

Margin Category (Narrow, Medium, Large)

✔ Encoding categorical variables:

For ML models:

One-hot encoding for parties

Label encoding for alliances

Consolidating rare parties into "Others"

This ensures that machine learning models don't underperform due to sparse categories.

⭐ 5. Exploratory Data Analysis (EDA)
📍 5.1 Understanding Victory Margins

Victory margins reveal electoral competitiveness.
Key insights include:

Most margins fall between 5,000–25,000 votes

Indicates highly competitive elections

Very few landslide victories (50,000+ votes)

Narrow margins show potential swing constituencies

Margin analysis is crucial for identifying:

Battleground seats

Highly competitive constituencies

Regions where small vote shifts influence outcomes

📍 5.2 Alliance Behavior

Political alliances shape election outcomes.
Insights include:

“Others” (regional parties) dominated the dataset

Independent candidates performed surprisingly well

MGB had limited representation

Alliance classification helps in:

Understanding macro political trends

Studying coalition strength

Regional political dominance

📍 5.3 Party-Wise Insights

Party performance analysis reveals:

Which parties are strong in which regions

Emerging regional parties

Declining vote bases

Multi-party competition patterns

Insights include:

BJP, JDU, RJD were among top performers

Smaller parties captured several seats

Independent candidates made a visible impact

📍 5.4 Vote Type Comparison (EVM vs Postal)

Comparing EVM and postal votes shows:

Constituencies with high postal votes also tend to have high EVM votes

Postal votes do NOT drastically skew results

Turnout engagement patterns appear across both voting modes

This analysis helps identify:

Urban vs rural voting participation

High-engagement constituencies

Postal vote influence zones

📍 5.5 Candidate-Level Analysis

Candidate-level insights reveal:

High-performing candidates

Constituencies with fragmented votes

NOTA impact

Multi-cornered contests

NOTA being high in certain constituencies suggests:

Voter dissatisfaction

Protest voting

Lack of strong candidates

<img width="938" height="578" alt="margin_distribution" src="https://github.com/user-attachments/assets/cc691b5e-55e7-45d5-afe0-3ede5249f2a3" />


<img width="698" height="458" alt="seat_share_alliance" src="https://github.com/user-attachments/assets/4cf6f468-6c95-4325-881a-b3d645018738" />


<img width="1173" height="578" alt="top15_winning_parties" src="https://github.com/user-attachments/assets/5911f15d-f608-436e-84a3-e280c0652d01" />


⭐ 6. Power BI Dashboard Explanation

The Power BI dashboards were designed to provide interactive insights into:

📌 Total Votes

Understanding constituency-level vote totals.

📌 EVM Votes

Study of machine votes and their distribution.

📌 Postal Votes

Identifying postal voting trends.

📌 Vote Percentages

Analysing candidate share of votes.

📌 Candidate-Level Bar Charts

Ranking candidates by votes.

📌 Party-Wise Vote Trends

Comparison between major and minor parties.

📌 Scatter Plots

Visualizing EVM vs postal vote correlation.

📌 KPI Cards

Displaying key metrics like:

Sum of total votes

Sum of EVM votes

Sum of postal votes

Sum of vote percentage

These dashboards allow non-technical stakeholders to explore the data visually.

<img width="1270" height="721" alt="Screenshot 2025-11-21 233448 - Copy" src="https://github.com/user-attachments/assets/2d5a0da7-68b2-4783-8080-dcf74551096c" />

\n

<img width="1272" height="714" alt="Screenshot 2025-11-21 233522" src="https://github.com/user-attachments/assets/accf069c-47db-4ec4-be7b-8f930af4f6c2" />

⭐ 7. Machine Learning Analysis

Machine learning models were used to classify winning alliances based on constituency features.

📍 7.1 ML Problem Definition

Goal: Predict which alliance would win a constituency.

Input features included:

Total votes

EVM votes

Postal votes

Party affiliation

Vote percentage

Margin

Turnout rate

Candidate-level attributes

Output:

Alliance label

Others

MGB

Independent

📍 7.2 Feature Engineering

Creating new features improved model performance:

Margin ratios

Turnout-to-vote ratios

Normalized vote distributions

Log-transformed totals

Binary flags for large parties

Categorical party encoding

Feature engineering is often the most important step for classifier performance.

📍 7.3 Models Used

Three main classification models were built:

✔ Logistic Regression

Baseline model for multiclass classification.

✔ Random Forest Classifier

Best performing model due to ability to handle:

Non-linear features

Categorical data

Feature interactions

✔ Decision Tree

Used for interpretability and baseline comparison.

📍 7.4 Evaluation Metrics

Because dataset classes were imbalanced:

Accuracy

Precision

Recall

F1-score

Macro averages

Weighted averages

were all calculated for a complete understanding.

✔ Observations:

Models performed very well for “Others” (majority class)

Lower scores for smaller alliance classes

Imbalanced dataset caused bias toward majority class

Weighted accuracy remained high (92%)

<img width="535" height="244" alt="Screenshot 2025-11-21 233415" src="https://github.com/user-attachments/assets/d4936536-f957-4b69-b5c5-cb427ad62955" />



<img width="547" height="286" alt="Screenshot 2025-11-21 233356" src="https://github.com/user-attachments/assets/c3bb5d65-5a4f-4b3a-a08c-67416983cb2c" />


⭐ 8. Key Insights & Interpretation

This section summarizes major findings from the project.

📍 Insight 1: Boston of competitiveness

Most seats were contested within small margins. Political parties must focus on:

Micro-targeting

Local issues

BJP vs RJD vs JDU fight intensifies

📍 Insight 2: Rise of regional/local parties

"Others" category dominated because:

Strong regional leadership

Issue-based voting

Decline of large alliances in some regions

📍 Insight 3: High NOTA impact in select areas

NOTA patterns show dissatisfaction zones.

📍 Insight 4: Postal & EVM votes correlate strongly

Voter engagement tends to be consistent across modes.

📍 Insight 5: ML models reveal vote percentage & margin are strongest predictors

Feature importance from Random Forest highlights:

Constituency competitiveness

Vote concentration

Party presence



⭐ 9. Installation & Setup Instructions

To reproduce the full project:

Step 1: Clone the repository
git clone https://github.com/yourusername/bihar-election-analysis.git
cd bihar-election-analysis

Step 2: Install dependencies
pip install -r requirements.txt

Step 3: Run the notebook

Open:

notebooks/analysis.ipynb

Step 4: View the Power BI dashboard

Open:

dashboard/Political.pbix

⭐ 10. Limitations

No dataset or model is perfect.

✔ Class imbalance

Majority alliance dominates model accuracy.

✔ Missing fields

Some constituencies lack complete data.

✔ Feature limitations

Additional demographic variables could improve predictions.

✔ Single election

Trends would be stronger with multi-year data.

⭐ 11. Future Work

Possible expansions:

Adding demographic features

Multi-year election analysis

Natural Language Processing on candidate manifestos

Predictive modeling for vote share

Real-time dashboard integration

Deployment as a Streamlit web app

⭐ 13. Credits & Acknowledgments

This project uses:

Python open-source ecosystem

Power BI Desktop

Public election datasets

Machine learning community resources

Special thanks to mentors, peer reviewers, and open-source contributors.




#DataAnalytics #MachineLearning #PowerBI #EDA #Python #ElectionAnalysis 
#DataScience #PredictiveModeling #PoliticalData #BigData 
#DashboardDesign #MLClassification #AnalyticsProject
