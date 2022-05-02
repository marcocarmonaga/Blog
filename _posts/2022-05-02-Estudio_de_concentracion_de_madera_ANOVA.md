---
layout: post
title:  "Tabla de Análisis de Varianza (ANOVA) aplicada al estudio de concentración de madera en la fabricación de papel"
date:   2022-03-27
author: Marco A. Carmona
---

Un fabricante de papel se interesa en mejorar la resistencia a la tensión. Un grupo de investigación piensa que la resistencia a la tensión es una función de la concentración de pulpa de madera dura en la formulación y que el rango de interés práctico de las concentraciones de madera dura esta entre 5 y 20%.

El equipo responsable del estudio decide investigar cuatro niveles de concentración de madera dura: 5, 10, 15 y 20%. Para ello se fabrican 6 especímenes de prueba para cada nivel de concentración, utilizando una planta piloto. Los 24 especímenes se someten a prueba en un probador de laboratorio, en un orden aleatorio. Los datos del experimento son:

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
    <th class="tg-9ydz">Concentración de<br>madera dura (%)</th>
    <th class="tg-9ydz" colspan="6">Observaciones: Resistencia a la tensión (PSI)</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-ao2g"></td>
    <td class="tg-9ydz">1</td>
    <td class="tg-9ydz">2</td>
    <td class="tg-9ydz">3</td>
    <td class="tg-9ydz">4</td>
    <td class="tg-9ydz">5</td>
    <td class="tg-9ydz">6</td>
  </tr>
  <tr>
    <td class="tg-ao2g">5</td>
    <td class="tg-ao2g">7</td>
    <td class="tg-ao2g">8</td>
    <td class="tg-ao2g">15</td>
    <td class="tg-ao2g">11</td>
    <td class="tg-ao2g">9</td>
    <td class="tg-ao2g">10</td>
  </tr>
  <tr>
    <td class="tg-ao2g">10</td>
    <td class="tg-ao2g">12</td>
    <td class="tg-ao2g">17</td>
    <td class="tg-ao2g">13</td>
    <td class="tg-ao2g">18</td>
    <td class="tg-ao2g">19</td>
    <td class="tg-ao2g">15</td>
  </tr>
  <tr>
    <td class="tg-ao2g">15</td>
    <td class="tg-ao2g">14</td>
    <td class="tg-ao2g">18</td>
    <td class="tg-ao2g">19</td>
    <td class="tg-ao2g">17</td>
    <td class="tg-ao2g">16</td>
    <td class="tg-ao2g">18</td>
  </tr>
  <tr>
    <td class="tg-ao2g">20</td>
    <td class="tg-ao2g">19</td>
    <td class="tg-ao2g">25</td>
    <td class="tg-ao2g">22</td>
    <td class="tg-ao2g">23</td>
    <td class="tg-ao2g">18</td>
    <td class="tg-ao2g">20</td>
  </tr>
</tbody>
</table>

Considerando el problema planteado con anterioridad, el análisis de varianza (ANOVA) puede emplearse para probar la hipótesis de que concentraciones de madera dura diferentes afectan la resistencia promedio a la tensión del papel. 

En otras palabras, lo que buscamos probar es la siguiente hipótesis:

$$
H_0 = \tau_{i} = 0\quad\forall\ i
$$

O caso contrario

$$
H_1 = \tau_{i} \neq 0\quad\text{(para al menos un $i$)}
$$

Equivalentes a decir que las concentraciones son iguales o diferentes, respectivamente.

Dicho lo anterior, nuestro ANOVA quedaría dado de la siguiente manera, utilizando Python.

### Importando módulos a utilizar


```python
import pandas as pd
import statsmodels.api as sm
from statsmodels.formula.api import ols
import seaborn as sns
import numpy as np
```

### Recreando tabla anterior dentro de Python con Pandas


```python
observaciones = np.array([[7,8,15,11,9,10],[12,17,13,18,19,15],[14,18,19,17,16,18],[19,25,22,23,18,20]])
concentraciones = ['5%','10%','15%','20%']

df = pd.DataFrame(observaciones.T)
df.columns = concentraciones
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
      <th>5%</th>
      <th>10%</th>
      <th>15%</th>
      <th>20%</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>12</td>
      <td>14</td>
      <td>19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>17</td>
      <td>18</td>
      <td>25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15</td>
      <td>13</td>
      <td>19</td>
      <td>22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11</td>
      <td>18</td>
      <td>17</td>
      <td>23</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9</td>
      <td>19</td>
      <td>16</td>
      <td>18</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10</td>
      <td>15</td>
      <td>18</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



### Transformando datos para facilitar análisis de varianza


```python
df_anova = pd.melt(df)
df_anova.columns = ['Concentracion','Valor']
df_anova
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
      <th>Concentracion</th>
      <th>Valor</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5%</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5%</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5%</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5%</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5%</td>
      <td>9</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5%</td>
      <td>10</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10%</td>
      <td>12</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10%</td>
      <td>17</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10%</td>
      <td>13</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10%</td>
      <td>18</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10%</td>
      <td>19</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10%</td>
      <td>15</td>
    </tr>
    <tr>
      <th>12</th>
      <td>15%</td>
      <td>14</td>
    </tr>
    <tr>
      <th>13</th>
      <td>15%</td>
      <td>18</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15%</td>
      <td>19</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15%</td>
      <td>17</td>
    </tr>
    <tr>
      <th>16</th>
      <td>15%</td>
      <td>16</td>
    </tr>
    <tr>
      <th>17</th>
      <td>15%</td>
      <td>18</td>
    </tr>
    <tr>
      <th>18</th>
      <td>20%</td>
      <td>19</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20%</td>
      <td>25</td>
    </tr>
    <tr>
      <th>20</th>
      <td>20%</td>
      <td>22</td>
    </tr>
    <tr>
      <th>21</th>
      <td>20%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>22</th>
      <td>20%</td>
      <td>18</td>
    </tr>
    <tr>
      <th>23</th>
      <td>20%</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



### Gráfico de cajas para nuestro problema


```python
sns.boxplot(x='Concentracion',y='Valor',data=df_anova)
```




    <AxesSubplot:xlabel='Concentracion', ylabel='Valor'>




    
![png](/Blog/assets/images/posts/output_10_1.png)
    


### Tabla de ANOVA


```python
model = ols('Valor ~ Concentracion',data=df_anova).fit()
anova_table = sm.stats.anova_lm(model,typ = 1)
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
      <th>Concentracion</th>
      <td>3.0</td>
      <td>382.791667</td>
      <td>127.597222</td>
      <td>19.605207</td>
      <td>0.000004</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>20.0</td>
      <td>130.166667</td>
      <td>6.508333</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Conclusión

De aquí podemos fijarnos en nuestro valor de $P$ dado para $F_{0}$ y podemos concluir que, con una asertividad de 0.000004, $H_0$ es cierta, sin embargo, es casi nula; por lo tanto, decimos que no tenemos pruebas suficientes para aceptarla, por lo tanto, la rechazamos y finalizamos diciendo que el porcentaje de concentración de madera dura en el papel afecta la tensión de este. 

---

El notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/72dcff8dce1528c1715775577dc9e55b).