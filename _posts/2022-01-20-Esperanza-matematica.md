---
layout: post
title:  "La esperanza matemática y porqué no comprar un boleto de lotería"
date:   2022-01-20
author: Marco A. Carmona
---

Para dar una breve introducción a la definición de lo que es la [esperanza matemática](https://es.wikipedia.org/wiki/Esperanza_(matem%C3%A1tica)) o también llamada valor esperado, me gustaría comenzar con un ejemplo básico con el afán de dar a entender su uso e importancia de ésta.

Supongamos que tenemos un **dado de 6 caras**; a partir de aquí sabemos, por simple intuición si así lo queremos ver, que cada cara tiene una probabilidad de $$1 / 6$$ de que al ser lanzado el dado, esta salga escogida. Ahora, pensemos que entramos a un juego donde tras un tiro, si sale el valor 1, ganamos 1 peso; si sale el valor 2, ganamos 2 pesos; y así sucesivamente hasta llegar al valor 6, en donde nuestra ganancia sería de 6 pesos. Entonces la pregunta es la siguiente:

> **¿Cuál es la ganancia esperada que podemos obtener tras un tiro?**

Aquí es donde surge el concepto de **valor esperado o esperanza matemática**, el cual, en nuestro caso particular está dado de la siguiente manera:

$$
\left(\frac{1}{6}\right)(\\$$1)+\cdots +\left(\frac{1}{6}\right)(\\$$6) = \\$$3.5
$$

***¿Pero qué significa este valor de \\$$3.5 y cuál es su importancia?*** Pues resulta que a simple vista puede parecer inútil a la hora de jugar, ya que es imposible obtener dicha ganancia en un tiro. Necesitaríamos que el dado tuviera una cara con valor igual a $$3 \frac{1}{2}$$.

Sin embargo, supongamos que **el costo por lanzar el dado es de \\$$3**; con este dato, el valor esperado empieza a tomar forma y significado a nuestro favor, ya que, aún cuando no nos garantiza ganancias tras una tirada, al menos nos da una idea del riesgo que vale o no tomar.

## ¿Ahora ves el porqué nunca comprar un boleto de lotería?

> **De cualquier manera yo esperaría que ya lo supieras desde antes.**

En caso de no conocer el porqué, veamos rápidamente un caso especial del famoso [Sorteo Gordo de Navidad de la Lotería Nacional de México](http://www.lotenal.gob.mx/Comercial/gordo.html).

En este sorteo de lotería, participan 80000 boletos, numerados del 0 al 80000; aquí deducimos que la probabilidad de cada boleto de ser el ganador es de $$\frac{1}{80000}$$; además, de acuerdo con la página oficial de la Lotería Nacional de México, cada boleto tiene un costo de \\$$120 y la ganancia es de \\$$2,550,000 en caso de salir ganador.

Dicho lo anterior, nuestro valor esperado sería el siguiente:

$$
\left(\frac{1}{80000}\right)(\\$$2,550,000)=\\$$31.875
$$

Lo que nos lleva a deducir que se tiene un indicador de pérdida en comparación con los \\$$120 que cuesta cada boleto.