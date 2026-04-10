# Projekt zaliczeniowy: Kompleksowa analiza eksploracyjna danych

## Informacje ogólne

| Parametr | Wartość |
|---|---|
| **Forma** | Projekt indywidualny lub w parach (2 osoby) |

---

## Opis projektu

Celem projektu jest przeprowadzenie **kompleksowej analizy eksploracyjnej** na wybranym
zbiorze danych z wykorzystaniem metod poznanych na zajęciach. Projekt powinien obejmować
pełny proces KDD — od pozyskania danych, przez przetwarzanie, eksplorację, aż do
interpretacji wyników.

---

## Wymagania

### Obowiązkowe (na ocenę 3.0)

1. **Zbiór danych** — wybrany z publicznego źródła (np. Kaggle, UCI ML Repository, 
   data.gov.pl, OpenML). Minimum 1000 rekordów i 10 cech.
2. **Przetwarzanie wstępne** (Lab 1):
   - Analiza braków danych i ich obsługa
   - Kodowanie zmiennych kategorycznych
   - Standaryzacja/normalizacja
3. **Eksploracyjna analiza danych (EDA)**:
   - Statystyki opisowe
   - Wykresy rozkładów, korelacji, pairplot
   - Minimum 5 różnych typów wykresów
4. **Minimum 2 metody eksploracji** z poniższych:
   - Klasyfikacja lub regresja (Lab 3)
   - Klasteryzacja (Lab 4)
   - Reguły asocjacyjne (Lab 4)
   - Analiza szeregów czasowych (Lab 5)
   - Analiza tekstu (Lab 6)
5. **Ewaluacja modeli** — metryki, walidacja krzyżowa
6. **Raport** — Jupyter Notebook lub PDF z opisem, kodem i wizualizacjami

### Na ocenę 4.0 (dodatkowo)

7. **Redukcja wymiarowości** (Lab 2) — PCA lub t-SNE z wizualizacją
8. **Strojenie hiperparametrów** — GridSearchCV lub RandomizedSearchCV
9. **Porównanie minimum 3 algorytmów** z uzasadnieniem wyboru najlepszego
10. **Interpretacja biznesowa** — co wyniki oznaczają w praktyce?

### Na ocenę 5.0 (dodatkowo)

11. **Pipeline** — pełny sklearn Pipeline od preprocessingu do predykcji
12. **Feature Engineering** — stworzenie nowych cech i ocena ich wpływu
13. **Zaawansowana wizualizacja** — interaktywne wykresy (plotly) lub dashboard
14. **Wnioski i rekomendacje** — co dalej? Jakie ograniczenia ma analiza?
15. **Jakość kodu** — czytelność, modularność, dokumentacja

---

## Proponowane tematy (student może zaproponować własny)

### Temat A: Przewidywanie cen nieruchomości
- **Zbiór**: Kaggle "House Prices" lub California Housing
- **Metody**: Regresja (Ridge, Lasso, Random Forest), PCA, Feature Engineering
- **Kontekst biznesowy**: Wycena nieruchomości, czynniki wpływające na cenę

### Temat B: Segmentacja klientów sklepu internetowego
- **Zbiór**: Kaggle "E-Commerce" lub "Online Retail"
- **Metody**: Klasteryzacja (K-Means, DBSCAN), reguły asocjacyjne, RFM
- **Kontekst biznesowy**: Personalizacja ofert, cross-selling

### Temat C: Klasyfikacja chorób na podstawie danych medycznych
- **Zbiór**: UCI Heart Disease, Diabetes, lub Breast Cancer
- **Metody**: Klasyfikacja (Random Forest, SVM, kNN), selekcja cech
- **Kontekst biznesowy**: Wspomaganie diagnostyki medycznej

### Temat D: Analiza sentymentu opinii produktów
- **Zbiór**: Amazon Reviews, IMDB Reviews, lub Twitter
- **Metody**: NLP, TF-IDF, klasyfikacja tekstu, word clouds
- **Kontekst biznesowy**: Monitorowanie reputacji marki

### Temat E: Prognozowanie i anomalie w danych czasowych
- **Zbiór**: Dane pogodowe, giełdowe, lub energetyczne (publiczne API)
- **Metody**: Dekompozycja, ARIMA/ETS, Isolation Forest
- **Kontekst biznesowy**: Prognozowanie popytu, wykrywanie awarii

### Temat F: Analiza danych sportowych
- **Zbiór**: Kaggle FIFA Players, NBA Stats, lub Football
- **Metody**: Klasteryzacja graczy, regresja wartości, PCA
- **Kontekst biznesowy**: Scouting, analiza wydajności

---

## Kryteria oceny szczegółowe

| Kryterium | Waga | Opis |
|-----------|------|------|
| Przetwarzanie danych | 15% | Czyszczenie, imputacja, kodowanie, transformacje |
| Eksploracja (EDA) | 15% | Wykresy, statystyki, zrozumienie danych |
| Metody eksploracji | 25% | Poprawność zastosowania algorytmów |
| Ewaluacja modeli | 15% | Metryki, walidacja krzyżowa, porównanie |
| Wizualizacje | 10% | Czytelność, estetyka, informatywność |
| Interpretacja wyników | 10% | Wnioski, kontekst biznesowy/naukowy |
| Jakość kodu i raportu | 10% | Czytelność, struktura, dokumentacja |

---

## Źródła danych (darmowe)

- **Kaggle**: https://www.kaggle.com/datasets (wymaga rejestracji — darmowe)
- **UCI ML Repository**: https://archive.ics.uci.edu/ml/
- **OpenML**: https://www.openml.org/
- **data.gov.pl**: https://dane.gov.pl/ (dane polskie)
- **Google Dataset Search**: https://datasetsearch.research.google.com/
- **scikit-learn**: wbudowane zbiory (`sklearn.datasets`)
- **seaborn**: wbudowane zbiory (`sns.load_dataset()`)

---

## Format oddania

1. **Jupyter Notebook** (`.ipynb`) — preferowany format:
   - Markdown z opisem każdego kroku
   - Kod z wynikami (uruchomiony)
   - Wizualizacje inline
2. **Alternatywnie**: skrypt `.py` + raport PDF
3. Projekt wgrać na platformę e-learningową lub przesłać mailem
4. **Prezentacja** — 10-15 minut na ostatnich zajęciach

---

