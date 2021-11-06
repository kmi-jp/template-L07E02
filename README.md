# L07E02: Data 3
Balíček `data` z minulého semináře rozšiřte o následující funkcionalitu. Ve zdrojovém kódu urdžujte pořádek a metody seskupujte dle významu.

Kdekoli je ve stávájícím kódu možné `for` cyklus přepsat pomoci comprehension, to udělejte.

## Třída `Index`
* `Index.__iter__(self)` - metoda pro podporu iterování, musí vracet generátor

```python
from data.index import Index


users = Index(["user 1", "user 2", "user 3", "user 4"], name="names")

assert [label for label in users] == ["user 1", "user 2", "user 3", "user 4"]
```

## Třída `Series`
* `Series.__iter__(self)` - metoda pro podporu iterování, musí vracet generátor
* `Series.items(self)` - metoda pro podporu iterování přes dvojice `(key, value)`, musí vracet iterator (`zip`). Funguje totožně jako `.items()` u slovníku (priklad u `DataFrame` níže, pro `Series` funguje obdobně)
* `Series.__getitem__(self, key)` - podpora operátoru `[]`, funkcionalita jako u slovníku, pokud `key` neexistuje, vyvolá `KeyError`. Upravte metodu `Series.get(self, key)`, aby používala nově implementovaný operátor `[]`

```python
from data.series import Series
from data.index import Index


users = Index(["user 1", "user 2", "user 3", "user 4"], name="names")

salaries = Series([20000, 300000, 20000, 50000], index=users)

assert [value for value in salaries] == [20000, 300000, 20000, 50000]
assert salaries["user 2"] == 300000
```

## Třída `DataFrame`
* `DataFrame.__iter__(self)` - metoda pro podporu iterování, musí vracet generátor. Generátor iteruje přes klíče (stejně jako v případě slovníku)
* `DataFrame.items(self)` - metoda pro podporu iterování přes dvojice `(key, value)`, musí vracet iterator (`zip`). Funguje totožně jako `.items()` u slovníku
* `DataFrame.index` - vlastnost, která vrací index první instance `Series` ve `DataFrame.values` (tedy index odpovídající řádkům v `DataFrame`)

```python
from data.series import Series
from data.index import Index
from data.dataframe import DataFrame


users = Index(["user 1", "user 2", "user 3", "user 4"], name="names")

salaries = Series([20000, 300000, 20000, 50000], index=users)
names = Series(["Lukas Novak", "Petr Pavel", "Pavel Petr", "Ludek Skocil"], index=users)
cash_flow = Series([-100, 10000, -2000, 1100], index=users)

data = DataFrame([names, salaries, cash_flow], columns=Index(["names", "salary", "cash flow"]))

assert [key for key in data] == ["names", "salary", "cash flow"]

assert [pair for pair in data.items()] == list(zip(["names", "salary", "cash flow"], [names, salaries, cash_flow]))

assert data.index == data.values[0].index
```