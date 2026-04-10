# Laboratorium 1: Proces KDD i przetwarzanie wstępne danych

## Część 1: Proces KDD — teoria w praktyce

### Polecenie

1. Załaduj zbiór danych **Titanic** z biblioteki seaborn (`sns.load_dataset('titanic')`).
2. Zidentyfikuj etapy procesu KDD na przykładzie tego zbioru:
   - **Selekcja danych** — wybierz kolumny istotne do predykcji przeżycia.
   - **Przetwarzanie wstępne** — zbadaj jakość danych (braki, typy, duplikaty).
   - **Transformacja** — przygotuj dane do analizy.
3. Wyświetl podstawowe statystyki zbioru (`describe()`, `info()`, `isnull().sum()`).
4. Narysuj wykres przedstawiający procent brakujących wartości w każdej kolumnie.

---

## Część 2: Czyszczenie danych i obsługa braków

### Polecenie

1. Dla kolumny `age`:
   - Uzupełnij brakujące wartości **medianą** w podziale na klasy podróży (`pclass`).
   - Porównaj rozkład przed i po imputacji (histogram).
2. Dla kolumny `embarked`:
   - Uzupełnij braki **dominantą** (najczęstszą wartością).
3. Dla kolumny `deck`:
   - Oceń, czy ta kolumna jest użyteczna (jaki % braków?). Jeśli >50% — usuń ją.
4. Usuń ewentualne duplikaty wierszy.
5. Wykryj i obsłuż wartości odstające w kolumnie `fare` używając metody IQR.

---

## Część 3: Integracja i łączenie danych z różnych źródeł

### Polecenie

1. Stwórz dwa dodatkowe DataFrame'y symulujące dane z różnych źródeł:
   - `df_bilety` — zawiera `ticket` (numer biletu), `ticket_class` (opis klasy: "Ekonomiczna", "Biznesowa", "Pierwsza"), `meal_plan` ("Standard", "Premium", "VIP").
   - `df_porty` — zawiera `embark_town` (nazwa portu), `country` ("UK", "France", "Ireland"), `port_size` ("Duży", "Średni", "Mały").
2. Połącz te dane z głównym zbiorem Titanic za pomocą odpowiednich operacji `merge`/`join`.
3. Zbadaj, czy po złączeniu nie powstały dodatkowe braki danych. Obsłuż je.
4. Wyeksportuj oczyszczony zbiór do pliku CSV.

---

## Część 4: Kodowanie zmiennych i transformacje

### Polecenie

1. Przeprowadź **kodowanie etykietowe** (Label Encoding) dla kolumny `sex`.
2. Przeprowadź **kodowanie One-Hot** dla kolumny `embarked`.
3. Przeprowadź **standaryzację** (StandardScaler) kolumn numerycznych: `age`, `fare`.
4. Przeprowadź **normalizację** (MinMaxScaler) tych samych kolumn i porównaj wyniki.
5. Przeprowadź **dyskretyzację** kolumny `age` na przedziały: dziecko (0-12), nastolatek (13-17), dorosły (18-60), senior (60+).
6. Narysuj wykres porównawczy rozkładów przed i po transformacjach.

---

## Zadanie do samodzielnej realizacji

Pobierz zbiór danych **Adult Income** (UCI ML Repository) za pomocą:
```python
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data"
columns = ['age','workclass','fnlwgt','education','education_num','marital_status',
           'occupation','relationship','race','sex','capital_gain','capital_loss',
           'hours_per_week','native_country','income']
df = pd.read_csv(url, names=columns, skipinitialspace=True)
```

Wykonaj pełny proces przetwarzania wstępnego:
- Identyfikacja i obsługa wartości `?` jako braków
- Analiza rozkładów zmiennych
- Kodowanie zmiennych kategorycznych
- Standaryzacja zmiennych numerycznych
- Raport z przetwarzania (ile braków usunięto, jakie transformacje zastosowano)
