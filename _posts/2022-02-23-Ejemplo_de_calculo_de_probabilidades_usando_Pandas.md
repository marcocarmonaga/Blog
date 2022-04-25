---
layout: post
title:  "Cálculo de probabilidades utilizando Pandas."
date:   2022-02-23
author: Marco A. Carmona
---

En un post anterior llegué a hacer una rápida introducción al término de variable aleatoria con Python, es por esto que en esta ocasión te explicaré una manera similar de calcular probabilidades, pero con ayuda de una librería muy popular como lo es [**Pandas**](https://pandas.pydata.org/).  

De manera muy breve me gustaría decirte una analogía para lo que es que entiendas lo que es **Pandas**: 

> **Pandas** es como el Excel de Python 

La manera más famosa de trabajar con esta librería es haciendo utilidad de DataFrames, que, en términos generales y básicos, es una tabla de datos, ojo aquí, pues algo increíble es que se pueden poner todo tipo de datos que Python permita. 

Si aún no tienes instalada esta librería en tu sistema, no te preocupes, es tan sencillo como escribir la siguiente línea, según sea tu caso de ambiente con el que instales librerías: 

- Si utilizas `pip`: 

```bash 
pip install pandas 
``` 

- Si utilizas `conda`: 

```bash 
conda install pandas 
``` 

Una vez tengas esta librería instalada en tu sistema, la puedes importar de la siguiente manera a tu proyecto, o en este caso, a este ejemplo para calcular probabilidades. 


```python
import pandas as pd
```

Hecho lo anterior, vamos a plantear la manera con la que vamos a ejemplificar el cálculo de probabilidades, y es que para esto vamos a poner el caso donde existen dos dados de 6 caras cada uno; sin embargo, uno de ellos está "truqueado", dando lugar a que cada cara de este segundo dado tenga la siguiente probabilidad: 

$$ 
\mathbb{P}(1)+\mathbb{P}(2)+\mathbb{P}(3) = \frac{1}{9} 
$$ 

$$ 
\mathbb{P}(4)+\mathbb{P}(5)+\mathbb{P}(6) = \frac{2}{9} 
$$ 

Nota que nuestro primer dado no tiene truco alguno, por lo que cada cara tiene una probabilidad de $$\frac{1}{6}$$. 

Con esto dicho, lo que nosotros buscaremos, con ayuda de Python, es la probabilidad de obtener cada una de las sumas entre el primer lanzamiento y el segundo lanzamiento. Por ejemplo, conocer cuál es la probabilidad de obtener como suma el valor 7, el cual puede ser combinación de los valores (1,6), (2,5), ...

Para ello comenzaremos creando nuestra tabla que tendrá como columnas: 

- `sm`: suma 
- `d1`: valor del dado 1. 
- `d2`: valor del dado 2. 
- `pd1`: probabilidades de obtener el valor del dado 1. 
- `pd2`: probabilidades de obtener el valor del dado 2. 
- `p`: probabilidades de obtener el valor del dado 1 y el valor del dado 2. 

Y como índice de cada fila, los valores de todas las posibles combinaciones de los valores del 1 al 7 en grupos de dos: (1,1), (1,2), ..., (6,5), (6,6). 

Como te mencioné al inicio, una de las herramientas más utilizadas de Pandas es el DataFrame, aquí te muestro cómo utilizarla para crear nuestra tabla con las columnas. 


```python
df = pd.DataFrame(index=[(i,j) for i in range(1,7) for j in range(1,7)], columns=['sm','d1','d2','pd1','pd2', 'p'])
```

A continuación, te muestro las primeras 10 filas del DataFrame que acabamos de crear, en caso de que te gustaría ver toda la tabla, únicamente elimina `.head(10)` de la siguiente línea. 


```python
df.head(10)
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
      <th>sm</th>
      <th>d1</th>
      <th>d2</th>
      <th>pd1</th>
      <th>pd2</th>
      <th>p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>(1, 1)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 2)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 3)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 4)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 5)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 6)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 1)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 2)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 3)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 4)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Verás que tenemos como entradas valores `NaN` (Not A Number), así que comencemos a llenar nuestra tabla. Y para esto comenzaremos ingresando las columnas `d1` y `d2`. Las cuales no son más que la extracción de los valores de su respectivo índice, dado por `(i[0],i[1])`. 

Esto se puede hacer de manera muy simple haciendo uso de [list comprehensions](https://docs.python.org/3/tutorial/datastructures.html). 


```python
df.d1 = [i[0] for i in df.index]
df.d2 = [i[1] for i in df.index]
```

Observa ahora cómo luce nuestra tabla. ¡Ya tenemos dos columnas llenas!


```python
df.head(10)
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
      <th>sm</th>
      <th>d1</th>
      <th>d2</th>
      <th>pd1</th>
      <th>pd2</th>
      <th>p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>(1, 1)</th>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 2)</th>
      <td>NaN</td>
      <td>1</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 3)</th>
      <td>NaN</td>
      <td>1</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 4)</th>
      <td>NaN</td>
      <td>1</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 5)</th>
      <td>NaN</td>
      <td>1</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 6)</th>
      <td>NaN</td>
      <td>1</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 1)</th>
      <td>NaN</td>
      <td>2</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 2)</th>
      <td>NaN</td>
      <td>2</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 3)</th>
      <td>NaN</td>
      <td>2</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 4)</th>
      <td>NaN</td>
      <td>2</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Ahora continuemos con nuestra columna `sm`, la cual, como sabemos, es la suma de la columna `d1` más `d2`. 

Seguramente pensarás que realizar esto puede ser complicado, sin embargo, pandas nos la pone fácil, ya que esto es tan sencillo como escribir la siguiente línea, en la que le indicamos que sume las columnas antes dichas y se la asigne a la columna `sm`.


```python
df.sm = df.d1 + df.d2
```

Y veamos cómo va nuestro progreso...


```python
df.head(10)
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
      <th>sm</th>
      <th>d1</th>
      <th>d2</th>
      <th>pd1</th>
      <th>pd2</th>
      <th>p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>(1, 1)</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 2)</th>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 3)</th>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 4)</th>
      <td>5</td>
      <td>1</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 5)</th>
      <td>6</td>
      <td>1</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 6)</th>
      <td>7</td>
      <td>1</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 1)</th>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 2)</th>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 3)</th>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 4)</th>
      <td>6</td>
      <td>2</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Con esto prácticamente tenemos casi completa nuestra tabla, pero nos falta de lo más importante, asignar los valores de probabilidad dependiendo del dado y del valor de la cara.


```python
# Probabilidad de cada una de las caras del dado 1
df.pd2 = 1/6

# Probabilidad de cada una de las caras del dado 2, según su valor.
df.loc[df.d2<=3,'pd1'] = 1/9
df.loc[df.d2>3,'pd1'] = 2/9
```

La siguiente línea, lo que hace es tomar 10 muestras aleatorias de toda la tabla. Esto lo hago con el afán de que veas valores distintos, ya que si utilizamos `df.head()` verás valores idénticos, con lo que no lograrás ver la diferencia. 


```python
df.sample(10)
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
      <th>sm</th>
      <th>d1</th>
      <th>d2</th>
      <th>pd1</th>
      <th>pd2</th>
      <th>p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>(2, 3)</th>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 3)</th>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 6)</th>
      <td>8</td>
      <td>2</td>
      <td>6</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(3, 5)</th>
      <td>8</td>
      <td>3</td>
      <td>5</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(3, 1)</th>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 4)</th>
      <td>5</td>
      <td>1</td>
      <td>4</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(2, 2)</th>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(3, 6)</th>
      <td>9</td>
      <td>3</td>
      <td>6</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(6, 3)</th>
      <td>9</td>
      <td>6</td>
      <td>3</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>(1, 2)</th>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Finalmente, antes de llenar nuestra última columna, correspondiente a la probabilidad de obtener el valor `sm`, permíteme recordarte que cada uno de los lanzamientos se trata de un evento independiente el uno del otro, por lo que, gracias a conocimientos en probabilidad, podemos decir que se cumple lo siguiente: 

$$ 
\mathbb{P}(\text{sm})=\mathbb{P}(\text{d1})*\mathbb{P}(\text{d2}) 
$$ 

Entonces, llevándolo a Python, podemos escribir lo siguiente: 


```python
df.p = df.pd1 * df.pd2
```


```python
df.sample(10)
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
      <th>sm</th>
      <th>d1</th>
      <th>d2</th>
      <th>pd1</th>
      <th>pd2</th>
      <th>p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>(5, 5)</th>
      <td>10</td>
      <td>5</td>
      <td>5</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>0.037037</td>
    </tr>
    <tr>
      <th>(5, 6)</th>
      <td>11</td>
      <td>5</td>
      <td>6</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>0.037037</td>
    </tr>
    <tr>
      <th>(1, 4)</th>
      <td>5</td>
      <td>1</td>
      <td>4</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>0.037037</td>
    </tr>
    <tr>
      <th>(6, 5)</th>
      <td>11</td>
      <td>6</td>
      <td>5</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>0.037037</td>
    </tr>
    <tr>
      <th>(6, 6)</th>
      <td>12</td>
      <td>6</td>
      <td>6</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>0.037037</td>
    </tr>
    <tr>
      <th>(6, 3)</th>
      <td>9</td>
      <td>6</td>
      <td>3</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>0.018519</td>
    </tr>
    <tr>
      <th>(6, 4)</th>
      <td>10</td>
      <td>6</td>
      <td>4</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>0.037037</td>
    </tr>
    <tr>
      <th>(5, 2)</th>
      <td>7</td>
      <td>5</td>
      <td>2</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>0.018519</td>
    </tr>
    <tr>
      <th>(1, 5)</th>
      <td>6</td>
      <td>1</td>
      <td>5</td>
      <td>0.222222</td>
      <td>0.166667</td>
      <td>0.037037</td>
    </tr>
    <tr>
      <th>(1, 3)</th>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>0.111111</td>
      <td>0.166667</td>
      <td>0.018519</td>
    </tr>
  </tbody>
</table>
</div>



Y es con esto como hemos completado nuestra tabla, pero no hemos dado respuesta a nuestro planteamiento principal, pues lo único que hemos hecho hasta el momento es obtener la probabilidad de obtener cada una de las posibles combinaciones, ahora lo único que nos falta es agrupar los datos según el valor `sm` (2, 3, ..., 11, 12) y sumar los valores de `p`. 

Esto con el propósito de obtener la probabilidad final de obtener dichos valores (2, 3, ..., 11, 12). 

Gracias a Python y Pandas, esto es muy fácil con la siguiente línea. 


```python
probabilidades = df.groupby('sm')['p'].sum()
probabilidades.sort_index()
```




    sm
    2     0.018519
    3     0.037037
    4     0.055556
    5     0.092593
    6      0.12963
    7     0.166667
    8     0.148148
    9      0.12963
    10    0.111111
    11    0.074074
    12    0.037037
    Name: p, dtype: object



Así podemos completar nuestra respuesta a el enunciado inicial, conociendo el valor de cada uno de los valores anteriores.

---
    
El notebook de este post, los puedes encontrar [aquí](https://colab.research.google.com/drive/1cuTKm5W3ngxRXCinQoVUAJpjjmDe1cuC?usp=sharing).