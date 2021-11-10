# Python funkcije

## min, max

Metode min i max su slične metodama istog naziva u drugim programskim jezicima. One se koriste za izračunavanje minimuma i maksimuma `n` brojeva. To je i razlika između programskog jezika Python i većine drugih jezika. Broj parametara ovih funkcija nije ograničen.

```python
min(5, 1, 6, 3, 5, 1, 9)
```

|Output>|`'1'`|
|-------|:-------:|

```python
max(5, 1, 6, 3, 5, 1, 9)
```

|Output>|`'9'`|
|-------|:-------:|

Kao što smo ranije videli, Python ima još jednu specifičnost, a to je `starred expression`. Na ovaj način je moguće bilo koju kolekciju proslediti kao listu argumenata ovim funkcijama, iako to nije neophodno, zato što argument može da bude i sama lista.

```python
from random import randint

lista = [randint(1, 100) for _ in range(1, 10)]

print(f"Minimum je: {min(lista)}, dok je maksimum: {max(lista)}")
print(f"Minimum je: {min(*lista)}, dok je maksimum: {max(*lista)}")
```

|Output>|`Minimum je: 4, dok je maksimum: 90`|
|-------|:-------:|
|       |`Minimum je: 4, dok je maksimum: 90`|

## map, filter

Specijalne funkcije, kao što su `map` i `filter` prihvataju kao argument funkciju, kao i listu koju je potrebno obraditi. Ovakav način rada sa kolekcijama je jako intuitivan i jednostavan. Dovoljno je o svakom elementu niza razmišljati kao o jedinom elementu koji je potrebno obraditi korišćenjem funkcije koju pišemo i šaljemo kao argument. Obrada niza se prepušta specijalnoj funkciji, koja našu funkciju izvršava nad svakim elementom niza.

Postoji nekoliko kategorija ovih funkcija, u zavisnosti od ulaznih argumenata i izlazne vrednosti. Funkcije mogu da prihvataju samo jedan niz, ali je moguće da prihvate i više njih. Takođe rezultat može da bude niz iste dužine, kraći niz ili čak broj ili logička vrednost.

Funkcija `map` prihvata niz i vraća niz iste dužine, čiji se svaki element dobija primenom funkcije koju prosleđujemo kao argument.

<p align="center">
  <img src="Slike/Map.png" />
</p>

```python
list(map(lambda x: x + 10, range(1, 10)))
```

|Output>|`[11, 12, 13, 14, 15, 16, 17, 18, 19]`|
|-------|:-------:|

Funkcija `map` ima povratnu vrednost, koja je tipa `map`. Ovaj tip predstavlja specijalni iterator, čiji naziv čuva informaciju da je dobijen primenom funkcije `map`. Da bi prikazali rezultat u obliku liste, neophodno je da ovaj specijalni tip prvo kastujemo u tip `list`.

Funkcija `filter` je slična funkciji `map`, ali za razliku od nje, izlazni niz je `iste dužine ili kraći` od ulaznog niza. Ovo je posledica funkcije koja se prosleđuje, koja u slučaju funkcije `filter` očekuje `bool` povratnu vrednost, pa se u rezultujućoj kolekciji mogu pronaći elementi koji ovaj uslov zadovoljavaju.

<p align="center">
  <img src="Slike/Filter.png" />
</p>

```python
list(filter(lambda x: not x % 2, range(1, 10)))
```

|Output>|`[2, 4, 6, 8]`|
|-------|:-------:|

Kao i u prethodnom slučaju, funkcija `filter` ima povratnu vrednost tipa `filter`. Ovaj tip je jako sličan tipu `map` i takođe je iteracija. Konverzija u listu se vrši na isti način kao i u prethodnom primeru.

## zip

Funkcija `zip` se koristi za spajanje dve kolekcije. Kolekcije se prosleđuju kroz argumente, a funkcija koja obavlja ovo spajanje se ne prosleđuje, već je uvek ista. Svaki element prve kolekcije, sa elementom druge kolekcije na istom indeksu kreira jedan `tuple` tip u rezultujućem nizu.

Treba napomenuti da ova funkcija ne vrši spajanje elemenata niza koji nemaju odgovarajući par u drugom nizu. Spajanje se vrši do kraće dimenzije oba niza.

<p align="center">
  <img src="Slike/Zip.png" />
</p>

```python
list(zip([1, 2, 3], ["1", "2", "3"]))
```

|Output>|`[(1, '1'), (2, '2'), (3, '3')]`|
|-------|:-------:|

Povratni tip ove funkcije je `zip` iteracija, pa je neophodno, kao i u prethodnim primerima kastovati u listu da bi bila prikazana.

## Funkcije iz Python modula
Uključivanje biblioteka: [Biblioteke](Library.md)

## Modul functools



##

|Navigacija|
|:-------|
|[Funkcije](Funkcije.md)|
|[Monad](Monad.md)|
|[Memoizacija](Memoizacija.md)|
|[Lambda izrazi](Lambda.md)|
|[Lazy evaluation](Lazy.md)|
|[Curry, Uncurry, Compose](Curry.md)|
|[Python funkcije (min, max, map, filter, zip, moduli)](Functions.md)|
|[Biblioteke](Library.md)|
|[Comprehensions](Comprehensions.md)|
|[Regularni izrazi](RegularExpressions.md)|
|[Pattern matching](PatternMatching.md)|