![[/image 23.png|image 23.png]]

## **Propiedad fundamental del impulso unitario**

---

### 1. La primera integral

![[/image 1 3.png|image 1 3.png]]

👉 Significa que cuando multiplicás una señal x(t) por el impulso en el **origen** y la integrás, el resultado es simplemente el valor de la señal en t=0

El dibujito lo muestra: el impulso está en t=0, entonces “captura” el valor de x(t) justo en ese punto.

---

### 2. La segunda integral

![[/image 2 4.png|image 2 4.png]]

👉 Ahora el impulso ya no está en 00, sino en t0.

La integral devuelve el valor de la señal **exactamente en ese instante** t0

En el gráfico: la flecha del impulso está en t0, entonces la integral agarra x(t0)

---

### 3. Interpretación (la frase en rojo)

“La integral de menos infinito a más infinito de una señal por un impulso es la señal valuada en donde está el impulso”.

✅ En criollo:

El impulso es como un “dedo” que **pincha la señal en un punto**.

Si el impulso está en 00, devuelve x(0).

Si está en t0, devuelve x(t0).

Es la **propiedad de muestreo del impulso**.

---

📌 Esta propiedad es la que hace posible que definamos la convolución, porque podemos reconstruir cualquier señal como una suma (o integral) de impulsos desplazados y escalados.

---

![[/image 3 4.png|image 3 4.png]]

Esta imagen muestra la idea de que **el impulso unitario δ(t) es el elemento neutro de la convolución**. Te la explico paso a paso:

---

## 1. Fórmula de convolución

  

![[/image 4 3.png|image 4 3.png]]

Si en particular tomamos h(t)=δ(t)

![[/image 5 2.png|image 5 2.png]]

---

## 2. ¿Qué pasa dentro de la integral?

- δ(t−τ) es un **impulso desplazado**, que está ubicado en τ=t.
- La propiedad del impulso dice que la integral “agarra” el valor de x(τ) **donde está el impulso**.

Entonces:

y(t)=x(t)

---

## 3. Interpretación (por qué es **elemento neutro**)

Así como en suma el **0** es neutro (a+0=a), y en multiplicación el **1** es neutro (a⋅1=a),

en convolución el **impulso unitario** es neutro:

x(t)∗δ(t)=x(t)

---

## 4. Los gráficos de la imagen

- **Arriba:** se muestra la integral de convolución con h(t)=δ(t).
- **Paso 1 (Desplazamiento):** se ve que el impulso se ubica en t.
- **Paso 2 (Reflexión):** al escribir δ(t−τ)\delta(t-\tau), se interpreta como un impulso reflejado y desplazado, pero lo importante es que termina en τ=t\tau=t.
- El cartelito rojo “El impulso está en tt” marca justo dónde la integral evalúa x(τ)x(\tau).

---

## 5. Resumen en palabras simples

El **impulso unitario** δ(t)\delta(t) es el **elemento neutro de la convolución** porque:

x(t)∗δ(t)=x(t)x(t) * \delta(t) = x(t)

La integral con el impulso no “modifica” a la señal, simplemente la devuelve tal cual está.

---

👉 ¿Querés que te muestre un **ejemplo numérico** (tipo convolución de un escalón con δ(t)\delta(t) o con δ(t−2)\delta(t-2)) para ver cómo esto se traduce en práctica?