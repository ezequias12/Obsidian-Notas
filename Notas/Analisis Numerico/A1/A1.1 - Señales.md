
# Operaciones de señales - Impulso y escalón unitario

---
# 📘 **Resumen A1 – Señales y Sistemas (Unidad 1, parte 1)**

### 📌 1. Introducción

* El curso trabaja con **señales** y **sistemas**.
* Señal: función matemática de una o más variables independientes que representa un fenómeno físico.

  * En este curso: señales de **una sola variable independiente**.
  * Variables típicas:

    * Tiempo continuo → $t$
    * Tiempo discreto → $n$

---

### 📌 2. Señales

* **Tiempo continuo**: definidas para todo valor real de $t$.
* **Tiempo discreto**: definidas solo en valores enteros de $n$ (muestras).

#### Operaciones básicas sobre señales (en TC):

1. **Desplazamiento en el tiempo**:

   * $x(t - t_0)$ → corre la señal a la derecha $t_0$.
   * $x(t + t_0)$ → corre la señal a la izquierda.
2. **Escalamiento**:

   * $x(a t)$, con $a$ constante.
   * Si $a > 1$, la señal se “acorta” en el tiempo.
   * Si $0 < a < 1$, la señal se “estira”.
3. **Reflexión**:

   * $x(-t)$, la señal se refleja respecto al eje vertical.

➡️ Estas operaciones se pueden combinar (ejemplo: desplazar y luego reflejar).

---

### 📌 3. Señales básicas

#### 🔹 **Impulso unitario** – $\delta(t)$ en TC

* No es una función convencional, es una **función generalizada**.
* Definición:

  * $\delta(t) = 0$ para $t \neq 0$.
  * $\int_{-\infty}^{\infty} \delta(t) \, dt = 1$.
* Propiedad de muestreo:

  * $x(t)\delta(t) = x(0)\delta(t)$.
  * $\int_{-\infty}^{\infty} x(t)\delta(t)\,dt = x(0)$.
* Impulso desplazado: $\delta(t - t_0)$ → toma el valor en $x(t_0)$.
* **Representación incremental**: aproximar $\delta(t)$ como un pulso rectangular de ancho $\Delta$ y altura $1/\Delta$, manteniendo el área = 1.

#### 🔹 **Escalón unitario** – $u(t)$ en TC

* Definición:

  * $u(t) = 0$ si $t < 0$.
  * $u(t) = 1$ si $t \geq 0$.
* Desplazado: $u(t - t_0)$ comienza en $t_0$.
* Aproximación incremental: $u_\Delta(t)$ → transición suave que tiende a un escalón ideal cuando $\Delta \to 0$.
* Ejemplos de combinaciones:

  * $u(t-2) - u(t-4)$ → genera una “ventana” entre 2 y 4.

---

### 📌 4. Señales en tiempo discreto

* **Impulso unitario**:

  $$
  \delta[n] = 
  \begin{cases} 
  1 & n = 0 \\ 
  0 & n \neq 0
  \end{cases}
  $$
* **Escalón unitario**:

  $$
  u[n] = 
  \begin{cases} 
  1 & n \geq 0 \\ 
  0 & n < 0
  \end{cases}
  $$
* Mismas propiedades que en TC, pero sin complicaciones analíticas (ya que en TD es función bien definida).

---

### 📌 5. Sistemas

* **Sistema**: proceso que transforma una señal de entrada $x(t)$ en una salida $y(t)$.

  $$
  x(t) \; \xrightarrow{\text{SISTEMA}} \; y(t)
  $$
* Interconexión de sistemas:

  * **Serie/cascada** → salida de uno es entrada del siguiente.
  * **Paralelo/suma** → las salidas se suman.

---

### 📌 6. Propiedades de los sistemas (mencionadas en A1, se desarrollan más en A3)

1. **Memoria**:

   * Sin memoria: salida en $t$ depende solo de la entrada en $t$.
2. **Invertibilidad**: se puede recuperar la entrada a partir de la salida.
3. **Causalidad**: salida depende solo del presente y pasado, no del futuro.
4. **Estabilidad**: entrada acotada → salida acotada.
5. **Linealidad**: cumple el principio de superposición.
6. **Invarianza en el tiempo**: un corrimiento en la entrada produce igual corrimiento en la salida.

---

# ✅ **Conclusión del A1**

* Introduce **las operaciones sobre señales** (desplazamiento, escalamiento, reflexión).
* Define y trabaja con las **señales básicas** (impulso y escalón, tanto en TC como TD).
* Presenta la idea de **sistema** y sus **propiedades**, pero su desarrollo completo se da en el apunte **A3**.

---

¿Querés que te arme también un **mapa visual (tipo esquema o cuadro comparativo)** con las operaciones y las propiedades del impulso/escalón para que te quede más fácil de repasar para el parcial?
