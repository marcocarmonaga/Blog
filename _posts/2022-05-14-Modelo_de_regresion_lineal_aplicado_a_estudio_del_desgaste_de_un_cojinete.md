---
layout: post
title:  "Modelo de regresión lineal aplicado al estudio del desgaste de un cojinete dada su relación con la viscosidad del aceite y la carga"
date:   2022-05-14
author: Marco A. Carmona
---

Consideremos un estudio sobre el desgaste de un cojinete Y y su relación con X1= viscosidad del aceite y X2 = carga. Los datos del experimento se muestran en la siguiente tabla: 

|Y|X1|X2|
|:----:|:----:|:----:|
|193 | 1.6 | 851 |
|230 | 15.5 | 816 |
|172 | 22 | 1058 |
|91  |43 | 1201 |
|113 | 33 | 1357 |
|125|  40 | 1115 |

### Importemos las librerías que vamos a utilizar dentro de R


```R
library("scatterplot3d")
```

### Creemos la tabla anterior dentro de nuestro entorno


```R
Y <- c(193,230,172,91,113,125)
X1 <- c(1.6,15.5,22,43,33,40)
X2 <- c(851,816,1058,1201,1357,1115)

df <- data.frame("Y" = Y,"X1" = X1,"X2" = X2); df
```


<table class="dataframe">
<caption>A data.frame: 6 × 3</caption>
<thead>
	<tr><th scope=col>Y</th><th scope=col>X1</th><th scope=col>X2</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>193</td><td> 1.6</td><td> 851</td></tr>
	<tr><td>230</td><td>15.5</td><td> 816</td></tr>
	<tr><td>172</td><td>22.0</td><td>1058</td></tr>
	<tr><td> 91</td><td>43.0</td><td>1201</td></tr>
	<tr><td>113</td><td>33.0</td><td>1357</td></tr>
	<tr><td>125</td><td>40.0</td><td>1115</td></tr>
</tbody>
</table>



### Veamos su gráfica de dispersión


```R
scatterplot3d(df)
```


    
![png](/Blog/assets/images/posts/output_6_0.png)
    


Consideremos ajustar estos datos a un modelo de regresión lineal múltiple


```R
model <- lm(Y ~ X1 + X2, data=df)
summary(model)
```


    
    Call:
    lm(formula = Y ~ X1 + X2, data = df)
    
    Residuals:
          1       2       3       4       5       6 
    -24.987  24.307  11.820 -20.460  12.830  -3.511 
    
    Coefficients:
                 Estimate Std. Error t value Pr(>|t|)  
    (Intercept) 350.99427   74.75307   4.695   0.0183 *
    X1           -1.27199    1.16914  -1.088   0.3562  
    X2           -0.15390    0.08953  -1.719   0.1841  
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 25.5 on 3 degrees of freedom
    Multiple R-squared:  0.8618,	Adjusted R-squared:  0.7696 
    F-statistic: 9.353 on 2 and 3 DF,  p-value: 0.05138



### Gráfica de residuos, probabilidad normal


```R
par(mfrow=c(2,2))
plot(model)
```


    
![png](/Blog/assets/images/posts/output_10_0.png)
    


### Su respectiva tabla de anova


```R
anova(model)
```


<table class="dataframe">
<caption>A anova: 3 × 5</caption>
<thead>
	<tr><th></th><th scope=col>Df</th><th scope=col>Sum Sq</th><th scope=col>Mean Sq</th><th scope=col>F value</th><th scope=col>Pr(&gt;F)</th></tr>
	<tr><th></th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>X1</th><td>1</td><td>10240.369</td><td>10240.3691</td><td>15.751003</td><td>0.02858872</td></tr>
	<tr><th scope=row>X2</th><td>1</td><td> 1921.209</td><td> 1921.2087</td><td> 2.955066</td><td>0.18410102</td></tr>
	<tr><th scope=row>Residuals</th><td>3</td><td> 1950.422</td><td>  650.1407</td><td>       NA</td><td>        NA</td></tr>
</tbody>
</table>



### Ecuación de regresión

$$
\boxed{Y = 350.99427 -1.27199*X1-0.15390*X2}
$$

---

El notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/59abf0e8ab40a26a6c8437ef5bf38dc7).