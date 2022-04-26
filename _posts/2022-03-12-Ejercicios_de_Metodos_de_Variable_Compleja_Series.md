---
layout: post
title:  "Ejercicios de Métodos de Variable Compleja. Series"
date:   2022-03-12
author: Marco A. Carmona
---

- Expandir $$\frac{z-1}{3-z}$$ en serie de Taylor alrededor de $$z_0=1$$ y dar el radio de convergencia.

Recordemos que una serie de Taylor, por definición está dada por

$$
f(z)=f(z_0)+\sum_{n=1}^{\infty}\frac{f^{(n)}(z_0)}{n!}(z-z_0)^{n}
$$

En donde, para nuestro caso en particular, $$z_0 = 1$$. Además, otro aspecto muy importante que debemos considerar es la definición de serie geométrica, la cual queda expresada de la siguiente manera:

$$
\sum_{n=0}^{\infty}z^{n}=\frac{1}{1-z}
$$

Entonces, comencemos a descomponer en fracciones parciales nuestra expresión principal, lo cual lo haremos utilizando Sympy, una librería de Python.


```python
from sympy import *

# Definiendo nuestra variable z
z = Symbol('z')

# Descomponiendo en fracciones parciales
apart((z-1) / (3- z))
```




$$\displaystyle -1 - \frac{2}{z - 3}$$



Así podemos decir que siendo $$f(z) = \frac{z-1}{3-z}$$, entonces:

$$
\begin{split}
f(z) & = -1 - \frac{2}{z-3}\\
& = -1-2\cdot\frac{1}{(z-1)-2}\\
& = -1+2\cdot\frac{1}{2-(z-1)}\\
& = -1+\frac{1}{1-\frac{z-1}{2}}\\
& = -1+\sum_{n=0}^{\infty}\left(\frac{z-1}{2}\right)^{n}\\
& = \sum_{n=1}^{\infty}\left(\frac{z-1}{2}\right)^{n}
\end{split}
$$

$$

$$
\boxed{\therefore\ \frac{z-1}{3-z} = \sum_{n=1}^{\infty}\left(\frac{z-1}{2}\right)^{n}}\quad z_0=1
$$

Finalmente; nos falta dar su radio de convergencia, el cual está dado por

$$
R = \frac{1}{l}
$$

donde

$$
l=\lim_{n=\infty}\left|\frac{a_{n+1}}{a_{n}}\right|
$$

Tomando a $$a_{n} = \frac{1}{2^{n}}$$ para nuestro caso particular, entonces:

$$
l=\lim_{n=\infty}\left|\frac{\frac{1}{2^{n+1}}}{\frac{1}{2^{n}}}\right|=\lim_{n=\infty}\left|\frac{2^{n}}{2^{n+1}}\right|=\frac{1}{2}
$$

$$
\boxed{\therefore\ R = 2}
$$

---

- Hallar las series de Laurent asociadas a las siguientes funciones en los dominios especificados:

a) $$f(z)=\frac{1}{z(z-1)}$$ para $$0<\|z-1\|<1$$ y $$\|z-1\|>1$$

Prestemos mucha atención a cada uno de los intervalos que se nos brinda, ya que aquí podemos deducir que nuestro $$z_0 = 1$$, entonces, en pocas palabras lo que buscamos son expresiones en que contengan al término $$z-1$$; ahora, por otro lado, sabemos que las series de Laurent están dadas de la siguiente forma, por definición:

$$
f(z)=\sum_{n=0}^{\infty}a_n(z-z_0)^{n}+\sum_{n=0}^{\infty}b_n(z-z_0)^{-n}
$$

donde

$$
a_n = \frac{1}{2\pi i}\int_{\mathcal{C_{1}}}\frac{f(w)}{(w-z_0)^{n+1}}dw
$$

$$
b_n = \frac{1}{2\pi i}\int_{\mathcal{C_{2}}}\frac{f(w)}{(w-z_0)^{n+1}}dw
$$

Así, al igual que en el ejercicio anterior, comenzaremos descomponiendo en fracciones parciales nuestra expresión principal:


```python
from sympy import *

# Definiendo nuestra variable z
z = Symbol('z')

# Descomponiendo en fracciones parciales
apart(1/ (z * (z-1)))
```




$$\displaystyle \frac{1}{z - 1} - \frac{1}{z}$$



Entonces, tomando a $$f(z)=\frac{1}{z(z-1)}$$:

$$
f(z) = \frac{1}{z-1}-\frac{1}{z}
$$

De aquí logramos ver que ya contamos con nuestro primer término que contiene a $$z-1$$, por lo que nos queda trabajar únicamente con $$\frac{1}{z}$$.

$$
\begin{split}
f(z) & = \frac{1}{z-1}-\frac{1}{z}\\
& = \frac{1}{z-1}-\frac{1}{(z-1)+1}
\end{split}
$$

Aquí presta mucha atención, ya que son dos las desigualdades que tenemos, la primera de ellas que me indica que $$\|z-1\|<1$$ y la segunda que nos indica que $$\frac{1}{\|z-1\|}<1$$, y es aquí donde nuevamente te pido que recuerdes la expresión dada para las series geométricas que te mostré con anterioridad.

Así, comencemos a trabajar con nuestra primera desigualdad $$\|z-1\|<1$$, la cual nos lleva a lo siguiente:

$$
\begin{split}
f(z) & = \frac{1}{z-1}-\frac{1}{(z-1)+1}\\
& = \frac{1}{z-1}-\frac{1}{1-[-(z-1)]}\\
& = \frac{1}{z-1}-\sum_{n=0}^{\infty}(-(z-1))^{n}\\
& = \frac{1}{z-1}-\sum_{n=0}^{\infty}(-1)^{n}(z-1)^{n}\\
\end{split}
$$

Ahora pasemos con nuestra segunda desigualdad $$\frac{1}{\|z-1\|}<1$$:

$$
\begin{split}
f(z) & = \frac{1}{z-1}-\frac{1}{(z-1)+1}\\
& = \frac{1}{z-1}-\frac{1}{(z-1)(1 - \left[-\frac{1}{z-1}\right])}\\
& = \frac{1}{z-1}-\frac{1}{z-1}\cdot\frac{1}{1 - \left[-\frac{1}{z-1}\right]}\\
& = \frac{1}{z-1}-\frac{1}{z-1}\sum_{n=0}^{\infty}\left(-\frac{1}{z-1}\right)^{n}\\
& = \frac{1}{z-1}-\left(\frac{1}{z-1}-\frac{1}{(z-1)^2}+\frac{1}{(z-1)^3}-\cdots\right)\\
& = \frac{1}{(z-1)^2}-\frac{1}{(z-1)^3}-\cdots\\
& = \sum_{n=2}^{\infty}\left(-\frac{1}{z-1}\right)^n
\end{split}
$$

$$
\boxed{\therefore\ \frac{1}{z(z-1)} = \frac{1}{z-1}-\sum_{n=0}^{\infty}(-1)^{n}(z-1)^{n}}\quad 0<|z-1|<1
$$

$$
\boxed{\therefore\ \frac{1}{z(z-1)} = \sum_{n=2}^{\infty}(-1)^{n}(z-1)^{-n}}\quad |z-1|>1
$$

b) $$f(z)=\frac{1}{z(4-z)^2}$$ para $$0<\|z\|<4$$

Para este caso, $$z_0 = 0$$ y $$\|\frac{z}{4}\|<1$$, entonces:

$$
\begin{split}
f(z) & = \frac{1}{z(4-z)^2}\\
& = \frac{1}{z}\frac{1}{(4-z)^2}\\
& = \frac{1}{z}\frac{1}{[4(1-\frac{z}{4})]^2}\\
& = \frac{1}{16z}\frac{1}{(1-\frac{z}{4})^2}\\
\end{split}
$$

Ahora, consideremos lo siguiente, para poder encontrar la serie infinita de $$\frac{1}{(1-\frac{z}{4})^2}$$:

$$
\frac{1}{1-\frac{z}{4}}=\sum_{n=0}^{\infty}\left(\frac{z}{4}\right)^{n}
$$

$$
\begin{split}
\Rightarrow \frac{d}{dz}\frac{1}{1-\frac{z}{4}} & =\frac{d}{dz}\sum_{n=0}^{\infty}\left(\frac{z}{4}\right)^{n}\\
& =\sum_{n=0}^{\infty}\frac{d}{dz}\left(\frac{z}{4}\right)^{n}\\
& =\sum_{n=0}^{\infty}\frac{d}{dz}\frac{z^{n}}{4^{n}}\\
& =\sum_{n=0}^{\infty}\frac{1}{4^{n}}\frac{d}{dz}z^{n}\\
& =\sum_{n=0}^{\infty}\frac{n}{4^{n}}z^{n-1}\\
\end{split}
$$

Y de antemano sabemos que $$\frac{d}{dz}\frac{1}{1-\frac{z}{4}}=\frac{1}{4(1-\frac{z}{4})^2}$$, entonces:

$$
\begin{split}
\Rightarrow f(z) & = \frac{1}{16z}\frac{1}{(1-\frac{z}{4})^2}\\
& = \frac{1}{4z}\frac{1}{4(1-\frac{z}{4})^2}\\
& = \frac{1}{4z}\sum_{n=0}^{\infty}\frac{n}{4^{n}}z^{n-1}\\
& = \sum_{n=0}^{\infty}\frac{n}{4^{n+1}}z^{n-2}\\
\end{split}
$$

$$
\boxed{\therefore\ \frac{1}{z(4-z)^2} = \sum_{n=0}^{\infty}\frac{n}{4^{n+1}}z^{n-2}}\quad 0<|z|<4
$$

c) $$f(z)=\frac{1}{z}\cos\frac{1}{z}$$ para $$z\neq 0$$

Ojo, presta mucha atención porque para este caso vamos a utilizar la expansión ya conocida del $$\cos$$, la cual de acuerdo con [Wikibooks](https://en.wikibooks.org/wiki/Trigonometry/Power_Series_for_Cosine_and_Sine) tenemos lo siguiente:

$$
\cos(x)=1-\frac{x^2}{2!}+\frac{x^4}{4!}-\cdots = \sum_{n=0}^{\infty}\frac{(-1)^{n}x^{2n}}{(2n)!}
$$

$$
\Rightarrow \cos\left(\frac{1}{z}\right)=\frac{1}{z^{0}}-\frac{1}{2!z^{2}}+\frac{1}{4!z^{4}}-\cdots = \sum_{n=0}^{\infty}\frac{(-1)^{n}z^{-2n}}{(2n)!}
$$

$$
\Rightarrow \frac{1}{z}\cos\left(\frac{1}{z}\right)=\frac{1}{z}-\frac{1}{2!z^{3}}+\frac{1}{4!z^{5}}-\cdots = \sum_{n=0}^{\infty}\frac{(-1)^{n}z^{-(2n+1)}}{(2n)!}
$$

$$
\boxed{\therefore\ \frac{1}{z}\cos\left(\frac{1}{z}\right) = \sum_{n=0}^{\infty}\frac{(-1)^{n}z^{-(2n+1)}}{(2n)!}}
$$

d) $$f(z)=\frac{z^2-2z+5}{(z-2)(z^2+1)}$$ para $$1<\|z\|<2$$

Por último, comencemos representando la expresión anterior en fracciones parciales utilizando Sympy.


```python
from sympy import *

# Definiendo nuestra variable z
z = Symbol('z')

# Descomponiendo en fracciones parciales
apart((z**2 - 2*z + 5) / ((z-2)* (z**2 + 1)))
```




$$\displaystyle - \frac{2}{z^{2} + 1} + \frac{1}{z - 2}$$



Con esto podemos decir lo siguiente:

$$
f(z)=\frac{z^2-2z+5}{(z-2)(z^2+1)}=-\frac{2}{z^2+1}+\frac{1}{z-2}
$$

Ahora veamos que de nuestra inecuación $$1<\|z\|<2$$ podemos desprender lo siguiente:

$$
\left|\frac{1}{z}\right|<1\quad\text{y}\quad|z|<2
$$

Con esto, vemos que en ambos casos vamos a tomar a $$z_0=0$$, pero consideraremos distintos intervalos a la hora de comenzar a encontrar sus respectivas series.

Entonces tenemos lo siguiente:

$$
\begin{split}
f(z) & =-\frac{2}{z^2+1}+\frac{1}{z-2}\\
& =-\frac{2}{z^2(1-[-1/z^2])}-\frac{1}{2-z}\\
& =-\frac{2}{z^2}\frac{1}{1-[-1/z^2]}-\frac{1}{2}\frac{1}{1-(z/2)}\\
& =-\frac{2}{z^2}\sum_{n=0}^{\infty}\left(-\frac{1}{z^2}\right)^{n}-\frac{1}{2}\sum_{n=0}^{\infty}\left(\frac{z}{2}\right)^{n}\\
& =-\sum_{n=0}^{\infty}\frac{2(-1)^{n}}{z^{2n+2}}-\sum_{n=0}^{\infty}\frac{z^{n}}{2^{n+1}}\\
& =-\sum_{n=0}^{\infty}\left(\frac{2(-1)^{n}}{z^{2n+2}}+\frac{z^{n}}{2^{n+1}}\right)\\
& =-\sum_{n=0}^{\infty}\frac{2^{n+2}(-1)^{n}+z^{3n+2}}{2^{n+1}z^{2n+2}}
\\
\end{split}
$$

$$
\boxed{\therefore\ \frac{z^2-2z+5}{(z-2)(z^2+1)} = -\sum_{n=0}^{\infty}\frac{2^{n+2}(-1)^{n}+z^{3n+2}}{2^{n+1}z^{2n+2}}}\quad 1<|z|<2
$$

---
    
El notebook de este post, los puedes encontrar [aquí](https://gist.github.com/marcocarmonaga/7777bd9d7c6bd89230d7d3e2065a7e6a).