# Data

Raw data is not committed. Run `notebooks/00_data_acquisition.ipynb` to download and verify it.

## Hillstrom MineThatData email test

- **What it is:** 64,000 customers who had purchased within the previous twelve months, randomly assigned in equal thirds to a men's merchandise email, a women's merchandise email, or no email. Outcomes (site visit, purchase, spend) are measured over the following two weeks.
- **Source:** Kevin Hillstrom, MineThatData E-Mail Analytics and Data Mining Challenge, 2008. Published openly; no licence file accompanies it, so credit the source wherever results are shown.
- **Columns:** `recency`, `history_segment`, `history` (past-year spend), `mens` and `womens` (past purchase of men's / women's merchandise), `zip_code`, `newbie`, `channel`, `segment` (assigned arm), `visit`, `conversion`, `spend`.
- **Known quirks:**
  - One `zip_code` category is spelled `Surburban` in the source file. Notebook 01 corrects the label after loading.
  - There is no customer identifier. About 10% of rows are exact duplicates of another row; notebook 01 examines this and keeps them.
  - About 12% of customers share the minimum `history` value of $29.99.

## Criteo uplift dataset

Added when the project reaches the notebook that uses it. Cite: Diemert, Betlei, Renaudin and Amini, "A Large Scale Benchmark for Uplift Modeling", AdKDD 2018.
