# Comprehensions

Comprehension sintaksa u Python programskom jeziku omogućava jednostavno kreiranje kolekcija, korišćenjem postojeće kolekcije. Kolekcija koja se kreira na ovaj način može da bude `list`, `tuple`, `dict` ili `set`.

Sintaksa je jednostavna i podseća na korišćenje ternarnog uslovnog operatora. Pogledajmo na primeru kako se ova sintaksa koristi:

```python
[x for x in range(1, 11)]
```

|Output>|`[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`|
|-------|:-------:|

Identičan rezultat možemo da postignemo korišćenjem `for petlje`.

```python
newList = []

for x in range(1, 11):
    newList.append(x)

print(newList)
```

|Output>|`[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`|
|-------|:-------:|

Element koji se upisuje u rezultujući niz ne mora da bude prepisan iz iteracije koja se koristi za njegovo kreiranje:

```python
list((x, y) for x in range(1, 3) for y in range(1, 3))
```

|Output>|`[(1, 1), (1, 2), (2, 1), (2, 2)]`|
|-------|:-------:|

Na prethodnom primeru možemo da primetimo nekoliko novih odlika `comprehensions` sintakse. Osim što tip elementa ne mora da bude prost, već možemo da kreiramo `tuple` tip, sama kolekcija ne mora da bude lista, već takođe `tuple`, koji kasnije konvertujemo u listu. Pored toga, nije neophodno koristiti jednu petlju kada koristimo `comprehensions` sintaksu, već možemo da ih imamo više.

Treba obratiti pažnju i na `set` tip podataka i njegove odlike. Ukoliko kreiramo kolekciju tog tipa, elementi ne mogu da se ponavljaju, a redosled nije zagarantovan:

```python
{ x % 3 + 1 for x in range(1, 11) }
```

|Output>|`{ 1, 2, 3 }`|
|-------|:-------:|

Na sledećem primeru ćemo videti kako ovu sintaksu možemo da koristimo za kreiranje rečnika:

```python
{ x: chr(x) for x in list(range(97, 97 + 26 )) + list(range(65, 65 + 26)) }
```

|Output>|`{97: 'a', 98: 'b', 99: 'c', 100: 'd', 101: 'e', ... , 87: 'W', 88: 'X', 89: 'Y', 90: 'Z'}`|
|-------|:-------:|

Prethodno kreirani rečnik sadrži ASCII vrednosti za karaktere koji predstavljaju mala i velika slova engleske abecede.

U `comprehensions` sintaksi postoji mogućnost za korišćenje uslova. Uslovi mogu da se koriste za dobijanje vrednosti (ternarni operator) ili na kraju izraza, kada se vrši filtriranje elemenata koji će kreirati rezultujuću kolekciju.

```python
print([x if x % 2 == 0 else 100 for x in range(1, 11)])
print([x for x in range(1, 11) if x < 5])
```

|Output>|`[100, 2, 100, 4, 100, 6, 100, 8, 100, 10]`|
|-------|:-------|
|       |`[1, 2, 3, 4]`|

Primenom ternarnog operatora vrednost za neparne brojeve možemo da zamenimo konstantnom vrednošću, dok svi ostali elementi kolekcije neometano prelaze u rezultujuću kolekciju.

Kada se koristi uslov na kraju izraza, elementi ulazne kolekcije se filtriraju po uslovu. Samo elementi koji zadovoljavaju uslov će kreirati element u rezultujućoj kolekciji.

## map, filter

Sintaksa koju koristimo za kreiranje `comprehensions` kolekcija može da se koristi na sličan način kao `map` i `filter` funkcije.

Pogledajmo jedan primer:

```python
print(list(map(lambda x: x + 10, range(1, 11))))
print([x + 10 for x in range(1, 11)])
```

|Output>|`[11, 12, 13, 14, 15, 16, 17, 18, 19, 20]`|
|-------|:-------|
|       |`[11, 12, 13, 14, 15, 16, 17, 18, 19, 20]`|

Rezultat primenom obe metode je identičan, pa je u većini slučajeva moguće birati između `map` funkcije i `comprehensions` sintakse.

```python
print(list(filter(lambda x: x < 5, range(1, 11))))
print([x for x in range(1, 11) if x < 5])
```

|Output>|`[1, 2, 3, 4]`|
|-------|:-------|
|       |`[1, 2, 3, 4]`|

Na sličan način je moguće zameniti poziv `filter` funkcije `comprehensions` izrazom korišćenjem uslova. Uslov uspešno filtrira elemente koji su prosleđeni za kreiranje kolekcije.

## reduce

`Comprehensions` sintaksa ne može da se koristi kao zamena za `reduce` funkciju, ali postoje neki slučajevi kada ona može da proizvede iste rezultate. Pogledajmo sledeći primer:

```python
listT = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

print(reduce(lambda x, y: x + y, listT))

print([item for subList in listT for item in subList])
```

|Output>|`[1, 2, 3, 4, 5, 6, 7, 8, 9]`|
|-------|:-------|
|       |`[1, 2, 3, 4, 5, 6, 7, 8, 9]`|

Iako je pristup rešavanju problema eliminisanja podlisti potpuno drugačiji, rešenje je identično. `reduce` funkcija koristi `+` operator nad podlistama (`[1, 2] + [3, 4] = [1, 2, 3, 4]`) koje se nalaze u glavnoj listi, da bi kreirala konačnu listu. Pristup kod `comprehensions` sintakse je potpuno drugačiji i ova sintaksa je verovatno pogodnija za rešavanje ovakvog problema. Lista `listT` se najpre deli na podliste, koje se smeštaju u `sublist` promenjivu, a zatim se u drugoj petlji prolazi kroz elemente ove podliste, koji se direktno upisuju u rezultujući niz.

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