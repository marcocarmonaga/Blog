---
layout: post
title:  "Tabla de Análisis de Varianza (ANOVA) de doble factor, aplicada al estudio de métodos de deposición de películas antirreflejantes a una superficie óptica. "
date:   2022-05-05
author: Marco A. Carmona
---

Un investigador está estudiando el proceso de depositar una película antirreflejante a una superficie óptica. Dos métodos de deposición se seleccionan, por evaporación y por deposición química de vapor (CVD). Se hace un estudio de adherencia de la película. Sin embargo, existen tres tipos de películas diferentes que se pueden depositar y al investigador le interesa conocer si las tres películas diferentes, difieren en sus propiedades de adherencia. Se realizó un experimento factorial para investigar el efecto que tiene el tipo de película y el método de aplicación sobre la adherencia de la película. Se depositaron tres superficies de prueba con cada tipo de película utilizando cada uno de los dos métodos de aplicación, y se midió la fuerza de adherencia. En la tabla siguiente se muestran los resultados del experimento: 

|Tipo de película|Evaporación|CVD        |
|----------------|-----------|-----------|
|1               |4.0,4.5,4.3|5.4,4.9,5.6|
|2               |5.6,4.9,5.4|5.8,6.1,6.3|
|3               |3.8,3.7,4.0|5.5,5.0,5.0|

### Importando módulos a utilizar


```python
import pandas as pd
import numpy as np
from scipy import stats
import statsmodels.api as sm
import statsmodels.formula.api as smf
from statsmodels.graphics.factorplots import interaction_plot
import matplotlib.pyplot as plt
```

### Recreando tabla anterior dentro de Python con Pandas


```python
evaporacion_obs = np.array([[4.0,4.5,4.3],[5.6,4.9,5.4],[3.8,3.7,4.]])
tipo_de_pelicula = ['1','2','3']

df_1 = pd.DataFrame(evaporacion_obs.T)
df_1.columns = tipo_de_pelicula
df_1['Metodo'] = 'Evaporacion'; df_1
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
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>Metodo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4.0</td>
      <td>5.6</td>
      <td>3.8</td>
      <td>Evaporacion</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.5</td>
      <td>4.9</td>
      <td>3.7</td>
      <td>Evaporacion</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.3</td>
      <td>5.4</td>
      <td>4.0</td>
      <td>Evaporacion</td>
    </tr>
  </tbody>
</table>
</div>




```python
cvd_obs = np.array([[5.4,4.9,5.6],[5.8,6.1,6.3],[5.5,5.0,5.0]])
tipo_de_pelicula = ['1','2','3']

df_2 = pd.DataFrame(cvd_obs.T)
df_2.columns = tipo_de_pelicula
df_2['Metodo'] = 'CVD'; df_2
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
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>Metodo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.4</td>
      <td>5.8</td>
      <td>5.5</td>
      <td>CVD</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>6.1</td>
      <td>5.0</td>
      <td>CVD</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.6</td>
      <td>6.3</td>
      <td>5.0</td>
      <td>CVD</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.concat([df_1,df_2]); df
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
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>Metodo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4.0</td>
      <td>5.6</td>
      <td>3.8</td>
      <td>Evaporacion</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.5</td>
      <td>4.9</td>
      <td>3.7</td>
      <td>Evaporacion</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.3</td>
      <td>5.4</td>
      <td>4.0</td>
      <td>Evaporacion</td>
    </tr>
    <tr>
      <th>0</th>
      <td>5.4</td>
      <td>5.8</td>
      <td>5.5</td>
      <td>CVD</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>6.1</td>
      <td>5.0</td>
      <td>CVD</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.6</td>
      <td>6.3</td>
      <td>5.0</td>
      <td>CVD</td>
    </tr>
  </tbody>
</table>
</div>



### Transformando datos para facilitar análisis de varianza


```python
df_anova = pd.melt(df,id_vars='Metodo')
df_anova.columns = ['Metodo','Tipo','AD']; df_anova
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
      <th>Metodo</th>
      <th>Tipo</th>
      <th>AD</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Evaporacion</td>
      <td>1</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Evaporacion</td>
      <td>1</td>
      <td>4.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Evaporacion</td>
      <td>1</td>
      <td>4.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CVD</td>
      <td>1</td>
      <td>5.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CVD</td>
      <td>1</td>
      <td>4.9</td>
    </tr>
    <tr>
      <th>5</th>
      <td>CVD</td>
      <td>1</td>
      <td>5.6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Evaporacion</td>
      <td>2</td>
      <td>5.6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Evaporacion</td>
      <td>2</td>
      <td>4.9</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Evaporacion</td>
      <td>2</td>
      <td>5.4</td>
    </tr>
    <tr>
      <th>9</th>
      <td>CVD</td>
      <td>2</td>
      <td>5.8</td>
    </tr>
    <tr>
      <th>10</th>
      <td>CVD</td>
      <td>2</td>
      <td>6.1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>CVD</td>
      <td>2</td>
      <td>6.3</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Evaporacion</td>
      <td>3</td>
      <td>3.8</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Evaporacion</td>
      <td>3</td>
      <td>3.7</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Evaporacion</td>
      <td>3</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>CVD</td>
      <td>3</td>
      <td>5.5</td>
    </tr>
    <tr>
      <th>16</th>
      <td>CVD</td>
      <td>3</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>CVD</td>
      <td>3</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



### Tabla de ANOVA


```python
model = smf.ols('AD ~ Tipo + Metodo + Tipo*Metodo',data=df_anova).fit()
anova_table = sm.stats.anova_lm(model,typ=1)
anova_table.columns = ['Grados de libertad','Suma de cuadrados','Media de cuadrados','F0','PR(>F)']; anova_table
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
      <th>Grados de libertad</th>
      <th>Suma de cuadrados</th>
      <th>Media de cuadrados</th>
      <th>F0</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Tipo</th>
      <td>2.0</td>
      <td>4.581111</td>
      <td>2.290556</td>
      <td>27.858108</td>
      <td>0.000031</td>
    </tr>
    <tr>
      <th>Metodo</th>
      <td>1.0</td>
      <td>4.908889</td>
      <td>4.908889</td>
      <td>59.702703</td>
      <td>0.000005</td>
    </tr>
    <tr>
      <th>Tipo:Metodo</th>
      <td>2.0</td>
      <td>0.241111</td>
      <td>0.120556</td>
      <td>1.466216</td>
      <td>0.269342</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>12.0</td>
      <td>0.986667</td>
      <td>0.082222</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Conclusión

Para brindar una conclusión con respecto a los diversos valores de PR(>F) que se tienen, es importante observar que si consideramos un $\alpha = 0.05$, notamos que tanto el Tipo como el Método son menores a este valor de $\alpha$, caso contrario a lo que sucede con el factor de su interacción.

Con esto podemos decir que su interacción entre el Tipo y el Método no genera efecto alguno sobre la adherencia, sin embargo, ambos factores por separado sí generan diferencia alguna.

Su gráfica de interacción quedaría mostrada de la siguiente manera:


```python
fig = interaction_plot(x=df_anova['Tipo'], trace=df_anova['Metodo'], response=df_anova['AD'],xlabel='Tipo de película', ylabel='Adherencia')
fig.set_size_inches(9,4.5)
plt.xlabel('Tipo de película')
plt.ylabel('Adherencia promedio')
plt.title('Adherencia de películas')
plt.show()
```


    
![png](/Blog/assets/images/posts/output_31_0.png)
    


De esta gráfica podemos observar que la mayor y mejor adherencia se logra con el método CVD, aplicado al tipo de película número 2, caso contrario, se tiene una mala adherencia si se aplica el método de evaporación al tipo de película número 3.


```python
fig, ax = plt.subplots()
fig.set_size_inches(9,4.5)
ax = sm.qqplot(model.resid,stats.norm, line='r',ax=ax)
plt.show()
```


    
![png](/Blog/assets/images/posts/output_32_0.png)
    


Por otro lado, gracias a esta gráfica logramos notar que los datos experimentales son normales con una buena aproximación.
