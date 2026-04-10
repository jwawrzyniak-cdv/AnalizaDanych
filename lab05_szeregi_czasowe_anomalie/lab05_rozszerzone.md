# Laboratorium 5 — Zadania rozszerzone

## Zadanie R5.1: Prognozowanie z Prophet i porównanie z ARIMA

### Polecenie

Użyj biblioteki **Prophet** (Meta/Facebook) do prognozowania i porównaj z ARIMA/ETS:

```python
# pip install prophet
from prophet import Prophet
```

1. Przygotuj dane w formacie Prophet (kolumny `ds` i `y`).
2. Zbuduj model Prophet z:
   - Automatyczną detekcją sezonowości (roczna, tygodniowa, dzienna).
   - Uwzględnieniem świąt (`add_country_holidays(country_name='PL')`).
   - Changepoint detection (punkty zmiany trendu).
3. Porównaj prognozy Prophet vs ARIMA vs ETS na zbiorze testowym (MAE, RMSE, MAPE).
4. Narysuj:
   - Prognozę z przedziałem ufności.
   - Składowe modelu (trend, sezonowość, święta).
   - Wykres changepoints.
5. **Cross-validation** w Prophet:
   ```python
   from prophet.diagnostics import cross_validation, performance_metrics
   cv = cross_validation(model, initial='365 days', period='90 days', horizon='30 days')
   ```

Jeśli Prophet jest niedostępny, użyj alternatywnie `statsforecast` lub `sktime`:
```python
# pip install statsforecast
from statsforecast import StatsForecast
from statsforecast.models import AutoARIMA, AutoETS
```

---

## Zadanie R5.2: Wielowymiarowa detekcja anomalii na danych rzeczywistych

### Polecenie

Zbuduj system detekcji anomalii na danych metrycznych serwera:

```python
np.random.seed(42)
n = 2000
dates = pd.date_range('2024-01-01', periods=n, freq='h')

# Normalne zachowanie: dzienne wzorce + trend + szum
hour = np.arange(n) % 24
daily_pattern = 20 * np.sin(hour * 2 * np.pi / 24 - np.pi/2) + 50
trend = np.linspace(0, 10, n)
noise = np.random.normal(0, 3, n)

df_metrics = pd.DataFrame({
    'timestamp': dates,
    'cpu_usage': daily_pattern + trend + noise,
    'memory_mb': 2000 + 500 * np.sin(hour * 2*np.pi/24) + np.random.normal(0, 50, n),
    'requests_per_sec': np.maximum(0, 100 + 80*np.sin(hour*2*np.pi/24) + np.random.normal(0, 15, n)),
    'error_rate': np.maximum(0, 0.02 + np.random.exponential(0.01, n)),
    'response_time_ms': np.maximum(10, 50 + 30*np.sin(hour*2*np.pi/24) + np.random.normal(0, 10, n))
})

# Wprowadź anomalie różnych typów
df_metrics.loc[100:105, 'cpu_usage'] = 95  # CPU spike
df_metrics.loc[500, 'error_rate'] = 0.8  # Error burst
df_metrics.loc[800:850, 'response_time_ms'] *= 3  # Slowdown
df_metrics.loc[1200:1210, 'requests_per_sec'] = 0  # Outage
df_metrics.loc[1500, 'memory_mb'] = 7500  # Memory leak
```

1. **Feature engineering** dla wykrywania anomalii:
   - Statystyki okna ruchomego (średnia, std, min, max) dla okien 1h, 6h, 24h.
   - Zmiany procentowe (rate of change).
   - Korelacje między metrykami (np. CPU vs response time).
   - Cechy kalendarzowe (godzina, dzień tygodnia, weekend).

2. **Metody detekcji** — zastosuj każdą i porównaj:
   - Isolation Forest (wielowymiarowy)
   - Local Outlier Factor
   - One-Class SVM
   - Autoenkoder (jeśli PyTorch/TF dostępny)

3. **Ewaluacja** — dla każdej metody:
   - Precision, Recall, F1 (traktując znane anomalie jako ground truth).
   - Narysuj timeline z zaznaczonymi anomaliami.
   - Analiza false positives — czy mają sens?

4. **Dashboard anomalii** — stwórz jeden wykres z:
   - 5 panelami (po jednym na metrykę).
   - Zaznaczonymi anomaliami (czerwone punkty).
   - Pasami ufności (szare tło = normalne).

---

## Zadanie R5.3: Prognozowanie wielu szeregów jednocześnie

### Polecenie

Wygeneruj dane sprzedażowe dla 5 produktów i zastosuj prognozowanie wieloszeregowe:

```python
np.random.seed(42)
dates = pd.date_range('2022-01', periods=104, freq='W')  # 2 lata tygodniowo
products = ['Produkt_A', 'Produkt_B', 'Produkt_C', 'Produkt_D', 'Produkt_E']

data = {}
for i, prod in enumerate(products):
    trend = np.linspace(50+i*20, 100+i*30, 104)
    season = 15 * np.sin(np.arange(104) * 2*np.pi/52 + i)
    noise = np.random.normal(0, 5+i, 104)
    # Korelacja między produktami (A wpływa na B)
    data[prod] = np.maximum(0, trend + season + noise)

# Dodaj korelację: wzrost A → wzrost B z opóźnieniem 2 tygodni
data['Produkt_B'][2:] += 0.3 * data['Produkt_A'][:-2]
df_sales = pd.DataFrame(data, index=dates)
```

1. Zbadaj **korelacje krzyżowe** między produktami (cross-correlation).
   - Czy istnieją opóźnione zależności? Użyj `ccf` z statsmodels.

2. **VAR** (Vector Autoregression) — model wieloszeregowy:
   ```python
   from statsmodels.tsa.api import VAR
   ```
   - Dobierz optymalny lag (AIC/BIC).
   - Wytrenuj i prognozuj 12 tygodni do przodu.

3. **Granger Causality Test** — czy sprzedaż produktu A
   "powoduje" (w sensie Grangera) sprzedaż produktu B?
   ```python
   from statsmodels.tsa.stattools import grangercausalitytests
   ```

4. **Porównaj z modelami indywidualnymi** — ARIMA osobno na każdym produkcie.
   Czy VAR (uwzględniający korelacje) daje lepsze prognozy?

5. Narysuj prognozy wszystkich produktów na jednym wykresie z przedziałami ufności.
