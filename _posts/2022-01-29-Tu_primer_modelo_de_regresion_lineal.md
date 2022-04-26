---
layout: post
title:  "Tu primer modelo de machine learning: Regresión Lineal aplicado a un conjunto de datos sobre plantas iris"
date:   2022-01-29
author: Marco A. Carmona
---

La idea de fabricar máquinas inteligentes, sensibles y autoconscientes no es algo que haya surgido de repente en los últimos años. De hecho, muchos relatos de la **mitología griega** hablan de máquinas e inventos inteligentes que tienen conciencia de sí mismos e inteligencia propia. Los orígenes y la evolución del ordenador han sido realmente revolucionarios a lo largo de varios siglos, desde el ábaco básico y su descendiente la regla de cálculo en el siglo XVII hasta el primer ordenador de propósito general diseñado por [Charles Babbage](https://en.wikipedia.org/wiki/Charles_Babbage) en el siglo XIX. 

Con ordenadores más rápidos, mejor procesamiento, mejor poder de cálculo y más almacenamiento, hemos estado viviendo en lo que me gusta llamar, la **"era de la información"** o la **"era de los datos"**. Día tras día, nos enfrentamos a la gestión de **Big Data** y a la construcción de sistemas inteligentes utilizando conceptos y metodologías de la Ciencia de los Datos, la Inteligencia Artificial, la Minería de Datos y el **Aprendizaje Automático**, o mejor conocido como **Machine Learning**. 

Para esta ocasión nos enfocaremos en este último término y en su modelo más simple: **La Regresión**. 

Los modelos de regresión utilizan **atributos** o **características** de los datos de entrada (también denominados variables explicativas o independientes) y sus correspondientes valores numéricos continuos de salida (también denominados variables de respuesta, dependientes o de resultado) para aprender **relaciones** y **asociaciones** específicas entre las entradas y sus correspondientes salidas. 

El ejemplo más claro se encuentra en la famosa Regresión Lineal con la que se trata de modelar las relaciones en los datos con una característica o variable explicativa $$x$$ y una única variable de respuesta $$y$$ donde el objetivo es predecir $$y$$. Los métodos como los **mínimos cuadrados ordinarios (OLS)** se utilizan normalmente para obtener el mejor ajuste lineal durante el entrenamiento del modelo. 

En **términos matemáticos** (y sin entrar en mucho detalle) lo que buscamos es una función $$y=mx+b$$ que se ajuste y se aproxime lo mejor posible a una serie finita de puntos. 

Afortunadamente existen librerías, como Sklearn, que están especializadas en este tipo de áreas y permiten al desarrollador trabajar directamente con los datos. En este post, y a manera de ejemplo e intentando dirigir esta herramienta hacia el ámbito científico, trabajaremos con un conjunto de datos de plantas de nombre **iris**. El cual contiene las siguientes columnas: 

- sepal length:longitud de los sépalos en cm 
- sepal width: ancho de los sépalos en cm 
- petal length: longitud de los pétalos en cm 
- petal width: ancho de los pétalos en cm 
- class: 
    - Iris-Setosa 
    - Iris-Versicolour 
    - Iris-Virginica 

Ahora, *¿dónde se puede encontrar esta base de datos?* Sklearn la trae precargada, para ello basta con que tengas instalada dicha librería en tu entorno de Python. En caso de no tenerla, basta con escribir el siguiente comando de tu terminal y pulsar `Enter`. 

```bash 
pip install sklearn 
``` 

Una vez instalado Sklearn en tu sistema, dirígete tu editor de código favorito, el cuál puede ser [Visual Studio Code](https://code.visualstudio.com/) o [Google Colab](https://colab.research.google.com/), o cualquier otro; y comenzaremos cargando los módulos que utilizaremos: 


```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
```

Posterior, procedemos a cargar los datos con los que vamos a trabajar; pero con un extra: vamos a unirlos en un solo dataframe para así poder observar su relación.


```python
iris_X, iris_Y = load_iris(return_X_y=True,as_frame=True)
dataset = pd.DataFrame(iris_X)
dataset['target'] = iris_Y
dataset.sample(5, ignore_index=True)
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
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
      <th>target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.6</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>5.5</td>
      <td>4.2</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>7.7</td>
      <td>2.8</td>
      <td>6.7</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <td>5.7</td>
      <td>2.5</td>
      <td>5.0</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <td>5.0</td>
      <td>2.0</td>
      <td>3.5</td>
      <td>1.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



Al tratarse de una [regresión lineal](https://es.wikipedia.org/wiki/Regresi%C3%B3n_lineal), lo que buscamos es una correlación coherente entre dos columnas de nuestra base de datos. Así, en nuestro caso plantearemos la idea de pensar que existe una cierta correlación entre la longitud y el ancho de los pétalos. 

Entonces, para poder trabajar con únicamente esas dos columnas, procedemos a separarlas en dos conjuntos, uno de ellos correspondiente a la longitud de los pétalos $$X$$ y el otro correspondiente a su respectivo ancho $$Y$$. 

> **Nota** 
>  
> Para una correcta implementación de nuestra regresión lineal con Sklearn, es importante que nuestro conjunto $$X$$ lo representemos como un vector vertical y nuestro conjunto $$Y$$ sea un vector horizontal. 


```python
X = dataset.iloc[:,2:-2].values
Y = dataset.iloc[:,3].values
```

Una vez tomado los conjuntos de datos con las que vamos a trabajar, es importante hacer una separación entre datos de entramiento y datos de testeo. 

El objetivo de un modelo de aprendizaje automático es identificar patrones en los datos de entrenamiento. Estos patrones se utilizan para hacer predicciones utilizando nuevos datos. 

Así, utilizaremos el método `train_test_split()` para dividir los datos en conjuntos de entrenamiento y de prueba. El porcentaje de división de los datos viene determinado por el parámetro `test_size`. El fragmento que se muestra a continuación mantiene el 20 por ciento de los datos originales para el conjunto de prueba. 


```python
X_train, X_test, Y_train, Y_test = train_test_split(X,Y, random_state=True, test_size=0.2)
```

Y aquí viene una de las partes más importantes para el entramiento de nuestro modelo, pues las siguientes líneas lo que harán es entrenar y ajustar el modelo a nuestros datos de entramiento (`X_train`, `Y_train`). 


```python
regressor = LinearRegression()
regressor.fit(X_train,Y_train)
```




    LinearRegression()



Así de sencillo es entrenar nuestro modelo; posterior a ello y con el afán de poder observar su comportamiento, vamos a graficar el conjunto de datos.


```python
fig, (ax1,ax2) = plt.subplots(1,2,figsize=(15,5), sharey=True)
fig.tight_layout() # Or equivalently,  "plt.tight_layout()"
ax1.scatter(X_train,Y_train)
ax1.plot(X_train,regressor.predict(X_train), color='black')
ax1.set_title(r'Datos de entrenamiento')
ax1.set_xlabel(r'Longitud del pétalo')
ax1.set_ylabel(r'Ancho del pétalo')
ax2.scatter(X_test,Y_test)
ax2.plot(X_train,regressor.predict(X_train), color='black')
ax2.set_title(r'Datos de testeo')
ax2.set_xlabel(r'Longitud del pétalo')
plt.show()
```


    
![png](/Blog/assets/images/posts/output_11_0.png)
    


A simple vista podemos ver que se ajusta muy bien a nuestros datos de testeo, ¿pero con qué exactitud? La respuesta es muy fácil de conocer gracias al siguiente comando:


```python
print(f'El porcetaje de exactitud es de {regressor.score(X_test,Y_test) * 100}%.')
```

    El porcetaje de exactitud es de 92.7934211107492%.


Hablando en términos prácticos, es una muy buena aproximación, sin embargo, si nos ponemos científicamente estrictos, es importante considerar que el error aceptable es de $$\pm 5 \%$$, entonces sería importante considerar este dato para mejorar nuestros datos en el dataset y así mejorar la exactitud. 

---  

## Ejercicio para el lector  

Considerando el procedimiento anterior, 

> ¿Existe relación entre la longitud del sépalo y su respectivo ancho?  

### *Pista* 


```python
X = dataset.iloc[:,0:-4].values
Y = dataset.iloc[:,1].values
```

La solución a este ejercicio y la misma notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/224126c2accb35bb3afcc11f7c08891f).

