# 🛍️ Review Opinion Miner: What Customers Praise and Complain About

**DS311 Data Mining · Fall 2026 · Project P05**

An end-to-end opinion-mining pipeline on **Amazon Reviews 2023 (All_Beauty)**. It finds what customers praise and complain about in beauty products, and how those opinions shift over time. Results are served in an interactive Streamlit dashboard.

🔗 **Live app:** [add Streamlit link] · 📓 **Main notebook:** [`notebooks/A1_load_and_overview.ipynb`](notebooks/A1_load_and_overview.ipynb)

---

## 📌 Project overview

Online shoppers can't touch or test a product, so they rely on reviews. But no brand can read hundreds of thousands of them. This project turns raw reviews into answers: which product aspects are praised or criticised, what the main complaint themes are, and which reviews other customers will find helpful.

**Why All_Beauty?** About 700K reviews fit free Colab after sampling. Beauty reviews name concrete aspects (scent, texture, packaging, skin reaction, price, authenticity) that suit aspect mining. The category also has clear industrial value as a voice-of-customer tool for cosmetics brands and e-commerce sellers.

## ❓ Questions we answer

| # | Question | Technique |
|---|---|---|
| Q1 | Which aspects do customers praise or criticise, by brand? | TF-IDF sentiment classifier (Naive Bayes / Logistic Regression) + FP-Growth aspect mining |
| Q2 | What are the main complaint themes, and which are growing over time? | TruncatedSVD + K-means clustering on negative reviews |
| Q3 | Can we predict whether a review will be found helpful when it is posted? | Classification (Logistic Regression, Decision Tree, Random Forest) with a time-based split |

## 📊 Dataset

| | |
|---|---|
| **Source** | [Amazon Reviews 2023](https://amazon-reviews-2023.github.io/), McAuley Lab, UC San Diego |
| **Category** | All_Beauty (reviews up to Sept 2023) |
| **Size** | 701.5K reviews · 632.0K users · 112.6K products |
| **Tables** | Reviews (rating, text, date, helpful votes, verified purchase) joined with product metadata (title, brand, price) on `parent_asin` |
| **Labels** | From stars: 1–2 negative, 3 neutral, 4–5 positive; checked against 200 hand-labelled reviews |
| **Licence** | Released by McAuley Lab for research use; see the dataset page |

> The raw data is **not stored in this repo** (it's too large). The notebooks download it from the official source.

## 🔄 Pipeline

```
Acquire & join → Data quality → EDA + hypothesis tests → Transform & reduce
      → Mine (sentiment · aspects · clustering · helpfulness) → Evaluate → Interpret → Serve (Streamlit)
```

Every model uses scikit-learn `Pipeline`s (preprocessing fitted on training folds only), fixed seeds (`42`), comparison against a baseline, and an explicit leakage check.

## 📁 Repository structure

```
review-opinion-miner/
├── notebooks/        # Colab notebooks (prefixed by owner: A_, B_, C_)
├── src/              # Shared code: data loading and cleaning (common.py)
├── app/              # Streamlit dashboard
├── outputs/          # Saved models and result CSVs used by the app
├── figures/          # Charts for the report and proposal
├── reports/          # Proposal, work split, final report
├── requirements.txt
└── README.md
```

## ▶️ How to run

**Option 1: Google Colab (recommended)**
1. Open a notebook from `notebooks/` in Colab (File → Open notebook → GitHub tab → paste this repo's link).
2. Runtime → **Run all**. The data downloads automatically.

**Option 2: Locally**
```bash
git clone https://github.com/[your-username]/review-opinion-miner.git
cd review-opinion-miner
pip install -r requirements.txt
```

**Run the dashboard**
```bash
streamlit run app/streamlit_app.py
```

## 👥 Team

| Member | Lead role | Owns |
|---|---|---|
| [Name A] | Data lead | Data loading, data quality, EDA, helpfulness classification (Q3) |
| [Name B] | Mining lead | Sentiment model, FP-Growth aspect mining (Q1) |
| [Name C] | Product lead | Complaint clustering (Q2), dashboard, deployment, report |

## 🗓️ Milestones

| Week | Milestone | Status |
|---|---|---|
| 6 | M0: Proposal, dataset loaded, repo created | ✅ |
| 7 | M1: Data quality and EDA | ⏳ |
| 8 | M2: First model / first rules | ⏳ |
| 11 | M3: Core complete, app skeleton | ⏳ |
| 13 | M4: Full evaluation, app deployed | ⏳ |
| 16 | Final submission, demo and viva | ⏳ |

## 📝 Contribution log

| Date | Member | What was done |
|---|---|---|
| 2026-10-13 | [Name A] | Created repo, loaded dataset, category overview charts, proposal |
| | [Name B] | |
| | [Name C] | |

## 🤖 AI-use disclosure

Generative AI tools were used for [describe, e.g. planning the work split, drafting boilerplate code, and proofreading]. All code was reviewed, run and understood by the team.

## 📚 Citation

```bibtex
@article{hou2024bridging,
  title   = {Bridging Language and Items for Retrieval and Recommendation},
  author  = {Hou, Yupeng and Li, Jiacheng and He, Zhankui and Yan, An and Chen, Xiusi and McAuley, Julian},
  journal = {arXiv preprint arXiv:2403.03952},
  year    = {2024}
}
```