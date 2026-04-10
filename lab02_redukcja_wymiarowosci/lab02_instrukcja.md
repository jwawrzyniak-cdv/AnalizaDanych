# Laboratorium 2: Redukcja wymiarowości danych

## Część 1: Selekcja cech — metody filtracyjne

### Polecenie

Załaduj zbiór danych **Breast Cancer Wisconsin** z `sklearn.datasets`:

```python
from sklearn.datasets import load_breast_cancer
data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['target'] = data.target
```

1. Oblicz **macierz korelacji** między wszystkimi cechami. Wyświetl jako heatmapę.
2. Zidentyfikuj pary cech o korelacji > 0.9 — które z nich warto usunąć?
3. Zastosuj metodę **Variance Threshold** — usuń cechy o wariancji < 0.1 (po standaryzacji).
4. Zastosuj test **chi-kwadrat** (`SelectKBest` z `chi2`) — wybierz 10 najlepszych cech.
5. Zastosuj **mutual information** (`mutual_info_classif`) — porównaj ranking cech z testem chi².
6. Stwórz wykres porównawczy rankingów cech z obu metod.

---

## Część 2: PCA — Principal Component Analysis

### Polecenie

1. Na zbiorze Breast Cancer zastosuj **standaryzację** (`StandardScaler`).
2. Wykonaj PCA na standaryzowanych danych.
3. Narysuj **wykres osypiska** (scree plot) — wyjaśniona wariancja vs. numer składowej.
4. Ile składowych potrzeba, aby zachować **95%** wariancji?
5. Narysuj dane w przestrzeni **2 pierwszych składowych głównych** (scatter plot),
   kolorując punkty wg klasy (benign/malignant).
6. Wyświetl **loadings** — które oryginalne cechy mają największy wpływ na PC1 i PC2?

---

## Część 3: SVD — Singular Value Decomposition

### Polecenie

1. Na tych samych standaryzowanych danych zastosuj **TruncatedSVD** z `sklearn.decomposition`.
2. Porównaj wyniki SVD (2 składowe) z PCA (2 składowe) na jednym wykresie (subplot).
3. Czy wyniki są identyczne? Dlaczego?
4. Zasymuluj zastosowanie SVD do **kompresji obrazu**:
   - Załaduj dowolny obraz w skali szarości (np. z `sklearn.datasets.load_sample_image`
     lub wygeneruj macierz losową).
   - Wykonaj SVD i zrekonstruuj obraz używając k=5, 20, 50, 100 składowych.
   - Narysuj porównanie jakości rekonstrukcji.

---

## Część 4: t-SNE i Feature Engineering

### Polecenie

1. Zastosuj **t-SNE** do wizualizacji zbioru Breast Cancer w 2D.
   - Przetestuj różne wartości `perplexity` (5, 30, 50).
   - Narysuj 3 wykresy obok siebie i porównaj.
2. Porównaj wizualizacje PCA 2D vs t-SNE 2D — który lepiej separuje klasy?
3. **Feature Engineering** — stwórz nowe cechy:
   - Stosunek `mean radius` do `mean texture`
   - Iloczyn `mean area` × `mean compactness`
   - Średnia z grup cech (`mean`, `se`, `worst`)
4. Oceń, czy nowe cechy poprawiają separowalność klas (narysuj scatter ploty).

---

## Zadanie do samodzielnej realizacji

Załaduj zbiór **Wine** (`sklearn.datasets.load_wine`, 13 cech, 3 klasy):
1. Przeprowadź pełną analizę redukcji wymiarowości (PCA + t-SNE).
2. Zastosuj selekcję cech (minimum 2 metody).
3. Porównaj wyniki klasyfikacji (prostym modelem, np. kNN) na:
   - Pełnym zbiorze cech
   - Zbiorze po PCA (95% wariancji)
   - Zbiorze po selekcji (top 5 cech)
4. Opisz wnioski — kiedy redukcja pomaga, a kiedy szkodzi?
