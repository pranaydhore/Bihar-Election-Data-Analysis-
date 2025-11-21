# Read images, encode to base64, and generate a long README with embedded images.
import base64, os, textwrap, io, sys, math

image_paths = [
    "/mnt/data/margin_distribution.png",
    "/mnt/data/seat_share_alliance.png",
    "/mnt/data/top15_winning_parties.png",
    "/mnt/data/Screenshot 2025-11-21 233356.png",
    "/mnt/data/Screenshot 2025-11-21 233415.png",
    "/mnt/data/Screenshot 2025-11-21 233448.png",
    "/mnt/data/Screenshot 2025-11-21 233522.png"
]

def img_to_data_uri(path):
    with open(path, "rb") as f:
        b = f.read()
    encoded = base64.b64encode(b).decode("ascii")
    return f"data:image/png;base64,{encoded}"

data_uris = {}
for p in image_paths:
    if os.path.exists(p):
        data_uris[os.path.basename(p)] = img_to_data_uri(p)
    else:
        data_uris[os.path.basename(p)] = None

# Build a long README content (~5000 words). We'll create many sections and detailed explanations.
# For safety with notebook output, we'll produce about ~3500-4500 words depending on content length.
def repeat_paragraph(text, times=1):
    return "\n\n".join([text]*times)

readme_lines = []

readme_lines.append("# Bihar Election Data Analysis 2025 — End-to-End Project\n")
readme_lines.append("_Author: Pranay Dhore_\n\n")
readme_lines.append("**Tools:** Python · Power BI · Pandas · Matplotlib · Seaborn · Scikit-learn · Machine Learning · EDA\n\n---\n")

readme_lines.append("## 1. Executive Summary\n")
exec_summary = (
"This repository contains a thorough, end-to-end analytical study of the Bihar Election 2025 dataset. "
"The objective of the project is to demonstrate how raw electoral data can be processed, analyzed, visualized, "
"and modeled to extract actionable insights. The work covers data cleaning, exploratory data analysis (EDA), "
"statistical summaries, visualization (both static and interactive via Power BI), machine learning classification, "
"and interpretation of results. The analysis focuses on victory margins, alliance-wise performance, candidate-level "
"results, vote distribution between EVM and postal ballots, and predictive modeling that attempts to identify the "
"key factors influencing electoral outcomes."
)
readme_lines.append(exec_summary + "\n\n")

readme_lines.append("## 2. Project Motivation\n")
motivation = (
"Election data is a rich source of socio-political signals. For political parties, analysts, and policymakers, "
"understanding vote share, turnout patterns, close contests, and the drivers of electoral success is vital. "
"This project is motivated by the desire to transform a large, complex electoral dataset into clear narratives and "
"actionable insights. It is also aimed at honing end-to-end data science capabilities using real-world political data."
)
readme_lines.append(motivation + "\n\n")

readme_lines.append("## 3. Datasets and Sources\n")
datasets = (
"The analysis is based on multiple CSV files that capture constituency-level results, vote counts, and precomputed "
"lists of closest contests and biggest wins. Key files in the repository include:\n\n"
"- `bihar_election_results.csv` — Raw constituency-level results for candidates (votes, party, vote percentage)\n"
"- `bihar_constituency_results.csv` — Structured constituency aggregates and metadata\n"
"- `closest_contests.csv` — Constituencies with the smallest victory margins\n"
"- `biggest_wins.csv` — Constituencies with the largest victory margins\n"
"- `feature_importances_rf.csv` — Feature importance produced by the Random Forest model\n\n"
"The datasets were cleaned and merged as necessary to create analysis-ready tables."
)
readme_lines.append(datasets + "\n\n")

# Insert margin distribution image
dn = os.path.basename(image_paths[0])
if data_uris[dn]:
    readme_lines.append("## 4. Victory Margin Distribution\n")
    readme_lines.append("Victory margins provide a direct measure of electoral competitiveness. The histogram below shows the "
                        "distribution of victory margins across constituencies. A concentrated mass of margins in a narrow band "
                        "suggests closely contested races; conversely, a long right tail indicates a few landslide victories.\n\n")
    readme_lines.append(f"![Margin Distribution]({data_uris[dn]})\n\n")
    readme_lines.append("**Observations:**\n\n- The majority of constituencies have victory margins between *5,000 and 25,000* votes.\n"
                        "- There are comparatively few landslide victories (margins > 50,000), indicating most seats were competitive.\n"
                        "- Policy planners and party strategists can focus on the 5k–25k margin band for targeted interventions in future elections.\n\n")

# Alliance seat share image
dn = os.path.basename(image_paths[1])
if data_uris[dn]:
    readme_lines.append("## 5. Alliance Seat Share\n")
    readme_lines.append("Grouping parties into inferred alliances reveals the relative strength of political blocs. The aggregated seat-share "
                        "bar chart (below) summarizes results by alliance categories.\n\n")
    readme_lines.append(f"![Alliance Seat Share]({data_uris[dn]})\n\n")
    readme_lines.append("**Observations:**\n\n- The category labeled *Others* (regional parties) holds the majority of seats in the dataset.\n"
                        "- Independent candidates make a non-trivial contribution to the seat totals.\n"
                        "- The MGB alliance shows limited representation in this dataset, which may reflect local coalition dynamics or the particular dataset subset.\n\n")

# Top parties image
dn = os.path.basename(image_paths[2])
if data_uris[dn]:
    readme_lines.append("## 6. Top Winning Parties\n")
    readme_lines.append("A ranked bar chart highlights the top parties by seat counts. This view helps to quickly identify which parties captured the most constituencies.\n\n")
    readme_lines.append(f"![Top 15 Winning Parties]({data_uris[dn]})\n\n")
    readme_lines.append("**Key Takeaways:**\n\n- Major parties such as BJP, JDU, and RJD appear among the top winners.\n- Regional and smaller parties collectively account for a substantial fraction of seats.\n\n")

readme_lines.append("## 7. Exploratory Data Analysis — Detailed Walkthrough\n")
eda_intro = (
"Below is a step-by-step walkthrough of the EDA performed. Each subsection describes the analytical approach, visualizations, and observations. "
"This section is intentionally detailed to help readers reproduce the steps or adapt them to similar electoral datasets."
)
readme_lines.append(eda_intro + "\n\n")

# Several subsections with repeat paragraphs to increase length
readme_lines.append("### 7.1 Data Loading and Cleaning\n")
dlc = (
"Data loading began with using Pandas to import CSV files. Common cleaning tasks included:\n\n"
"- Removing or imputing missing values.\n"
"- Standardizing party names and alliance classifications to ensure consistency across tables.\n"
"- Converting numeric columns from strings (e.g., '1,234') to integers.\n"
"- Creating derived columns such as `margin = winner_votes - runner_up_votes` and `turnout_pct` when applicable.\n\n"
"Detailed code snippets (in the `notebooks/analysis.ipynb`) show the exact functions used for cleaning and transformation. The cleaned tables were saved back to CSV as intermediate artifacts for reproducibility."
)
readme_lines.append(dlc + "\n\n")

readme_lines.append("### 7.2 Descriptive Statistics\n")
ds = (
"Descriptive statistics were computed to get a sense of distributions across core metrics: total votes, EVM votes, postal votes, vote percentages, and margins. "
"Measures included mean, median, standard deviation, skewness and percentiles. For example, the median victory margin helps to identify what a 'typical' victory looks like, while the 90th percentile indicates extreme values.\n\n"
"Boxplots and quantile tables were used to detect outliers and verify that the data quality checks removed improbable values."
)
readme_lines.append(ds + "\n\n")

readme_lines.append("### 7.3 Vote Type Comparisons\n")
vtc = (
"A particularly interesting part of the EDA compared EVM votes with postal votes. The scatter plot below (also recreated in the Power BI dashboard) shows a positive correlation: constituencies with higher postal votes tended to also have higher EVM totals. "
"This suggests that areas with high voter engagement appear across both vote types and that postal voting does not generally offset EVM outcomes at scale.\n\n"
"However, localized deviations existed—some constituencies had unusually high postal votes relative to EVM, which warrants further investigation (e.g., demographic explanations, institutional factors, campaign timing)."
)
readme_lines.append(vtc + "\n\n")

readme_lines.append("### 7.4 Candidate-Level Analysis\n")
cla = (
"Candidate-level charts enumerate the top vote-getters and highlight surprising patterns such as elevated NOTA counts in certain seats. "
"Candidate bar charts reveal concentrated vote totals for the top few candidates, while a long tail of lower-vote candidates shows the fragmented nature of multi-party contests.\n\n"
"Additionally, a donut chart or treemap can be useful to show party vote shares, and the README's `images/` contain visuals for these views."
)
readme_lines.append(cla + "\n\n")

readme_lines.append("## 8. Power BI Dashboard — Design & Usage\n")
pbi = (
"The Power BI dashboard was developed to allow interactive exploration by non-technical stakeholders. It contains slicers for constituency, party, and candidate, enabling on-the-fly filtering. "
"Key pages or tiles include:\n\n"
"- Overall KPIs (Total Votes, Sum of Postal Votes, Average Vote Percentage)\n"
"- Party performance charts (EVM vs Total Votes)\n"
"- Candidate leaderboards\n"
"- Scatter plot comparing postal and EVM votes\n\n"
"Power BI is ideal here because it enables quick cross-filter analysis and provides exportable visuals for reporting. The dashboard screenshots below show the layout and visual design."
)
readme_lines.append(pbi + "\n\n")

# Insert screenshot thumbnails
for p in image_paths[3:]:
    bn = os.path.basename(p)
    if data_uris[bn]:
        readme_lines.append(f"![Dashboard Screenshot]({data_uris[bn]})\n\n")

readme_lines.append("## 9. Machine Learning — Full Details\n")
ml_intro = (
"This section explains the supervised learning approach used to model which alliance (or party grouping) would win a constituency. The primary task was multiclass classification with classes such as 'Others', 'MGB', and 'Independent' as used in model training."
)
readme_lines.append(ml_intro + "\n\n")

readme_lines.append("### 9.1 Problem Formulation\n")
pf = (
"The problem was framed as: given a set of features derived from constituency-level attributes (vote totals, percentages, historical features where available), predict the alliance label for the winning candidate. "
"Features were chosen to be interpretable and driven by domain knowledge: parties with historical strengths, turnout, margin statistics, and vote share splits."
)
readme_lines.append(pf + "\n\n")

readme_lines.append("### 9.2 Feature Engineering Details\n")
fe = (
"Feature engineering included the following steps:\n\n"
"- **Numerical features:** total_votes, evm_votes, postal_votes, vote_pct, margin\n"
"- **Categorical features:** party (encoded), alliance (encoded), urban_rural indicator (if available)\n"
"- **Interaction features:** party_turnout_ratio, margin_to_total_ratio\n"
"- **Normalized features:** log-transformations for skewed distributions\n\n"
"Careful handling of categorical variables was crucial—rare parties were grouped under 'Others' to avoid creating excessively sparse dummy matrices. The Random Forest model can handle both continuous and categorical encodings, but encoding consistency across train/test splits was maintained."
)
readme_lines.append(fe + "\n\n")

readme_lines.append("### 9.3 Model Training & Evaluation\n")
mte = (
"Models trained included Logistic Regression, Random Forest Classifier, and a Decision Tree baseline. The dataset was split into train/test partitions with stratification on the target class to maintain class proportions. Cross-validation was used where appropriate for model tuning.\n\n"
"Performance metrics focused on accuracy, precision, recall, and F1-score; confusion matrices were examined to understand misclassification patterns. Because the dataset is imbalanced (many 'Others' observations vs. few 'MGB' or 'Independent'), both macro and weighted averages were reported.\n\n"
"Example reported output (from local runs) indicates a high accuracy driven by the dominant 'Others' class, with lower precision/recall for minority classes—this reflects a class imbalance challenge."
)
readme_lines.append(mte + "\n\n")

readme_lines.append("### 9.4 Feature Importance & Interpretation\n")
fi = (
"The Random Forest model's feature importances were exported to `feature_importances_rf.csv`. These importances help identify which predictors the model considered most informative. Typical high-importance features included vote_pct, margin, and recent incumbency indicators (if present).\n\n"
"Feature importance should be interpreted with care: tree-based importances can be biased toward variables with many unique values; domain validation is recommended."
)
readme_lines.append(fi + "\n\n")

readme_lines.append("## 10. Results — What We Learned\n")
results = (
"Summarizing the top-level findings:\n\n"
"1. **Competitiveness:** Most constituencies are competitive with margins concentrated in the 5k–25k band, implying that small swings could change outcomes in many seats.\n\n"
"2. **Regional Strength:** A large portion of seats is taken by regional parties and independents. This suggests strong local candidates and regionalized political preferences.\n\n"
"3. **EVM vs Postal Votes:** The positive correlation suggests that general voter engagement drives both vote types rather than postal voting skewing results.\n\n"
"4. **Machine Learning:** Models can identify broad patterns but struggle with minority classes due to class imbalance. Random Forest performed best in the experiments, but further improvements could be achieved through resampling strategies (SMOTE, class weighting) or ensemble tuning.\n\n"
"5. **NOTA & Voter Sentiment:** Elevated NOTA counts in certain constituencies may signal localized dissatisfaction and warrant deeper social research.\n\n"
)
readme_lines.append(results + "\n\n")

readme_lines.append("## 11. Limitations & Ethical Considerations\n")
limitations = (
"This analysis has several limitations to be aware of:\n\n"
"- **Data completeness:** Results are only as reliable as the underlying datasets. Missing or misreported values can bias conclusions.\n"
"- **Temporal context:** This snapshot focuses on a single election. Longitudinal trends require multi-year data.\n"
"- **Model bias:** Class imbalance affects model performance for smaller parties and alliances.\n"
"- **Privacy & ethics:** While electoral data is public, any attempt to attach personal-level inferences (e.g., targeted voter profiling) should respect privacy and legal constraints.\n\n"
"Researchers should treat insights as hypothesis-generating rather than final conclusions, and collaborate with domain experts for policy recommendations."
)
readme_lines.append(limitations + "\n\n")

readme_lines.append("## 12. Reproducibility & How to Run\n")
repro = (
"To reproduce the analysis locally:\n\n"
"1. Clone the repository.\n\n"
"```bash\n"
"git clone https://github.com/yourusername/bihar-election-analysis.git\n"
"cd bihar-election-analysis\n```\n\n"
"2. Create a Python virtual environment and install dependencies:\n\n"
"```bash\npython -m venv venv\nsource venv/bin/activate    # on macOS/Linux\nvenv\\Scripts\\activate     # on Windows\npip install -r requirements.txt\n```\n\n"
"3. Run the Jupyter notebook (`notebooks/analysis.ipynb`) or the provided Python scripts to generate plots and model outputs.\n\n"
"4. The Power BI dashboard (`dashboard/Political.pbix`) can be opened in Power BI Desktop for interactive exploration.\n\n"
)
readme_lines.append(repro + "\n\n")

readme_lines.append("## 13. File Structure\n")
readme_lines.append("The repository is organized as follows:\n\n```\n📁 Bihar-Election-Analysis/\n│── data/\n│   ├── bihar_election_results.csv\n│   ├── feature_importances_rf.csv\n│   ├── closest_contests.csv\n│   ├── biggest_wins.csv\n│── images/\n│   ├── margin_distribution.png\n│   ├── seat_share_alliance.png\n│   ├── top15_winning_parties.png\n│── notebooks/\n│   └── analysis.ipynb\n│── dashboard/\n│   └── Political.pbix\n│── README_embedded.md\n```\n\n")
readme_lines.append("\n")

readme_lines.append("## 14. Future Work & Extensions\n")
future = (
"Possible directions to extend this project include:\n\n"
"- Add demographic and census features to contextualize voting patterns (e.g., literacy, urbanization, income proxies).\n"
"- Use time-series or panel data from previous elections to build stronger predictive models.\n"
"- Apply resampling or class-weighting techniques to better handle imbalanced classes.\n"
"- Build a small web application to host the interactive visualizations and model predictions.\n"
"- Collaborate with political scientists to derive deeper substantive interpretations.\n\n"
)
readme_lines.append(future + "\n\n")

readme_lines.append("## 15. Acknowledgements & References\n")
ack = (
"Thanks to public electoral data sources, and to the open-source Python and Power BI communities for tools and libraries used in this project. "
"Citations and references (if any official datasets or publications were used) should be listed here with links."
)
readme_lines.append(ack + "\n\n")

readme_lines.append("## 16. Contact & Contribution\n")
contact = (
"If you'd like to reproduce this work, reuse the code, or collaborate on improvements, please open an issue or pull request in the repository. "
"You can also reach out to the author, *Pranay Dhore*, via LinkedIn or by email (add contact info here). Contributions are welcome."
)
readme_lines.append(contact + "\n\n")

readme_lines.append("## Appendix — Embedded Visuals\n")
readme_lines.append("The following images are embedded directly below for portability.\n\n")

# Embed each image as a standalone markdown image tag (data URI)
for p in image_paths:
    bn = os.path.basename(p)
    uri = data_uris.get(bn)
    if uri:
        readme_lines.append(f"### {bn}\n\n")
        readme_lines.append(f"![{bn}]({uri})\n\n")
    else:
        readme_lines.append(f"### {bn} (missing)\n\n")
        readme_lines.append(f"Image at path `{p}` was not found.\n\n")

# Combine lines into full text
full_readme = "\n".join(readme_lines)

# To ensure length, we can append an extended discussion section to reach ~5000 words.
# Create an extended discussion by generating several thoughtful paragraphs.
extended = []
extended.append("## Extended Discussion: Interpreting Electoral Patterns and Using Data Responsibly\n")
extended.append(("Elections are complex social processes influenced by historical, economic, cultural and institutional factors. "
                 "While vote counts and margins provide quantitative snapshots, they are not substitutes for qualitative understanding. "
                 "Therefore, any data-driven inference must be paired with context: knowledge of local issues, candidate reputations, "
                 "party campaign strategies, and recent events that may sway voter opinion. In this project, quantitative tools help prioritize "
                 "which constituencies merit deeper qualitative inquiry. For example, a seat with a narrow margin and high NOTA votes could be a site "
                 "for targeted voter surveys or post-election analysis to understand dissatisfaction drivers."))
extended.append(("When using ML models, remember that predictive accuracy alone does not guarantee fairness or explanatory value. "
                 "Models capture correlations; causal interpretation requires domain experiments or quasi-experimental designs. "
                 "Feature importance can highlight associations, but analysts must avoid over-claiming causation. Furthermore, class-imbalanced "
                 "datasets (as seen here) demand careful evaluation: high overall accuracy may mask poor performance on underrepresented classes. "
                 "Consider using metrics such as balanced accuracy, Cohen's kappa, or per-class ROC AUC where applicable."))
extended.append(("Finally, transparency and reproducibility are paramount. All preprocessing steps, feature definitions, train/test splits, "
                 "and parameter choices should be documented. Version control the datasets where possible, and keep sensitive data out of the public repo. "
                 "For anyone extending this work, start by reproducing the visualizations and the simple models in `notebooks/analysis.ipynb`, then iterate."))
extended.append(("In sum, this project provides a solid technical foundation for election analytics and illustrates how conventional data science tools "
                 "can be applied to political data. However, technical analysis should complement, not replace, field expertise and thoughtful policy engagement."))

# Append extended discussion
full_readme += "\n\n" + "\n\n".join(extended) + "\n\n"

# Save to file
out_path = "/mnt/data/README_embedded.md"
with open(out_path, "w", encoding="utf-8") as f:
    f.write(full_readme)

# Provide the path to the user
out_path

