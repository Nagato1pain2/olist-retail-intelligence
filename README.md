# Olist Retail Intelligence
**End-to-End E-Commerce Data Science & Business Analytics Case Study**

**Team:** Mahmoud & Ahmed  
**Project status:** Phase 01 — Business Understanding  
**Tools:** Google Colab, Python, SQL, GitHub; ML and deployment later.

## Business context
We are acting as a data science team serving a hypothetical e-commerce marketplace management group.
Our assignment is to define meaningful business questions using historical anonymized Olist marketplace data,
validate data limitations, and eventually build analytics, customer segmentation, and predictive models.

**Important:** The business scenario is a simulation. We must not invent results, imply causal impact
without evidence, or call sales revenue "profit" without cost data.

## Contributors
- **Mahmoud** — Data Scientist (GitHub profile: to be linked)
- **Ahmed** — Data Scientist (GitHub profile: to be linked)

Both teammates contribute through their own GitHub accounts, branches, commits, and pull requests.

## Current task — Phase 01
Start with [`docs/01_TASK_BRIEF.md`](docs/01_TASK_BRIEF.md). Independently prepare:
- [`Mahmoud's business notes`](docs/mahmoud_business_notes.md)
- [`Ahmed's business notes`](docs/ahmed_business_notes.md)

After reviewing each other's work, agree on the official [`Business Understanding`](docs/01_business_understanding.md).
Do **not** clean data, build ML models, or state computed findings at this stage.

## Data source
[Olist Brazilian E-Commerce Public Dataset (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).
Historical orders from 2016–2018, released in nine related CSV files. Download from the source;
the repository intentionally excludes third-party data. Check and follow the dataset's license terms.

## Working with Colab
- Open `.ipynb` files from GitHub in [Google Colab](https://colab.research.google.com/).
- Run `notebooks/00_colab_environment_check.ipynb` to verify your environment.
- Notebook runtimes are temporary: data must be mounted/uploaded again in a new session.
- Save notebooks back to the **correct feature branch**, not directly into `main`.
- If your Colab save dialog does not offer the desired branch, download the `.ipynb` and commit it
  to your feature branch through the GitHub website or local Git.
- Never commit Kaggle credentials, tokens, API keys, raw data or private Drive links.

## Roadmap
1. Business Understanding **(current)**
2. Data Understanding & Data Audit
3. Cleaning & relational SQL analysis
4. EDA, Statistics & Business KPIs
5. RFM / Customer Segmentation
6. Delivery-risk predictive modeling
7. BI dashboard and presentation
8. Deployment, monitoring & final case study

## Team rules
Read [`CONTRIBUTING.md`](CONTRIBUTING.md) before committing.
Each change should have a descriptive commit message and a pull request reviewed by the other teammate.
