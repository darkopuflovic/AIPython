# Biblioteke

Python je jezik poznat po velikom broju biblioteka. U njima je moguće pronaći različite vrste funkcionalnosti. Uključivanje biblioteka i modula u programskom jeziku Python se vrši `import` naredbama.

```python
import math
```

Ova import direktiva uključuje celokupan modul `math`. Ukoliko želimo da koristimo neku od funkcija iz ovog modula, to možemo da uradimo na sledeći način:

```python
import math

math.pow(10, 2)
```

|Output>|`100.0`|
|-------|:-------:|

Osim što možemo da uključimo celokupan modul/biblioteku, moguće je uključiti samo jednu njegovu funkciju:

```python
from math import pow

pow(10, 2)
```

|Output>|`100.0`|
|-------|:-------:|

Na ovaj način, nemoguće je pristupiti ostalim funkcijama iz ovog mogula, ali se funkciji `pow` može pristupiti direktno, bez naziva modula. Moguće je uključiti i više funkcija ili konstanti:

```python
from math import pow, pi

pow(pi, 2)
```

|Output>|`9.869604401089358`|
|-------|:-------:|

Ukoliko imamo modul ili biblioteku, koje želimo da uključimo na ovaj način, ali su nam potrebne sve njihove funkcije i konstante, koristimo sledeći oblik:

```python
from math import *

pow(pi, 2)
```

|Output>|`9.869604401089358`|
|-------|:-------:|

Na ovaj način, sve što se nalazi u modulu `math` će moći da se koristi direktno, bez korišćenja naziva modula.

Takođe, može da nam bude korisno da uključimo funkciju, ali želimo da joj promenimo naziv:

```python
from math import pow as stepen

stepen(10, 2)
```

|Output>|`100.0`|
|-------|:-------:|

Pored svih pobrojanih načina, postoje i relativne import naredbe, (`from . import funkcija`), međutim njih nećemo obrađivati na ovom kursu. One mogu da budu korisne kada imamo projekat koji u sebi sadrži fajlove koje želimo da uključimo.

Biblioteke koje se nalaze na `Python Package Index`-u je moguće uključiti u projekat korišćenjem `Package Installer for Python`-a (`pip` komanda). Ova komanda se koristi iz terminala po izboru (npr. Command Prompt).

`pip install tail-recursive` - Biblioteka koja omogućava repnu rekurziju.

Nakon što smo preuzeli biblioteku sa `Python Package Index`-a, možemo da je koristimo iz koda na sličan način kao i module:

```python
from tail_recursive import tail_recursive
# ...
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
|[Python funkcije (min, max, map, filter, zip, moduli)](Functions.md)|
|[Biblioteke](Library.md)|
|[Comprehensions](Comprehensions.md)|
|[Regularni izrazi](RegularExpressions.md)|
|[Pattern matching](PatternMatching.md)|