---
layout: post
title:  "Supresión de crecimiento bacterial en películas orgánicas almacenadas."
date:   2022-03-27
author: Marco A. Carmona
---

La vida de anaquel de películas orgánicas almacenadas es el tiempo que una muestra previamente almacenada está en condiciones de ser usada en el laboratorio.

Una película normal expuesta al aire ambiental tiene una vida aproximada de 48 horas, después comienza a deteriorarse (degradación de color y encogimiento) por contaminación microbiana. El almacenamiento al vacío suprime el desarrollo de microorganismos, pero no reduce otros problemas.

Estudios recientes sugieren el uso de atmósferas controladas de gas como alternativa. Dos atmósferas propuestas son:

1. Dióxido de Carbono puro $$(CO_{2})$$
2. Mezclas de:
    - Monóxido de Carbono $$(CO)$$
    - Oxígeno $$(O)$$
    - Nitrógeno $$(N)$$ 

## Hipótesis de investigación $$H_0$$

Se plantea la hipótesis de que alguna forma de atmósfera controlada proporciona un entorno efectivo de almacenamiento.

## Diseño de tratamiento

1. Aire del ambiente con almacenamiento comercial de plástico
2. Al vacío
3. Mezcla de: $$1\%$$ $$CO$$, $$40\%$$ $$O_{2}$$ y $$59\%$$ $$N$$
4. $$100\%$$ $$CO_{2}$$

Los almacenamientos con aire y al vacío sirven como tratamientos de control ya que ambos son estándares con cuya efectividad se pueden comparar los nuevos almacenamientos.

## Diseño de experimento

Se usa un diseño totalmente aleatorizado. A cada conjunto de condiciones de almacenamiento se le asignan al azar 3 películas del mismo tamaño $$(9\ \text{cm}^{2})$$. 

## Preparando dataframe

### Importando librerías


```python
import pandas as pd
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
```

### Importando datos


```python
Tratamientos = ['Almacenamiento_en_plastico', 'Almacenamiento_al_vacio', 'Mezcla_CO_O_N', 'CO_2']
Muestra_1 = [7.66,5.26,7.41,3.51]
Muestra_2 = [6.98,5.44,7.33,2.91]
Muestra_3 = [7.80,5.80,7.04,3.66]

data = np.array([Tratamientos,Muestra_1,Muestra_2,Muestra_3])
```

### Creando dataframe


```python
df = pd.DataFrame(data.T, columns=['Tratamientos','Muestra_1','Muestra_2','Muestra_3'])
df
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
      <th>Tratamientos</th>
      <th>Muestra_1</th>
      <th>Muestra_2</th>
      <th>Muestra_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Almacenamiento_en_plastico</td>
      <td>7.66</td>
      <td>6.98</td>
      <td>7.8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Almacenamiento_al_vacio</td>
      <td>5.26</td>
      <td>5.44</td>
      <td>5.8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mezcla_CO_O_N</td>
      <td>7.41</td>
      <td>7.33</td>
      <td>7.04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CO_2</td>
      <td>3.51</td>
      <td>2.91</td>
      <td>3.66</td>
    </tr>
  </tbody>
</table>
</div>



### Transformando el tipo de datos de objecto a flotante


```python
df.Muestra_1 = df.Muestra_1.astype(float)
df.Muestra_2 = df.Muestra_2.astype(float)
df.Muestra_3 = df.Muestra_3.astype(float)
```

### Verificando la efectividad de la transformación


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4 entries, 0 to 3
    Data columns (total 4 columns):
     #   Column        Non-Null Count  Dtype  
    ---  ------        --------------  -----  
     0   Tratamientos  4 non-null      object 
     1   Muestra_1     4 non-null      float64
     2   Muestra_2     4 non-null      float64
     3   Muestra_3     4 non-null      float64
    dtypes: float64(3), object(1)
    memory usage: 256.0+ bytes


### Agregando columna de suma total y promedio por cada uno de los tratamientos


```python
df['Total'] = df.iloc[:,1:4].sum(axis=1)
df['Mean'] = df.iloc[:,1:4].mean(axis=1)
```


```python
df
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
      <th>Tratamientos</th>
      <th>Muestra_1</th>
      <th>Muestra_2</th>
      <th>Muestra_3</th>
      <th>Total</th>
      <th>Mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Almacenamiento_en_plastico</td>
      <td>7.66</td>
      <td>6.98</td>
      <td>7.80</td>
      <td>22.44</td>
      <td>7.48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Almacenamiento_al_vacio</td>
      <td>5.26</td>
      <td>5.44</td>
      <td>5.80</td>
      <td>16.50</td>
      <td>5.50</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mezcla_CO_O_N</td>
      <td>7.41</td>
      <td>7.33</td>
      <td>7.04</td>
      <td>21.78</td>
      <td>7.26</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CO_2</td>
      <td>3.51</td>
      <td>2.91</td>
      <td>3.66</td>
      <td>10.08</td>
      <td>3.36</td>
    </tr>
  </tbody>
</table>
</div>



## Obteniendo la variabilidad total de los datos $$SS_{T}$$

Por definición sabemos que lo siguiente:

$$
SS_{T}=\sum_{i=1}^{a}\sum_{j=1}^{n}(Y_{ij}-\overline{Y})^{2}
$$

lo cual, por identidad, podemos deducir lo siguiente:

$$
\sum_{i=1}^{a}\sum_{j=1}^{n}(Y_{ij}-\overline{Y})^{2}=n\sum_{i=1}^{n}(\overline{Y}_{i*}-\overline{Y})^{2}+\sum_{i=1}^{a}\sum_{j=1}^{n}(Y_{ij}-\overline{Y}_{i*})^{2}
$$

Donde $$(\overline{Y}\_{i\*}-\overline{Y})^{2}$$ mide la diferencia entre los tratamientos y $$(Y\_{ij}-\overline{Y}\_{i\*})^{2}$$ mide dentro de un tratamiento la diferencia de las observaciones respecto a la media del tratamiento, esto último debido al error aleatorio.

De esta manera, podemos escribir a $$SS_{T}$$ de la siguiente manera:

$$
SS_{T}=SS_{tratamiento} + SS_{error}
$$

Donde

$$
SS_{tratamiento}=n\sum_{i=1}^{n}(\overline{Y}_{i*}-\overline{Y})^{2}
$$

es la suma de cuadrados de los tratamientos, y...

$$
SS_{error} = \sum_{i=1}^{a}\sum_{j=1}^{n}(Y_{ij}-\overline{Y}_{i*})^{2}
$$

es la suma de cuadrado del error.

### Calculando $$SS_{tratamiento}$$

Sabemos que $$SS_{tratamiento}$$ está dada de la siguiente manera:

$$
SS_{tratamiento}=n\sum_{i=1}^{n}(\overline{Y}_{i*}-\overline{Y})^{2}
$$

Donde $$n=3$$, para nuestro caso particular, pues son 3 las muestras para cada uno de los tratamientos.


```python
# Definiendo el valor de n
n = 3

# Calculando el promedio de los promedios
Y = df['Mean'].mean(axis=0)

# Definiendo la columna de los promedios
Y_i = df.Mean.values

# Definiendo SS_tratamiento
SS_trat = n * sum((Y_i - Y)**2)

# Imprimiendo valor de SS_tratamiento
print(SS_trat.round(3))
```

    32.873


$$
\boxed{\therefore\ SS_{tratamiento} = 32.873}
$$

### Calculando $$SS_{error}$$

Por otro lado, sabemos que $$SS_{error}$$ está dado de la siguiente forma:

$$
SS_{error} = \sum_{i=1}^{a}\sum_{j=1}^{n}(Y_{ij}-\overline{Y}_{i*})^{2}
$$


```python
# Definiendo el valor de los promedios para cada tratamiento
Y_i = df.Mean.values

# Definiendo el conjunto de datos de cada uno de los tratamientos 
# para las 3 muestras
Y_ij = df.iloc[:,1:4].values

# Definiendo la suma de cuadrados del error
SS_E_1 = sum((Y_ij[0]-Y_i[0])**2)
SS_E_2 = sum((Y_ij[1]-Y_i[1])**2)
SS_E_3 = sum((Y_ij[2]-Y_i[2])**2)
SS_E_4 = sum((Y_ij[3]-Y_i[3])**2)

SS_E = SS_E_1+SS_E_2+SS_E_3+SS_E_4

# Imprimiendo el valor de SS_error con una precisión de 3 decimales
print(SS_E.round(3))
```

    0.927


$$
\boxed{\therefore\ SS_{erro} = 0.927}
$$

Con lo que podemos concluir que $$SS_{T}$$ tiene el siguiente valor.

$$
\boxed{\therefore\ SS_{T} = 33.8}
$$

Esto recordando que $$SS_{T}=SS_{tratamiento}+SS_{error}$$.

## Definiendo la media de cuadrados para tratamientos y el error cuadrático medio.

Por definición tenemos que la media de cuadrados para tratamientos está dada de la siguiente forma:

$$
MS_{tratamiento} = \frac{SS_{tratamiento}}{a-1}
$$

donde $$a$$ es el número de tratamientos; 4 para nuestro caso en particular.

De esta manera, llevando este término a nuestro problema, tenemos lo siguiente:


```python
# Definiendo el número de tratamientos
a = 4

# Definiendo nuestra media de cuadrados para tratamientos
M_S_trat = SS_trat / (a - 1)

# Imprimiendo el valor de M_S_tratamiento
print(M_S_trat.round(3))
```

    10.958


Con lo que podemos concluir lo siguiente:

$$
MS_{tratamiento} = 10.958
$$

Por otro lado, tenemos el error cuadrático medio; el cual está definido de la siguiente manera:

$$
MS_{error} = \frac{SS_{error}}{a(n-1)}
$$

Donde $$a$$ es el número de tratamientos (4 para nuestro ejemplo), y $$n$$ es el número de muestras (3 para nuestro caso).

Así, podemos expresar lo siguiente:


```python
# Definiendo el número de tratamientos
a = 4

# Definiendo el número de muestras
n = 3

# Definiendo el error cuadrático medio
M_S_E = SS_E / (a * (n-1))

# Imprimiendo el valor de M_S_E con una precisión de 3 decimales
print(M_S_E.round(3))
```

    0.116


Con esto concluimos lo siguiente:

$$
MS_{error} = 0.116
$$

## ¿Para qué necesitamos estos dos últimos valores?

Resulta que estos dos últimos valores, $$MS_{tratamiento}$$ y $$MS_{error}$$, son de gran ayuda, pues son aquellos que nos permitirán aceptar o rechazar nuestra hipótesis de investigación $$H_{0}$$.

Esto gracias a que si $$F_{0}>F_{\alpha,a-1,a(n-1)}$$, se puede rechazar con certeza el valor de $$H_{0}$$; donde $$F_{\alpha,a-1,a(n-1)}$$ es una distribución de Fisher, con $$a-1$$ grados de libertad en el numerador y $$a(n-1)$$ grados de libertad en el denominador; y $$F_{0}$$ está dado por:

$$
F_{0} = \frac{MS_{tratamiento}}{MS_{error}}
$$

para un $$\alpha = 0.01$$.

De esta manera, regresando a nuestro problema particular, tenemos o siguiente:


```python
# Definiendo alpha
alpha = 0.05

# Definiendo mi distribución de Fisher con a-1 grados de libertad en el  
# numerador y a(n-1) grados de libertad en el denominador
F = stats.f(a-1,a*(n-1))

# Graficando mi distribución
x = np.linspace(0,10,100)
y = F.pdf(x)

fig, ax = plt.subplots()
fig.set_size_inches(12,8)
ax.set_ylim(y.min(),y.max())
ax.set_xlim(x.min(),x.max())
ax.plot(x,y, color='black')
plt.title(r'Distribución $$F_{3,8}$$',size = 25)
plt.show()
```


    
![png](/Blog/assets/images/posts/output_29_0.png)
    



```python
# Conociendo el valor de F_{alpha,a-1,a(n-1)}
F_alpha = F.ppf(1-alpha)

# Graficando el intervalo para F_aplha dentro de la distribución F_3,8
x = np.linspace(0,10,100)
y = F.pdf(x)

fig, ax = plt.subplots()
fig.set_size_inches(12,8)
ax.set_ylim(y.min(),y.max())
ax.set_xlim(x.min(),x.max())
ax.plot(x,y, color='black')
ax.fill_between(x,y,0, where = (x > F_alpha), color='r')
plt.title(r'Distribución $$F_{3,8}$$',size = 25)
plt.show()
```


    
![png](/Blog/assets/images/posts/output_30_0.png)
    


Si $$F_{0}$$ cae dentro del área de color rojo, $$H_{0}$$ se rechaza.


```python
# Calculando el valor de F_0
F_0 = M_S_trat / M_S_E

#Imprimiendo valor de F_0 con precisión de 3 decimales
print(F_0.round(3))
```

    94.584


Entonces, de aquí sabemos que si $$F_0 > F_{\alpha , 3,8}$$; entonces rechazamos $$H_0$$.


```python
print(F_0 > F_alpha)
```

    True


$$
\boxed{\therefore\ F_0 > F_{\alpha,3,8}}
$$

Por lo tanto, podemos rechazar con seguridad a $$H_{0}$$. 

Llevándonos a las siguientes preguntas: 

- Para reducir el desarrollo de bacterias. ¿Es más efectiva el uso de una atmósfera artificial que el almacenamiento en aire ambiental? 
- ¿Son más efectivos los gases que el vacío? 
- ¿Es más efectivo el $$CO_{2}$$ que una mezcla de $$CO$$, $$O_{2}$$ y $$N$$? 

---

El notebook de este post, los puedes encontrar [aquí](https://colab.research.google.com/drive/13W3QjyGQt8QFGpk0JPbrH46wjIM7Z9MW?usp=sharing).