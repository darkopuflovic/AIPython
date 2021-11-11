# Regularni izrazi (Regular Expressions)

Regularni izrazi se sastoje od niza karaktera, koji se tretira na poseban način, da bi se omogućila pretraga string-ova.

Python sadrži modul `re` koji podržava rad sa regularnim izrazima.

```python
import re

print('\\d+')
print(r'\d+')
print(r'Regularni izraz')
```

|Output>|`\d+`|
|-------|:-------|
|       |`\d+`|
|       |`Regularni izraz`|

Regularni izrazi mogu da koriste i specijalni tip string literala u Python-u. Ovaj literal počinje slovom `r`. Moguće je koristiti i regularni string literal, ali u tom slučaju je neophodno brinuti o escape karakterima i specijalnim karakterima.

Mogul `re` sadrži veliki broj funkcija, ali mi ćemo obraditi samo neke:

- `match` funkcija

```python
import re

re.match(r"(\d+)", "123abcdef1234")
```

|Output>|`<re.Match object; span=(0, 3), match='123'>`|
|-------|:-------|

Funkcija `match` pretražuje string samo od početka. Ukoliko se karakteri na početku ne poklapaju sa šablonom kojim pretražujemo, neće biti poklapanja. Ukoliko poklapanje na početku string-a postoji, kao u ovom primeru, vraća se samo prvo poklapanje.

Zagrade u izrazu za pretraživanje znače grupisanje. Ukoliko postoji poklapanje, svaka grupa u zagradama će se tretirati kao grupa koju je moguće pretražiti u `Match` tipu podataka koji ova funkcija vraća.

- `search` funkcija

```python
import re

print(re.search(r"(\d+)", "123abcdef1234"))
print(re.search(r"(\d+)", "123abcdef1234").group())
print(re.search(r"(\d+)", "123abcdef1234").group(1))
print(re.search(r"(\d+)", "123abcdef1234").group(2))
```

|Output>|`<re.Match object; span=(0, 3), match='123'>`|
|-------|:-------|
|       |`123`|
|       |`123`|
|       |`IndexError: no such group`|

Funkcija `search` za razliku od funkcije `match` pretražuje celokupan string. Sličnost je to što je povratna vrednost samo jedno poklapanje.

Pored toga, u ovom primeru možemo da vidimo kako se koriste grupe. Funkcija `group` nam vraća vrednost prve grupe koja je pronađena, dok argumentom (broj grupe), možemo da preuzmemo i više grupi ukoliko ih ima.

- `findall` funkcija

```python
import re

print(re.findall(r"\d+", "123abcdef1234"))
print(re.findall(r"(1(2(\d+)))", "123abcdef1234"))
```

|Output>|`['123', '1234']`|
|-------|:-------|
|       |`[('123', '23', '3'), ('1234', '234', '34')]`|

Funkcija `findall` vraća sva poklapanja, kao listu. Ukoliko želimo, možemo u šablon da ubacimo i grupe, pa će onda rezultat biti lista `tuple` podataka, gde su vrednosti grupe koje su poklopljene.

- `compile` funkcija

```python
import re

cre = re.compile(r"(\d+)")
cre.findall("1234ABCD1234")
```

|Output>|`['1234', '1234']`|
|-------|:-------|

Kada često koristimo šablon za poklapanje stringova, Python nam pruža mogućnost da ga kompilujemo, da bi se brže izvršavao. To se radi korišćenjem `compile` funkcije u modulu `re`.

- `escape` funkcija

Funkcija `escape` prihvata string, a kao rezultat vraća string kome su svi specijalni karakteri zamenjeni escape verzijama istog karaktera ('.' -> '\.'...).

```python
import re

re.escape("Ovo je neki string.")
```

|Output>|`'Ovo\\ je\\ neki\\ string\\.'`|
|-------|:-------|

Korisno je poznavati i neke konstante koje je moguće pronaći u modulu `re`.

- `re.A` ili `re.ASCII`
  - Ovo je konstanta koja specijalne vrednosti (\w, \W, \b, \B, \d, \D, \s, \S), pretražuje po `ASCII` vrednostima, ukoliko sadrže `Unicode` karaktere, oni neće biti prihvaćeni.
- `re.I` ili `re.IGNORECASE`
  - Pretraga se vrši bez obzira na veličinu teksta
- `re.M` ili `re.MULTILINE`
  - Karakter '^' pretražuje početak teksta, ali i svake nove linije. Isto važi i za karakter '$', koji pretražuje kraj linije i teksta
- `re.S` ili `re.DOTALL`
  - Omogućava karakteru '.' da poklopi svaki karakter, pa i novu liniju. Bez ove konstante, nova linija se ne posmatra kao karakter

```python
re.findall("(.+)", "123ABCD1234\nNOVA LINIJA", re.IGNORECASE | re.DOTALL)
```

|Output>|`['123ABCD1234\nNOVA LINIJA']`|
|-------|:-------|

Više o regularnim izrazima možete da saznate na sledećim linkovima:

[Regexr](https://regexr.com/)

[Regex101](https://regex101.com/)

[PythonRE](https://docs.python.org/3/library/re.html)

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