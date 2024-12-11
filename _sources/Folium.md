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

# Folium

```{code-cell} ipython3
from IPython.display import Image
Image(filename='imagenes/folium.png', width=300, height=100)
```

El objetivo principal de este capítulo es guiar paso a paso en el uso de Folium para crear mapas visualmente impactantes que sean también funcionales. Desde un simple mapa base de un proveedor internacional (CartoDB, OpenStreetMap) o un mapa base Argenmap hasta la inclusión de datos complejos como GeoJSON o mapas coropléticos con tooltips y popups, exploraremos cómo maximizar el potencial de esta biblioteca en proyectos.

Las secciones del capítulo cubren:

1. **Creación de mapas básicos:** Aprende a inicializar un mapa con Folium, definiendo coordenadas específicas y explorando los diferentes proveedores de teselas disponibles, como **CartoDB** y **Argenmap**.
2. **Mapas coropléticos:** Descubre cómo combinar datos estadísticos con geometrías geográficas para representar patrones espaciales, como la densidad de población o los ingresos por región, usando capas coropléticas.
3. **Integración de datos GeoJSON:** Aprende a cargar archivos GeoJSON desde URLs, convertir datos en un DataFrame, y enriquecer tus mapas con información detallada, destacando elementos individuales con **popups** y **tooltips**.

A medida que avances en la práctica guiada, exploraremos cómo mantener múltiples capas de información sin interferir entre ellas, logrando mapas informativos y visualmente atractivos. Folium no solo facilita la creación de mapas interactivos, sino que también abre un abanico de posibilidades para analizar y comunicar datos geoespaciales de manera efectiva.

Este capítulo es una invitación a conocer Folium, desde los conceptos básicos hasta las configuraciones más avanzadas, para que puedas crear mapas que no solo informen, sino que también sorprendan.

## Creando un mapa

A continuación mostramos un ejemplo básico de creación de un mapa:

```{code-cell} ipython3

import folium
m = folium.Map(location =(-38.95091337121393, -68.05931323468721))
m
```

Invocando a la variable m pedimos la representación del objeto


```{code-cell} python3
m
```


Tambien podemos guardar el mapa web en una página html


```python
# m.save("index.html")
```

# Proveedores de Teselas: CartoDB

Es posible mostrar el mapa web con un proveedor de teselas específico. En este caso **CartoBD Positron**


```{code-cell} python3
z = folium.Map(location =(-38.95091337121393, -68.05931323468721), tiles="cartodb positron", zoom_start = 15)
z
```

# Argenmap

El tiles por defecto está configurado con *OpenStreetMap* pero es posible pasar cualquier conjunto de mosaicos como plantilla de URL. En este caso utilizamos Argenmap. Tenga en cuenta que es necesario definir también la atribución: 


```{code-cell} python3
c = folium.Map(tiles='https://wms.ign.gob.ar/geoserver/gwc/service/tms/1.0.0/capabaseargenmap@EPSG%3A3857@png/{z}/{x}/{-y}.png', attr='IGN-Argenmap')

folium.Map(location =(-38.95091337121393, -68.05931323468721), 
           tiles="https://wms.ign.gob.ar/geoserver/gwc/service/tms/1.0.0/capabaseargenmap@EPSG%3A3857@png/{z}/{x}/{-y}.png", 
           attr='IGN-Argenmap', zoom_start = 14)

c
```


Es posible elegir otros tiles de Argenmap desde: [https://www.ign.gob.ar/NuestrasActividades/InformacionGeoespacial/ServiciosOGC](https://www.ign.gob.ar/NuestrasActividades/InformacionGeoespacial/ServiciosOGC)
    
La url (y atribucion) de otros proveedores de teselas está disponible en: 
    [https://leaflet-extras.github.io/leaflet-providers/preview/](https://leaflet-extras.github.io/leaflet-providers/preview/)

# Mapa Coroplético

```{code-cell} ipython3
import requests
import folium
from folium import GeoJson, Choropleth
import pandas as pd
```

## Leer el archivo GeoJSON desde la URL

```{code-cell} ipython3
url = "https://cdn.buenosaires.gob.ar/datosabiertos/datasets/ministerio-de-educacion/comunas/comunas.geojson"

## Desactiva el uso de proxy
proxies = {
    "http": None,
    "https": None,
}

response = requests.get(url, proxies=proxies)
geojson_data = response.json()
```

## Crear un mapa base

```{code-cell} python3
m2 = folium.Map(location=[-34.61, -58.38], zoom_start=12)
```

## Añadir el archivo GeoJSON al mapa

```{code-cell} python3
GeoJson(geojson_data).add_to(m2)
```

## Convertir los datos de características en un DataFrame

```{code-cell} python3
features = geojson_data['features']
data_list = [(feature['properties']['id'], feature['properties']['area']) for feature in features]
data = pd.DataFrame(data_list, columns=['ID', 'Área'])
```

## Crear el mapa coroplético utilizando el DataFrame

```{code-cell} python3
Choropleth(
    geo_data=geojson_data,
    data=data,
    columns=['ID', 'Área'],  # Nombres de las columnas
    key_on='feature.properties.id',  # Ajustar según el GeoJSON
    fill_color='BuPu',  # Paleta de colores
    fill_opacity=1,
    line_opacity=1,
    legend_name='Área'
).add_to(m2)

m2
```

# Mapa Coroplético con Tooltip

```{code-cell} ipython3
import requests
import folium
from folium import GeoJson, GeoJsonPopup, GeoJsonTooltip, Choropleth
import pandas as pd
```

```{code-cell} python3
m2 = folium.Map(location=[-34.61, -58.38], zoom_start=12)
```

## Convertir los datos de características en un DataFrame

```{code-cell} python3
features = geojson_data['features']
data_list = [(feature['properties']['id'], feature['properties']['area']) for feature in features]
data = pd.DataFrame(data_list, columns=['ID', 'Área'])
```


## Crear el mapa coroplético utilizando el DataFrame

```{code-cell} python3
choropleth = Choropleth(
    geo_data=geojson_data,
    data=data,
    columns=['ID', 'Área'],  # Nombres de las columnas
    key_on='feature.properties.id',  # Ajustar según el GeoJSON
    fill_color='BuPu',  # Paleta de colores
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name='Área'
)
```

## Añadir el mapa coroplético al mapa base

```{code-cell} python3

choropleth.add_to(m2)
```

## Añadir el archivo GeoJSON al mapa con popups y tooltips, sin interferir con el coroplético

```{code-cell} python3
GeoJson(
    geojson_data,
    style_function=lambda feature: {
        'fillColor': '#00000000',  # Color transparente
        'color': '#000000',
        'weight': 0.5,
    },
    tooltip=GeoJsonTooltip(
        fields=['id', 'area'],
        aliases=['ID', 'Área'],
        localize=True
    ),
    popup=GeoJsonPopup(
        fields=['id', 'area'],
        aliases=['ID', 'Área'],
        localize=True
    )
).add_to(m2)


m2
```
