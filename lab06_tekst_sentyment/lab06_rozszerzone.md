# Laboratorium 6 — Zadania rozszerzone

## Zadanie R6.1: Word Embeddings i klasyfikacja z Word2Vec

### Polecenie

Porównaj reprezentacje tekstu: BoW, TF-IDF, Word2Vec, i (opcjonalnie) BERT embeddings:

1. **Wytrenuj Word2Vec** na zbiorze 20 Newsgroups:
   ```python
   from gensim.models import Word2Vec
   # pip install gensim
   ```
   - Tokenizuj dokumenty (po przetwarzaniu NLP z Lab 6).
   - Wytrenuj model Skip-gram i CBOW (porównaj).
   - Parametry: `vector_size=100`, `window=5`, `min_count=5`.

2. **Eksploracja embeddingów**:
   - Znajdź 5 najbliższych słów do: "computer", "space", "game", "president".
   - Zbadaj analogie: "king" - "man" + "woman" = ?
   - Narysuj t-SNE wybranych 100 słów (kolorując wg kategorii semantycznej).

3. **Document embeddings** — uśrednione embeddingi słów:
   ```python
   def doc2vec_mean(doc_tokens, model):
       vectors = [model.wv[w] for w in doc_tokens if w in model.wv]
       return np.mean(vectors, axis=0) if vectors else np.zeros(model.vector_size)
   ```

4. **Klasyfikacja** — porównaj 4 reprezentacje + SVM:
   - BoW + SVM
   - TF-IDF + SVM
   - Word2Vec (mean) + SVM
   - TF-IDF + Word2Vec (konkatenacja) + SVM
   - Tabela: Accuracy, F1-macro, czas treningu.

5. **(Opcjonalnie)** Użyj pretrenowanych embeddingów:
   ```python
   import gensim.downloader as api
   glove = api.load("glove-wiki-gigaword-100")
   ```

---

## Zadanie R6.2: Topic Modeling — odkrywanie tematów w dokumentach

### Polecenie

Zastosuj modelowanie tematyczne do zbioru 20 Newsgroups (4-8 kategorii):

1. **LDA** (Latent Dirichlet Allocation):
   ```python
   from sklearn.decomposition import LatentDirichletAllocation
   ```
   - Zastosuj LDA z k=4 (bo 4 kategorie), k=6, k=8 tematów.
   - Wyświetl top 15 słów w każdym temacie.
   - Czy tematy odpowiadają oryginalnym kategoriom?

2. **NMF** (Non-negative Matrix Factorization):
   ```python
   from sklearn.decomposition import NMF
   ```
   - Te same eksperymenty co z LDA. Porównaj jakość tematów.

3. **Wizualizacja tematów**:
   - Heatmapa: dokument × temat (prawdopodobieństwa przypisania).
   - Word clouds per temat.
   - Wykres: rozkład tematów w każdej oryginalnej kategorii.

4. **Optymalizacja liczby tematów**:
   - Oblicz **koherencję** (coherence score) dla k=2..15:
     ```python
     # Użyj gensim dla koherencji
     from gensim.models.coherencemodel import CoherenceModel
     ```
   - Narysuj wykres koherencji vs k. Optymalny k?

5. **Interaktywna wizualizacja** (opcjonalnie):
   ```python
   # pip install pyLDAvis
   import pyLDAvis.lda_model
   ```

---

## Zadanie R6.3: System rekomendacyjny — Collaborative Filtering

### Polecenie

Rozszerz system rekomendacyjny z Lab 6 o **filtrowanie kolaboratywne**:

```python
np.random.seed(42)
n_users, n_items = 100, 50
# Macierz ocen (1-5), z ~80% braków (użytkownik nie ocenił)
ratings = np.zeros((n_users, n_items))
for u in range(n_users):
    # Każdy użytkownik ocenia 5-15 filmów
    rated = np.random.choice(n_items, np.random.randint(5, 15), replace=False)
    for item in rated:
        # Ocena zależy od ukrytego profilu użytkownika
        user_pref = np.random.normal(3.5, 1)
        ratings[u, item] = np.clip(round(user_pref + np.random.normal(0, 0.5)), 1, 5)

movies = [f'Film_{i}' for i in range(n_items)]
users = [f'User_{i}' for i in range(n_users)]
df_ratings = pd.DataFrame(ratings, index=users, columns=movies)
df_ratings = df_ratings.replace(0, np.nan)  # 0 = brak oceny
```

1. **User-based Collaborative Filtering**:
   - Oblicz macierz podobieństwa między użytkownikami (cosine similarity).
   - Dla użytkownika X znajdź k=10 najbardziej podobnych.
   - Przewiduj ocenę = ważona średnia ocen podobnych użytkowników.

2. **Item-based Collaborative Filtering**:
   - Macierz podobieństwa między filmami.
   - Przewiduj ocenę na podstawie filmów, które użytkownik już ocenił.

3. **SVD do rekomendacji** (Matrix Factorization):
   ```python
   from scipy.sparse.linalg import svds
   ```
   - Uzupełnij brakujące oceny średnią.
   - Zastosuj SVD z k=10 składowymi.
   - Zrekonstruuj macierz i porównaj z oryginalnymi ocenami.

4. **Ewaluacja**:
   - Ukryj 20% znanych ocen (test set).
   - Oblicz RMSE predykcji dla: user-based, item-based, SVD.
   - Który model najlepszy?

5. **Hybridowy system** — połącz content-based (z Lab 6) z collaborative filtering.
   Ważona suma predykcji obu systemów.
