
Project: Alzheimer & Dementia Reddit Analysis (Data Collection + Annotations)
===============================================================

Brief
-----
This repository contains the data, notebooks, and prompts used to collect and annotate Reddit posts from the r/Alzheimer and r/dementia subreddits (2010 — Jul 2025). The analysis produced: (1) a scraped dataset (CSV), (2) a first-level author-type categorization (Qwen-14B, zero-shot), and (3) a focused second-level annotation for the "Caregiver about patient" class (GPT-4o) extracting demographics, symptoms and caregiver motivations.

Repository layout
-----------------
- `code/`
  - `1_data_scraping.ipynb` — data collection and CSV export (arctic-shift photon Reddit query)
  - `2_author_categorization.ipynb` — preprocessing and author-type categorization using Qwen-14B with zero-shot prompts
  - `3_demographic_extraction.ipynb` — focused annotation on "Caregiver about patient" posts using GPT-4o; extracts age, gender, symptoms, motivation and writes CSV outputs
- `data/` — scraped CSV files (raw and cleaned versions) and the 2-level annotation CSVs (first-level categories and second-level caregiver annotations)
- `prompt/` — prompt templates used for zero-shot categorization and demographic extraction (`author_category.txt`, `demographic.txt`)

Goals
-----
- Reproducibility: provide clear steps to re-run the three notebooks in Google Colab
- Transparency: share datasets and prompt text used for annotations
- Responsible release: highlight ethical and privacy considerations for publishing sensitive user-derived content

Quick reproduction (Colab)
-------------------------
Prerequisites
- A Google account and access to Google Colab
- (Optional) API access to the LLM providers used (Qwen-14B and GPT-4o). If you don't have API keys, notebooks include example stubs and local fallback options.

Steps
1. Open Google Colab and upload or open the notebook from this repository. Recommended order:
	- `code/1_data_scraping.ipynb` — run to scrape (or skip if you will use the provided `data/` CSVs).
	- `code/2_author_categorization.ipynb` — run preprocessing and first-level categorization. This notebook expects the raw/clean CSV in `data/` or an uploaded file with matching column names (`post_id`, `created_utc`, `author`, `title`, `selftext`, ...).
	- `code/3_demographic_extraction.ipynb` — run focused second-level annotation on the `Caregiver about patient` subset produced by notebook

Notes on models & prompts
------------------------
- First-level categorization: Qwen-14B used in a zero-shot prompt configuration. The prompt template is in `prompt/author_category.txt` and is loaded by `2_author_categorization.ipynb`.
- Second-level extraction: GPT-4o was used to extract age, gender, symptoms, and poster motivation from the caregiver-post subset. The prompt template is in `prompt/demographic.txt` and used by `3_demographic_extraction.ipynb`.
- The notebooks are implemented to store LLM outputs as CSV columns so results are auditable and reproducible.

Data & outputs
---------------
- The `data/` folder includes the raw scraped CSV(s), cleaned CSV(s), first-level category CSV, and second-level caregiver annotation CSV. Filenames and example columns are described at the top of each notebook.
- When publishing the dataset, remove or mask direct user identifiers according to platform policies (see Ethical & privacy section below).

Ethics, privacy, and licensing
-----------------------------
- The dataset is derived from public Reddit posts. Before publishing, ensure compliance with Reddit's API terms and the target repository's privacy policy. Prefer publishing de-identified or aggregated data if individual-level privacy is a concern.
- Do not publish API keys or any private credentials. The notebooks deliberately load credentials from Colab environment variables or secrets.

Caveats & limitations
---------------------
- Automatic annotations may contain errors. Report inter-annotator agreement or manual validation if you perform any human checks.
- Model behavior and outputs depend on the model versions and API. If providers update models, outputs may differ slightly.

Citation
--------
When referencing this work in publications, include the repository URL and a short DOI or Zenodo record if you mint one. Example citation placeholder:

Your Name. (2025). Alzheimer & Dementia Reddit Analysis (code and dataset). GitHub repository: https://github.com/UmmeKulsumTumpa/alzheimers-dementia-reddit-analysis

Contact
-------
For questions about the code or dataset, open an issue or contact the repository owner.
