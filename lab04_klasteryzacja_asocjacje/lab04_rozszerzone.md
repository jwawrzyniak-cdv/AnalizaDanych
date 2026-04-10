# Laboratorium 4 — Zadania rozszerzone

## Zadanie R4.1: Segmentacja z oceną stabilności klastrów

### Polecenie

Przeprowadź pogłębioną analizę klastrowania, badając **stabilność** wyników:

1. **Bootstrap stability** — powtórz K-Means 50 razy na losowych podzbiorach
   (80% danych) i zbadaj:
   - Jak często punkty lądują w tym samym klastrze?
   - Stwórz **macierz współwystępowania** (co-occurrence matrix): wartość [i,j] = 
     procent powtórzeń, w których punkt i i j były w tym samym klastrze.
   - Narysuj tę macierz jako heatmapę.

2. **Porównanie metryk wewnętrznych**:
   - Silhouette Score
   - Calinski-Harabasz Index (`sklearn.metrics.calinski_harabasz_score`)
   - Davies-Bouldin Index (`sklearn.metrics.davies_bouldin_score`)
   - Narysuj wszystkie 3 metryki na jednym wykresie dla k=2..10.
   - Czy wszystkie metryki sugerują to samo k?

3. **Gaussian Mixture Models** jako alternatywa dla K-Means:
   ```python
   from sklearn.mixture import GaussianMixture
   ```
   - GMM z 2-6 komponentami. Wybierz k wg BIC/AIC.
   - Porównaj: hard clustering (labels) vs soft clustering (prawdopodobieństwa).
   - Narysuj kontury prawdopodobieństwa nałożone na dane 2D.

4. **OPTICS** — alternatywa DBSCAN bez konieczności ustalania eps:
   ```python
   from sklearn.cluster import OPTICS
   ```
   - Narysuj reachability plot.
   - Porównaj wyniki z DBSCAN.

---

## Zadanie R4.2: Reguły asocjacyjne — analiza koszykowa na danych rzeczywistych

### Polecenie

Pobierz zbiór **Online Retail** z UCI ML Repository (lub Kaggle) i przeprowadź
pełną analizę koszykową:

```python
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/00352/Online%20Retail.xlsx"
df = pd.read_excel(url)
```

Jeśli pobranie nie działa, wygeneruj realistyczne dane z 200+ produktami i 10000+ transakcji.

1. **Preprocessing specyficzny dla retail**:
   - Usuń zwroty (Quantity < 0), anulowane faktury (InvoiceNo zaczyna się od 'C').
   - Ogranicz do jednego kraju (np. UK — największy udział).
   - Zbadaj rozkład: ile produktów per transakcja? Ile transakcji per klient?

2. **Wielopoziomowa analiza**:
   - Poziom 1: Reguły na poziomie produktów (min_support=0.02)
   - Poziom 2: Reguły na poziomie **kategorii** produktów
     (zgrupuj produkty po słowach kluczowych: "CANDLE", "BAG", "HEART", itd.)

3. **Wizualizacja reguł**:
   - Graf skierowany z NetworkX (top 30 reguł wg lift).
   - Heatmapa: macierz lift między top 20 produktami.
   - Parallel coordinates plot: antecedent → consequent z grubością = confidence.

4. **Analiza sezonowa**:
   - Czy reguły asocjacyjne różnią się między miesiącami?
   - Porównaj reguły z Q1 vs Q4 (sezon świąteczny).

---

## Zadanie R4.3: Klasteryzacja obrazów (bez deep learning)

### Polecenie

Zastosuj klasteryzację do zbioru obrazów MNIST (lub Fashion-MNIST):

```python
from sklearn.datasets import fetch_openml
mnist = fetch_openml('mnist_784', version=1, as_frame=False, parser='auto')
X, y = mnist.data[:5000], mnist.target[:5000].astype(int)  # podzbiór
```

1. **Bezpośrednie klasteryzanie** (K-Means na surowych pikselach):
   - k=10 (bo 10 cyfr). Oceń ARI (Adjusted Rand Index) vs. prawdziwe etykiety.
   - Narysuj centroidy jako obrazy 28×28.

2. **PCA + K-Means**:
   - Zredukuj do 50 składowych, potem K-Means.
   - Czy wynik jest lepszy (ARI)?

3. **t-SNE + DBSCAN**:
   - t-SNE do 2D, potem DBSCAN do wykrycia klastrów.
   - Narysuj kolorowy scatter plot — czy klastry odpowiadają cyfrom?

4. **Mini-Batch K-Means** dla skalowalności:
   ```python
   from sklearn.cluster import MiniBatchKMeans
   ```
   - Porównaj czas i jakość z pełnym K-Means.

5. Stwórz **confusion matrix**: klaster vs. prawdziwa etykieta.
   Który klaster najlepiej odpowiada której cyfrze?
