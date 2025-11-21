# Generate an HTML version of the long README with embedded base64 images and save to /mnt/data/bihar_readme_embedded.html
import os, base64, textwrap
image_files = [
    "margin_distribution.png",
    "seat_share_alliance.png",
    "top15_winning_parties.png",
    "Screenshot 2025-11-21 233356.png",
    "Screenshot 2025-11-21 233415.png",
    "Screenshot 2025-11-21 233448.png",
    "Screenshot 2025-11-21 233522.png"
]

def encode_image(path):
    with open(path, "rb") as f:
        return "data:image/png;base64," + base64.b64encode(f.read()).decode("ascii")

encoded = {}
for p in image_files:
    if os.path.exists(p):
        encoded[os.path.basename(p)] = encode_image(p)
    else:
        encoded[os.path.basename(p)] = None

html = f"""<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Bihar Election Data Analysis 2025 — Full Report</title>
<style>
  body{{font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial; color:#222; background:#f8fafc; margin:0; padding:24px;}}
  .wrap{{max-width:1100px;margin:0 auto;background:#fff;padding:32px;border-radius:10px;box-shadow:0 6px 30px rgba(16,24,40,0.08);}}
  h1{{margin-top:0;font-size:30px;color:#0b3b66;}}
  h2{{color:#0b3b66;margin-top:28px;}}
  p.lead{{font-size:16px;color:#333;}}
  pre.code{{background:#0f1724;color:#fff;padding:12px;border-radius:8px;overflow:auto;}}
  img.preview{{max-width:100%;height:auto;border:1px solid #e6eef6;border-radius:6px;margin:12px 0;}}
  ul{{line-height:1.6}}
  .kpis{{display:flex;flex-wrap:wrap;gap:10px;margin:12px 0}}
  .badge{{background:#eef6ff;color:#024b8a;padding:8px 12px;border-radius:8px;font-weight:600}}
  footer{{font-size:13px;color:#667085;margin-top:28px;border-top:1px solid #eef2ff;padding-top:14px}}
  .file-links a{{display:block;color:#0969da;text-decoration:none;margin:6px 0}}
</style>
</head>
<body>
  <div class="wrap">
    <h1>Bihar Election Data Analysis 2025 — End-to-End Project</h1>
    <p class="lead"><strong>Author:</strong> Pranay Dhore &nbsp;|&nbsp; <strong>Tools:</strong> Python · Power BI · Pandas · Matplotlib · Seaborn · Scikit-learn · Machine Learning · EDA</p>

<h2>1. Executive Summary</h2>
    <p>This report documents a thorough, end-to-end analysis of the Bihar Election 2025 dataset and demonstrates how raw electoral records can be processed into meaningful insights. The analysis includes data cleaning, exploratory data analysis (EDA), visual storytelling with static and interactive visuals (Power BI), and machine learning classification to surface predictive signals.</p>
    <h2>2. Project Motivation</h2>
    <p>Election data provides a valuable lens into civic behaviour and local politics. By combining EDA, visualization and predictive models this project aims to: (a) reveal voting patterns, (b) identify competitive constituencies, (c) compare party and alliance performance, and (d) provide reproducible analyses and dashboards for stakeholders.</p>
    <h2>3. Dataset & Files</h2>
    <p>The work uses the following datasets and artifacts (available in the repository):</p>
    <div class="file-links">
      <a href="bihar_election_results.csv">/mnt/data/bihar_election_results.csv</a>
      <a href="bihar_constituency_results.csv">/mnt/data/bihar_constituency_results.csv</a>
      <a href="closest_contests.csv">/mnt/data/closest_contests.csv</a>
      <a href="biggest_wins.csv">/mnt/data/biggest_wins.csv</a>
      <a href="feature_importances_rf.csv">/mnt/data/feature_importances_rf.csv</a>
      <a href="political.pdf">/mnt/data/political.pdf</a>
      <a href="Political.pbix">/mnt/data/Political.pbix</a>
    </div>

<h2>4. Victory Margin Distribution</h2>
    <p>Victory margins measure how decisive or competitive a race is. Below is the margin distribution histogram; most seats lie in a 5k–25k margin range. This clustering indicates many constituencies are within reach of small vote swings.</p>
    <img src="{encoded.get('margin_distribution.png')}" alt="Margin distribution" class="preview" />

 <h2>5. Alliance Seat Share</h2>
    <p>Grouping winners into alliance categories helps reveal coalition strength. The chart below shows the seat share by inferred alliance.</p>
    <img src="{encoded.get('seat_share_alliance.png')}" alt="Seat share by alliance" class="preview" />

 <h2>6. Top Winning Parties</h2>
    <p>Top parties by seats are plotted below. Major national parties appear at the top while regional parties and independents collectively form a significant portion.</p>
    <img src="{encoded.get('top15_winning_parties.png')}" alt="Top winning parties" class="preview" />

  <h2>7. Detailed Exploratory Data Analysis</h2>
    <h3>7.1 Data loading & cleaning</h3>
    <p>Data was ingested using Pandas. Key cleaning steps:</p>
    <ul>
      <li>Standardize party and alliance names</li>
      <li>Convert numeric fields to integers</li>
      <li>Impute or remove missing records where appropriate</li>
      <li>Create derived metrics such as <code>margin = winner_votes - runner_up_votes</code></li>
    </ul>
    <pre class="code"># Example (see notebooks/analysis.ipynb)
df = pd.read_csv('data/bihar_election_results.csv')
df['total_votes'] = df['total_votes'].str.replace(',','').astype(int)
df['margin'] = df['winner_votes'] - df['runner_up_votes']</pre>

 <h3>7.2 Descriptive statistics</h3>
    <p>We computed central tendency and spread metrics, including medians and percentiles for vote totals and margins. Boxplots and quantile analyses were used to flag outliers.</p>

<h3>7.3 Vote type comparisons</h3>
    <p>Comparing EVM and postal votes highlighted a generally positive correlation — regions with higher postal participation also tended to have higher EVM totals. However, a handful of constituencies deviated from this pattern, suggesting localized behaviors worth further study.</p>

  <h3>7.4 Candidate-level patterns</h3>
    <p>Candidate leaderboards and candidate-level bar charts expose concentrated vote winners and a long tail of low-vote candidates. Several seats show elevated NOTA tallies, which might reflect voter dissatisfaction or protest votes.</p>

  <h2>8. Power BI Dashboard — Design & Highlights</h2>
    <p>The Power BI dashboard organizes interactive analysis for stakeholders. Key elements include KPIs (sum of EVM votes, postal votes), party leaderboards, scatter plots, and slicers for on-demand filtering.</p>
    <img src="{encoded.get('Screenshot 2025-11-21 233356.png')}" alt="Power BI page 1" class="preview" />
    <img src="{encoded.get('Screenshot 2025-11-21 233415.png')}" alt="Power BI page 2" class="preview" />
    <img src="{encoded.get('Screenshot 2025-11-21 233448.png')}" alt="Power BI page 3" class="preview" />
    <img src="{encoded.get('Screenshot 2025-11-21 233522.png')}" alt="Power BI page 4" class="preview" />

<h2>9. Machine Learning</h2>
    <p>The ML component framed a multiclass classification: predict the alliance label for the winning candidate based on constituency-level features.</p>

<h3>9.1 Feature engineering</h3>
    <p>Constructed features included:</p>
    <ul>
      <li><strong>Numerical:</strong> total_votes, evm_votes, postal_votes, vote_pct, margin</li>
      <li><strong>Categorical:</strong> encoded party and alliance; rare parties grouped as 'Others'</li>
      <li><strong>Interaction:</strong> margin_to_total_ratio, party_turnout_ratio</li>
      <li>Log transforms for skewed features</li>
    </ul>

 <h3>9.2 Model training & evaluation</h3>
    <p>Models tested: Logistic Regression, Random Forest, Decision Tree. Data splits were stratified to maintain class proportions; cross-validation and grid search used for tuning.</p>
    <p>Due to class imbalance (dominance of 'Others'), we reported both macro and weighted metrics. The Random Forest produced the most stable results in experiments, but minority classes (MGB, Independent) had low recall due to few examples.</p>

 <h3>9.3 Feature importance</h3>
    <p>Feature importances exported to <code>feature_importances_rf.csv</code> revealed vote percentage, margin, and recent incumbency indicators as top predictors. Interpret these carefully — tree importances can be biased for variables with many categories.</p>

 <h2>10. Results & Insights</h2>
    <ul>
      <li><strong>Competitiveness:</strong> Most seats have margins in 5k–25k, suggesting many marginal constituencies.</li>
      <li><strong>Regional strength:</strong> Local parties and independents aggregate to a significant seat share.</li>
      <li><strong>EVM vs Postal:</strong> Positive correlation indicates consistent engagement across vote types.</li>
      <li><strong>ML:</strong> Predictive models can capture broad patterns but require treatment for class imbalance.</li>
      <li><strong>NOTA:</strong> Elevated NOTA in some areas suggests pockets of voter dissatisfaction.</li>
    </ul>

 <h2>11. Limitations & Ethics</h2>
    <p>Key limitations include data completeness, single-election temporal scope, and model bias due to class imbalance. Ethically, avoid using these analyses for invasive micro-targeting; use them for research, reporting, and public policy deliberation.</p>

 <h2>12. Reproducibility & How to Run</h2>
    <ol>
      <li>Clone the repository:</li>
    </ol>
    <pre class="code">git clone https://github.com/yourusername/bihar-election-analysis.git
cd bihar-election-analysis</pre>
    <ol start="2">
      <li>Create virtual environment and install:</li>
    </ol>
    <pre class="code">python -m venv venv
source venv/bin/activate   # macOS/Linux
venv\\Scripts\\activate    # Windows
pip install -r requirements.txt</pre>
    <p>Open <code>notebooks/analysis.ipynb</code> to reproduce figures and model outputs. Open <code>dashboard/Political.pbix</code> with Power BI Desktop to interact with the dashboard.</p>

<h2>13. Project Structure</h2>
    <pre class="code">📁 Bihar-Election-Analysis/
│── data/
│   ├── bihar_election_results.csv
│   ├── feature_importances_rf.csv
│   ├── closest_contests.csv
│   ├── biggest_wins.csv
│── images/
│   ├── margin_distribution.png
│   ├── seat_share_alliance.png
│   ├── top15_winning_parties.png
│── notebooks/
│   └── analysis.ipynb
│── dashboard/
│   └── Political.pbix
│── README_embedded.md</pre>

<h2>14. Future Work</h2>
    <p>Extensions include adding demographic features, longitudinal data for trend analysis, advanced sampling methods to handle imbalance, deploying a web visualization app, and collaborating with political scientists for richer interpretation.</p>

<h2>15. References & Acknowledgements</h2>
    <p>Thanks to public electoral sources and open-source libraries that made this project possible. Please cite original datasets if you republish derived analyses.</p>

<h2>16. Contact</h2>
    <p>Author: <strong>Pranay Dhore</strong>. Connect on LinkedIn or open an issue/pull request in the repository for feature requests or questions.</p>

<h2>17. Embedded Visual Appendix</h2>
"""

# append embedded images in appendix
for fname, uri in encoded.items():
    if uri:
        html += f"<h3>{fname}</h3>\n<img src=\"{uri}\" alt=\"{fname}\" class=\"preview\" />\n\n"
    else:
        html += f"<h3>{fname} (missing)</h3>\n<p>Image file not found at the expected path.</p>\n\n"

html += """
    <footer>
      <p>Generated HTML export of the project README. Files referenced above are available in the repository paths listed. This standalone HTML includes embedded images for portability.</p>
    </footer>
  </div>
</body>
</html>
"""

out_path = "/mnt/data/bihar_readme_embedded.html"
with open(out_path, "w", encoding="utf-8") as f:
    f.write(html)

out_path

