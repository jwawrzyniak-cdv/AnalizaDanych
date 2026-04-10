# Laboratorium 1 — Zadania rozszerzone

## Zadanie R1.1: Automatyczny raport jakości danych

### Polecenie

Napisz funkcję `data_quality_report(df)`, która dla dowolnego DataFrame generuje
kompletny raport jakości danych w formie słownikowej/DataFrame, zawierający dla każdej kolumny:

- typ danych (numeryczna / kategoryczna / datetime)
- liczbę i procent braków
- liczbę unikalnych wartości
- liczbę outlierów (metoda IQR dla numerycznych)
- skośność i kurtozę (dla numerycznych)
- top 3 najczęstsze wartości (dla kategorycznych)
- sugerowaną akcję: "OK", "imputacja", "usunięcie kolumny", "wymaga uwagi"

Przetestuj na zbiorach: Titanic, Tips (`sns.load_dataset('tips')`), Penguins (`sns.load_dataset('penguins')`).

Funkcja powinna automatycznie generować wykres podsumowujący (heatmapa braków + histogram typów problemów).

```python
def data_quality_report(df: pd.DataFrame) -> pd.DataFrame:
    """Zwraca DataFrame z raportem jakości dla każdej kolumny."""
    ...
```

---

## Zadanie R1.2: Pipeline przetwarzania z logowaniem zmian

### Polecenie

Stwórz klasę `PreprocessingPipeline`, która:

1. Przyjmuje DataFrame i wykonuje sekwencję kroków przetwarzania.
2. **Loguje każdy krok**: co zostało zmienione, ile rekordów/kolumn dotkniętych.
3. Pozwala na **cofnięcie ostatniego kroku** (undo).
4. Generuje raport końcowy: "przed vs. po" z podsumowaniem wszystkich zmian.

Wymagane kroki:
- `remove_duplicates()`
- `handle_missing(strategy='median'|'mean'|'mode'|'drop', columns=None)`
- `cap_outliers(method='iqr'|'zscore', threshold=1.5|3.0)`
- `encode_categorical(method='onehot'|'label'|'target', columns=None)`
- `scale(method='standard'|'minmax'|'robust', columns=None)`

```python
pipe = PreprocessingPipeline(df)
pipe.remove_duplicates()
pipe.handle_missing(strategy='median', columns=['age', 'fare'])
pipe.cap_outliers(method='iqr')
pipe.encode_categorical(method='onehot', columns=['sex', 'embarked'])
pipe.scale(method='robust')
pipe.undo()  # cofnij skalowanie
pipe.summary()  # raport zmian
```

---

## Zadanie R1.3: Detekcja i naprawa niespójności między źródłami

### Polecenie

Pobierz dwa pliki CSV (lub wygeneruj je) reprezentujące dane o klientach z dwóch systemów:

```python
np.random.seed(42)
n = 200

# System A — CRM
df_crm = pd.DataFrame({
    'client_id': range(1, n+1),
    'name': [f'Klient {i}' for i in range(1, n+1)],
    'email': [f'klient{i}@firma.pl' for i in range(1, n+1)],
    'revenue': np.random.normal(5000, 2000, n).round(2),
    'segment': np.random.choice(['A', 'B', 'C'], n)
})
# Wprowadź niespójności
df_crm.loc[10, 'name'] = 'klient 11'  # inna wielkość liter
df_crm.loc[20, 'email'] = 'klient20@Firma.PL'  # case email
df_crm.loc[30, 'revenue'] = -500  # błędna wartość
df_crm.loc[40:45, 'segment'] = 'D'  # nieznany segment

# System B — ERP
df_erp = pd.DataFrame({
    'id_klienta': range(1, n+1),
    'nazwa': [f'Klient {i}' for i in range(1, n+1)],
    'przychod': np.random.normal(5000, 2000, n).round(2),
    'region': np.random.choice(['Północ', 'Południe', 'Wschód', 'Zachód'], n)
})
df_erp.loc[15, 'nazwa'] = 'Klient  15'  # podwójna spacja
df_erp = pd.concat([df_erp, df_erp.iloc[[5, 10, 15]]])  # duplikaty
```

Zadania:
1. Zmapuj kolumny między systemami (client_id ↔ id_klienta, name ↔ nazwa, itd.).
2. Znajdź i napraw niespójności: wielkość liter, spacje, duplikaty, wartości spoza dziedziny.
3. Połącz dane w jeden spójny DataFrame.
4. Wygeneruj raport niespójności: ile problemów znaleziono, jakie, jak naprawiono.
5. Zaproponuj **reguły walidacji** (assertions), które można uruchomić na przyszłych danych.
