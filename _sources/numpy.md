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

# Numpy

Aprender NumPy es fundamental antes de abordar técnicas de inteligencia artificial (IA) porque proporciona una base sólida para trabajar con datos y realizar operaciones matemáticas esenciales. Ejemplo para operar con matrices y vectores, recordemos que una imagen satelital se puede modelar como una matriz.

```{code-cell} ipython3
from IPython.display import Image
Image(filename='imagenes/numpy.png', width=400, height=150)
```
> ¿Porqué es útil aprender Numpy?

Las imágenes satelitales se suelen aplanar (convertir de una matriz multidimensional a un vector unidimensional) en ciertas aplicaciones debido a razones prácticas y computacionales. Aquí están las principales razones:

1. **Facilitar el análisis y procesamiento**
  
   + Estadísticas globales: Convertir la matriz en un vector permite calcular fácilmente estadísticas globales como el promedio, la mediana o la desviación estándar de todos los píxeles.
   + Comparaciones: Aplanar facilita comparar imágenes o regiones usando técnicas como la distancia euclidiana entre vectores.

2. **Entrenamiento de modelos de aprendizaje automático**

   + Los algoritmos clásicos de aprendizaje automático, como máquinas de soporte vectorial (SVM) o regresión logística, requieren que los datos de entrada estén en formato vectorial.
   + Aplanar la imagen permite representar cada píxel (o conjunto de características derivadas de los píxeles) como un único valor en un vector, haciendo que cada imagen sea una fila de un dataset tabular.

Ejemplo: Una imagen de 100x100 píxeles con 3 bandas (RGB) se convierte en un vector de tamaño 100 × 100 × 3 = 30000. Este vector puede usarse como entrada para un modelo.

3. **Reducción de dimensionalidad**
 
  + Las imágenes satelitales suelen ser grandes (pueden tener miles o millones de píxeles). Aplanarlas es un paso previo para aplicar técnicas de reducción de dimensionalidad como PCA (Análisis de Componentes Principales), que simplifican los datos manteniendo la mayor parte de la información.
  + Esto es especialmente útil en aplicaciones donde es necesario procesar grandes cantidades de datos satelitales con rapidez y eficiencia.

4. **Compresión y almacenamiento**
  
   + Al aplanar la imagen, es más sencillo aplicar métodos de compresión (como JPEG o PNG) o serializarla para su almacenamiento o transmisión.  

5. **Visualización de datos**
  
   + En estudios exploratorios, convertir una imagen en un vector permite crear gráficos como histogramas o analizar la distribución de valores de los píxeles.
   Ejemplo práctico: Supongamos que queremos clasificar terrenos (agua, vegetación, urbano) basándonos en imágenes satelitales. Un flujo típico sería: 

    1. Imagen original: Una matriz de tamaño 1000×1000 (1.000.000 píxeles).
    2. Aplanamiento: La matriz se convierte en un vector de tamaño 1,000,000.
    3. Modelado: El vector se utiliza como entrada en un clasificador o red neuronal.

Aunque el aplanamiento es útil, no siempre es necesario ni recomendable. Por ejemplo, redes neuronales convolucionales (CNN) trabajan directamente con datos en formato matricial porque aprovechan la estructura espacial de la imagen. Sin embargo, en análisis más simples, aplanar sigue siendo una práctica común y efectiva.

## Creación de Arreglos y Operaciones Simples

Crear un arreglo de Numpy que contenga los números del 1 al 10:

```{code-cell} ipython3
import numpy

# Creación del arreglo
arr = np.arange(1, 11)
```

Multiplicar todos los elementos del arreglo por 2, luego sumar los elementos del arreglo y finalmente crear un nuevo arreglo con el cuadrado de cada elemento:

```{code-cell} ipython3
# Operaciones
arr_mult = arr * 2
suma = np.sum(arr)
arr_cuadrado = arr ** 2
```

## Indexación y Slicing

Dado el siguiente arreglo:

```{code-cell} ipython3
arr = np.array([5, 10, 15, 20, 25, 30])
```

Selecciona los elementos en las posiciones pares.

```{code-cell} ipython3
pares = arr[::2]
```

Selecciona los elementos mayores a 15.

```{code-cell} ipython3
mayores = arr[arr > 15]
```

Invierte el orden del arreglo.

```{code-cell} ipython3
invertido = arr[::-1]
```

## Operaciones con matrices

Crea una matriz de 3x3 con números aleatorios entre 0 y 1 usando np.random.rand. 
```{code-cell} ipython3
matriz = np.random.rand(3, 3)

```

Luego: Encontrar el valor máximo de la matriz.

```{code-cell} ipython3
max_val = np.max(matriz)

```

Encontrar la suma de los valores de cada fila.

```{code-cell} ipython3
suma_filas = np.sum(matriz, axis=1)

```

Multiplica cada elemento por 10.
```{code-cell} ipython3
matriz_mult = matriz * 10
```

## Transformar una imágen satelital
 

+ **Objetivo:** Utilizar NumPy para procesar una imagen satelital.

+ **Contexto:** Supongamos que tienes una imagen satelital representada como una matriz de 3 dimensiones (alto, ancho, y bandas de color). Por ejemplo, una imagen de 100x100 píxeles con 3 bandas RGB puede representarse como un arreglo de forma (100, 100, 3).

1. Cargar la matriz de la imagen satelital (se usa np.random.randint para simular una matriz con valores entre 0 y 255).

```{code-cell} ipython3
# Crear una matriz simulada de 100x100x3 (valores de 0 a 255)
imagen = np.random.randint(0, 256, size=(100, 100, 3), dtype=np.uint8)
```

2. Normalizar los valores para que estén en el rango [0, 1].

```{code-cell} ipython3
# Normalización
imagen_normalizada = imagen / 255.0
```

3. Convertir la matriz en un vector unidimensional.

```{code-cell} ipython3
# Transformación en vector
# La función .ravel() aplana la matriz en un vector unidimensional.
vector = imagen_normalizada.ravel()
```

4. Calcular el promedio de cada banda (R, G, B).

```{code-cell} ipython3
# Promedio por banda
promedios = np.mean(imagen_normalizada, axis=(0, 1))

print(f"Tamaño del vector: {vector.shape}")
print(f"Promedios de las bandas: {promedios}")
```
