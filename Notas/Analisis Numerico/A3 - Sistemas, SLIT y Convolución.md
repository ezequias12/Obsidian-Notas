
---

# 📘 **Resumen A3 – Sistemas, SLIT y Convolución**

### 📌 1. Continuación de Unidad 1 – Sistemas

Un **sistema** transforma una señal de entrada $x(t)$ en una salida $y(t)$.
Los sistemas se pueden interconectar:

* **En serie (cascada)** → salida de uno alimenta al siguiente.
* **En paralelo** → la salida es la suma de varias ramas.

#### **Propiedades de los sistemas**

1. **Memoria**:

   * Sin memoria → salida depende solo del valor presente de la entrada.
   * Con memoria → salida depende de valores pasados/futuros de la entrada.

2. **Invertibilidad**:

   * Un sistema es invertible si existe un **sistema inverso** que, aplicada la salida, devuelve la entrada original.

3. **Causalidad**:

   * Causal → salida depende del presente y pasado, nunca del futuro.
   * Ejemplo causal: $y(t) = x(t) - x(t-1)$.
   * Ejemplo no causal: $y(t) = x(t) - x(t+1)$.

4. **Estabilidad**:

   * Entrada acotada → salida también acotada (**BIBO: Bounded Input, Bounded Output**).
   * Ejemplo: si $x(n) = u(n)$ (escalón), pero la salida crece indefinidamente → sistema inestable.

5. **Invarianza en el tiempo**:

   * Un corrimiento en la entrada produce igual corrimiento en la salida.
   * Ejemplo: si $x(t-1)$ genera $y(t-1)$.

6. **Linealidad**:

   * Cumple **superposición**:

     $$
     x_1(t)\to y_1(t), \;\; x_2(t)\to y_2(t) \quad \Rightarrow \quad a x_1(t)+b x_2(t) \to a y_1(t)+ b y_2(t)
     $$

➡️ El curso se enfoca principalmente en **Sistemas Lineales Invariantes en el Tiempo (SLIT o LTI)**.

---

### 📌 2. Unidad 2 – Sistemas Lineales Invariantes en el Tiempo (SLIT)

* Muchos sistemas reales se modelan con éxito como **lineales e invariantes en el tiempo**.
* Esto permite usar **el principio de superposición** y caracterizarlos mediante su **respuesta al impulso**.

---

### 📌 3. Representación incremental y escudriñamiento del impulso

Para analizar sistemas SLIT, se representa una señal cualquiera como **suma de impulsos**:

* En **tiempo continuo** → integral de impulsos escalados y desplazados.
* En **tiempo discreto** → suma de impulsos desplazados ($\delta[n-k]$).

Esto se llama **escudriñamiento del impulso unitario**.
Ejemplo en TC:

$$
x(t) = \int_{-\infty}^{\infty} x(\tau)\, \delta(t-\tau)\, d\tau
$$

---

### 📌 4. Convolución en Tiempo Continuo

* Definición:

  $$
  y(t) = (x * h)(t) = \int_{-\infty}^{\infty} x(\tau) \, h(t-\tau)\, d\tau
  $$
* Donde:

  * $x(t)$ = entrada.
  * $h(t)$ = **respuesta al impulso** del sistema.
  * $y(t)$ = salida.

➡️ Un sistema SLIT queda **completamente caracterizado por su respuesta al impulso** $h(t)$.

---

### 📌 5. Convolución en Tiempo Discreto

* Definición:

  $$
  y[n] = (x * h)[n] = \sum_{k=-\infty}^{\infty} x[k] \, h[n-k]
  $$
* Interpretación: misma idea que en TC pero con sumatoria en lugar de integral.

---

### 📌 6. Propiedades de la convolución

1. **Conmutativa**:

   $$
   x(t) * h(t) = h(t) * x(t)
   $$
2. **Asociativa**:

   $$
   x(t) * [h_1(t) * h_2(t)] = [x(t) * h_1(t)] * h_2(t)
   $$
3. **Distributiva**:

   $$
   x(t) * [h_1(t)+h_2(t)] = x(t)*h_1(t) + x(t)*h_2(t)
   $$
4. **Elemento neutro**:

   * El impulso unitario $\delta(t)$ o $\delta[n]$.
   * $x(t) * \delta(t) = x(t)$.

---

### 📌 7. Interpretación práctica

* La convolución describe cómo un sistema SLIT “filtra” una entrada.
* Ejemplo:

  * Entrada: señal cualquiera $x(t)$.
  * Sistema: caracterizado por $h(t)$.
  * Salida: suma (o integral) de réplicas desplazadas y escaladas de $h(t)$.

---

# ✅ **Conclusión del A3**

* Completa las **propiedades de los sistemas** que quedaron iniciadas en A1.
* Introduce formalmente los **Sistemas Lineales Invariantes en el Tiempo (SLIT)**.
* Desarrolla la **convolución** como herramienta central para calcular la salida de un sistema.
* Presenta las **propiedades de la convolución** (asociativa, conmutativa, distributiva, elemento neutro).

---

¿Querés que en el próximo paso te arme una **guía de ejercicios típicos** (ejemplo: cómo calcular una convolución entre señales básicas) para fijar lo de A3?
