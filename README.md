
Project: Alzheimer & Dementia Reddit Analysis (Data Collection + Annotations)
===============================================================

Brief
-----
This repository contains the notebooks and prompts used to collect and annotate Reddit posts from the r/Alzheimer and r/dementia subreddits (2010 — Jul 2025). The analysis produced: (1) a scraped dataset (CSV) — note that raw scraped social-media posts are not included in this repository for confidentiality and privacy reasons, (2) a first-level author-type categorization (Qwen-14B, zero-shot), and (3) a focused second-level annotation for the "Caregiver about patient" class (GPT-4o) extracting demographics, symptoms and caregiver motivations. The LLM-derived annotation CSVs are provided in the `extracted-results/` directory and the scraping code is included in `code/` so the full pipeline can be reproduced by researchers who can lawfully and ethically perform the scrape.

Repository layout
-----------------
- `code/`
  - `1_data_scraping.ipynb` — data collection and CSV export (arctic-shift photon Reddit query)
  - `2_author_categorization.ipynb` — preprocessing and author-type categorization using Qwen-14B with zero-shot prompts
  - `3_demographic_extraction.ipynb` — focused annotation on "Caregiver about patient" posts using GPT-4o; extracts age, gender, symptoms, motivation and writes CSV outputs
- `extracted-results/` — LLM-derived annotation CSVs (first-level author categorization and second-level caregiver demographic extraction)
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
	- `code/1_data_scraping.ipynb` — run to scrape data (this repository does not include raw scraped social-media posts; run the scraper only if you have the necessary permissions and ethical approvals). If you prefer not to re-run scraping, use the provided LLM-extracted outputs in `extracted-results/`.
	- `code/2_author_categorization.ipynb` — run preprocessing and first-level categorization. This notebook expects the raw/clean CSV in `data/` or an uploaded file with matching column names (`post_id`, `created_utc`, `author`, `title`, `selftext`, ...). When using the provided extracted outputs, adapt input loading to point to `extracted-results/` instead of `data/`.
	- `code/3_demographic_extraction.ipynb` — run focused second-level annotation on the `Caregiver about patient` subset produced by notebook

Notes on models & prompts
------------------------
- First-level categorization: Qwen-14B used in a zero-shot prompt configuration. The prompt template is in `prompt/author_category.txt` and is loaded by `2_author_categorization.ipynb`.
- Second-level extraction: GPT-4o was used to extract age, gender, symptoms, and poster motivation from the caregiver-post subset. The prompt template is in `prompt/demographic.txt` and used by `3_demographic_extraction.ipynb`.
- The notebooks are implemented to store LLM outputs as CSV columns so results are auditable and reproducible.

Data & outputs
---------------
- Raw scraped social-media posts are not published in this repository due to confidentiality and privacy considerations. Instead, we provide the processed, LLM-derived annotation outputs in the `extracted-results/` directory:
	- `extracted-results/author-categorization-dataset/` — first-level author categorization CSVs (Qwen-14B outputs)
	- `extracted-results/demographic-extraction-dataset/` — second-level caregiver annotation CSVs (GPT-4o outputs)

	The `code/1_data_scraping.ipynb` notebook implements the scraper used to collect the raw posts; it is included so that others can reproduce the data collection step where permitted. When reproducing the scraping step, please follow Reddit's API terms, any applicable institutional review board (IRB) requirements, and privacy best practices (e.g., de-identification, masking user identifiers) before sharing or publishing collected data.

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
