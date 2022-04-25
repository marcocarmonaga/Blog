---
layout: post
title:  "Ejercicios de Métodos de Variable Compleja. Residuos y polos"
categories: [ Métodos Matemáticos, Avanzado ]
image: assets/images/banners/Residuos_y_polos.png
author: marco
usemathjax: true
---

- Clasificar las singularidades de las siguientes funciones y calcular los residuos cuando sea posible.

Al momento de clasificar singularidades nos será de mucha ayuda considerar cada uno de los puntos $z_{i}$ donde se indetermina el denominador de nuestra función y desarrollar su serie de potencias alrededor de dicho punto $z_{i}$, con el afán de identificar su [parte principal](https://en.wikipedia.org/wiki/Principal_part) de dicha serie.

Para esto únicamente basta con recordar cómo se expresa una función $f(z)$ en su serie de Laurent.

$$
f(z)=\sum_{n=0}^{\infty}a_n(z-z_0)^{n}+\sum_{n=0}^{\infty}b_n(z-z_0)^{-n}
$$

Donde nuestro residuo en $x_{i}$,$\text{Res}[f(z),z_{i}]$, va a estar dado por $b_{1}$ y $z_{i}$ será una singularidad de orden $n$, donde $n$ es el índice más grande de la parte principal. 

a) $f(z)=\frac{1}{z(z-1)}$

Dicho lo anterior, para este caso en particular observamos que se indetermina en $z_0 = 0$ y $z_{1} = 1$, entonces, desarrollando nuestra serie de potencias alrededor de $z_0$ tenemos lo siguiente:

$$
\begin{split}
\Rightarrow f(z) & =\frac{1}{z}\cdot \frac{1}{z-1}\\
& =-\frac{1}{z}\cdot \frac{1}{1-z}\\
& =-\frac{1}{z}\sum_{n=0}^{\infty}z^{n}\\
& =-\sum_{n=0}^{\infty}z^{n-1}\\
& = -\frac{1}{z}-\sum_{n=0}^{\infty}z^{n}
\end{split}
$$

Con lo que podemos concluir que $z_0=0$ es un polo simple con:

$$
\boxed{b_1 = \text{Res}[f(z),0] = -1}
$$

Por otro lado, para $z_1 = 1$ tenemos lo siguiente:

$$
\begin{split}
\Rightarrow f(z) & =\frac{1}{z-1}\cdot \frac{1}{z}\\
& = \frac{1}{z-1}\cdot \frac{1}{z-1+1}\\
& = \frac{1}{z-1}\cdot \frac{1}{1-(-(z-1))}\\
& = \frac{1}{z-1}\sum_{n=0}^{\infty}(-1)^{n}(z-1)^{n}\\
& = \sum_{n=0}^{\infty}(-1)^{n}(z-1)^{n-1}\\
& = \frac{1}{z-1}-\sum_{n=0}^{\infty}(-1)^{n}(z-1)^{n}\\
\end{split}
$$

Con lo que podemos concluir que $z_1=1$ es un polo simple con:

$$
\boxed{b_1 = \text{Res}[f(z),1] = 1}
$$

b) $f(z)=\frac{1}{z(4-z)^2}$

Para este caso podemos ver que tenemos dos indeterminaciones, $z_0=0$ y $z_1 = 4$, entonces, para $z_0$ tenemos los siguiente: 

$$
\begin{split}
\Rightarrow f(z) & =\frac{1}{z}\cdot \frac{1}{(4-z)^{2}}\\
& = \frac{1}{z}\cdot\frac{1}{16}\cdot \frac{1}{(1-\frac{z}{4})^{2}}\\
& = \frac{1}{z}\cdot\frac{1}{16}\sum_{n=0}^{\infty}\frac{n\cdot z^{n-1}}{4^{n-1}}\\
& = \sum_{n=0}^{\infty}\frac{n\cdot z^{n-2}}{4^{n+1}}\\
& = \frac{1}{4^{2}z}+\sum_{n=0}^{\infty}\frac{(n+2)\cdot z^{n}}{4^{n+3}}\\
\end{split}
$$

Con lo que podemos concluir que $z_0=0$ es un polo simple con:

$$
\boxed{b_1 = \text{Res}[f(z),0] = \frac{1}{16}}
$$

Por otro lado, para $z_1=4$ tenemos lo siguiente:

$$
\begin{split}
\Rightarrow f(z) & =\frac{1}{(4-z)^{2}}\cdot \frac{1}{z}\\
& =\frac{1}{(4-z)^{2}}\cdot \frac{1}{z-4+4}\\
& =\frac{1}{(4-z)^{2}}\cdot \frac{1}{4 - (-(z-4))}\\
& =\frac{1}{(4-z)^{2}}\cdot\frac{1}{4}\cdot \frac{1}{1 - (-\frac{z-4}{4})}\\
& =\frac{1}{(4-z)^{2}}\cdot\frac{1}{4}\sum_{n=0}^{\infty}\left(-\frac{z-4}{4}\right)^{n}\\
& = \sum_{n=0}^{\infty}\frac{(-1)^{n}\cdot (z-4)^{n-2}}{4^{n+1}}\\
& = \frac{1}{4(z-4)^{2}}-\frac{1}{4^2(z-4)}+\sum_{n=0}^{\infty}\frac{(-1)^{n}\cdot (z-4)^{n}}{4^{n+3}}
\end{split}
$$

Con lo que podemos concluir que $z_1=4$ es un polo doble con:

$$
\boxed{b_1 = \text{Res}[f(z),4] = -\frac{1}{16}}
$$

c) $f(z)=\frac{1}{z}\cos\frac{1}{z}$

Por otro lado, este ejemplo, a pesar de lucir complicado, su solución es tan trivial con el simple hecho de considerar la expansión en [serie de potencias](https://es.wikipedia.org/wiki/Coseno) de la función $\cos(z)$, la cual está dada de la siguiente manera:

$$
\cos z=\sum_{n=0}^{\infty}(-1)^{n}\frac{z^{2n}}{(2n)!}
$$

$$
\Rightarrow \cos\frac{1}{z}=\sum_{n=0}^{\infty}(-1)^{n}\frac{z^{-2n}}{(2n)!}
$$

$$
\begin{split}
\Rightarrow \frac{1}{z}\cos\frac{1}{z} & =z^{-1}\sum_{n=0}^{\infty}(-1)^{n}\frac{z^{-2n}}{(2n)!}\\
& =\sum_{n=0}^{\infty}(-1)^{n}\frac{z^{-(2n+1)}}{(2n)!}\\
\end{split}
$$

Con lo que podemos concluir que $z_0=0$ es una singularidad esencial con:

$$
\boxed{b_1 = \text{Res}[f(z),0] = 1}
$$

---
    
- Demostrar que
    
    $$
    \int_{\mathcal{C}}\frac{dz}{z^4+z^3-2z^2}=0
    $$
    
donde $\mathcal{C}:\|Z\|=4$.

Partamos notando lo siguiente:

$$
\Rightarrow \int_{\mathcal{C}}\frac{dz}{z^4+z^3-2z^2}=\int_{\mathcal{C}}\frac{dz}{z^2 (z + 2) (z - 1)}
$$

De aquí vemos que se tienen 3 singularidad, un polo doble en $z_0=0$, un polo simple en $z_1=-2$ y otro más en $z_{2}=1$, de esta manera, de acuerdo con el **Teorema del Residuo**, el cual no dice lo siguiente:

*Sea $\mathcal{D}$ un dominio simple conexo y $\mathcal{C}$ un contrno simple cerrado contenido en $\mathcal{D}$. Si $f$ es una función analítica en $\mathcal{C}$ y en el interior de $\mathcal{C}$ salvo en un número finito de singularidades aisladas $z_1$, $z_2$, ..., $z_{m}$, entonces:*

$$
\oint_{\mathcal{C}}f(z)dz=2\pi i\sum_{n=1}^{m}\text{Res}[f(z),z_{n}]
$$

Donde $\text{Res}[f(z),z_{n}]$ de forma general está dada por la siguiente expresión:

$$
\text{Res}[f(z),z_{n}]=\frac{1}{(n-1)!}\lim_{z\rightarrow z_{n}}\frac{d^{n-1}}{dz^{n-1}}[(z-z_{n})^{n}f(z)]
$$

Con esto debería bastarnos para comenzar a darle solución al ejercicio. Y para esto comenzaremos encontrando el residuo para $z_{0}=0$, el cual como mencionamos se trata de un polo doble, por lo que $n=2$

$$
\begin{split}
\Rightarrow \text{Res}[f(z),0] & =\frac{1}{1!}\lim_{z\rightarrow 0}\frac{d}{dz}\left[z^{2}\cdot \frac{1}{z^2 (z + 2) (z - 1)}\right]\\
& =\lim_{z\rightarrow 0}\frac{d}{dz}\left[\frac{1}{(z + 2) (z - 1)}\right]\\
\end{split}
$$

Derivando la expresión dentro de los corchetes tenemos lo siguiente:



```python
from sympy import *

# Definimos nuestra variable z
z = Symbol('z')

# Definimos f(z)
f_z = 1 / ((z + 2)* (z-1))

# Definimos nuestra segunda derivada
derivada = diff(f_z,z,1)

# Simplificamos la expresión de la derivada
simplify(derivada)
```




$\displaystyle \frac{- 2 z - 1}{z^{4} + 2 z^{3} - 3 z^{2} - 4 z + 4}$



$$
\begin{split}
\Rightarrow \text{Res}[f(z),0] & =\lim_{z\rightarrow 0}\frac{d}{dz}\left[\frac{1}{(z + 2) (z - 1)}\right]\\
& = \lim_{z\rightarrow 0}\frac{-2z-1}{z^4+2z^3-3z^2-4z+4}\\
& = -\frac{1}{4}
\end{split}
$$

Con ello podemos pasar a encontrar el valor del residuo cuando $z_{1}=-2$, el cual es un polo simple, por lo tanto, la expresión para encontrar el residuo en dicho punto se reduce a la siguiente:

$$
\begin{split}
\Rightarrow \text{Res}[f(z),-2] & =\lim_{z\rightarrow -2}\left[(z+2)\cdot \frac{1}{z^2 (z + 2) (z - 1)}\right]\\
& = \lim_{z\rightarrow -2}\left[\frac{1}{z^2(z - 1)}\right]\\
& = -\frac{1}{12}
\end{split}
$$

Finalmente podemos pasar a encontrar el valor del residuo cuando $z_{2}=1$, el cual de igual manera se trata de un polo simple.

$$
\begin{split}
\Rightarrow \text{Res}[f(z),1] & =\lim_{z\rightarrow 1}\left[(z-1)\cdot \frac{1}{z^2 (z + 2) (z - 1)}\right]\\
& =\lim_{z\rightarrow 1}\left[\frac{1}{z^2 (z + 2)}\right]\\
& = \frac{1}{3}
\end{split}
$$

Así, considerando el **Teorema del Residuo**, podemos llegar a lo siguiente:

$$
\Rightarrow \int_{\mathcal{C}}\frac{dz}{z^4+z^3-2z^2} = 2\pi i \left(-\frac{1}{4}-\frac{1}{12}+\frac{1}{3}\right)
$$

$$
\boxed{\therefore\ \int_{\mathcal{C}}\frac{dz}{z^4+z^3-2z^2}=0}
$$

---
    
- Calcular las siguientes integrales reales por el método de los residuos:
    
a) $\int_0^{2\pi}\frac{\sin^2\theta}{2+\cos\theta}d\theta$

Partamos considerando el cambio de variable dado por $z=e^{i\theta}$ donde $dz=ie^{i\theta}d\theta$, de esta manera, considerando a la función $\sin\theta$ y $\cos\theta$ en su equivalencia compleja, obtenemos los siguiente:

$$
\begin{split}
\int_0^{2\pi}\frac{\sin^2\theta}{2+\cos\theta}d\theta & = \int_0^{2\pi}\frac{\left(\frac{e^{i\theta}-e^{-i\theta}}{2}\right)^2}{2+\frac{e^{i\theta}+e^{-i\theta}}{2}}d\theta\\
& = \int_{\mathcal{C}} \frac{\left(\frac{z-z^{-1}}{2}\right)^2}{2+\frac{z+z^{-1}}{2}}\cdot\frac{dz}{iz}\\
& =-\frac{i}{2}\int_{\mathcal{C}}\frac{1 - 2z^2 + z^4}{z^2 + 4z^3 + z^4}dz\\
& = -\frac{i}{2}\int_{\mathcal{C}}\frac{1 - 2z^2 + z^4}{z^2 [z - (\sqrt(3) - 2)] [z + (\sqrt(3) + 2)]}dz\\
\end{split}
$$

De esta última expresión logramos ver que existen 3 indeterminaciones, $z_{0}= 0$ siendo un polo doble, $z_{1}=\sqrt(3) - 2$ y $z_{2}=-(\sqrt(3) + 2)$, estos dos últimos siendo polos simples.

Y únicamente con el afán de verificar que todos estos puntos contribuyen al contorno $\mathcal{C}:\|z\|=1$, podemos graficar lo siguiente:


```python
import matplotlib.pyplot as plt
import numpy as np

t = np.linspace(0,2*np.pi,1000)

x = np.cos(t)
y = np.sin(t)

fig, ax = plt.subplots()
fig.set_size_inches(9.5, 4)
ax.plot(x,y, color='black')
ax.scatter([0,np.sqrt(3)-2,-np.sqrt(3)-2],[0,0,0])
ax.set_xlabel(r'Re($z$)')
ax.set_ylabel(r'Im($z$)')
ax.text(0.06,0.06,'$z_{0}$')
ax.text(np.sqrt(3)-2.2,-0.15,r'$z_{1}$')
ax.text(-np.sqrt(3)-2,0.1,r'$z_{2}$')
plt.show()
```


    
![png](/assets/images/posts/output_9_0.png)
    


Por lo que de aquí podemos corroborar que los únicos puntos que aportan a $\mathcal{C}$ son $z_0$ y $z_1$, entonces podemos obtener sus respectivos residuos utilizando la fórmula brindada con anterioridad.

$$
\begin{split}
\Rightarrow \text{Res}[f(z),0] & =\frac{1}{1!}\lim_{z\rightarrow 0}\frac{d}{dz}\left[z^{2}\cdot \frac{1 - 2z^2 + z^4}{z^2 [z - (\sqrt(3) - 2)] [z + (\sqrt(3) + 2)]}\right]\\
& =\lim_{z\rightarrow 0}\frac{d}{dz}\left[\frac{1 - 2z^2 + z^4}{[z - (\sqrt(3) - 2)] [z + (\sqrt(3) + 2)]}\right]\\
\end{split}
$$


```python
from sympy import *

# Definimos nuestra variable z
z = Symbol('z')

# Definimos f(z)
f_z = (1 - 2*z**2 + z**4) / ((z - sqrt(3) +2) * (z+ sqrt(3) + 2))

# Definimos nuestra segunda derivada
derivada = diff(f_z,z,1)

# Simplificamos la expresión de la derivada
simplify(derivada)
```




$\displaystyle \frac{2 \left(z^{5} + 6 z^{4} + 2 z^{3} - 4 z^{2} - 3 z - 2\right)}{z^{4} + 8 z^{3} + 18 z^{2} + 8 z + 1}$



$$
\begin{split}
\Rightarrow \text{Res}[f(z),0] & =\lim_{z\rightarrow 0}\frac{d}{dz}\left[\frac{1 - 2z^2 + z^4}{[z - (\sqrt(3) - 2)] [z + (\sqrt(3) + 2)]}\right]\\
& =\lim_{z\rightarrow 0}\frac{2(z^5+6z^4+2z^3-4z^2-3z-2)}{z^4+8z^3+18z^2+8z+1}\\
& = -4
\end{split}
$$

Por otro lado, para $z_1 = \sqrt(3) - 2$ tenemos lo siguiente:

$$
\begin{split}
\Rightarrow \text{Res}[f(z),\sqrt(3) - 2] & =\lim_{z\rightarrow \sqrt(3) - 2}\left[[z-(\sqrt(3) - 2)]\cdot f(z)\right]\\
& =\lim_{z\rightarrow \sqrt(3) - 2}\left[\frac{1 - 2z^2 + z^4}{z^2 [z + (\sqrt(3) + 2)]}\right]\\
& = 2\sqrt{3}
\end{split}
$$

Con lo que podemos llegar a lo siguiente:

$$
\int_0^{2\pi}\frac{\sin^2\theta}{2+\cos\theta}d\theta=-\frac{i}{2}\cdot 2\pi i \left(-4 + 2\sqrt{3}\right)
$$

$$
\boxed{\therefore\ \int_0^{2\pi}\frac{\sin^2\theta}{2+\cos\theta}d\theta=(2\sqrt{3}-4)\pi}
$$

b) $\int_{-\infty}^{\infty}\frac{x^2}{(x^2+4)(x^2+1)}dx$

La clave para este ejercicio está en considerar a $\mathcal{C}=\mathcal{C}\_{R}+\mathcal{C}\_{p}$, donde $\mathcal{C}\_{R}$ es la semicircunferencia superior de radio $R$ y $\mathcal{C}\_{p}$ es el eje real.

O lo que sería equivalente a lo siguiente, gráficamente hablando:


```python
import matplotlib.pyplot as plt
import numpy as np

t = np.linspace(0,np.pi)
x = np.cos(t)
y = np.sin(t)

fig, ax = plt.subplots()
fig.set_size_inches(8, 5)
ax.plot(x,y, color='black')
ax.plot([-1,1],[0,0],color='black')
ax.text(0,0.05,r'$C_{p}$')
ax.text(0,0.9,r'$C_{R}$')
ax.set_xlabel(r'Re($z$)')
ax.set_ylabel(r'Im($z$)')
plt.show()
```


    
![png](/assets/images/posts/output_14_0.png)
    


Y, además, si notamos el grado del numerador $m=2$ y el grado del denominador $n=4$, entonces que $n\leq m+2$; ¿pero de donde sale esto?

Resulta que sea $f$ un cociente de polinomios $f(x)=\frac{p(x)}{q(x)}$, donde $q(x)\neq 0$ para todo $x$ y el grado de $q$ es $n\leq n+2$ ($m$ es el grado de $p$). Entonces se puede escoger el contorno cerrado $\mathcal{C}$ en $\mathbb{C}$ que consiste en el eje real (que llamamos $\mathcal{C}\_{p}$) y la semicircunferencia de radio $R$, $\mathcal{C}\_{R}$: 

$$
\int_{\mathcal{C}}f(z)dz=\int_{-\infty}^{\infty}f(x)dx+\int_{\mathcal{C}_{R}}f(z)dz
$$

En donde gracias a nuestra condición $n\leq m+2$, podemos deducir que cuando $R\rightarrow \infty$ sobre la integral $\mathcal{C}_{R}$, esta se hace cero. Por lo tanto:

$$
\int_{-\infty}^{\infty}=\int_{\mathcal{C}}
$$

De esta manera, para nuestro caso particular tenemos lo siguiente:

$$
\Rightarrow \int_{-\infty}^{\infty}\frac{x^2}{(x^2+4)(x^2+1)}dx=\int_{\mathcal{C}}\frac{z^2}{(z^2+4)(z^2+1)}dz
$$

$$
\Rightarrow \int_{-\infty}^{\infty}\frac{x^2}{(x^2+4)(x^2+1)}dx=\int_{\mathcal{C}}\frac{z^2}{(z-2i)(z+2i)(z-i)(z+i)}dz
$$

De donde podemos obtener que se cuentan con 4 polos simples en $z_0=2i$,$z_1=-2i$,$z_2=-i$,$z_3=i$.

De las cuales notemos que solo podemos considerar las que están por arriba del eje real; $z_{0}$ y $z_{2}$. Así que nos basta con aplicar el **Teorema de Residuos** para encontrar el valor de la integral de $f(z)$.

Entonces, para $z_0=2i$ tenemos lo siguiente:

$$
\begin{split}
\text{Res}[f(z),2i] & =\lim_{z\rightarrow 2i}\left[(z-2i)\cdot \frac{z^2}{(z-2i)(z+2i)(z-i)(z+i)}\right]\\
& =\lim_{z\rightarrow 2i}\left[\frac{z^2}{(z+2i)(z-i)(z+i)}\right]\\
& = -\frac{i}{3}
\end{split}
$$

Por otro lado, para $z_{2}=i$, tenemos:

$$
\begin{split}
\text{Res}[f(z),i] & =\lim_{z\rightarrow i}\left[(z-i)\cdot \frac{z^2}{(z-2i)(z+2i)(z-i)(z+i)}\right]\\
& =\lim_{z\rightarrow i}\left[\frac{z^2}{(z-2i)(z+2i)(z+i)}\right]\\
& = \frac{i}{6}
\end{split}
$$

Lo que finalmente nos lleva a la siguiente expresión:

$$
\Rightarrow \int_{-\infty}^{\infty}\frac{x^2}{(x^2+4)(x^2+1)}dx = 2\pi i\left( -\frac{i}{3}+ \frac{i}{6}\right)
$$

$$
\boxed{\therefore\ \int_{-\infty}^{\infty}\frac{x^2}{(x^2+4)(x^2+1)}dx = \frac{\pi}{3}}
$$

c) $\int_{-\infty}^{\infty}\frac{dx}{x(x^2-3x+2)}$

Para este ejercicio claramente observamos que se cumple que $n\leq m+2$, donde $n=3$ y $m=0$; por lo tanto, nuestra integral se resume a lo siguiente:

$$
\int_{-\infty}^{\infty}\frac{dx}{x(x^2-3x+2)}=\int_{\mathcal{C}}\frac{dz}{z(z^2-3z+2)}
$$

$$
\Rightarrow \int_{-\infty}^{\infty}\frac{dx}{x(x^2-3x+2)}=\int_{\mathcal{C}}\frac{dz}{z(z-1)(z-2)}
$$

Notando así que se cuentan con 3 polos simples, el primero de ellos en $z_0=0$, el segundo en $z_1=1$ y el tercero en $z_2=2$; los cuales se encuentran sobre la recta real, dando a entender que todos contribuyen a la integral.

Por lo tanto, nos bastará con encontrar los residuos en cada uno de estos puntos $z_{i}$, entonces, para $z_{0}$ tenemos lo siguiente:

$$
\begin{split}
\Rightarrow \text{Res}[f(z),0] & =\lim_{z\rightarrow 0}\left[z\cdot \frac{1}{z(z-1)(z-2)}\right]\\
& =\lim_{z\rightarrow 0}\left[\frac{1}{(z-1)(z-2)}\right]\\
& = \frac{1}{2}
\end{split}
$$

Por otro lado, para $z_1=1$

$$
\begin{split}
\Rightarrow \text{Res}[f(z),1] & =\lim_{z\rightarrow 1}\left[(z-1)\cdot \frac{1}{z(z-1)(z-2)}\right]\\
& =\lim_{z\rightarrow 1}\left[\frac{1}{z(z-2)}\right]\\
& = -1
\end{split}
$$

Y finalmente para $z_2=2$, tenemos lo siguiente:

$$
\begin{split}
\Rightarrow \text{Res}[f(z),2] & =\lim_{z\rightarrow 2}\left[(z-2)\cdot \frac{1}{z(z-1)(z-2)}\right]\\
& =\lim_{z\rightarrow 2}\left[\frac{1}{z(z-1)}\right]\\
& = \frac{1}{2}
\end{split}
$$

Lo que finalmente nos lleva a la siguiente expresión:

$$
\Rightarrow \int_{-\infty}^{\infty}\frac{dx}{x(x^2-3x+2)} = 2\pi i\left( \frac{1}{2}-1+\frac{1}{2}\right)
$$

$$
\boxed{\therefore\ \int_{-\infty}^{\infty}\frac{dx}{x(x^2-3x+2)} = 0}
$$
        
d) $\int_0^{\infty}\frac{x\sin x}{x^2+16}dx$

Antes de comenzar a resolver dicha integral, comencemos extendiendo nuestro límite de integración considerando que $f(x)=\frac{x\sin x}{x^2+16}$ es par, por lo tanto $f(x)$ puede quedar dada de la siguiente forma: 

$$
\Rightarrow \int_0^{\infty}\frac{x\sin x}{x^2+16}dx=\frac{1}{2}\int_{-\infty}^{\infty}\frac{x\sin x}{x^2+16}dx
$$

Y de aquí consideremos a la función:

$$
f(z)=\frac{ze^{iz}}{z^2+16}
$$

$$
\begin{split}
\Rightarrow \int_0^{\infty}\frac{x\sin x}{x^2+16}dx & =\frac{1}{2}\text{Im}\int_{\mathcal{C}}\frac{ze^{iz}}{z^2+16}dz\\
& =\frac{1}{2}\text{Im}\int_{\mathcal{C}}\frac{ze^{iz}}{(z-4i)(z+4i)}dz\\
\end{split}
$$

Integral sobre la cual podemos notar dos polos simples $z_{0}=4i$ y $z_{1}=-4i$, llevándonos a únicamente calcular el valor del residuo en $z_{0}$, pues es el único que se aporta a la integral. 

$$
\begin{split}
\Rightarrow \text{Res}[f(z),4i] & =\lim_{z\rightarrow 4i}\left[(z-4i)\cdot \frac{ze^{iz}}{(z-4i)(z+4i)}\right]\\
& =\lim_{z\rightarrow 4i}\left[\frac{ze^{iz}}{(z+4i)}\right]\\
& = \frac{1}{2e^{4}}
\end{split}
$$

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}}\frac{ze^{iz}}{(z-4i)(z+4i)}dz = 2\pi i \cdot \frac{1}{2e^{4}}=\frac{\pi i}{e^{4}}
\end{split}
$$

$$
\boxed{\therefore\ \int_0^{\infty}\frac{x\sin x}{x^2+16}dx = \frac{\pi}{2e^{4}}}
$$
        
e) $\int_{-\infty}^{\infty}\frac{\cos 2x}{(x^2+1)^2}dx$

Para esta integral consideremos la función

$$
f(z)=\frac{e^{2iz}}{(z^2+1)^{2}}
$$

$$
\begin{split}
\Rightarrow \int_{-\infty}^{\infty}\frac{\cos 2x}{(x^2+1)^2}dx & =\text{Re}\int_{\mathcal{C}}\frac{e^{2iz}}{(z^2+1)^{2}}dz\\
& =\text{Re}\int_{\mathcal{C}}\frac{e^{2iz}}{(z-i)^{2}(z+i)^{2}}dz\\
\end{split}
$$

En donde podemos observar que se tienen dos polos dobles, el primero en $z_0=i$ y otro más en $z_1=-i$, de aquí sabemos que el único que contribuye a la integral es $z_{0}$, entonces nos bastará con encontrar su residuo en dado punto:

$$
\begin{split}
\Rightarrow \text{Res}[f(z),i] & =\frac{1}{1!}\lim_{z\rightarrow i}\frac{d}{dz}\left[(z-i)^{2}\cdot \frac{e^{2iz}}{(z-i)^{2}(z+i)^{2}}\right]\\
& =\lim_{z\rightarrow i}\frac{d}{dz}\left[\frac{e^{2iz}}{(z+i)^{2}}\right]\\
\end{split}
$$

Entonces, para encontrar su derivada procedemos a hacer uso de la librería Sympy dentro de Python.


```python
from sympy import *

# Definimos nuestra variable z
z = Symbol('z')

# Definimos f(z)
f_z = exp(2* I *z) / (z+I)**2

# Definimos nuestra segunda derivada
derivada = diff(f_z,z,1)

# Simplificamos la expresión de la derivada
simplify(derivada)
```




$\displaystyle \frac{2 \left(i \left(z + i\right) - 1\right) e^{2 i z}}{\left(z + i\right)^{3}}$



$$
\begin{split}
\Rightarrow \text{Res}[f(z),i] & =\lim_{z\rightarrow i}\frac{2(i(z+i)-1)e^{2iz}}{(z+i)^3}\\
& = -\frac{3i}{4e^{2}}
\end{split}
$$

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}}\frac{e^{2iz}}{(z-i)^{2}(z+i)^{2}}dz = 2\pi i \cdot -\frac{3i}{4e^{2}}=\frac{3\pi}{2e^{2}}
\end{split}
$$

$$
\boxed{\therefore\ \int_{-\infty}^{\infty}\frac{\cos 2x}{(x^2+1)^2}dx = \frac{3\pi}{2e^{2}}}
$$

---

El notebook de este post, los puedes encontrar [aquí](https://colab.research.google.com/drive/1lmMFiW8uC8z0tAvb6GJ4nqkUwMjpIjcl?usp=sharing).