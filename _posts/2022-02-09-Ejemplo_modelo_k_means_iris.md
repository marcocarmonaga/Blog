---
layout: post
title:  "Clasificando plantas por el largo y ancho de sus pétalos: k-nearest neighbors"
date:   2022-02-09
author: Marco Antonio Carmona Galván
---

En un [artículo anterior](http://unrealblog.xyz/Tu_primer_modelo_de_regresion_lineal/), describimos el proceso de un modelo de regresión lineal en el que corroboramos la correlación existente entre el largo y ancho de los pétalos de una planta de nombre "iris". 

En este breve post, describiremos cómo utilizar un proceso de clasificación para poder separar nuestros datos de acuerdo a estas mismas propiedades, buscando corroborar la existencia de 3 tipos: setosa, versicolor y virginica. 

Para ello comenzaremos importando nuestros módulos a utilizar y nuestra base de datos, misma incluida dentro de nuestro módulo principal: [sklearn](https://scikit-learn.org/). 

## Importando módulos a utilizar


```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.datasets import load_iris
from sklearn.preprocessing import MinMaxScaler
from sklearn import metrics
```

Una vez importados nuestros módulos, procedemos a importar nuestra base de datos directamente de `datasets.load_iris()`; lo cual lo podemos hacer de la siguiente manera:

## Importando nuestro conjunto de datos


```python
import pandas as pd

# Importanto base de datos de plantas iris
iris = load_iris()

# Transformando base de datos a un dataframe de librería pandas
df = pd.DataFrame(iris.data, columns=iris.feature_names)
df['species'] = iris.target

# Imprimiendo primeras 5 entradas
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



Una vez cargada y procesada, en un dataframe, nuestra base de datos. Podemos comenzar a eliminar los datos innecesarios o simplemente en aquellos en los que nos vamos a enfocar. 

Es por eso que en las siguientes líneas eliminaremos las primeras dos columnas; ya que únicamente nos vamos a enfocar en el entrenamiento del modelo considerando el largo y ancho de los pétalos. 

## Limpiando datos innecesarios para nuestro análisis


```python
import numpy as np

df.drop(['sepal length (cm)', 'sepal width (cm)'], axis=1, inplace=True)
```

A partir de aquí es donde comienza una de las partes más interesantes de este modelo de clasificación; ya que este método, el cuál ocupa un algoritmo de nombre $$k$$-nearest neighbors nos permitirá agrupar $$k$$ elementos de acuerdo a las propiedades y similitudes que obtenga.   

En términos más generales, los datos de entrenamiento son vectores en un espacio de características multidimensional (features), cada uno con una etiqueta de clase (target). La fase de entrenamiento del algoritmo consiste únicamente en almacenar los vectores de características y las etiquetas de clase de las muestras de entrenamiento.   

En la fase de clasificación, $$k$$ es una constante definida por el usuario, y un vector no etiquetado (un punto de consulta o de prueba) se clasifica asignando la etiqueta más frecuente entre las $$k$$ muestras de entrenamiento más cercanas a ese punto de consulta.   

Dicho lo anterior, podemos comenzar a separar nuestros datos en features y target, lo cual en Python se puede realizar de la siguiente forma: 

## Separando nuestro conjunto de datos entre features y target


```python
X_data = df.iloc[:,0:2].values # features
y_data = df['species'].values # target
```

Ahora, antes de comenzar a definir el modelo, es importante considerar una métrica de asertividad, la cuál va a estar dada por el porcentaje de datos correctos entre la predicción y nuestro target; con esto podremos medir qué tan efectivo es nuestro modelo dependiendo del $$k$$ que escojamos. 

Para ello bastará con sumar todos los datos que son iguales entre nuestra predicción (`y_pred`) y nuestra data original (`y_data`); y dividir esta suma entre el número total de datos, o en otras palabras sobre la longitud de `y_data`. 

## Definiendo función de acertividad


```python
def accuracy(real, predict):
    return sum(y_data == y_pred) / float(real.shape[0])
```

Una vez definida nuestra función para medir la asertividad de nuestro modelo, procederemos a entrenarlo para diferentes valores de $$k$$ y así poder considerar la asertividad dependiendo de este dato. 

## Haciendo pruebas para diferentes $$k$$


```python
scores = list()

for k in range(1,21):
    knn = KNeighborsClassifier(n_neighbors= k)
    knn = knn.fit(X_data, y_data)
    y_pred = knn.predict(X_data)

    score = metrics.rand_score(y_data,y_pred)
    scores.append((k,score))
```


```python
scores_df = pd.DataFrame(scores, columns=['k','accuracy'])
scores_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>k</th>
      <th>accuracy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.982461</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.965638</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>0.973960</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>0.965638</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>0.957494</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>0.957494</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0.957494</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>0.957494</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>15</th>
      <td>16</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>18</th>
      <td>19</td>
      <td>0.949530</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>0.949530</td>
    </tr>
  </tbody>
</table>
</div>



Una vez obtenido el dataframe anterior, podemos pasarlo a la gráfica con el propósito de poder observar de manera gráfica estos datos.

## Graficando asertividad 


```python
import matplotlib.pyplot as plt

%matplotlib inline
fi, ax = plt.subplots()
ax.plot(scores_df.k,scores_df.accuracy)
ax.set_xticks(range(1,21))
```
    
![png](/assets/images/posts/output_21_1.png)
    


> Pregunta para el lector 
> 
> ¿Por qué no se considera el $$k=0$$? 
  
A partir de aquí podemos ver que tenemos una mejor asertividad cuando $$k=3$$, exactamente los 3 conjuntos de datos que propusimos al inicio de este post. 

Y es de esta manera como finalmente podemos hacer un gráfico de dispersión, considerando nuestra clasificación para cuando $$k=3$$. 

## Graficando modelo


```python
knn = KNeighborsClassifier(n_neighbors= 3)
knn = knn.fit(X_data, y_data)
y_pred = knn.predict(X_data)
```


```python
%matplotlib inline
plt.scatter(X_data[:,0],X_data[:,1], c=y_pred)
plt.title('Clasificación de acuerdo a largo y ancho de los pétalos')
plt.xlabel('Largo de pétalos (cm)')
plt.ylabel('Ancho de pétalos (cm)')
```




    Text(0, 0.5, 'Ancho de pétalos (cm)')




    
![png](/assets/images/posts/output_25_1.png)

---
    
El notebook de este post, los puedes encontrar [aquí](https://colab.research.google.com/drive/1ZMQyYEgWZIrcesuNaH9li5fOInWcryPE?usp=sharing).