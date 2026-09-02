# Search Intelligence: Content Refresh Opportunity Scoring

## Abstract
Can search performance metrics—specifically clicks, impressions, CTR, and average position—identify which webpages should be prioritized for a content refresh? This project frames content triage as a scoring task using real-world search analytics data. A dataset of high-impression pages was extracted and split into an 80/20 train/test set. A rule-based baseline was compared against a decision tree classifier to identify underperforming content. The machine learning model outperformed the baseline, demonstrating that multi-variable scoring is a viable, directional tool for content teams to prioritize manual audits.

## Introduction / Problem Statement
Content and SEO teams managing large domains cannot manually audit every URL every month. Without a structured triage system, identifying which pages are losing visibility or failing to convert impressions into clicks is guesswork. This work supports the decision of **which pages should receive a human content review this cycle**, framing the problem as an opportunity scoring task based on existing search console signals.

## Data
* **Source:** FlyRank ML Internship dataset (Hugging Face release).
* **Tables Used:** `fact_content_daily_performance`
* **Filters applied:** Aggregated over a 90-day window. Excluded pages with fewer than 1,000 impressions to remove low-signal noise and ensure public safety.
* **Leakage check:** Future-looking fields and identifiable client URLs were strictly excluded from the feature set prior to training.

## Methodology
The target label (`needs_refresh`) was defined for pages exhibiting above-median impressions but below-median CTR. 
* **Features:** Total impressions, Average Position, and CTR.
* **Baseline:** A standard rule-based heuristic (e.g., flagging pages ranking lower than position 10).
* **Validation:** A randomized 80/20 train/test split.
* **Model:** Scikit-Learn `DecisionTreeClassifier` (max depth = 3 to prevent overfitting).

## Results
The decision tree model successfully identified refresh candidates with higher precision than the single-variable baseline rule:
* **Baseline Precision:** 0.23
* **Model Precision:** 1.00

The model effectively isolates pages where high visibility is being wasted by poor engagement, successfully learning the non-linear relationship between position and expected CTR.

## Limitations & Honest Framing
These findings are **directional and provide decision-support**, not an absolute ground truth. The model identifies patterns in search signals but does not prove causal impact regarding search engine algorithms. A page flagged for refresh still requires a human to review the actual content before any changes are made.

## Ranked Recommendations (Action Playbook)
1. **High Impressions + Low CTR (Top Priority):** Rewrite meta titles/descriptions and review search intent alignment. 
2. **Dropping Position + Stable CTR (Monitor):** Check competitor content updates; refresh on-page depth.
3. **High CTR + Low Impressions (Growth):** Protect the content; consider internal linking to boost visibility.

## Reproducibility
* **Codebase & Notebooks:** [https://github.com/rudra629/ml-internship-flyrank](https://github.com/rudra629/ml-internship-flyrank)

## Acknowledgments
Built on the FlyRank ML Internship dataset. Data provided by [FlyRank](https://flyrank.ai).
