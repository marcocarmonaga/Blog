---
layout: post
title:  "Aprendiendo a graficar una función de densidad acumulada para una región dada, con Python"
date:   2022-02-11
author: Marco A. Carmona
---

En este breve post aprenderás rápidamente cómo graficar la función de distribución acumulada de una distribución normal estandarizada, aunque es importante decir que el siguiente proceso funciona perfectamente para otro tipo de distribuciones como la distribución Z inversa, distribución "t" de Student, Ji-cuadrada, entre otras. 

De manera general, se sabe que la función de distribución acumulada de una distribución calcula la probabilidad acumulada de un valor dado $$Z$$. O lo que en otras palabras significa que se busca el valor del área bajo la curva entre dos puntos, o de manera más particular en este ejemplo, desde menos infinito hasta un punto $$Z$$. 

Y para esto nos basaremos en el siguiente ejemplo:

> Obtener la probabilidad de que Z obtenga los siguientes valores

$$
\mathbb{P}(Z \leq 1.17)
$$

Y al igual que siempre, comenzaremos importando los módulos con los que trabajaremos en Python. La parte más importante y a prestar atención es el módulo de `scipy.stats`, ya que este nos brindará la distribución normal.


```python
import numpy as np
from scipy.stats import norm
import matplotlib.pyplot as plt
```

Y hacemos esto debido a que no buscamos adentrarnos al tema desde el punto matemático, sino al punto de poder lograr graficar el área bajo la desde menos infinito hasta el punto $$Z=1.17$$ y obtener su valor. 

Esto último es muy sencillo con el apartado `norm` de `scipy.stats`, ya que automáticamente nos brinda la distribución normal **estandarizada**, la cual trae consigo un método para calcular el valor de la función de distribución acumulada en un punto dado, que para nuestro caso es 1.17. 


```python
prob = norm.cdf(1.17)
print(f'P(Z =< 1.17) = {round(prob,5)}')
```

    P(Z =< 1.17) = 0.879


Sin embargo, esto únicamente es encontrar el valor numérico, ¿de qué manera lo podemos graficar? Aquí es donde comienza el proceso de graficación. 

Iniciaremos definiendo nuestro intervalo del eje X con el que trabajaremos, el cual estará dado por los valores de Z para cuando la probabilidad es 0.001 y 0.999. 

Es muy sencillo decir por qué no podemos escribir menos infinito hasta infinito, y esto es porque las computadoras no tienen memoria infinita para almacenar infinitos valores. 


```python
X = np.linspace(norm.ppf(0.001),norm.ppf(0.999),num=1000)
```

Por consiguiente, definiremos otro intervalo más para el valor de Z para cuando la probabilidad es 0.001 y 1.17; ya que es hasta el valor que queremos conocer. 


```python
x = np.linspace(norm.ppf(0.001),norm.ppf(prob), num=1000)
```

Básicamente esta es la clave para poder mostrar el área bajo la curva, ya que lo demás lo haremos con ayuda de la librería Matplotlib. En seguida te muestro los pasos a seguir. 


```python
# 1. Creamos el recuadro de nuestra figura y e
# specificamos su tamaño
fig, ax = plt.subplots()
fig.set_size_inches(10,6.6)


# 2. Definimos el título y el nombre de nuestros ejes;
# además de poner sus límites
plt.title(r'Distribución acumulada para $$Z\leq 1.17$$', fontsize=20)
plt.ylim(0,0.45)
plt.xlim(X[0],X[999])
ax.set_xlabel(r'Valores de $$Z$$', fontsize=16)
ax.set_xticks([0,X[0],1.17,X[999]])
ax.set_ylabel(r'Probabilidades', fontsize=16)

# 3. Graficamos nuestra distribución normal estandarizada
ax.plot(X,norm.pdf(X))

# 4. Graficamos el área con ayuda del método fill_between
ax.fill_between(x,norm.pdf(x))

# 5. Anexamos una etiqueta extra a nuestra figura
ax.text(-0.4,0.15,f'{round(prob,5)}',c='white',fontsize=20)

# 6. Mostramos la figura
plt.show()
```


    
![png](/Blog/assets/images/posts/output_9_2.png)
    


Y es así como rápidamente podemos graficar el área indicada en nuestro ejercicio.

---
    
El notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/3b3736193254e279718756f08e8ccc76).