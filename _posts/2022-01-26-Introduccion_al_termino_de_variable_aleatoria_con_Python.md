---
layout: post
title:  "Introducción al término de variable aleatoria con Python"
date:   2022-01-26
author: Marco A. Carmona
---

En este post se abordará el tema de variable aleatoria desde una perspectiva problemática y aplicada dentro de la probabilidad con la ayuda de Python. 

Para ello consideremos un problema interesante y básico donde lanzamos dos dados, y asumiendo que cada lanzamiento es independiente, significa que la salida de uno no influye en la salida del otro. Entonces nuestra primera pregunta a resolver es... 

> ¿Cuál es el conjunto correspondiente para este ejemplo?

Para dar respuesto a esto, vamos a ejemplificarlo con Python. Por lo que comenzaremos representando el dado de 6 caras dentro de este ambiente. Eso lo podemos hacer fácilmente de la siguiente forma:


```python
carasDado = [1,2,3,4,5,6]
print(f'Caras del dado: {carasDado}')
```

    Caras del dado: [1, 2, 3, 4, 5, 6]


Con lo anterior hemos logrado representar correctamente las 6 caras de un dado.   

Posterior a lo anterior, importemos el módulo itertools, ya que este módulo nos servirá para poder representar interactivamente la cantidad de combinaciones posibles. O lo que, en término más técnicos, indicará el conjunto correspondiente al problema. 


```python
import itertools as it
```

Una vez importado, procedemos a conocer dinámicamente el conjunto que se puede formar con ambos dados de la siguiente manera:


```python
conjunto = list(it.product(carasDado,repeat=2))
print(conjunto)
```

    [(1, 1), (1, 2), (1, 3), (1, 4), (1, 5), (1, 6), (2, 1), (2, 2), (2, 3), (2, 4), (2, 5), (2, 6), (3, 1), (3, 2), (3, 3), (3, 4), (3, 5), (3, 6), (4, 1), (4, 2), (4, 3), (4, 4), (4, 5), (4, 6), (5, 1), (5, 2), (5, 3), (5, 4), (5, 5), (5, 6), (6, 1), (6, 2), (6, 3), (6, 4), (6, 5), (6, 6)]


De esta manera obtenemos todas las combinaciones posibles, las cuales teóricamente estarían dada por $$6^2$$, sin embargo, si tu interés está en conocerlas por medio de Python, basta con hacer uso de la función `len` de la siguiente manera: 


```python
numElementos = len(conjunto)
print(f'El número de elementos dentro del conjunto es: {numElementos}')
```

    El número de elementos dentro del conjunto es: 36


Por lo que podemos decir que existen 36 posibles maneras de que los dados resulten tras los dos lanzamientos, mismo resultado que obtendríamos de resolver $$6^2$$. Lo cual nos lleva a decir que cada una de estas combinaciones tiene una probabilidad de $$\frac{1}{36}$$. 

Sin embargo, vamos a una pregunta aún más interesante:   

> ¿Cuál es la probabilidad de que la suma de los dados sea 7? 

Para ello vamos a crear en Python un diccionario ilustrando cada par de números con su suma. 


```python
d={(i,j):i+j for i in carasDado for j in carasDado}
print(d)
```

    {(1, 1): 2, (1, 2): 3, (1, 3): 4, (1, 4): 5, (1, 5): 6, (1, 6): 7, (2, 1): 3, (2, 2): 4, (2, 3): 5, (2, 4): 6, (2, 5): 7, (2, 6): 8, (3, 1): 4, (3, 2): 5, (3, 3): 6, (3, 4): 7, (3, 5): 8, (3, 6): 9, (4, 1): 5, (4, 2): 6, (4, 3): 7, (4, 4): 8, (4, 5): 9, (4, 6): 10, (5, 1): 6, (5, 2): 7, (5, 3): 8, (5, 4): 9, (5, 5): 10, (5, 6): 11, (6, 1): 7, (6, 2): 8, (6, 3): 9, (6, 4): 10, (6, 5): 11, (6, 6): 12}


Una vez hecho lo anterior, agrupemos los datos, tomando como índice principal los posibles valores resultantes de las sumas (2,3,4,5,6,7,8,9,10,11,12). 

Aquí surge un paso muy interesante, pues para realizar esto haremos importaremos la función `defaultdict` del módulo `collections`, y le agregaremos como parámetro `list` que nos permitirá agrupar una secuencia de pares clave-valor en un diccionario de listas. 


```python
from collections import defaultdict
dinv = defaultdict(list)
for i, j in d.items():
    dinv[j].append(i)

dinv
```




    defaultdict(list,
                {2: [(1, 1)],
                 3: [(1, 2), (2, 1)],
                 4: [(1, 3), (2, 2), (3, 1)],
                 5: [(1, 4), (2, 3), (3, 2), (4, 1)],
                 6: [(1, 5), (2, 4), (3, 3), (4, 2), (5, 1)],
                 7: [(1, 6), (2, 5), (3, 4), (4, 3), (5, 2), (6, 1)],
                 8: [(2, 6), (3, 5), (4, 4), (5, 3), (6, 2)],
                 9: [(3, 6), (4, 5), (5, 4), (6, 3)],
                 10: [(4, 6), (5, 5), (6, 4)],
                 11: [(5, 6), (6, 5)],
                 12: [(6, 6)]})



Con esto ya podemos comenzar a ver y asemejar la cantidad de posibilidades que tiene cada número del 2 al 12 de que la suma de los dados sea dicho número.

---
> **Pregunta para el lector**
>
> *¿Por qué no se cuenta el número 1?*
---

Así con lo anterior podemos decir que las posibilidades de que caiga el número 7 están dadas por:


```python
dinv[7]
```




    [(1, 6), (2, 5), (3, 4), (4, 3), (5, 2), (6, 1)]



Sin embargo, si lo que nosotros buscamos es conocer de manera numérica su probabilidad, también es muy sencillo realizarlo con Python. 

Para ello creemos nuevamente un diccionario en donde tengamos como índices los números del 2 al 12 y sus respectivas probabilidades. Para ello es importante recordar que cada posible combinación del lanzamiento de ambos dados, al ser cada lanzamiento independiente el uno del otro, entonces la probabilidad de cada combinación es de $$1 / 36$$. 

Entonces con lo anterior, podemos realizar lo siguiente: 


```python
probabilidades = {i:round(len(j) / 36,5) for i, j in dinv.items()}
probabilidades
```




    {2: 0.02778,
     3: 0.05556,
     4: 0.08333,
     5: 0.11111,
     6: 0.13889,
     7: 0.16667,
     8: 0.13889,
     9: 0.11111,
     10: 0.08333,
     11: 0.05556,
     12: 0.02778}



De esta manera podemos observar que la probabilidad que la suma de los dados sea el valor 7 es de 0.16667, la cuál es la mayor con respecto a los demás valores. 

Y así es como podemos dar utilidad a un lenguaje tan poderoso como lo es Python, dentro de aplicaciones básicas de la probabilidad. 

--- 

# Ejercicio para el lector 

Con este marco, podemos hacer otras preguntas: 

> ¿Cuál es la probabilidad de que la mitad del producto de tres dados supere su suma? 

## *Pista* 


```python
d={(i,j,k):((i*j*k)/2>i+j+k) for i in range(1,7) for j in range(1,7) for k in range(1,7)}
```

La solución a este ejercicio y la misma notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/1279e9a5ccaba21c40c629ce68ec3f5e).

