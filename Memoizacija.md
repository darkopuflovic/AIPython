# Memoizacija

Određeni broj rekurzivnih funkcija, pogotovo kada se radi o kodu koji rešava složenije probleme, se realizuje tako da se rekurzivni pozivi funkciji ponavljaju. To znači da će isti rezultat biti određivan u više različitih poziva funkcije.

Očigledno je da ovaj pristup nije najoptimalniji. Da bi rešili ovaj problem i optimizovali naš kod, neophodno je pronaći način da se rešenja prethodnih poziva funkcije zapamte, da bi kasnije mogla da se koriste po potrebi.

Memoizacija je naziv za ovu optimizaciju. Izmene koje kod mora da pretrpi uglavnom nisu velike, ali ušteda u vremenu izvršanja može biti ogromna. Ipak, potrebno je voditi računa na koji način se ova optimizacija koristi, zato što ona može da upotrebi isuviše veliki deo memorije računara za pamćenje svih stanja, pa na taj način možemo da izgubimo više nego što dobijamo od ubrzanja.

Najbolje ćemo memoizaciju demonstrirati na prostom primeru, određivanja n-tog Fibonačijevog broja:

```python
def fibonacci(n):
    if (n <= 1):
        return n
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```

Kod iznad ne koristi memoizaciju. Poziv ove funkcije može da traje jako puno, pogotovo za velike brojeve (veće od 35).

```python
from timeit import default_timer as timer
from datetime import timedelta

def testFibonacci():
    start = timer()
    val = fibonacci(40)
    end = timer()
    return (val, str(timedelta(seconds = end - start)))

print(testFibonacci())
```
|Output>|`(102334155, '0:00:44.224355')`|
|-------|:-------:|

Poziv za argument `n` koji je `40`, traje oko `40 sekundi`.
Ukoliko želimo da pronađemo `100-ti` Fibonačijev broj ili bilo šta veće od toga, ovaj pristup nas ne bi odveo daleko. Zato ćemo da pokušamo da ovaj kod izmenimo, dodavanjem memoizacije.

```python
def fibonacciMemo(n, memo = {}):
    if (n <= 1):
        memo[n] = n
        return n
    else:
        if ((n - 1) in memo.keys()):
            nm1 = memo[n - 1]
        else:
            nm1 = fibonacciMemo(n - 1, memo)
            memo[n - 1] = nm1
        if ((n - 2) in memo.keys()):
            nm2 = memo[n - 2]
        else:
            nm2 = fibonacciMemo(n - 2, memo)
            memo[n - 2] = nm2
    return nm1 + nm2
```

Kod se razlikuje od jednostavnog rekurzivnog rešenja, ali ukoliko bolje pogledamo, ne razlikuje se previše. Dodat je argument `memo`, koji će biti naš rečnik koji pamti sve prethodno izračunate vrednosti. Osim toga, kod se razlikuje samo u proveri, da li vrednost za dati argument `n` već postoji u tom rečniku. Ukoliko ne postoji, računa se na isti način kao i ranije i dodaje u rečnik. Ukoliko postoji, preskočićemo rekurzivni poziv i rezultat preuzeti direktno iz rečnika. Na kraju, rezultat se dobija kao zbir prethodna dva elementa, kao i u originalnom primeru, uz izmenu, da sada vrednosti mogu da potiču od rekurzivnog poziva, ali i iz rečnika, što nam drastično skraćuje vreme izvršenja funkcije.

```python
from timeit import default_timer as timer
from datetime import timedelta

def testFibonacciMemo():
    start = timer()
    val = fibonacciMemo(100)
    end = timer()
    return (val, str(timedelta(seconds = end - start)))

print(testFibonacciMemo())
```

|Output>|`(354224848179261915075, '0:00:00.000006')`|
|-------|:-------:|

Vidimo da je vreme potrebno za računanje `100-tog` Fibonačijevog broja sada neuporedivo manje. Na ovaj način možemo da izračunamo i mnogo veći broj, bez potrebe da kod transformišemo u iterativni oblik ili da tražimo rešenje koje će da problem reši na način koji je teže shvatiti od rekurzivnog rešenja.

Na ovaj način je moguće izmeniti i komplikovaniji algoritam, ali modifikacije koda su nepoželjne kada imamo intuitivan algoritam, kao što su to rekurzivni algoritmi. Zato, memoizaciju iz Python programskog jezika možemo da koristimo i na mnogo jednostavniji način, korišćenjem modula `functools`.

```python
from functools import cache

def fibonacciCache(n):
    if n <= 1:
        return n
    else:
        return fibonacciCache(n - 1) + fibonacciCache(n - 2)

fibonacciCache = cache(fibonacciCache)
```

Ili korišćenjem dekoratora:

```python
from functools import cache

@cache
def fibonacciCacheD(n):
    if n <= 1:
        return n
    else:
        return fibonacciCacheD(n - 1) + fibonacciCacheD(n - 2)
```

Na ovaj način, uključivanjem funkcije `cache` iz modula `functools` i korišćenjem jednog poziva ove funkcije ili dekoratora `@cache`, moguće je implementirati metodu na gotovo identičan način kao što je to bio slučaj sa `fibonacciMemo` funkcijom. Sada, možemo da pozovemo ovu metodu na identičan način kao i `fibonacciMemo` metodu i vidimo koliko će joj vremena trebati da odredi `100-ti` Fibonačijev broj.

```python
from timeit import default_timer as timer
from datetime import timedelta

def testFibonacciCache():
    start = timer()
    val = fibonacciCache(100)
    end = timer()
    return (val, str(timedelta(seconds = end - start)))

print(testFibonacciCache())
```

|Output>|`(354224848179261915075, '0:00:00.000118')`|
|-------|:-------:|

```python
from timeit import default_timer as timer
from datetime import timedelta

def testFibonacciCacheD():
    start = timer()
    val = fibonacciCacheD(100)
    end = timer()
    return (val, str(timedelta(seconds = end - start)))

print(testFibonacciCacheD())
```

|Output>|`(354224848179261915075, '0:00:00.000094')`|
|-------|:-------:|

Vidimo da je rezultat gotovo identičan, uz mala odstupanja, koja su prisutna zato što se radi o veoma kratkim vremenskim intervalima. Kako se broj n povećava, vremena izvršenja za sve 3 verzije memoizacije su približno jednaka.

Treba napomenuti i to da pored `cache` funkcije, modul `functools` sadrži i funkciju `lru_cache`. Razlika između ove dve funkcije je postojanje dodatnih argumenata kod funkcije `lru_cache`: `maxsize` i `typed`. `maxsize` nam omogućava da postavimo maksimalnu veličinu rečnika koji će pamtiti vrednosti, dok nam `typed` omogućava da različite tipove posmatramo na isti ili različiti način na mestu argumenta (npr. 3 i 3.0 su kod `typed=True` različiti unosi u rečniku, dok su kod `typed=False` isti).

```python
from timeit import default_timer as timer
from datetime import timedelta
from functools import lru_cache

def fibonacciLRU(n):
    if n <= 1:
        return n
    else:
        return fibonacciLRU(n - 1) + fibonacciLRU(n - 2)

fibonacciLRU = lru_cache(maxsize=4, typed=True)(fibonacciLRU)

def testFibonacciLRU():
    start = timer()
    val = fibonacciLRU(100)
    end = timer()
    return (val, str(timedelta(seconds = end - start)))

print(testFibonacciLRU())
```

|Output>|`(354224848179261915075, '0:00:00.000454')`|
|-------|:-------:|

Ili ekvivalentni kod napisan korišćenjem dekoratora:

```python
from timeit import default_timer as timer
from datetime import timedelta
from functools import lru_cache

@lru_cache(maxsize=4, typed=True)
def fibonacciLRUD(n):
    if n <= 1:
        return n
    else:
        return fibonacciLRUD(n - 1) + fibonacciLRUD(n - 2)

def testFibonacciLRUD():
    start = timer()
    val = fibonacciLRUD(100)
    end = timer()
    return (val, str(timedelta(seconds = end - start)))

print(testFibonacciLRUD())
```

|Output>|`(354224848179261915075, '0:00:00.000092')`|
|-------|:-------:|

Vreme izvršenja se ne razlikuje previše, čak ni kada je broj unosa ograničen na `4`. Da vidimo šta bi se desilo kada bi `maxsize` bio još manji.

```python
from timeit import default_timer as timer
from datetime import timedelta
from functools import lru_cache

@lru_cache(maxsize=2, typed=True)
def fibonacciLRUD2(n):
    if n <= 1:
        return n
    else:
        return fibonacciLRUD2(n - 1) + fibonacciLRUD2(n - 2)

def testFibonacciLRUD2():
    start = timer()
    val = fibonacciLRUD2(40)
    end = timer()
    return (val, str(timedelta(seconds = end - start)))

print(testFibonacciLRUD2())
```

|Output>|`(102334155, '0:00:00.355868')`|
|-------|:-------:|

Primetimo da se sada radi o `40-tom` Fibonačijevom broju, a dužina trajanja je već `trećina sekunde`. I dalje je ovaj kod brži od čistog rekurzivnog poziva, ali ne kao u slučaju kada nam je rečnik bar za jedan unos veći (3 bi nam bilo dovoljno da upamtimo sve vrednosti koje su nam potrebne u narednim iteracijama, zato što se radi o Fibonačijevim brojevima).

Pored ovoga, možemo da vidimo i statistiku broja pogodaka i promašaja u našem rečniku. To možemo da uradimo korišćenjem funkcije `cache_info`.

```python
fibonacciLRUD2(40)
print(fibonacciLRUD2.cache_info())
```

|Output>|`CacheInfo(hits=217285, misses=1045216, maxsize=2, currsize=2)`|
|-------|:-------:|

```python
fibonacciLRUD(40)
print(fibonacciLRUD.cache_info())
```

|Output>|`CacheInfo(hits=38, misses=41, maxsize=4, currsize=4)`|
|-------|:-------:|

Ono što možemo da zaključimo iz ove statistike je da ukoliko imamo veći rečnik, neće ni biti toliko rekurzivnih poziva funkcije, zato što rečnik u sebi sadrži dovoljno informacija, da ćemo uglavnom iz njega i dobijati rezultate. Kod rečnika koji ima `maxsize = 2`, broj pogodaka je veći, ali i broj promašaja, što samo znači da se rekurzija desila veći broj puta.

osim funkcije `cache_info`, postoji i funkcija `cache_clear`, koja nam omogućava, da ukoliko nam je to potrebno, obrišemo prethodnu keš memoriju i krenemo sa pamćenjem podataka iz početka.

```python
fibonacciLRU2.cache_clear()
```

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