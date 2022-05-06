---
layout: post
title:  "Prueba de Tukey para problema de contraste entre distitas muestras de liquidos residuales de diferentes plantas."
date:   2022-05-05
author: Marco A. Carmona
---

Una empresa tiene cuatro plantas y sabe que la planta A satisface los requisitos impuestos por el gobierno para el control de desechos de fabricación, pero quisiera determinar cuál es la situación de las otras tres. Para el efecto se toman cinco muestras de los líquidos residuales de cada una de las plantas y se determina la cantidad de contaminantes. Los residuos del experimento aparecen en la siguiente table: 

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-9ydz{border-color:#333333;font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-ao2g{border-color:#333333;text-align:center;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-9ydz">Plantas</th>
    <th class="tg-9ydz" colspan="5">Contaminantes</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-ao2g">A</td>
    <td class="tg-ao2g">1.65</td>
    <td class="tg-ao2g">1.72</td>
    <td class="tg-ao2g">1.50</td>
    <td class="tg-ao2g">1.35</td>
    <td class="tg-ao2g">1.60</td>
  </tr>
  <tr>
    <td class="tg-ao2g">B</td>
    <td class="tg-ao2g">1.70</td>
    <td class="tg-ao2g">1.85</td>
    <td class="tg-ao2g">1.46</td>
    <td class="tg-ao2g">2.05</td>
    <td class="tg-ao2g">1.80</td>
  </tr>
  <tr>
    <td class="tg-ao2g">C</td>
    <td class="tg-ao2g">1.40</td>
    <td class="tg-ao2g">1.75</td>
    <td class="tg-ao2g">1.38</td>
    <td class="tg-ao2g">1.65</td>
    <td class="tg-ao2g">1.55</td>
  </tr>
  <tr>
    <td class="tg-ao2g">D</td>
    <td class="tg-ao2g">2.10</td>
    <td class="tg-ao2g">1.95</td>
    <td class="tg-ao2g">1.65</td>
    <td class="tg-ao2g">1.88</td>
    <td class="tg-ao2g">2.00</td>
  </tr>
</tbody>
</table>

### Importando módulos a utilizar


```python
import pandas as pd
import numpy as np
import statsmodels.api as sm
import statsmodels.formula.api as smf
import seaborn as sns
import matplotlib.pyplot as plt
```

### Recreando tabla anterior dentro de Python con Pandas


```python
observaciones = np.array([
    [1.65,1.72,1.50,1.35,1.60],
    [1.70,1.85,1.46,2.05,1.80],
    [1.40,1.75,1.38,1.65,1.55],
    [2.10,1.95,1.65,1.88,2.00]
])

plantas = ['A','B','C','D']

df = pd.DataFrame(observaciones.T,columns=plantas); df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.65</td>
      <td>1.70</td>
      <td>1.40</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.72</td>
      <td>1.85</td>
      <td>1.75</td>
      <td>1.95</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.50</td>
      <td>1.46</td>
      <td>1.38</td>
      <td>1.65</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.35</td>
      <td>2.05</td>
      <td>1.65</td>
      <td>1.88</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.60</td>
      <td>1.80</td>
      <td>1.55</td>
      <td>2.00</td>
    </tr>
  </tbody>
</table>
</div>



### Transformando datos para facilitar análisis de varianza


```python
df_anova = pd.melt(df)
df_anova.columns = ['Planta','Valor']; df_anova
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
      <th>Planta</th>
      <th>Valor</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>1.65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A</td>
      <td>1.72</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A</td>
      <td>1.50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A</td>
      <td>1.35</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A</td>
      <td>1.60</td>
    </tr>
    <tr>
      <th>5</th>
      <td>B</td>
      <td>1.70</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B</td>
      <td>1.85</td>
    </tr>
    <tr>
      <th>7</th>
      <td>B</td>
      <td>1.46</td>
    </tr>
    <tr>
      <th>8</th>
      <td>B</td>
      <td>2.05</td>
    </tr>
    <tr>
      <th>9</th>
      <td>B</td>
      <td>1.80</td>
    </tr>
    <tr>
      <th>10</th>
      <td>C</td>
      <td>1.40</td>
    </tr>
    <tr>
      <th>11</th>
      <td>C</td>
      <td>1.75</td>
    </tr>
    <tr>
      <th>12</th>
      <td>C</td>
      <td>1.38</td>
    </tr>
    <tr>
      <th>13</th>
      <td>C</td>
      <td>1.65</td>
    </tr>
    <tr>
      <th>14</th>
      <td>C</td>
      <td>1.55</td>
    </tr>
    <tr>
      <th>15</th>
      <td>D</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>16</th>
      <td>D</td>
      <td>1.95</td>
    </tr>
    <tr>
      <th>17</th>
      <td>D</td>
      <td>1.65</td>
    </tr>
    <tr>
      <th>18</th>
      <td>D</td>
      <td>1.88</td>
    </tr>
    <tr>
      <th>19</th>
      <td>D</td>
      <td>2.00</td>
    </tr>
  </tbody>
</table>
</div>



### Tabla de ANOVA


```python
model = smf.ols('Valor ~ Planta',data=df_anova).fit()
anova_table = sm.stats.anova_lm(model,typ=1)
anova_table.columns = ['Grados de libertad','Suma de cuadrados','Media de cuadrados','F0','PR(>F)']
anova_table
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
      <th>Planta</th>
      <td>3.0</td>
      <td>0.470255</td>
      <td>0.156752</td>
      <td>5.170763</td>
      <td>0.010912</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>16.0</td>
      <td>0.485040</td>
      <td>0.030315</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Prueba de Tukey


```python
tukey = sm.stats.multicomp.pairwise_tukeyhsd(endog=df_anova['Valor'],
                          groups=df_anova['Planta'],
                          alpha=.05); print(tukey)
```

    Multiple Comparison of Means - Tukey HSD, FWER=0.05
    ===================================================
    group1 group2 meandiff p-adj   lower  upper  reject
    ---------------------------------------------------
         A      B    0.208 0.2713 -0.1071 0.5231  False
         A      C   -0.018 0.9984 -0.3331 0.2971  False
         A      D    0.352  0.026  0.0369 0.6671   True
         B      C   -0.226 0.2107 -0.5411 0.0891  False
         B      D    0.144 0.5714 -0.1711 0.4591  False
         C      D     0.37 0.0187  0.0549 0.6851   True
    ---------------------------------------------------


### Gráfico de cajas


```python
fig, ax = plt.subplots()
fig.set_size_inches(9,4.5)
ax = sns.boxplot(data=df_anova, x = 'Planta',y='Valor')
plt.show()
```


    
![png](/Blog/assets/images/posts/output_13_1.png)
    


### Conclusión

Para brindar una conclusión es importante notar que estamos considerando que la planta A es la única que sabemos que cumple con las normas, por lo que, de nuestra tabla de Tukey, únicamente nos bastará con tomar en cuenta aquellas que contengas a esta planta. 

Dicho lo anterior notemos que la única que no cumple con dichas normas de la planta A, es la planta D: lo cual nuestro gráfico de cajas nos lo permite notar gráficamente, ya que vemos que la caja correspondiente a la planta D, sale de un rango visual marcado por las plantas A, B y C. 

---

El notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/97863138e6f4b08bf3b32e7295009320).