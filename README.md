# Machine-Learning-Feature-Selection
Project wykonany na przedmiot Zaawansowane Metody Uczenia Maszynowego na wydziale MINI PW

## Cel projektu

Nalezy zaproponowac metody selekcji zmiennych oraz klasyfikacji które umozliwiaja zbudowanie
modelu o duzej mocy predykcyjnej przy uzyciu mozliwie małej liczby zmiennych.

## Zbiór danych

Zbiór *artificial* to sztuczny zbiór w którym sa ukryte istotne zmienne 
(pliki: *artificial train.data, artificial train.labels, artificial valid.data*).

| Dane       | Liczba zmiennych | Liczba obserwacji (treningowy) | Liczba obserwacji (walidacyjny) |
| ---------- | ---------------- | ------------------------------ | ------------------------------- |
| artificial | 500              | 2000                           | 600                             |

Dane potrzebne do wykonania projektu znajduja sie na stronie https://home.ipipan.waw.pl/p.teisseyre/TEACHING/ZMUM/index.html.

## Testowane Modele

- Linear Regression
- K-Neighbours Classifier
- Decision Tree Classifier
- Random Forest Classifier

## Korelacje danych

Najbardziej skorelowane kolumny:

<!--Correlation Matrix-->

Widać że istnieją wysoko skorelowane cechy - selekcja jest niezbędna, aby uzyskać zoptymalizowane pod kątem uczenia maszynowego dane.

## Metody selekcji zmiennych

### Metoda R² Score Denoisser

Algorytm polega na wytrenowaniu modelu dla każdej zmiennej z osobna. Następnie wybranu tych zmiennych dla których funkcja `score()` odpowiadającego im modelu zwraca wartość metryki R² > 0.

Wewnątrz algorytmu umożliwiłem wybór jednego z dwóch modeli regresyjnych do wyliczenia wartości R².

Cechy jakie zostały wybrane przez te modele:

 ***DecisionTreeRegressor***

```
R² Score Denoisser
Regressor: DecisionTreeRegressor
Number of Selected Features: 18
Selected Feaures: 
28 48 64 105 128 153 241 281 318 336 378 433 442 451 453 472 475 493
```

***KNeighborsRegressor***

```
R² Score Denoisser
Regressor: KNeighborsRegressor
Number of Selected Features: 21
Selected Feaures: 
4 59 64 105 128 153 156 241 280 281 295 336 338 392 433 442 453 455 472 475 493
```

### Metoda Boruta

"Matoda Boruta, polega na dodawaniu dodatkowych, mało istotnych cech (tzw. cieni) do istniejącego zbioru cech w danym systemie informacyjnym. Następnie są one wykorzystywane jako odniesienie dla oceny istotności oryginalnych atrybutów w kontekście pełnej struktury analizowanych danych, przy czym atrybuty są oceniane w oparciu o miarę przydatności obliczoną na podstawie lasów losowych wuczanych podczas treningu na danych. Na podstawie tej miary metoda Boruta prowadzi selekcję iteracyjnie, usuwając 
z systemu informacyjnego stopniowo cechy uznane za nieistotne, przy czym dzięki dodanym cieniom 
i dobranej mierze przydatności atrybutów, selekcja jest bardziej stabilna i niepodatna na przeuczene czy też szumy w danych." ~ [Stabilne i wydajne metody selekcji cech z wykorzystaniem systemów uczących się - Miron Kursa](https://www.mimuw.edu.pl/sites/default/files/recenzja_mgr_kursa_bazan.pdf)

Cechy jakie zwrócił algorytm:

```
Boruta
Confirmed: 	19
Tentative: 	1
Number of Selected Features: 20
Selected Features: 
10 28 48 64 105 128 153 241 281 318 336 338 378 433 442 451 453 472 475 493
```

Poniżej rozkłady cech wybranych przez algorytm boruta.

 <!--Features Distribution-->

Część z nich ma rozkład zbliżony do Gausowskiego, część wykazuje lekki charakter dwumodalny.  Uznałem za konieczne użycie jedynie standaryzacji.

W ostatecznym rozrachunku to właśnie cechy wybranę przez Borutę posłużyły do wyterenowania modelu, dlatego zaprezentuję działania modeli klasyfikatorów właśnie na tych cechach.

## Modele Klasyfikacyjne

Wszystkie modele testowane w tym projekcie miały parametry dobrane na podstawie wyniku zachłannego poszukiwania w przestrzeni tych parametrów. 

 <!--Log Reg Parameters Grid Search.png-->

Miarą oceny algorytmów jest *accuracy* ponieważ klasy są równomiernie reprezentowane.

### Regresja Logistyczna

Ten algorytm prezentuje podejście naiwne w problemie predykcji jednej dwóch klas. 

Wybrane Parametry:

```
logreg__C            0.151991
```

Wynik:

```
accuracy on the test set:  0.598
accuracy on the train set:  0.628
```

Macierz pomyłek na zbiorze testowym:

|               | **Predicted -1** | **Predicted 1** |
| ------------: | ---------------: | --------------- |
| **Actual -1** |              115 | 123             |
|  **Actual 1** |              134 | 128             |

### K-Neighbors Classifier

Wybrane Parametry:

```
knn__n_neighbors                  3
knn__weights                uniform
```

Wynik:

```
accuracy on the train set:  0.94
accuracy on the test set: 0.88
```

Macierz pomyłek na zbiorze testowym:

|               | **Predicted -1** | **Predicted 1** |
| ------------: | ---------------: | --------------- |
| **Actual -1** |              211 | 27              |
|  **Actual 1** |               33 | 229             |

### DecisionTree Classifier

Wybrane Parametry:

```
dectree__class_weight               None
dectree__criterion                  gini
```

Wynik:

```
accuracy on the train set:  1.0
accuracy on the test set:  0.79
```

Macierz pomyłek na zbiorze testowym:

|               | **Predicted -1** | **Predicted 1** |
| ------------: | ---------------: | --------------- |
| **Actual -1** |              108 | 130             |
|  **Actual 1** |              125 | 137             |

### Random Forest

Wybrane Parametry:

```
randtree__max_depth                  14
```

Wynik:

```
accuracy on the train set:  1.0
accuracy on the test set:  0.896
```

Macierz pomyłek na zbiorze testowym:

|               | **Predicted -1** | **Predicted 1** |
| ------------: | ---------------: | --------------- |
| **Actual -1** |              117 | 121             |
|  **Actual 1** |              129 | 133             |

### Porównanie jakości

Dokładność na zbiorze testowym:

```
Logistic Regression: 0.59
K-Neighbors Classifier: 0.88
Decision Tree Classifier: 0.79
RandomForestClassifier: 0.89
```

## Wyniki

- Najlepsze cechy (20 kolumn) wybrał algorytm Boruta
- Najlepszym modelem okazał się: K-Neighbors Classifier

