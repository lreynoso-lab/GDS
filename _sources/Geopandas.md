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

1. Introducción al Uso de GeoPandas

El análisis de datos geoespaciales es una disciplina esencial en diversos campos, desde la planificación urbana hasta la gestión ambiental. **GeoPandas** una biblioteca de Python basada en *pandas* y *shapely*, se destaca como una herramienta poderosa y accesible para trabajar con datos vectoriales en entornos de código abierto. Su versatilidad permite realizar desde tareas básicas de lectura y escritura hasta análisis complejos de geometrías espaciales.

GeoPandas es compatible con una amplia variedad de formatos geoespaciales, incluidos **Shapefile**, **GeoJSON**, **GML** y archivos comprimidos como **.zip**, lo que la convierte en una opción ideal para integrar datos provenientes de diversas fuentes. En este capítulo, se explorará su potencial mediante ejemplos prácticos que permitirán comprender su uso y aplicarlo en tus propios proyectos.

Las secciones del capítulo incluyen:

+ **Lectura y manipulación de datos geoespaciales:** Donde es posible aprender a cargar un archivo Shapefile utilizando gpd.read_file(), una función clave que también admite formatos como .zip y .gml. Además, se ejemplifica cómo establecer índices y realizar inspecciones iniciales de los datos con métodos como .head() y .tail().

+ **Cálculo de propiedades geométricas:** Describe cómo calcular áreas, perímetros y centroides para cada geometría en tu conjunto de datos. Estas métricas son fundamentales para el análisis espacial y la toma de decisiones basada en datos.

+ **Análisis de distancias y relaciones espaciales:** Utiliza herramientas como .distance() y .intersects() para evaluar la proximidad entre objetos geográficos y determinar relaciones espaciales clave, como superposiciones y buffers.

+ **Manipulación de geometrías y reproyección:** Detalla como crear envolventes convexas, buffers alrededor de geometrías y a cambiar el sistema de referencia espacial (CRS) con métodos como .to_crs(). La flexibilidad en la gestión de sistemas de coordenadas es crucial para trabajar con datos provenientes de diferentes fuentes.

+ **Visualización:** GeoPandas incluye capacidades nativas para graficar geometrías y atributos, lo que permite crear mapas rápidos y efectivos con métodos como .plot() y especificar características visuales como colores, leyendas y transparencias.

GeoPandas no solo simplifica tareas complejas, sino que también habilita a los usuarios para integrar análisis geoespaciales directamente en sus flujos de trabajo con Python. A lo largo de este capítulo, se ejemplifica a combinar potentes capacidades de análisis con un enfoque práctico, utilizando datos reales como los ejidos de gobiernos locales.


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

