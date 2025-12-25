# Protein Prediction (Linear Regression / OLS)

Ovaj projekat predvidja nutritivnu vrednost **`protein`** na osnovu odabranih nutritivnih kolona iz *fast food* dataseta, koristeci **linearnu regresiju (OLS)**.

Sta sam uradio (ukratko)
- Ucitao dataset `fastfood.csv` i standardizovao “missing” vrednosti (`'NA'`, prazno, razmak, `'-'`).
- Filtrirao ekstremne vrednosti: zadrzao redove gde je `total_fat <= 125`.
- Uklonio kolone koje nisu relevantne ili imaju mnogo nedostajucih vrednosti:
  - `calcium` (≈40% missing),
  - `item` i `restaurant` (ne koriste se za numericku regresiju u ovom projektu).
- Proverio raspodelu kolona sa missing vrednostima **Shapiro-Wilk** testom i popunio missing vrednosti **medianom** (jer p < 0.05 → nije normalna raspodela).
- Napravio korelacionu matricu i heatmap radi izbora prediktora.
- Definisao cilj i feature-e:
  - **Target:** `protein`
  - **Features (inicijalno):** `cholesterol`, `calories`, `sodium`
- Podelio podatke na train/test (80/20, `random_state=123`).
- Fitovao OLS model u `statsmodels` i generisao predikcije na test skupu.
- Uradio dijagnostiku modela (Residuals vs Fitted, Normal Q-Q, Scale-Location,leverage/Cook’s distance).
- Proverio multikolinearnost preko **√VIF** i zatim refitovao model bez `calories`
  (finalni skup prediktora: `sodium` i `cholesterol`).

Struktura repozitorijuma
- `ProteinPrediction.ipynb` — kompletan workflow (EDA → preprocessing → trening → evaluacija/dijagnostika)
- `fastfood.csv` — dataset koriscen u projektu

Koriscene tehnologije
- **Python**
- **Jupyter Notebook**
- **pandas** (rad sa tabelama)
- **numpy** (numerika)
- **scipy** (`shapiro` test normalnosti)
- **matplotlib** (grafici)
- **seaborn** (heatmap / residplot)
- **scikit-learn** (`train_test_split`)
- **statsmodels** (OLS regresija, summary, dijagnostika, VIF)

Model i evaluacija
# Model
- OLS linearna regresija (`statsmodels.formula.api.ols`)
- Inicijalni model:
  - `protein ~ cholesterol + calories + sodium`
- Zbog multikolinearnosti (√VIF), model je zatim pojednostavljen:
  - `protein ~ sodium + cholesterol`

# Rezultati (iz `statsmodels` summary)
- U notebook-u je interpretiran model sa **R² ≈ 0.816** (≈81.6% objasnjene varijanse na train podacima).
- Statisticka znacajnost modela proverena kroz F-statistiku i p-vrednosti koeficijenata.
- Dodatno su analizirani reziduali i uticajne tacke (Cook’s distance).

## Pokretanje projekta
#lokalno (Jupyter)
1. Instalirati zavisnosti:
--bash
pip install pandas numpy scipy matplotlib seaborn scikit-learn statsmodels jupyter
2. jupyter notebook

# Sta bih sledece unapredio
Dodao eksplicitne metrike na test skupu (npr. RMSE/MAE/R2).
Cross-validation (stabilnija procena performansi).
