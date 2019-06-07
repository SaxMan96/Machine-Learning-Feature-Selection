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

## Testowane Metody Selekcji Zmiennych

- Boruta
- R_2 Denoisser

## Testowane Modele

- Linear Regression
- K-Neighbours Classifier
- Decision Tree Classifier
- Random Forest Classifier

## Wyniki

- Najlepsze cechy (20 kolumn) wybrał algorytm Boruta
- Najlepszym modelem okazał się: K-Neighbors Classifier

