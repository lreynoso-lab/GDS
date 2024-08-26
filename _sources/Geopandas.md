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

# Geopandas

```{code-cell} ipython3
from IPython.display import Image
Image(filename='imagenes/geopandas.png', width=300, height=100)
```



```{code-cell} ipython3
import ee
import numpy
import geopandas as gpd
```


```{code-cell} ipython3
ejidos = gpd.read_file(r"D:\jupyterBook\cdg\archivos\ejidos.shp")
```


```{code-cell} ipython3
ejidos.head(3)
```



```{code-cell} ipython3
# de igual forma, con la misma función gpd.read_file() se pueden leer archivos .zip, .gml 
```



```{code-cell} ipython3
ejidos = ejidos.set_index("EJL_NOMBRE")
ejidos.tail(3)
```



```{code-cell} ipython3
ejidos.head(3)
```







```{code-cell} ipython3
ejidos.tail(3)
```


```{code-cell} ipython3
ejidos["AREA"] = ejidos.area
```


```{code-cell} ipython3
ejidos.head(3)
```


```{code-cell} ipython3

ejidos["AREA"]
```



```{code-cell} ipython3
ejidos["PERIMETRO"] = ejidos.boundary
ejidos.head(2)
```








```{code-cell} ipython3
ejidos["CENTROIDE"] = ejidos.centroid
```


```{code-cell} ipython3
ejidos.head(3)
```







```{code-cell} ipython3
ejidos
```






```{code-cell} ipython3
# defino como punto inicial a Neuquén
punto_inicial = ejidos["CENTROIDE"].iloc[48]
punto_inicial
```





```{code-cell} ipython3
ejidos["DISTANCIA"] = ejidos["CENTROIDE"].distance(punto_inicial)
```


```{code-cell} ipython3
ejidos["DISTANCIA"] 
```






```{code-cell} ipython3
ejidos["DISTANCIA"].mean()
```



```{code-cell} ipython3
ejidos.plot()
```




```{code-cell} ipython3
ejidos.plot("AREA", legend = True)
```

```{code-cell} ipython3
ejidos = ejidos.set_geometry("CENTROIDE")
ejidos.plot()
```




```{code-cell} ipython3
ejidos.plot("AREA", legend = True)
```




```{code-cell} ipython3
ejidos.head(3)
```





```{code-cell} ipython3
ejidos = ejidos.set_geometry("geometry")
```


```{code-cell} ipython3
ejidos.plot()
```




```{code-cell} ipython3
ejidos["envolvente"] = ejidos.convex_hull
```


```{code-cell} ipython3
ejidos["envolvente"].plot(alpha = 0.5)
```



```{code-cell} ipython3
axis = ejidos["envolvente"].plot(alpha = 0.5)
ejidos["PERIMETRO"].plot(ax = axis)
```


```{code-cell} ipython3
ejidos["buffer"] = ejidos.buffer(10000)
```


```{code-cell} ipython3
ejidos["buffer_centroide"] = ejidos["CENTROIDE"].buffer(10000)
```


```{code-cell} ipython3
axis = ejidos["buffer"].plot(alpha = 0.5)
ejidos["buffer_centroide"].plot(ax = axis, color = "red", alpha = 0.5)
ejidos["PERIMETRO"].plot(ax = axis, color = "white", linewidth = 0.5)
```



```{code-cell} ipython3
neuquen = ejidos.loc["NEUQUEN", "geometry"]
neuquen
```


```{code-cell} ipython3
ejidos["buffer"].intersects(neuquen)
```


```{code-cell} ipython3
localidad29 = ejidos.query("ID_CATA18 == 29")
localidad29.plot()
```


```{code-cell} ipython3
ejidos.crs
```


```{code-cell} ipython3
ejidos = ejidos.set_geometry("geometry")

```


```{code-cell} ipython3
ejidos_4326 = ejidos.to_crs("EPSG:4326")
ejidos_4326.crs
```





```{code-cell} ipython3
ejidos_4326.plot()
```

