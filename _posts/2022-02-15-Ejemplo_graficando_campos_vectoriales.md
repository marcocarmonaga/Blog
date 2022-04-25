---
layout: post
title:  "Graficando campos vectoriales con ayuda de Numpy y Matplotlib, en menos de 10 líneas de código."
date:   2022-02-15
author: Marco A. Carmona
---

Ilustrar campos vectoriales a la hora de hacer un análisis visual de cualquier tipo de función, siempre es de gran ayuda para poder conocer su comportamiento y deducir una que otra propiedad de esta misma. Sin embargo, no siempre es trivial visualizar y/o bosquejarlo; es por ello que muchas ocasiones se debe recurrir a herramientas computacionales para su graficación. 

Es por tal motivo que en este tutorial te mostraré una manera muy práctica y rápida de poder visualizar campos vectoriales con ayuda de Python, y librerías como Numpy y Matplotlib. 

Y esto se realizará fijándonos en dos ejemplos, aunque es importante decir que se puede aplicar a cualquier otro tipo de campo vectorial. 

Dichos ejemplos están dados por:

- a) 

$$ 
G(x,y) = y\hat{i}-x\hat{j} 
$$ 

- b) 

$$ 
E(x,y) = \left(\frac{x}{x^2+y^2},-\frac{y}{x^2+y^2}\right) 
$$ 

Ya con esto, podemos comenzar a graficar; y para esto, comenzaremos importando las librerías a utilizar, Numpy y Matplotlib, dentro de Python. 


```python
import numpy as np
import matplotlib.pyplot as plt
```

Una vez hecho lo anterior, vamos a comenzar creando una "malla" dentro de Python, considerando todas las $$x$$ y todas las $$y$$ sobre las que queremos conocer el campo vectorial. 

Dicha maya es de fácil creación gracias al método que nos ofrece Numpy: `meshgrid`.   

Tanto en las $$x$$ y las $$y$$ nos vamos a fijar únicamente en el intervalo de `-10` a `10`. Sin embargo, puede ser modificado a tu consideración. El número `256` dentro de la siguiente línea, lo único que indica es el número de divisiones que se le va a hacer al intervalo anterior. 

> **Nota:** El número de divisiones debe de ser el mismo para ambos intervalos, aun cuando los intervalos no sean no sean los mismos. 


```python
# Creación de intervalos x, y
x = np.linspace(-10, 10, 256)
y = np.linspace(-10, 10, 256)

# Creación de mallas
X, Y = np.meshgrid(x, y)
```

Y una vez con esto, fijemos nos en nuestro primer inciso *a)*. 

$$  
G(x,y) = y\hat{i}-x\hat{j}  
$$  

Del cuál vamos a separar sus componentes en variables `U` y `V`, dentro de Python, de la siguiente manera: 

$$ 
U = y\quad\wedge\quad V = -x 
$$


```python
U = Y
V = -X
```

Y prácticamente esto es todo lo necesario para poder graficar con Matplotlib; la cual nos provee de dos métodos, `streamplot` y `quiver` para poder hacer gráficos de este estilo, sin embargo, por esta ocasión vamos a utilizar la primera. 

Pero antes es importante hacer mención rápida de lo que hacer cada línea, para un mejor entendimiento. 

Por ejemplo, el siguiente fragmento de código, lo único que hace es crear el plano sobre el que se va a trabajar y definir las medidas de este en pulgadas. 


```python
# Crea el plano
fig, ax = plt.subplots()

# Define las medidas en pulgadas
fig.set_size_inches(10,6.6)

# Muestra en pantalla
plt.show()
```


    
![png](/Blog/assets/images/posts/output_7_0.png)
    


Básicamente esto es lo necesario para todo tipo de gráfico con Matplotlib, sin embargo, nos hace falta lo más importante: el campo vectorial. 

Este se realizar de únicamente anexando el método `streamplot` de la siguiente forma. 


```python
fig, ax = plt.subplots()
fig.set_size_inches(10,6.6)
ax.streamplot(X,Y,U,V)
plt.show()
```


    
![png](/Blog/assets/images/posts/output_9_1.png)
    


¡Y así de rápido se logra realizar un campo vectorial! Veamos el siguiente caso *b)*.

$$ 
E(x,y) = \left(\frac{x}{x^2+y^2},-\frac{y}{x^2+y^2}\right) 
$$

Para el cual unicamente nos bastará con definir `U` y `V`, al igual que el caso anterior.


```python
U = X / (X**2 + Y**2)
V = -Y / (X**2 + Y**2)
```

Así, creando nuestro gráfico con Matplotlib quedaría dado de la siguiente manera:


```python
fig, ax = plt.subplots()
fig.set_size_inches(10,6.6)
ax.streamplot(X,Y,U,V)
plt.show()
```


    
![png](/Blog/assets/images/posts/output_13_0.png)
    


¡Felicidades! Ahora conoces una manera rápida de poder graficar campos vectoriales con Python.

---
    
El notebook de este post, los puedes encontrar [aquí](https://colab.research.google.com/drive/1pJrv5PCsJzO1DuMCwW-JnduL03PWF5Yu?usp=sharing).