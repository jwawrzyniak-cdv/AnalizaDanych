# Laboratorium 2 — Zadania rozszerzone

## Zadanie R2.1: Autoenkoder do redukcji wymiarowości

### Polecenie

Zbuduj prosty **autoenkoder** (neural network) do nieliniowej redukcji wymiarowości
i porównaj go z PCA na zbiorze Breast Cancer.

1. Zaimplementuj autoenkoder w PyTorch lub Keras:
   - Architektura: 30 → 16 → 8 → **2** → 8 → 16 → 30
   - Warstwa środkowa (bottleneck = 2) to reprezentacja zredukowana.
   - Funkcja straty: MSE, optymalizator: Adam, 100 epok.
2. Wytrenuj autoenkoder na danych standaryzowanych.
3. Wyciągnij reprezentację 2D z warstwy bottleneck.
4. Narysuj dane w 2D: autoenkoder vs PCA vs t-SNE (3 subploty).
5. Porównaj separowalność klas (wizualnie + Silhouette Score na 2D).
6. Narysuj **learning curve** (loss vs. epoka).

```python
import torch
import torch.nn as nn

class Autoencoder(nn.Module):
    def __init__(self, input_dim=30, latent_dim=2):
        super().__init__()
        self.encoder = nn.Sequential(
            nn.Linear(input_dim, 16), nn.ReLU(),
            nn.Linear(16, 8), nn.ReLU(),
            nn.Linear(8, latent_dim)
        )
        self.decoder = nn.Sequential(
            nn.Linear(latent_dim, 8), nn.ReLU(),
            nn.Linear(8, 16), nn.ReLU(),
            nn.Linear(16, input_dim)
        )
    def forward(self, x):
        z = self.encoder(x)
        return self.decoder(z)
```

---

## Zadanie R2.2: Porównanie metod na danych syntetycznych

### Polecenie

Wygeneruj **3 rodzaje danych syntetycznych** o różnej strukturze i porównaj
skuteczność metod redukcji wymiarowości na każdym z nich:

```python
from sklearn.datasets import make_swiss_roll, make_moons, make_circles

# Dane 1: Swiss Roll (3D → 2D, nieliniowa rozmaitość)
X_swiss, color_swiss = make_swiss_roll(n_samples=1500, noise=0.5, random_state=42)

# Dane 2: Concentryczne okręgi (2D, ale dodaj szum w kolejnych wymiarach)
X_circles, y_circles = make_circles(n_samples=1000, noise=0.1, factor=0.3, random_state=42)
X_circles = np.hstack([X_circles, np.random.randn(1000, 8) * 0.5])  # 10D

# Dane 3: Dwa półksiężyce w wysokim wymiarze
X_moons, y_moons = make_moons(n_samples=1000, noise=0.15, random_state=42)
X_moons = np.hstack([X_moons, np.random.randn(1000, 18) * 0.3])  # 20D
```

Dla każdego zbioru zastosuj:
- PCA (2 składowe)
- t-SNE (perplexity=30)
- UMAP (jeśli zainstalowany: `pip install umap-learn`)
- Isomap (`sklearn.manifold.Isomap`)

Stwórz tabelę porównawczą (4 metody × 3 zbiory = 12 wykresów) i opisz,
która metoda najlepiej zachowuje strukturę danych w każdym przypadku.

---

## Zadanie R2.3: Feature Selection z modelem wrapper (RFE + Boruta)

### Polecenie

Na zbiorze Breast Cancer porównaj metody selekcji cech typu **wrapper**:

1. **RFE** (Recursive Feature Elimination):
   ```python
   from sklearn.feature_selection import RFE
   ```
   - Użyj Random Forest jako estymatora bazowego.
   - Wyznacz optymalną liczbę cech za pomocą `RFECV` (z walidacją krzyżową).
   - Narysuj wykres: accuracy vs. liczba cech.

2. **Boruta** (jeśli zainstalowany: `pip install boruta`):
   ```python
   from boruta import BorutaPy
   ```
   - Uruchom algorytm Boruta z Random Forest.
   - Wyświetl cechy: potwierdzone, odrzucone, niezdecydowane.

3. **Permutation Importance**:
   ```python
   from sklearn.inspection import permutation_importance
   ```
   - Oblicz ważność cech na zbiorze testowym.
   - Porównaj z feature importance z Random Forest (czy kolejność się zgadza?).

4. Stwórz **diagram Venna** (lub tabelę) pokazujący, które cechy zostały wybrane
   przez poszczególne metody. Ile cech jest wspólnych?

5. Porównaj accuracy klasyfikatora kNN na cechach wybranych przez każdą metodę.
