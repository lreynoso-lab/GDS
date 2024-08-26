---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Pandas

```{code-cell} ipython3
from IPython.display import Image
Image(filename='imagenes/pandas.png', width=300, height=100)
```


## Introducción

Una de las librerías mas populares para trabajar con datos es:
[pandas](http://pandas.pydata.org/).

```{code-cell} ipython3
import pandas as pd
import numpy as np     
```
Pandas es rápido, eficiente, flexible y bien diseñado.

Aquí hay un ejemplo simple, usando algunos datos ficticios generados con Numpy
Excelente funcionalidad "aleatoria".

```{code-cell} ipython3

np.random.seed(1234)

data = np.random.randn(5, 2)  # 5x2 matrix of N(0, 1) random draws
dates = pd.date_range('28/12/2010', periods=5)

df = pd.DataFrame(data, columns=('price', 'weight'), index=dates)
print(df)
```

```{code-cell} ipython3
df.mean()
```

