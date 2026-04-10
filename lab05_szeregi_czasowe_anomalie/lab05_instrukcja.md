# Laboratorium 5: Szeregi czasowe i detekcja anomalii

## Część 1: Analiza i dekompozycja szeregów czasowych

### Polecenie

Załaduj dane **Air Passengers** (klasyczny zbiór):
```python
import statsmodels.api as sm
data = sm.datasets.co2.load_pandas()  # CO2 Mauna Loa
df = data.data
df = df.fillna(df.interpolate())
# Alternatywnie — wygeneruj dane:
# dates = pd.date_range('2010-01', periods=120, freq='M')
# values = 100 + np.cumsum(np.random.randn(120)) + 10*np.sin(np.arange(120)*2*np.pi/12)
```

1. Narysuj **wykres szeregu czasowego** — zidentyfikuj trend i sezonowość.
2. Wykonaj **dekompozycję** szeregu na składowe (trend, sezonowość, reszty):
   ```python
   from statsmodels.tsa.seasonal import seasonal_decompose
   ```
   - Przetestuj model addytywny i multiplikatywny.
3. Sprawdź **stacjonarność** szeregu:
   - Test Dickey-Fullera (ADF): `from statsmodels.tsa.stattools import adfuller`
   - Jeśli niestacjonarny — zastosuj różnicowanie.
4. Narysuj wykres **autokorelacji** (ACF) i **częściowej autokorelacji** (PACF).

---

## Część 2: Prognozowanie szeregów czasowych

### Polecenie

1. Podziel szereg na **zbiór treningowy** (80%) i **testowy** (20%).
2. Zbuduj model **ARIMA** (lub SARIMA dla danych sezonowych):
   - Użyj wykresu ACF/PACF do wyznaczenia parametrów (p, d, q).
   - Alternatywnie: użyj `auto_arima` z biblioteki `pmdarima` (jeśli dostępna).
   ```python
   from statsmodels.tsa.arima.model import ARIMA
   ```
3. Zbuduj model **prostej średniej kroczącej** (SMA) i **wykładniczego wygładzania** (ETS):
   ```python
   from statsmodels.tsa.holtwinters import ExponentialSmoothing
   ```
4. Porównaj modele: oblicz **MAE**, **RMSE**, **MAPE** na zbiorze testowym.
5. Narysuj prognozy wszystkich modeli vs. wartości rzeczywiste.
6. Który model daje najlepsze prognozy? Dlaczego?

---

## Część 3: Detekcja anomalii

### Polecenie

Wygeneruj dane z anomaliami:
```python
np.random.seed(42)
n = 500
X_normal = np.random.multivariate_normal([0, 0], [[1, 0.5],[0.5, 1]], n)
X_anomaly = np.random.uniform(-6, 6, (25, 2))  # 5% anomalii
X_all = np.vstack([X_normal, X_anomaly])
y_true = np.array([0]*n + [1]*25)  # 0=normal, 1=anomalia
```

1. Zastosuj **metodę Z-score** — oznacz jako anomalie punkty z |z| > 3.
2. Zastosuj **Isolation Forest** (`sklearn.ensemble.IsolationForest`):
   - Parametr `contamination=0.05` (oczekiwane 5% anomalii).
3. Zastosuj **Local Outlier Factor** (`sklearn.neighbors.LocalOutlierFactor`):
   - Parametr `n_neighbors=20`.
4. Dla każdej metody:
   - Narysuj wykres scatter z zaznaczonymi anomaliami.
   - Oblicz Precision, Recall, F1 (traktując anomalie jako klasę pozytywną).
5. Porównaj metody — która wykrywa anomalie najskuteczniej?

---

## Część 4: Detekcja anomalii w szeregach czasowych

### Polecenie

1. Wygeneruj szereg czasowy z wprowadzonymi anomaliami:
   ```python
   dates = pd.date_range('2020-01', periods=365, freq='D')
   values = 50 + 10*np.sin(np.arange(365)*2*np.pi/30) + np.random.normal(0, 3, 365)
   # Wprowadź anomalie
   values[50] = 120   # spike
   values[100] = -20  # dip
   values[200:210] += 30  # level shift
   ```
2. Zastosuj **ruchomą średnią** (rolling mean ± 2σ) do wykrycia anomalii.
3. Zastosuj **Isolation Forest** na cechach: wartość, zmiana, średnia krocząca.
4. Narysuj szereg z zaznaczonymi anomaliami (obie metody).

---

## Zadanie do samodzielnej realizacji

Pobierz dane pogodowe (np. temperatura) z publicznego źródła lub wygeneruj:
1. Przeprowadź pełną analizę szeregu (dekompozycja, stacjonarność).
2. Zbuduj prognozę na 30 dni do przodu.
3. Wykryj anomalie pogodowe (ekstremalnie gorące/zimne dni).
