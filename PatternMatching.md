# Pattern matching

Pattern matching je deo programskog jezika Python od verzije 3.10.0. Omogućava strukturnu pretragu po šablonima, korišćenjem ključnih reči `match` i `case`. Svaka `case` naredba je praćena šablonom koji `match` naredbe pokušavaju da preklope.

Pretraga šablona se vrši odozgo na dole. Kada se prvi `case` preklopi, prekida se izvršenje `match` bloka i prelazi na ostatak koda.

Šabloni mogu da sadrže više od jednog uslova, koji mogu da se povezuju logičkim operatorima.

```python
match 'a':
    case ('A' | 'B' | 'C') as slovo:
        print(slovo)
    case 'a' if False:
        print("Uslov")
    case ('a', _):
        print("Novi")
    case _:
        print("Default")
```

|Output>|`Default`|
|-------|:-------|

U prethodnom primeru slovo `'a'` pokušavamo da preklopimo sa ukupno četiri `case` šablona. Prvi šablon se sastoji od preklapanja više karaktera (`|` se koristi kao logički operator `or`). Pošto se traže velika slova `'A'`, `'B'` ili `'C'`, naše slovo `'a'` neće biti preklopljeno ovim `case`-om. Još možemo da vidimo korišćenje ključne reči `as`. Ona nam omogućava da ukoliko imamo više preklapanja, kao u ovom slučaju, vrednost koju smo preklopili smestimo u promenjivu `slovo`. Na taj način možemo da je koristimo dalje u kodu (do kraja `case` naredbe, do kada i važi).

Drugi `case` šablon pokušava da preklopi slovo `'a'`, što u našem slučaju i jeste pravo slovo. Međutim, nakon uslova (`if`), stoji uslov `False`. Ovo znači, da bez obzira na uspešno preklapanje, uslov onemogućava da se ovaj `case` izvrši.

Treći `case` izraz takođe sadrži slovo `'a'`, međutim ovaj put u `tuple` tipu podataka. To znači da će `case` biti preklopljen samo ukoliko to nije jedini podatak, već pored njega ima bar još jednog, iako je on u ovom slučaju zanemaren.

Konačno, imamo `default case` (`case _`) koji poklapa svaku vrednost koju prethodni izrazi nisu. Zato će u ovom slučaju biti ispisano `Default`.

```python
[prvi, drugi] = input("Unesite inicijale.")

match (prvi, drugi):
    case ('P', 'P'):
        print("Dobrodošli, Petar Petrović.")
    case { 'P': x, **ostali }:
        print(f"Rečnik, ključ P, vrednosti: {x}")
    case ('P', x):
        print(f'Dobrodošli, Petar {x}.')
    case _:
        print("Žao nam je, nema Vas na spisku.")
```

|Input> |`PD`|
|-------|:-------|
|Output>|`Dobrodošli, Petar D.`|

Na malo složenijem primeru iznad, možemo da vidimo još neke mogućnosti koje nam pattern matching u Python programskom jeziku nudi.

`case` može da bude bilo kog tipa, pa u prvom primeru imamo `tuple` tip. Ipak, inicijali koje smo uneli (`PD`) se ne slažu sa ovim `tuple` tipom, zato na izlazu nemamo `Dobrodošli, Petar Petrović.`.

Drugi `case` je nešto komplikovaniji. Radi se o `dict` tipu podataka, koji ima ključ `'P'`, a vrednosti smešta u promenjivu `x`. Sve vrednosti koje je nemoguće poklopiti, smeštaju se u promenjivu `ostali`. Naš `tuple` podatak, ovaj `case` svakako ne može da preklopi.

Treći `case` odgovara našem unosu. Prvi podatak u `tuple`-u je `'P'`, pa je očignedno da je pogođen. Druga vrednost u našem slučaju nije literal, već promenjiva, a to znači da će preklopiti bilo koji podatak. Zato će naš karakter `'D'` da bude smešten u promenjivu `x`. `print` naredba sada štampa (korišćenjem specijalnog string literala, `f`, koji omogućava da se unutar string-a koriste promenjive, u vitičastim zagradama), `Dobrodošli, Petar D.`, gde `D` potiče iz promenjive `x` koja je preklopila promenjivu `drugi` iz `match`-a.

Svi prosti tipovi i kolekcije mogu da se koriste u `case`. Korišćenje pattern matching-a može da uprosti kod, naročito kada se on sastoji od velikog broja uslova (`if`, `elif`, `else`).
Kada je reč o kolekcijama, pattern matching omogućava jednostavnije uslove, zato što omogućava da se slična kolekcija nađe u `case` izrazu.

Više informacija:

[Python Pattern Matching](https://www.python.org/dev/peps/pep-0636/)

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