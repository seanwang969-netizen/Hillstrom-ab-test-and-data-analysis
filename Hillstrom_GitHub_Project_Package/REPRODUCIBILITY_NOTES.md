# Reproducibility notes and notebook audit

This project documentation was drafted from the **uploaded 67-cell Jupyter notebook**, not from a fresh execution against the raw source data. The user-uploaded notebook remains unchanged. The packaged copy has code-only repairs and *cleared outputs* to avoid presenting old output cells as if the patched code had produced them.

## Confirmed issues in the original notebook

1. **Missing split** (original cell 56): `df_model` is loaded, but `train_df`, `val_df` and `test_df` are never assigned. Yet cells 58 and 60–66 depend on them. Added two `train_test_split` calls to establish the intended stratified 60%/20%/20% split, `random_state=42`.
2. **ANCOVA loop shape mismatch** (original cell 37): it unpacks four fields from `comparisons`, while `comparisons` was defined earlier as 2-element tuples. Added explicit group/contrast specifications and avoided hidden notebook state.
3. **Undefined conversion predictions** (original cell 60): `conversion_preds` is referenced but never created within saved notebook code. Added an explicitly trained Logistic T-Learner with 5-fold CV for the supplementary conversion-validation analysis. The resulting numbers should be rechecked upon execution.
4. **Holm adjustment undocumented in code** (original cell 26): the markdown says the three A/B tests remain significant after Holm correction, but the saved code didn't calculate adjusted p-values. Added `multipletests(..., method='holm')`. The Holm values in the report were calculated from recorded raw p-values.
5. **Machine-specific data path** (original cell 1): references a personal Desktop folder. Replaced with CSV discovery in `data/` or `HILLSTROM_CSV` and an in-memory DuckDB database.

## What was not changed

- Original statistical choices, data definitions, treatment-arm groups and outcome definitions.
- Causal Forest specification: 300 trees, honest estimation, 3-fold nuisance cross-fitting, `model_y=Ridge(alpha=1000)`, `model_t=DummyClassifier(strategy='prior')`.
- Reported test estimates, A/B results, precision checks and HTE significance values were recorded from the original notebook. They are **not represented as newly replicated**.
- Raw data was **not** provided, so the patched notebook has only undergone JSON and Python syntax checks, not full execution or statistical parity testing.

## Before you publish or use in interviews

- [ ] Obtain the public Hillstrom CSV and place it in `data/`.
- [ ] Create an environment and run the patched notebook **Restart Kernel → Run All**; inspect every numerical output.
- [ ] Confirm the final stratified split matches the one used to create the original saved result outputs. If it does not, update the report with the rerun results and say so transparently.
- [ ] Fix any version-specific EconML issues; freeze tested package versions in `requirements.txt` after successful run.
- [ ] Check that `history_segment`/`history` and other pre-treatment columns are correctly interpreted. Do not use `visit` or `conversion` as features of revenue CATE models.
- [ ] Consider an independent, prospectively held-out randomized evaluation if making *confirmatory* personalization claims. Historic HTE was explored on all customers.
- [ ] Distinguish **statistically significant ATE**, **exploratory HTE**, and **non-significant policy differences** when describing the project.
