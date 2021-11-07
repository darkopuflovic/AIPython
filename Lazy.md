# Lenjo izračunavanje (Lazy evaluation)

Lenjo izračunavanje je odlika funkcionalnih programskih jezika, da se vrednost izraza ne izračunava sve dok njegova vrednost nije neophodna za dalje izvršenje programa. Ovo sa sobom nosi nekoliko prednosti i mana.

- Za početak, prednost ovog pristupa u odnosu na standardno izvršenje izraza je ta da izrazi koji se nalaze u kodu, a čija se vrednost nikada ne koristi se neće nikada ni izračunati. Ovo može da ubrza kod u slučaju da je on napisan nepažljivo i sa dosta koda koji je nepotreban, što je čest slučaj u programiranju.
- Još jedna prednost je ta što ovaj pristup omogućava izvršavanje koda koji ne bi bio moguć bez korišćenja lenjog izračunavanja, kao što su beskonačne kolekcije. U ovom slučaju, kreiranje ovakve kolekcije samo aplikaciji sugeriše kako treba kreirati jednu takvu iteraciju, ali ne kreira same elemente, što bi rezultiralo kodom koji se nikada neće izvršiti do kraja (problem beskonačne petlje). Onda kada je poznato koji od elemenata će biti potrebni za dalje izvršenje aplikacije, tada će `samo ti elementi` i biti kreirani.
- Sve ovo nam omogućava i optimizacije koje zavise od lenjog izračunavanja. Naime, kod koji se koristi, kreira stabla izvršenja izraza. Ova stabla je moguće kombinovati ukoliko se u ostatku koda pojavi izraz koji zavisi od prethodnog izraza. U tom slučaju je moguće stablo transformisati, tako da je rezultujuće stablo jednostavnije od proste kombinacije dva stabla.
   - Primer za ovo bi mogao da bude optimizacija celobrojnog deljenja u Python-u. Ovo je samo primer, ne i način na koji Python funkcioniše, ali je na njemu jednostavno demonstrirati ovakve optimizacije. Zamisimo da u kodu imamo liniju u kojoj se računa količnik dva broja korišćenjem `/` operatora, pre koga se nalazi funkcija koja nam ovako dobijen floating point broj kastuje u `int` tip. Ove dve naredbe bi lako mogle da budu u stablu zamenjene `//` operatorom, koji je brži, zato što se primenjuje celobrojno deljenje, a pored toga, potpuno zaobilazimo potrebu za kastovanjem.
- Konačno, prednost ovog pristupa je i korišćenje memorije. Vrednost koja se računa neposredno pre nego što se i koristi omogućava da se odmah nakon njene upotrebe ona i obriše iz memorije, što nam veći deo vremena pruža mogućnost da memorija bude manje iskorišćena.

- Mana ovog pristupa je ta što se izraz izvršava svaki put kada je njegov rezultat potreban. Ovaj problem može biti rešen kopiranjem rezultata koji je potreban više puta u memoriju i korišćenjem ove memorijske reprezentacije svaki sledeći put. Ipak, Python u većini slučajeva ovaj problem može da reši i sam, zato što se vrednost prethodnih izvršenja pamti u memoriji automatski, ukoliko je u ostatku koda potreban ponovo, pa nam to olakšava implementaciju i uklanja brigu o ovom problemu.

Osnovni tip podataka sa kojima Python radi, kada je reč o lenjom izračunavanju jesu iteracije. Transformacija kolekcija podataka, kao što su liste, može da se izvrši u iteracije korišćenjem funkcije `iter`. Takođe, kod pisanja funkcija, moguće je kreirati iteracije korišćenjem ključne reči `yield` koja može da se koristi kao "zamena" za `return` ključnu reč. Razlika je, osim u tipu podataka koji funkcija proizvodi i u tome što `return` ključna reč prekida izvršenje funkcije, dok `yield` ključa reč vraća podatak kao deo iteracije koju kreira, ali ne prekida izvršenje.

Kada je potrebno pristupiti n-tom elementu iteracije, postoje dva pristupa. Oba imaju svoje mane. 
1. Transformisanje iteracije u kolekciju (npr. listu), pa je nakon toga moguće koristiti indekse ili funkcije za pristup odgovarajućem elementu. Negativna strana ovog pristupa je kreiranje memorijske strukture koja pamti kolekciju.
2. Korišćenjem `next` metode, koja pristupa sledećem elementu iteracije. Negativna strana ovog pristupa se pojavljuje u slučaju kada ne želimo da pristupimo svim elementima, već samo određenim, čije indekse znamo unapred. To je u ovom slučaju nemoguće, pa moramo da pristupimo svakom elementu pre našeg traženog.

```python
def iteracija():
    for i in range(1, 20):
        if i % 2 == 0:
            yield i

for x in iteracija():
    print(x, end = " ")
```

|Output>|`2 4 6 8 10 12 14 16 18`|
|-------|:-------:|

Argument koji se prosleđuje `print` funkciji (`end`), nam omogućava da zamenimo novu liniju koja bi se u osnovnom pozivu bez ovog argumenta kreirala, razmakom, da bi svi rezultati bili u jednoj liniji, što nam je u ovom slučaju preglednije.

Ključna reč `yield` koju vidimo u funkciji `iteracija` se koristi da bi funkcija, kao svoju povratnu vrednost, imala tip `generator`, a ne kolekciju parnih brojeva. Svaki od ovih elemenata se prikuplja onda kada je neophodan. `for` petlja može da radi sa iteratorima, pa nema potrebe koristiti specijalne funkcije ili transformisati ovu iteraciju u kolekciju. Ukoliko ne koristimo ugrađeni mehanizam `for` petlje, onda moramo da koristimo `next` metodu:

```python
generator = iteracija()
print(next(generator), end = " ")
print(next(generator), end = " ")
print(next(generator), end = " ")
```

|Output>|`2 4 6`|
|-------|:-------:|

Metoda `next` prihvata kao argument iteraciju. Ova iteracija, u svom stanju ima informaciju o trenutnom elementu, pa kada bude prosleđena `next` metodi, prosleđuje element koji je sledeći i ažurira stanje.

Transformacija kolekcije u iteraciju se vrši na sledeći način:

```python
listIter = iter([1, 2, 3, 4, 5])
print(next(listIter), end = " ")
print(next(listIter), end = " ")
print(next(listIter), end = " ")
print(next(listIter), end = " ")
print(next(listIter), end = " ")
```

|Output>|`1 2 3 4 5`|
|-------|:-------:|

Ukoliko još jednom pozovemo metodu `next`, kod će nam vratiti grešku, koja nam govori da je iteracija došla do poslednjeg elementa i da je nemoguće vratiti sledeći element.

```python
print(next(listIter), end = " ")
```

|Output>|`StopIteration`|
|-------|:-------:|

##

|Navigacija|
|:-------|
|[Funkcije](Funkcije.md)|
|[Monad](Monad.md)|
|[Memoizacija](Memoizacija.md)|
|[Lambda izrazi](Lambda.md)|
|[Lazy evaluation](Lazy.md)|
|[Curry, Uncurry, Compose](Curry.md)|
|[Python funkcije (min, max, map, filter, zip)](Functions.md)|
|[Biblioteke](Library.md)|
|[functools modul](functools.md)|
|[operator modul](operator.md)|
|[itertools modul](itertools.md)|
|[Comprehensions](Comprehensions.md)|
|[Regularni izrazi](RegularExpressions.md)|
|[Pattern matching](PatternMatching.md)|