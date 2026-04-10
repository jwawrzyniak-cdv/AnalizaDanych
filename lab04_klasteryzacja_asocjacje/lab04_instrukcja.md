# Laboratorium 4: Analiza skupień i reguły asocjacyjne

## Część 1: K-Means i metoda łokcia

### Polecenie

Załaduj zbiór **Mall Customers** (klasyczny zbiór do segmentacji klientów):
```python
from sklearn.datasets import make_blobs
# Symulacja danych klientów centrum handlowego
np.random.seed(42)
n = 200
df = pd.DataFrame({
    'Annual_Income': np.concatenate([
        np.random.normal(30, 10, 50), np.random.normal(55, 8, 50),
        np.random.normal(80, 12, 50), np.random.normal(55, 10, 50)
    ]),
    'Spending_Score': np.concatenate([
        np.random.normal(20, 8, 50), np.random.normal(80, 10, 50),
        np.random.normal(25, 8, 50), np.random.normal(45, 12, 50)
    ]),
    'Age': np.concatenate([
        np.random.normal(45, 12, 50), np.random.normal(30, 8, 50),
        np.random.normal(35, 10, 50), np.random.normal(55, 10, 50)
    ])
})
```

1. Zbadaj dane — rozkłady, korelacje, wykresy par (pairplot).
2. Standaryzuj dane.
3. Zastosuj **K-Means** dla k = 2, 3, 4, 5, 6, 7, 8.
4. Narysuj **wykres łokcia** (Elbow Method) — inercja vs. k.
5. Oblicz **Silhouette Score** dla każdego k. Narysuj wykres.
6. Wybierz optymalną liczbę klastrów i zinterpretuj wynik.
7. Zwizualizuj klastry w przestrzeni 2D (najlepsze 2 cechy lub PCA).

---

## Część 2: DBSCAN i klasteryzacja hierarchiczna

### Polecenie

1. Na tych samych danych zastosuj **DBSCAN**:
   - Przetestuj różne wartości `eps` (0.3, 0.5, 0.8, 1.0) przy `min_samples=5`.
   - Dla każdego `eps` policz liczbę klastrów i punktów szumowych.
   - Narysuj wyniki na wykresach.
2. Zastosuj **klasteryzację hierarchiczną** (`AgglomerativeClustering`):
   - Narysuj **dendrogram** (użyj `scipy.cluster.hierarchy`).
   - Przytnij dendrogram do wybranej liczby klastrów.
3. Porównaj wyniki K-Means, DBSCAN i hierarchicznego na jednym wykresie (subplots).
4. Który algorytm dał najlepsze wyniki? Dlaczego?

---

## Część 3: Reguły asocjacyjne — Apriori

### Polecenie

Wygeneruj dane transakcyjne (symulacja koszyka zakupów):
```python
# Symulacja danych transakcyjnych
np.random.seed(42)
products = ['Chleb', 'Masło', 'Mleko', 'Jajka', 'Ser', 'Szynka',
            'Pomidory', 'Ogórki', 'Jabłka', 'Kawa', 'Herbata', 'Cukier']
transactions = []
for _ in range(500):
    n_items = np.random.randint(2, 8)
    basket = list(np.random.choice(products, n_items, replace=False))
    # Dodaj korelacje
    if 'Chleb' in basket and np.random.random() > 0.3:
        basket.append('Masło') if 'Masło' not in basket else None
    if 'Kawa' in basket and np.random.random() > 0.4:
        basket.append('Cukier') if 'Cukier' not in basket else None
    if 'Mleko' in basket and 'Jajka' in basket and np.random.random() > 0.5:
        basket.append('Masło') if 'Masło' not in basket else None
    transactions.append(basket)
```

1. Przekształć transakcje do formatu **one-hot encoding** (macierz binarna).
2. Użyj biblioteki `mlxtend` do wygenerowania **częstych zbiorów elementów** (Apriori):
   ```python
   from mlxtend.frequent_patterns import apriori, association_rules
   ```
3. Ustaw `min_support=0.1` i wyświetl 10 najczęstszych zbiorów.
4. Wygeneruj **reguły asocjacyjne** z `min_threshold=0.5` (confidence).
5. Wyświetl top 10 reguł wg:
   - **Confidence** (pewność)
   - **Lift** (siła reguły)
   - **Conviction** (przekonanie)
6. Zinterpretuj 3 najciekawsze reguły biznesowo.
7. Narysuj wykres scatter: **support vs. confidence**, kolorując punkty wg **lift**.

---

## Część 4: Wizualizacja i interpretacja

### Polecenie

1. Dla najlepszego klastrowania (K-Means) stwórz **profil każdego klastra**:
   - Średnie wartości cech w każdym klastrze
   - Wykres radarowy (spider chart) porównujący klastry
2. Dla reguł asocjacyjnych stwórz **graf sieciowy** (network graph):
   - Węzły = produkty, krawędzie = reguły, grubość = lift
   - Użyj `networkx` do wizualizacji.

---

## Zadanie do samodzielnej realizacji

1. Zastosuj K-Means i DBSCAN do zbioru **Iris** (bez użycia etykiet klas).
2. Porównaj znalezione klastry z prawdziwymi klasami (Adjusted Rand Index).
3. Który algorytm lepiej odtwarza naturalną strukturę danych?
