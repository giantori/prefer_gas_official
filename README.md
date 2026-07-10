# PreFer Data Challenge — Team GAS

> 🏆 **Winner of the "Improving our Understanding of Fertility" Award** (explainability track) — [PreFer Data Challenge](https://preferdatachallenge.nl), University of Groningen. Presented at **ODISSEI Conference 2024**.

Predicting whether individuals aged 18–45 will have a child within three years (2021–2023), using longitudinal survey data from the Dutch [LISS panel](https://www.centerdata.nl/en/liss-panel) (2007–2020). Challenge paper: [arXiv:2402.00705](https://arxiv.org/abs/2402.00705).

## The problem

- **6,418 individuals × 31,634 features** from 13 years of annual surveys (family, health, work, income, personality, values…) — but only **~1,400 labelled outcomes**.
- Massive, structurally informative missingness: survey branching means a missing answer often *is* information.
- Wide-data regime: far more features than samples, most of them weak or redundant.

## Our approach

1. **Domain-driven feature curation** — instead of throwing 31k columns at a model, we selected survey items with a plausible causal link to fertility (fertility intentions, relationship status and stability, existing children, household economics), prioritising the most recent waves.
2. **Missingness-aware imputation** — branching-induced missing values were recovered from previous survey waves where consistent, and encoded as signal where not.
3. **Model comparison under stratified shuffle-split** (k=10) across 15 approaches — from logistic regression to stacking ensembles.
4. **Final model: weighted voting ensemble** of CatBoost (weight 0.5), LightGBM and Extra Trees, with Bayesian hyperparameter tuning. Internal F1 ≈ 0.78, accuracy ≈ 0.91.
5. **Explainability with SHAP** — Shapley values on the CatBoost estimator identified two dominant predictor groups: *stated fertility intentions* (7 of the top-10 features) and *relationship status/cohabitation history* — turning a black-box ensemble into findings about fertility patterns. This analysis earned the explainability-track award.

See [`description.md`](./description.md) for the full competition report and [`slides.html`](./slides.html) for the ODISSEI presentation.

## Team GAS

- [Alessio Piraccini](https://github.com/apiraccini)
- Simone Meneghello
- **Gianluca Ivo Tori** — [LinkedIn](https://www.linkedin.com/in/giantori/)

## Repository structure

| File | Purpose |
|------|---------|
| `submission.py` | Data cleaning and prediction pipeline (challenge submission format) |
| `training.py` | Model training and ensemble construction |
| `description.md` | Full competition report: EDA, preprocessing, model comparison, SHAP analysis |
| `slides.md` / `slides.html` | ODISSEI 2024 presentation |
| `saved/` | Trained model artefacts and SHAP plots |
