# Laboratorium 3 — Zadania rozszerzone

## Zadanie R3.1: Stacking i Voting — ensemble zaawansowany

### Polecenie

Zbuduj zaawansowane modele ensemble na zbiorze Heart Disease (lub innym z Lab 3):

1. **Voting Classifier** — połącz 3 najlepsze modele z Lab 3:
   ```python
   from sklearn.ensemble import VotingClassifier
   ```
   - Hard voting (głosowanie większościowe)
   - Soft voting (średnia prawdopodobieństw)
   - Porównaj oba podejścia z indywidualnymi modelami.

2. **Stacking Classifier** — dwupoziomowy model:
   ```python
   from sklearn.ensemble import StackingClassifier
   ```
   - Poziom 1: Random Forest + SVM + kNN + Gradient Boosting
   - Poziom 2 (meta-learner): Logistic Regression
   - Użyj 5-fold CV na poziomie 1.

3. **Gradient Boosting vs XGBoost vs LightGBM**:
   ```python
   from sklearn.ensemble import GradientBoostingClassifier
   # pip install xgboost lightgbm
   ```
   - Porównaj 3 implementacje boostingu.
   - Narysuj krzywe uczenia (learning curves) dla każdego.
   - Porównaj czas treningu.

4. Stwórz tabelę porównawczą **wszystkich** modeli (indywidualne + ensemble)
   z metrykami: Accuracy, F1, AUC, czas treningu.

---

## Zadanie R3.2: Interpretacja modeli — SHAP i LIME

### Polecenie

Dla najlepszego modelu z Lab 3 (lub stackingu z R3.1) zastosuj metody
interpretowalności:

1. **SHAP** (SHapley Additive exPlanations):
   ```python
   pip install shap
   import shap
   ```
   - Oblicz wartości SHAP dla zbioru testowego.
   - Narysuj **SHAP summary plot** (bee swarm).
   - Narysuj **SHAP dependence plot** dla 2 najważniejszych cech.
   - Narysuj **SHAP waterfall** dla jednej konkretnej predykcji.

2. **LIME** (Local Interpretable Model-agnostic Explanations):
   ```python
   pip install lime
   import lime.lime_tabular
   ```
   - Wyjaśnij 3 predykcje: poprawną pozytywną, poprawną negatywną, błędną.
   - Porównaj wyjaśnienia LIME z SHAP — czy się zgadzają?

3. **Partial Dependence Plots** (PDP):
   ```python
   from sklearn.inspection import PartialDependenceDisplay
   ```
   - Narysuj PDP dla 4 najważniejszych cech.
   - Zinterpretuj: jak zmiana wartości cechy wpływa na predykcję?

4. Napisz **krótki raport** (max 1 strona) wyjaśniający laikowi, jakie czynniki
   wpływają na diagnozę choroby serca. Użyj wykresów z SHAP/LIME.

---

## Zadanie R3.3: Wielowyjściowa regresja i target engineering

### Polecenie

Na zbiorze California Housing:

1. **Target engineering** — przetransformuj zmienną docelową:
   - Log-transformacja (`np.log1p`)
   - Box-Cox transformacja (`scipy.stats.boxcox`)
   - Kwantylowa transformacja (`QuantileTransformer`)
   - Porównaj RMSE modeli regresji dla każdej transformacji target.

2. **Wielowyjściowa regresja** — stwórz model przewidujący jednocześnie:
   - Cenę nieruchomości (oryginalny target)
   - Kategorię cenową (tani/średni/drogi — dyskretyzacja targetu)
   ```python
   from sklearn.multioutput import MultiOutputRegressor
   ```

3. **Regresja z cechami geograficznymi**:
   - Użyj latitude/longitude do stworzenia cech przestrzennych:
     - Odległość od centrum (np. San Francisco: 37.77, -122.42)
     - Gęstość zabudowy (binning lat/lon na siatce)
     - Interakcja lat × lon
   - Czy cechy geograficzne poprawiają model?

4. **Stacked Regression** — analogicznie do klasyfikacji:
   - Poziom 1: Ridge + Lasso + Random Forest + SVR
   - Poziom 2: Ridge (meta-learner)
   - Porównaj z indywidualnymi modelami.
