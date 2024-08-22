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

# GEE

## Introducción


```{code-cell} ipython3
import ee
import geemap

```
## Inicializar

```{code-cell} ipython3
ee.Initialize(project='ee-luisreynoso')
```

```{code-cell} ipython3
SPC = ee.FeatureCollection("projects/ee-luisreynoso/assets/SPCH")
Map = geemap.Map()
Map
```

