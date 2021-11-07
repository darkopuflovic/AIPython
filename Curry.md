# Curry, Uncurry, Compose

"Currying" predstavlja tehniku koja se koristi za transformisanje funkcije koja ima više argumenata u više poziva funkcija koje imaju jedan ili više argumenata u sebi (najčešće jedan). Naziv je dobila po matematičaru Haskelu Kariju (`Eng. Haskell Curry`).

```python
def dodaj(a, b, c):
    return a + b + c

curry = lambda a: lambda b: lambda c: dodaj(a, b, c)

curry(1)(2)(3)
```
|Output>|`'6'`|
|-------|:-------:|

Ovakav pristup može da bude koristan iz više razloga. Na ovaj način je moguće pojednostaviti pozive funkcija koje prihvataju više argumenata, ukoliko su neki od njih unapred poznati. Takođe je moguće kreirati funkciju koja prihvata manje argumenata iz funkcije koja ih prihvata više.
Još jedna prednost je kombinacija sa drugim funkcijama. Prilagođavanje broja argumenata može da bude korisno kod ulančavanja funkcija.

<p align="center">
  <img src="Slike/Curry.png" />
</p>

Kod koji smo videli u prethodnom primeru, korišćenjem lambda izraza, može da se napiše i korišćenjem metoda. U tom slučaju, kod bi izgledao ovako:

```python
def dodaj(a, b, c):
    return a + b + c

f = dodaj

def g(x):
    def h(y):
        def i(z):
            return f(x, y, z)
        return i
    return h

dodajCurry = g

dodajCurry(1)(2)(3)
```

|Output>|`'6'`|
|-------|:-------:|

Inverzna transformacija se naziva `"Uncurry"`.

```python
def dodaj(x, y, z):
    return x + y + z

curry = lambda x: lambda y: lambda z: dodaj(x, y, z)

uncurry = lambda x, y, z: curry(x)(y)(z)

uncurry(1, 2, 3)
```
|Output>|`'6'`|
|-------|:-------:|

Da se sada upoznamo sa još jednom odlikom Python programskog jezika. Argumenti funkcija, osim što mogu da budu predstavljeni nazivom, razmaknuti zarezom ili kao inicijalizovani argumenti, koji su u startu postavljeni na neku vrednost, mogu da budu predstavljeni i nečime što se zove `starred expression`. Na ovaj način, bilo koju kolekciju je moguće tretirati kao listu argumenata (prostih), dok je dictionary moguće tretirati kao listu inicijalizovanih argumenata. Sintaksa koja omogućava ovu funkcionalnost je sledeća:
1. `*args` ili bilo koji drugi naziv kolekcije je prosta lista argumenata
2. `**kwargs` ili bilo koji drugi naziv dictionary kolekcije je lista inicijalizovanih argumenata

Ukoliko unapred ne znamo koliko argumenata želimo da prosledimo funkciji ili želimo funkciju koja može da prihvati neograničeni broj argumenata, onda je ovo pravi način da se takva funkcionalnost realizuje.

```python
def funkcija(*a, **k):
    print("args: ", a)
    print("kwargs: ", k)

funkcija("Jedan", "Dva", "Tri", first = "Prvi", second = "Drugi")
```

|Output>|`args: ('Jedan', 'Dva', 'Tri')`|
|-------|:-------|
|       |`kwargs: {'first': 'Prvi', 'second': 'Drugi'}`|

Ovaj način prihvatanja argumenata može da nam bude od koristi ukoliko želimo da napišemo funkciju koja će da bilo koju drugu funkciju transformiše u `curry` verziju iste funkcije.

```python
def curry(func, *args, **kwargs):
    if (len(args) + len(kwargs)) > func.__code__.co_argcount:
        return "Greška u broju argumenata"
    if (len(args) + len(kwargs)) == func.__code__.co_argcount:
        return func(*args, **kwargs)
    return (lambda *x, **y: curry(func, *(args + x), **dict(kwargs, **y)))

def add(x, y, z):
    return x + y + z

curryVerzijaAdd = curry(add)

print(curryVerzijaAdd(10)(20)(30))
```

|Output>|`60`|
|-------|:-------:|

Ili na drugi način, korišćenjem dekoratora:

```python
def curry(func, *args, **kwargs):
    if (len(args) + len(kwargs)) > func.__code__.co_argcount:
        return "Greška u broju argumenata"
    if (len(args) + len(kwargs)) == func.__code__.co_argcount:
        return func(*args, **kwargs)
    return (lambda *x, **y: curry(func, *(args + x), **dict(kwargs, **y)))

@curry  #Dekorator
def add(x, y, z):
    return x + y + z

print(add(10)(20)(30))
```

|Output>|`60`|
|-------|:-------:|

Parametri:
- `func`
  - Funkcija koja se prosleđuje (korišćenjem dekoratora ili direktno, pozivom curry funkcije)
- `*args`
  - Lista prostih argumenata (samo se oni koriste u ovom primeru)
- `**kwargs`
  - Lista argumenata sa inicijalnim vrednostima

Funkcija `curry` prvenstveno proverava da li je broj argumenata veći od broja argumenata koji funkcija prihvata. Ukoliko je to slučaj, nemoguće je pozvati funkciju, pa se korisnik o tome obaveštava adekvatnom porukom.

Ukoliko ovaj uslov nije ispunjen funkcija `curry` proverava da li je broj argumenata koji se nalazi u promenjivama `args` i `kwargs` jednak broju argumenata koji funkcija prihvata. Ukoliko jeste, moguće je odmah pozvati funkciju sa odgovarajućim argumentima.

Ukoliko `curry` funkcija ima manje od broja argumenata koji je funkciji potreban, onda se kreira `lambda` izraz, koji prihvata, kao i `curry` funkcija, neograničeni broj novih parametara. U tom slučaju, iz funkcije se vraća `lambda expression`, koji dalje iz koda može da se poziva na isti način kao da se radi o funkciji koja ima onoliko manje argumenata koliko ih je do tada funkcija `curry` već prihvatila.

U svakom od ovih koraka, nije neophodno da se funkciji `curry` ili `lambda` izrazu prosleđuje samo po jedan argument. Moguće je proslediti i više. 

```python
print(add(10))

print(add(10, 20)(30))

print(add(10, 20))

print(add(10)(20)(30)(40))
```

|Output>|`<function __main__.curry.<locals>.<lambda>(*x, **y)>`|
|-------|:-------|
|       |`60`|
|       |`<function __main__.curry.<locals>.<lambda>(*x, **y)>`|
|       |`'Greška u broju argumenata'`|

Prvi poziv funkcije sa jednim argumentom, vraća nam lambda izraz, koji očekuje ostatak argumenata.

U drugom primeru, broj argumenata je jednak broju argumenata koji funkcija očekuje, iako su oni prosleđeni kroz dva poziva, prvi sa dva argumenta, a drugi put je pridodat još jedan. Zato je i rezultat ovog poziva broj `60`, koji predstavlja zbir ova tri broja.

Treći slučaj je sličan prvom. Razlika je u tome da je u ovom slučaju prosleđeno dva argumenta, pa se funkcija koja je vraćena razlikuje od prve po tome što očekuje još samo jedan, za razliku od prve, koja očekuje dva.

Poslednja naredba sadrži poziv koji nije validan, zato što prihvata četiri argumenta, a naša funkcija može da prihvati samo tri, pa se zato vraća string koji nam govori o tome da je nemoguće pozvati funkciju sa toliko argumenata. Ukoliko tako izaberemo, moguće je bilo proslediti funkciji samo tri prva ili poslednja argumenta i ipak je izvršiti, ali to ovde nismo odabrali kao opciju.

Funkcija `compose` nam omogućava da više različitih funkcija objedinimo u samo jedan poziv. Kombinaciju ovih funkcija izvršićemo automatski, iako je bilo moguće kreirati i lambda izraz ili funkciju koja će da ovu kompoziciju uradi za nas, ipak je jednostavnije prepustiti funkciji `compose` da to uradi za nas.

```python
add = lambda x: x + 10
subtract = lambda x: x - 2
multiply = lambda x: x * 10
divide = lambda x: x // 2

def compose(*funcs):
    head, *tail = funcs
    if len(tail) >= 2:
        return lambda x: compose(*tail)(head(x))
    else:
        [first] = tail
        return lambda x: first(head(x))

tempFunc = compose(add, multiply, add, subtract, divide)

print(tempFunc(10))

combination = lambda x: divide(subtract(add(multiply(add(x)))))

print(combination(10))  # Korišćenjem lambda izraza
```

|Output>|`104`|
|-------|:-------|
|       |`104`|

Čest slučaj kada ovakva funkcija može da nam koristi su transformacije jedinica SI sitema, jedinica vremena, temperature, valuta. Kod je uprošćen na ovaj način, tako da se svodi na jedan poziv funkcije.

U kodu iznad, linija `head, *tail = funcs` služi da podeli listu funkcija (`funcs`) na prvu funkciju (`head`) i ostale funkcije (`tail`).

`*tail` je `starred expression` koji smo ranije pomenuli i on omogućava da se lista funkcija koja se nalazi u tail prosledi funkciji kao lista parametara.

`[first] = tail` može da se koristi samo u slučaju kada znamo da `tail` sadrži isključivo jedan element. U tom slučaju će u first promenjivu da bude smešten taj jedan element kolekcije.

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