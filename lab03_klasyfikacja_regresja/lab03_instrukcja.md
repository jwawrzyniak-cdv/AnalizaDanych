# Laboratorium 3: Klasyfikacja i regresja

## Część 1: Klasyfikacja — porównanie algorytmów

### Polecenie

Załaduj zbiór **Heart Disease** z UCI (wbudowany w wersji uproszczonej):
```python
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/heart-disease/processed.cleveland.data"
columns = ['age','sex','cp','trestbps','chol','fbs','restecg','thalach',
           'exang','oldpeak','slope','ca','thal','target']
df = pd.read_csv(url, names=columns, na_values='?')
```

1. Oczyść dane (obsługa braków, kodowanie zmiennych).
2. Podziel na zbiór treningowy (80%) i testowy (20%).
3. Zbuduj i porównaj **4 klasyfikatory**:
   - **Drzewo decyzyjne** (`DecisionTreeClassifier`)
   - **Random Forest** (`RandomForestClassifier`)
   - **SVM** (`SVC` z kernelem RBF)
   - **kNN** (`KNeighborsClassifier`)
4. Dla każdego modelu oblicz:
   - Accuracy, Precision, Recall, F1-score
   - Macierz pomyłek (confusion matrix)
5. Narysuj **krzywe ROC** dla wszystkich modeli na jednym wykresie.
6. Który model jest najlepszy? Uzasadnij wybór.

---

## Część 2: Strojenie hiperparametrów

### Polecenie

1. Dla **najlepszego modelu** z Części 1 przeprowadź strojenie hiperparametrów:
   - Użyj `GridSearchCV` z 5-krotną walidacją krzyżową.
   - Dla Random Forest: `n_estimators` [50, 100, 200], `max_depth` [3, 5, 10, None], `min_samples_split` [2, 5, 10].
   - Dla SVM: `C` [0.1, 1, 10], `gamma` [0.01, 0.1, 1], `kernel` ['rbf', 'linear'].
2. Wyświetl najlepsze hiperparametry i wynik.
3. Porównaj model przed i po strojeniu.
4. Narysuj **importance cech** (dla modeli drzewiastych) lub **współczynniki** (dla modeli liniowych).

---

## Część 3: Regresja

### Polecenie

Załaduj zbiór **California Housing** z scikit-learn:
```python
from sklearn.datasets import fetch_california_housing
data = fetch_california_housing()
```

1. Zbadaj dane — wyświetl korelacje i wykresy par (pairplot) wybranych cech.
2. Podziel na zbiór treningowy i testowy.
3. Zbuduj i porównaj **4 modele regresji**:
   - **Regresja liniowa** (`LinearRegression`)
   - **Regresja Ridge** (`Ridge`)
   - **Regresja Lasso** (`Lasso`)
   - **Drzewo regresyjne** (`DecisionTreeRegressor`)
4. Dla każdego modelu oblicz: **MAE**, **RMSE**, **R²**.
5. Narysuj wykres **wartości rzeczywiste vs. przewidywane** dla najlepszego modelu.
6. Zbadaj **współczynniki** modeli liniowych — które cechy mają największy wpływ?

---

## Część 4: Pipeline i walidacja krzyżowa

### Polecenie

1. Stwórz **pipeline** (sklearn.pipeline.Pipeline) łączący:
   - Standaryzację
   - PCA (opcjonalnie)
   - Wybrany model
2. Wykonaj **10-krotną walidację krzyżową** na pipeline.
3. Porównaj wyniki pipeline z PCA i bez PCA.
4. Zapisz najlepszy model używając `joblib.dump()`.

---

## Zadanie do samodzielnej realizacji

Na zbiorze **Iris** (`sklearn.datasets.load_iris`):
1. Zbuduj klasyfikator wieloklasowy (3 klasy) używając 3 różnych algorytmów.
2. Zastosuj walidację krzyżową i strojenie hiperparametrów.
3. Narysuj granice decyzji (decision boundaries) w przestrzeni 2 pierwszych cech.
4. Porównaj wyniki i wybierz najlepszy model z uzasadnieniem.
