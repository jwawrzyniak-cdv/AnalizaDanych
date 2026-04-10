# Laboratorium 6: Eksploracja tekstu i analiza sentymentu

## Część 1: Przetwarzanie tekstu (NLP pipeline)

### Polecenie

Załaduj zbiór danych **20 Newsgroups** (wbudowany w scikit-learn):
```python
from sklearn.datasets import fetch_20newsgroups
categories = ['rec.sport.baseball', 'sci.space', 'comp.graphics', 'talk.politics.guns']
newsgroups = fetch_20newsgroups(subset='train', categories=categories, remove=('headers', 'footers', 'quotes'))
```

1. Wyświetl 3 przykładowe dokumenty i ich kategorie.
2. Przeprowadź **przetwarzanie tekstu**:
   - Zamiana na małe litery
   - Usunięcie znaków specjalnych i liczb
   - **Tokenizacja** (podział na słowa)
   - Usunięcie **stop words** (angielskie)
   - **Stemming** lub **Lemmatyzacja**
3. Porównaj tekst przed i po przetwarzaniu (3 przykłady).
4. Stwórz **chmurę słów** (word cloud) dla każdej kategorii.

---

## Część 2: Wektoryzacja i klasyfikacja tekstu

### Polecenie

1. Zastosuj **Bag of Words** (CountVectorizer):
   ```python
   from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer
   ```
   - Parametry: `max_features=5000`, `min_df=5`, `max_df=0.7`
2. Zastosuj **TF-IDF** (TfidfVectorizer) z tymi samymi parametrami.
3. Dla obu reprezentacji zbuduj klasyfikator **Naive Bayes** (`MultinomialNB`):
   - Podziel na train/test (80/20).
   - Oblicz accuracy, classification report.
4. Zbuduj też klasyfikator **SVM** (`LinearSVC`) na TF-IDF.
5. Porównaj wyniki — tabela z metrykami.
6. Wyświetl **macierz pomyłek** dla najlepszego modelu.
7. Wyświetl **najważniejsze słowa** dla każdej kategorii (top 10 cech wg TF-IDF/koeficjentów).

---

## Część 3: Analiza sentymentu

### Polecenie

Wygeneruj zbiór recenzji (symulacja):
```python
reviews = [
    ("This movie was absolutely fantastic! Great acting and story.", 1),
    ("Terrible film. Waste of time and money.", 0),
    ("I loved every minute of it. Best movie this year!", 1),
    ("Boring and predictable. Would not recommend.", 0),
    ("An excellent masterpiece with incredible performances.", 1),
    ("Awful. The worst movie I have ever seen.", 0),
    # ... dodaj więcej (minimum 100 — użyj wariantów)
]
```

Alternatywnie użyj NLTK:
```python
import nltk
nltk.download('movie_reviews')
from nltk.corpus import movie_reviews
```

1. Przetwórz teksty (pipeline z Części 1).
2. Zwektoryzuj za pomocą TF-IDF.
3. Zbuduj klasyfikator sentymentu (pozytywny/negatywny) — minimum 2 modele.
4. Oceń modele (accuracy, F1, confusion matrix).
5. Przetestuj model na **nowych, własnych recenzjach** (minimum 5).
6. Które słowa model uznaje za najbardziej pozytywne/negatywne?

---

## Część 4: Prosty system rekomendacyjny

### Polecenie

Stwórz prosty **system rekomendacyjny oparty na treści** (content-based):
```python
# Przykładowe dane filmów
movies = pd.DataFrame({
    'title': ['Matrix', 'Inception', 'Titanic', 'The Notebook', 'Interstellar',
              'Avatar', 'The Godfather', 'Pulp Fiction', 'Forrest Gump', 'Gladiator'],
    'genre': ['Sci-Fi Action', 'Sci-Fi Thriller', 'Romance Drama', 'Romance Drama',
              'Sci-Fi Drama', 'Sci-Fi Action', 'Crime Drama', 'Crime Thriller',
              'Drama Comedy', 'Action Drama'],
    'description': [
        'A computer hacker discovers reality is a simulation controlled by machines',
        'A thief who steals corporate secrets through dream-sharing technology',
        'A love story aboard the ill-fated ship', 'Summer love story told in two timelines',
        'Explorers travel through a wormhole near Saturn', 'Marines on alien planet Pandora',
        'The aging patriarch of an organized crime dynasty',
        'Lives of two mob hitmen and a boxer intertwine',
        'Life story of a slow-witted but kind-hearted man',
        'A Roman general seeks revenge against a corrupt emperor'
    ]
})
```

1. Zwektoryzuj opisy filmów za pomocą TF-IDF.
2. Oblicz **macierz podobieństwa** (cosine similarity).
3. Stwórz funkcję `recommend(title, n=3)` — zwraca n najbardziej podobnych filmów.
4. Przetestuj rekomendacje dla kilku filmów.
5. Narysuj **heatmapę** macierzy podobieństwa.

---

## Zadanie do samodzielnej realizacji

Pobierz zbiór SMS Spam Collection:
```python
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/00228/smsspamcollection.zip"
```
1. Zbuduj klasyfikator spam/ham.
2. Zastosuj pełny pipeline NLP.
3. Porównaj minimum 3 modele klasyfikacji.
4. Przetestuj na własnych przykładach SMS-ów.
