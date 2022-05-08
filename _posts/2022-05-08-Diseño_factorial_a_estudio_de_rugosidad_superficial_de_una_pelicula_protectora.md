---
layout: post
title:  "Diseño factorial a estudio de rugosidad superficial de una película protectora."
date:   2022-05-08
author: Marco A. Carmona
---

Se hace un estudio de rugosidad superficial de una película protectora para superficies ópticas usadas en cámaras espaciales. El interés recae en $$n=3$$ factores, (X) rapidez del depósito, (Y) espesor de la película y (Z) ángulo de posicionamiento relativo. A los 3 factores se les asigna dos niveles, y se realizan dos réplicas en un diseño factorial.

Los niveles asignados son los siguientes:

|Factor|Nivel Bajo|Nivel Alto|
|:------:|:----------:|:----------:|
|Rapidez ($$\mu\text{m/min}$$)|20|30|
|Espesor ($$\mu\text{m}$$)|25|40|
|Ángulo|15°|25°|

La rugosidad es medida con un rugosímetro y se expresa en micras

|Rapidez|Espesor|Ángulo|Rugosidad|
|:------:|:----------:|:----------:|:----------:|
|20|25|15|9|
|30|25|15|10|
|20|40|15|9|
|30|40|15|12|
|20|25|25|11|
|30|25|25|10|
|20|40|25|10|
|30|40|25|16|
|20|25|15|7|
|30|25|15|12|
|20|40|15|11|
|30|40|15|15|
|20|25|25|10|
|30|25|25|13|
|20|40|25|8|
|30|40|25|14|

### Importando librerías


```R
library(AlgDesign)
```

### Generando diseño factorial $$2^{k}$$ con k = 3 variables


```R
# Diseño principal
factorialDesign = gen.factorial(levels = 2,nVars=3,varNames=c("Rapidez","Espesor","Angulo"))

# Agregando primera y segunda corrida
df = rbind(factorialDesign,factorialDesign)
df$Rapidez <- mapply(df$Rapidez, FUN=function(x) if (x == -1) "20" else "30")
df$Espesor <- mapply(df$Espesor, FUN=function(x) if (x == -1) "25" else "40")
df$Angulo <- mapply(df$Angulo, FUN=function(x) if (x == -1) "15" else "25"); df
```


<table>
<thead><tr><th scope=col>Rapidez</th><th scope=col>Espesor</th><th scope=col>Angulo</th></tr></thead>
<tbody>
	<tr><td>20</td><td>25</td><td>15</td></tr>
	<tr><td>30</td><td>25</td><td>15</td></tr>
	<tr><td>20</td><td>40</td><td>15</td></tr>
	<tr><td>30</td><td>40</td><td>15</td></tr>
	<tr><td>20</td><td>25</td><td>25</td></tr>
	<tr><td>30</td><td>25</td><td>25</td></tr>
	<tr><td>20</td><td>40</td><td>25</td></tr>
	<tr><td>30</td><td>40</td><td>25</td></tr>
	<tr><td>20</td><td>25</td><td>15</td></tr>
	<tr><td>30</td><td>25</td><td>15</td></tr>
	<tr><td>20</td><td>40</td><td>15</td></tr>
	<tr><td>30</td><td>40</td><td>15</td></tr>
	<tr><td>20</td><td>25</td><td>25</td></tr>
	<tr><td>30</td><td>25</td><td>25</td></tr>
	<tr><td>20</td><td>40</td><td>25</td></tr>
	<tr><td>30</td><td>40</td><td>25</td></tr>
</tbody>
</table>



### Añadiendo valores de respuesta


```R
df$Rugosidad <- c(9,10,9,12,11,10,10,16,7,12,11,15,10,13,8,14); df
```


<table>
<thead><tr><th scope=col>Rapidez</th><th scope=col>Espesor</th><th scope=col>Angulo</th><th scope=col>Rugosidad</th></tr></thead>
<tbody>
	<tr><td>20</td><td>25</td><td>15</td><td> 9</td></tr>
	<tr><td>30</td><td>25</td><td>15</td><td>10</td></tr>
	<tr><td>20</td><td>40</td><td>15</td><td> 9</td></tr>
	<tr><td>30</td><td>40</td><td>15</td><td>12</td></tr>
	<tr><td>20</td><td>25</td><td>25</td><td>11</td></tr>
	<tr><td>30</td><td>25</td><td>25</td><td>10</td></tr>
	<tr><td>20</td><td>40</td><td>25</td><td>10</td></tr>
	<tr><td>30</td><td>40</td><td>25</td><td>16</td></tr>
	<tr><td>20</td><td>25</td><td>15</td><td> 7</td></tr>
	<tr><td>30</td><td>25</td><td>15</td><td>12</td></tr>
	<tr><td>20</td><td>40</td><td>15</td><td>11</td></tr>
	<tr><td>30</td><td>40</td><td>15</td><td>15</td></tr>
	<tr><td>20</td><td>25</td><td>25</td><td>10</td></tr>
	<tr><td>30</td><td>25</td><td>25</td><td>13</td></tr>
	<tr><td>20</td><td>40</td><td>25</td><td> 8</td></tr>
	<tr><td>30</td><td>40</td><td>25</td><td>14</td></tr>
</tbody>
</table>



### Graficando interacciones entre las variables


```R
par(mfrow=c(2,2))
interaction.plot(df$Rapidez,df$Espesor,df$Rugosidad)
interaction.plot(df$Espesor,df$Angulo,df$Rugosidad)
interaction.plot(df$Rapidez,df$Angulo,df$Rugosidad)
```


    
![png](/Blog/assets/images/posts/output_8_1.png)
    


### Graficando gráfica de cajas de las variables y sus interacciones


```R
par(mfrow=c(2,2))
boxplot(Rugosidad ~ Rapidez, data=df)
boxplot(Rugosidad ~ Espesor, data=df)
boxplot(Rugosidad ~ Angulo, data=df)
```


    
![png](/Blog/assets/images/posts/output_10_2.png)
    



```R
par(mfrow=c(2,2))
boxplot(Rugosidad ~ Rapidez*Espesor, data=df)
boxplot(Rugosidad ~ Espesor*Angulo, data=df)
boxplot(Rugosidad ~ Angulo*Rapidez, data=df)
boxplot(Rugosidad ~ Rapidez*Espesor*Angulo, data=df)
```


    
![png](/Blog/assets/images/posts/output_11_1.png)
    


### Diseñando primer modelo a estudiar a partir de los datos anteriores.


```R
model = lm(Rugosidad ~ Rapidez*Espesor*Angulo,data = df)

# Imprimiendo resumen del modelo
summary(model)
```


    
    Call:
    lm(formula = Rugosidad ~ Rapidez * Espesor * Angulo, data = df)
    
    Residuals:
       Min     1Q Median     3Q    Max 
      -1.5   -1.0    0.0    1.0    1.5 
    
    Coefficients:
                                 Estimate Std. Error t value Pr(>|t|)    
    (Intercept)                     8.000      1.104   7.247 8.83e-05 ***
    Rapidez30                       3.000      1.561   1.922   0.0909 .  
    Espesor40                       2.000      1.561   1.281   0.2361    
    Angulo25                        2.500      1.561   1.601   0.1480    
    Rapidez30:Espesor40             0.500      2.208   0.226   0.8265    
    Rapidez30:Angulo25             -2.000      2.208  -0.906   0.3915    
    Espesor40:Angulo25             -3.500      2.208  -1.585   0.1516    
    Rapidez30:Espesor40:Angulo25    4.500      3.123   1.441   0.1875    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 1.561 on 8 degrees of freedom
    Multiple R-squared:  0.7902,	Adjusted R-squared:  0.6066 
    F-statistic: 4.304 on 7 and 8 DF,  p-value: 0.02881



### Imprimiendo análisis de varianza


```R
anova(model)
```


<table>
<thead><tr><th></th><th scope=col>Df</th><th scope=col>Sum Sq</th><th scope=col>Mean Sq</th><th scope=col>F value</th><th scope=col>Pr(&gt;F)</th></tr></thead>
<tbody>
	<tr><th scope=row>Rapidez</th><td>1          </td><td>45.5625    </td><td>45.5625    </td><td>18.69230769</td><td>0.002534218</td></tr>
	<tr><th scope=row>Espesor</th><td>1          </td><td>10.5625    </td><td>10.5625    </td><td> 4.33333333</td><td>0.070931245</td></tr>
	<tr><th scope=row>Angulo</th><td>1          </td><td> 3.0625    </td><td> 3.0625    </td><td> 1.25641026</td><td>0.294848959</td></tr>
	<tr><th scope=row>Rapidez:Espesor</th><td>1          </td><td> 7.5625    </td><td> 7.5625    </td><td> 3.10256410</td><td>0.116197074</td></tr>
	<tr><th scope=row>Rapidez:Angulo</th><td>1          </td><td> 0.0625    </td><td> 0.0625    </td><td> 0.02564103</td><td>0.876749464</td></tr>
	<tr><th scope=row>Espesor:Angulo</th><td>1          </td><td> 1.5625    </td><td> 1.5625    </td><td> 0.64102564</td><td>0.446462920</td></tr>
	<tr><th scope=row>Rapidez:Espesor:Angulo</th><td>1          </td><td> 5.0625    </td><td> 5.0625    </td><td> 2.07692308</td><td>0.187512262</td></tr>
	<tr><th scope=row>Residuals</th><td>8          </td><td>19.5000    </td><td> 2.4375    </td><td>         NA</td><td>         NA</td></tr>
</tbody>
</table>



De aquí podemos observar que tomando un $$\alpha = 0.05$$ las interacciones no tienen un gran peso sobre el modelo, y que, por el contrario, el factor rapidez tiene una gran importancia dentro de este; todo esto se puede verificar visualmente observando los gráficos de caja. 


```R
par(mfrow=c(2,2))
plot(model)
```

    hat values (leverages) are all = 0.5
     and there are no factor predictors; no plot no. 5



    
![png](/Blog/assets/images/posts/output_17_1.png)
    


### Rediseñando modelo


```R
model_red = lm(Rugosidad ~ Rapidez + Espesor + Angulo,data = df)

# Imprimiendo resumen del modelo
summary(model_red)
```


    
    Call:
    lm(formula = Rugosidad ~ Rapidez + Espesor + Angulo, data = df)
    
    Residuals:
       Min     1Q Median     3Q    Max 
    -2.625 -1.125  0.250  1.062  2.000 
    
    Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept)   8.1250     0.8385   9.690 5.03e-07 ***
    Rapidez30     3.3750     0.8385   4.025  0.00168 ** 
    Espesor40     1.6250     0.8385   1.938  0.07652 .  
    Angulo25      0.8750     0.8385   1.043  0.31728    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 1.677 on 12 degrees of freedom
    Multiple R-squared:  0.6369,	Adjusted R-squared:  0.5461 
    F-statistic: 7.015 on 3 and 12 DF,  p-value: 0.005579



### Imprimiendo análisis de varianza


```R
anova(model_red)
```


<table>
<thead><tr><th></th><th scope=col>Df</th><th scope=col>Sum Sq</th><th scope=col>Mean Sq</th><th scope=col>F value</th><th scope=col>Pr(&gt;F)</th></tr></thead>
<tbody>
	<tr><th scope=row>Rapidez</th><td> 1         </td><td>45.5625    </td><td>45.5625    </td><td>16.200000  </td><td>0.001684493</td></tr>
	<tr><th scope=row>Espesor</th><td> 1         </td><td>10.5625    </td><td>10.5625    </td><td> 3.755556  </td><td>0.076519738</td></tr>
	<tr><th scope=row>Angulo</th><td> 1         </td><td> 3.0625    </td><td> 3.0625    </td><td> 1.088889  </td><td>0.317284058</td></tr>
	<tr><th scope=row>Residuals</th><td>12         </td><td>33.7500    </td><td> 2.8125    </td><td>       NA  </td><td>         NA</td></tr>
</tbody>
</table>




```R
par(mfrow=c(2,2))
plot(model_red)
```

    hat values (leverages) are all = 0.25
     and there are no factor predictors; no plot no. 5



    
![png](/Blog/assets/images/posts/output_22_1.png)
    
---

El notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/36a34bf7174c9431b158e55956a976ef).