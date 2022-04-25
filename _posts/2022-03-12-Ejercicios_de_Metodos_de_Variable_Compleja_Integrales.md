---
layout: post
title:  "Ejercicios de Métodos de Variable Compleja. Integrales"
categories: [ Métodos Matemáticos, Avanzado ]
image: assets/images/banners/Integrales.png
author: marco
---

- Calcular las siguientes integrales siendo $$\mathcal{C}$$ el segmento de recta que va desde $$-1-i$$ a $$1+i$$:

a) $$\int_{\mathcal{C}} \left[(\text{Re}z)^2-(\text{Im}z)^2\right]dz$$

Para resolver este y el siguiente inciso, partiremos de observar el segmento de recta sobre el que se va a integrar, con el propósito de ver gráficamente una posible parametrización.


```python
import matplotlib.pyplot as plt

# Definimos nuestras valores reales e imaginarios
Re_z = [-1,1]
Im_z = [-1,1]

# Realizamos ploteo de los valores anteriores
fig, ax = plt.subplots()
ax.plot(Re_z,Im_z)
ax.plot(Re_z,Im_z,'.',color='r')
ax.set_xlabel(r'Re($$z$$)')
ax.set_ylabel(r'Im($$z$$)')
plt.text(-.85,-1,r'$$-1-i$$')
plt.text(.75,.95,r'$$1+i$$')
plt.show()
```


    
![png](/assets/images/posts/output_3_0.png)
    


De esta manera, podemos decir que el segmento anterior lo podemos parametrizar con la siguiente función:

$$
z(t) = t+it\ ;\quad -1< t <1
$$


Entonces, considerando la definición de integral de contorno, dada por:

$$
\int_{\mathcal{C}}f(z)dz = \int_{a}^{b} f(z(t))z'(t)dt
$$

Donde para nuestro caso particular:

$$
f(z)=(\text{Re}z)^2-(\text{Im}z)^2
$$

Entonces:

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}} \left[(\text{Re}z)^2-(\text{Im}z)^2\right]dz & = \int_{-1}^{1} f(t+it)(1+i)dt\\
& = \int_{-1}^{1} (t^2 - t^2)(1+i)dt\\
& = \int_{-1}^{1} 0\cdot (1+i)dt\\
& = 0
\end{split}
$$

$$
\boxed{\therefore\ \int_{\mathcal{C}} \left[(\text{Re}z)^2-(\text{Im}z)^2\right]dz = 0}
$$

b) $$\int_{\mathcal{C}} \left(\cosh\frac{\pi x}{2}\cos\frac{\pi y}{2}+i\sinh\frac{\pi x}{2}\sin\frac{\pi y}{2}\right)dz$$

Nuevamente, sea $$z(t)=t+it$$ donde $$-1<t<1$$, y en este caso $$f(z)$$ esté dado por la siguiente expresión:

$$
f(z) = \cosh\frac{\pi x}{2}\cos\frac{\pi y}{2}+i\sinh\frac{\pi x}{2}\sin\frac{\pi y}{2}
$$

para $$z=x+iy$$.

De esta manera, $$f(z(t))$$ estaría dado por:

$$
f(z(t))=\cosh\frac{\pi t}{2}\cos\frac{\pi t}{2}+i\sinh\frac{\pi t}{2}\sin\frac{\pi t}{2}
$$

Entonces:

$$
\int_{\mathcal{C}} \left(\cosh\frac{\pi x}{2}\cos\frac{\pi y}{2}+i\sinh\frac{\pi x}{2}\sin\frac{\pi y}{2}\right)dz
$$

sería igual a

$$
(1+i)\int_{-1}^{1} \left(\cosh\frac{\pi t}{2}\cos\frac{\pi t}{2}+i\sinh\frac{\pi t}{2}\sin\frac{\pi t}{2}\right)dt
$$

gracias a la definición de integral de contorno mostrada en el ejercicio anterior.

De esta manera, haciendo uso de la librería Sympy y sus módulos de integración y simplificación, podemos encontrar el siguiente resultado.


```python
from sympy import *

# Definimos a nuestra variable t
t = Symbol('t')

# Definimos f(t)
f_t = cosh(t*pi / 2)*cos(t*pi / 2) + I * sinh(t*pi / 2) * sin(t*pi / 2)

# Resolvemos la integral de contorno dentro de los límites -1 a 1
valor_integral = integrate(f_t,(t,-1,1))

# Imprimimos la simplificación de I_c
simplify(valor_integral * (1+I))
```




$$\displaystyle \frac{4 i \cosh{\left(\frac{\pi}{2} \right)}}{\pi}$$



$$
\boxed{\therefore\ \int_{\mathcal{C}} \left(\cosh\frac{\pi x}{2}\cos\frac{\pi y}{2}+i\sinh\frac{\pi x}{2}\sin\frac{\pi y}{2}\right)dz=\frac{4i\cosh\left(\frac{\pi}{2}\right)}{\pi}}
$$

- Calcular las siguientes integrales, siendo $$\mathcal{C}$$ la circunferencia de radio unidad recorrida en sentido antihorario:

a) $$\int_{\mathcal{C}} \frac{dz}{z^3+4z}$$

Para estos ejercicios consideraremos la fórmula de Cauchy, dada por:

$$
f(z_0) = \frac{1}{2\pi i}\oint_{\mathcal{C}} \frac{f(z)}{z-z_0}dz
$$

Entonces, consideremos la siguiente modificación:

$$
\frac{1}{z^3+4z}=\frac{1}{z(z^2+4)}
$$

De donde notamos que tenemos singularidades en los puntos $$z_0=0$$, $$z_1=-2i$$, $$z_2=2i$$; sin embargo, $$z_0$$ es el único que se encuentra dentro de la circunferencia unitaria, por lo tanto, haciendo referencia a la fórmula de Cauchy y tomando a $$f(z)=\frac{1}{(z^2+4)}$$, podemos concluir lo siguiente:

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}} \frac{dz}{z^3+4z} & = \oint_{\mathcal{C}}\frac{f(z)}{z}dz\\
& = 2\pi i f(0)\\
& = \frac{\pi i}{2}
\end{split}
$$

$$
\boxed{\therefore\ \int_{\mathcal{C}} \frac{dz}{z^3+4z}=\frac{\pi i}{2}}
$$

b) $$\int_{\mathcal{C}}\frac{dz}{z^*}$$

Para este caso, notemos que $$\frac{1}{z^*}$$ no es analítica, por lo tanto, no podemos proceder por medio de la fórmula de Cauchy, sin embargo, sí podemos hacerlo por medio de la definición de integral de línea mostrada al inicio.

Entonces, tomando a $$f(z) = \frac{1}{z^*}$$ y $$z(t)=e^{it}$$ para $$0<t<2\pi$$, tenemos lo siguiente:

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}}\frac{dz}{z^*} & = \int_0^{2\pi} f(z(t))z'(t)dt\\
& = \int_0^{2\pi}e^{it}\cdot ie^{it}dt\\
& = i\int_0^{2\pi}e^{2it}dt
\end{split}
$$

Haciendo el cambio de variable $$m(t) = 2it$$ entonces $$dm = 2idt$$.

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}}\frac{dz}{z^*} & = \frac{1}{2}\int_0^{4\pi i}e^{m}dm\\
& = \frac{1}{2}\left[e^{m}\right]_{0}^{4\pi i}\\
& = 0
\end{split}
$$

$$
\boxed{\therefore\ \int_{\mathcal{C}}\frac{dz}{z^*}=0}
$$

c) $$\int_{\mathcal{C}}\frac{\sinh z^2}{z^3}$$

Para este inciso, bastará con considerar tres cosas, principalmente, la primera de ellas es la fórmula generalizada de Cauchy, la cual está dada de la siguiente manera:

$$
f^{(n)}(z)=\frac{n!}{2\pi i}\oint_{\mathcal{C}}\frac{f(w)}{(w-z)^{n+1}}dw
$$

Y la segunda de ellas que $$f(z)=\sinh z^2$$; de esta manera, podemos llegar a la siguiente expresión:

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}}\frac{\sinh z^2}{z^3} & = \oint_{\mathcal{C}}\frac{f(z)}{z^3}dz\\
& = \frac{2\pi i f^{(2)}(0)}{2!}
\end{split}
$$

Así, haciendo uso nuevamente de la librería sympy y sus módulos de derivación y evaluación tenemos lo siguiente:


```python
from sympy import *

# Definimos nuestra variable z
z = Symbol('z')

# Definimos f(z)
f_z = sinh(z**2)

# Definimos nuestra segunda derivada
derivada = diff(f_z,z,2)

# Evaluamos en z = 0
derivada.evalf(subs={z:0})
```




$$\displaystyle 2.0$$



$$
\boxed{\therefore\ \int_{\mathcal{C}}\frac{\sinh z^2}{z^3} = 2\pi i}
$$

d) $$\int_{\mathcal{C}}\frac{e^{z}-z-1}{z^2}dz$$

Al igual que en el ejemplo anterior, bastará con tomar en cuenta la fórmula generalizada de Cauchy, y para nuestro caso particular, considerar a $$f(z) = e^{z}-z-1$$, con esto deberíamos llegar a la siguiente expresión:

$$
\begin{split}
\Rightarrow\int_{\mathcal{C}}\frac{e^{z}-z-1}{z^2}dz & = \oint_{\mathcal{C}}\frac{f(z)}{z^2}dz\\
& = \frac{2\pi i f^{(1)}(0)}{1!}
\end{split}
$$

De esta manera, reutilizando el código del inciso anterior para conocer el valor de $$f^{(1)} (0)$$, tenemos lo siguiente:


```python
from sympy import *

# Definimos nuestra variable z
z = Symbol('z')

# Definimos f(z)
f_z = exp(z)-z-1

# Definimos nuestra segunda derivada
derivada = diff(f_z,z,1)

# Evaluamos en z = 0
round(derivada.evalf(subs={z:0}),2)
```




$$\displaystyle 0.0$$



$$
\boxed{\therefore\ \int_{\mathcal{C}}\frac{e^{z}-z-1}{z^2}dz = 0}
$$

e) $$\int_{\mathcal{C}}\frac{z^*\sin z}{z}dz$$

Para este caso consideraremos, en primer lugar, el cambio de variable $$z=e^{it}$$ y en segundo lugar la fórmula general de Cauchy; con esto deberíamos llegar a lo siguiente:

$$
\begin{split}
\Rightarrow \int_{\mathcal{C}}\frac{z^*\sin z}{z}dz & = \int_{\mathcal{C}}\frac{e^{-it}\sin(e^{it})}{e^{it}}\cdot ie^{it}dt\\
& = \int_{\mathcal{C}}\frac{\sin(e^{it})}{e^{2it}}\cdot ie^{it}dt\\
& = \int_{\mathcal{C}}\frac{\sin(z)}{z^2}dz\\
& = 2\pi i f^{(1)}(0)
\end{split}
$$

En donde $$f(z) = \sin(z)$$. Así, con ayuda de Python obtenemos lo siguiente:


```python
from sympy import *

# Definimos nuestra variable z
z = Symbol('z')

# Definimos f(z)
f_z = sin(z)

# Definimos nuestra segunda derivada
derivada = diff(f_z,z,1)

# Evaluamos en z = 0
derivada.evalf(subs={z:0})
```




$$\displaystyle 1.0$$



$$
\boxed{\therefore\ \int_{\mathcal{C}}\frac{z^*\sin z}{z}dz = 2\pi i}
$$

- Hallar los módulos máximo y mínimo de $$f(z)=2z+5i$$ en el circulo $$\|z\|\leq 2$$.


Sea $$\|f(z)\|$$ el módulo de $$f(z)$$, entonces:

$$
\begin{split}
\Rightarrow\|f(z)\|^2 & = \|2z+5i\|^2\\
& =4\|z\|^2+25+2\text{Re}[2z(-5i)]\\
& = 4\|z\|^2+25+20\text{Im}z
\end{split}
$$

De acuerdo con el teorema del módulo máximo, el cual nos dice que sea la función $$f(z)$$ no constante en el interior de un dominio cerrado $$\mathcal{D}$$. Si $$f$$ es continua en la frontera $$\mathcal{C}$$ de $$\mathcal{D}$$, entonces $$\|f(z)\|$$ adquiere su valor máximo en la frontera y nunca en su interior.

Por lo que si, además agregamos que $$f(z) \neq 0$$ para todo $$z\in \mathcal{C}$$, entonces también adquiere su mínimo en $$\mathcal{C}$$.

Con esto llegamos a que $$f(z)$$ alcanza su máximo en $$\|z\|=2$$. Entonces $$\|f(z)\|=\sqrt{41+20\text{Im}z}$$. De donde vemos que se alcanza un máximo en la parte imaginaria más grande, la cual es $$z=2i$$, con lo que concluimos que el máximo de $$\|f(z)\|$$ es:

$$
\begin{split}
\Rightarrow \|f(z)\|_{\text{max}} & =\|f(2i)\|\\
& =\sqrt{41+20\text{Im}(2i)}\\
& =\sqrt{81}\\
& = 9
\end{split}
$$

Por otro lado, el mínimo se alcanza en cuando $$z = -2i$$, entonces:

$$
\begin{split}
\Rightarrow \|f(z)\|_{\text{min}} & =\|f(-2i)\|\\
& =\sqrt{41+20\text{Im}(-2i)}\\
& =\sqrt{1}\\
& = 1
\end{split}
$$

$$
\boxed{\therefore\ \|f(z)\|_{\text{max}}=9\ ;\quad \|f(z)\|_{\text{min}}=1}
$$

---
    
El notebook de este post, los puedes encontrar [aquí](https://colab.research.google.com/drive/1MyEt02o7A6oR_gfSg5YLqtiMQHtsydQ9?usp=sharing).
