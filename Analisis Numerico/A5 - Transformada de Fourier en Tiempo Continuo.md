Este apunte sigue en la **Unidad 3: Fourier en tiempo continuo**, pero ya no para señales periódicas (Serie), sino para señales **no periódicas**.

---

# 📘 **Resumen A5 – Transformada de Fourier en Tiempo Continuo**

### 📌 1. Contexto

* Hasta ahora:

  * **Serie de Fourier** → descompone **señales periódicas** en exponenciales armónicas.
* Problema: ¿qué pasa con señales **no periódicas**?

  * Se resuelve mediante la **Transformada de Fourier (TF)**.
* Idea clave: una señal no periódica puede pensarse como el límite de una periódica con período infinito ($T_0 \to \infty$).

---

### 📌 2. Definición – Par de Transformadas

La **Transformada de Fourier (TF)** conecta el dominio tiempo y frecuencia:

* **Transformada directa**:

  $$
  X(\omega) = \int_{-\infty}^{\infty} x(t) e^{-j\omega t}\, dt
  $$

* **Transformada inversa**:

  $$
  x(t) = \frac{1}{2\pi} \int_{-\infty}^{\infty} X(\omega) e^{j\omega t}\, d\omega
  $$

Donde:

* $x(t)$ → señal en el tiempo.
* $X(\omega)$ → espectro en frecuencia.

---

### 📌 3. Condiciones de convergencia

Son las mismas que las de la Serie de Fourier (Condiciones de Dirichlet):

1. $x(t)$ absolutamente integrable.
2. Número finito de máximos y mínimos en un intervalo finito.
3. Número finito de discontinuidades en un intervalo finito.

---

### 📌 4. Herramientas prácticas

Para resolver ejercicios se usan:

* Fórmulas básicas de TF.
* **Tabla de transformadas** (ejemplos típicos: exponencial, coseno, pulso rectangular, delta).
* **Tabla de propiedades** para manipular señales y encontrar su transformada sin calcular integrales desde cero.

---

### 📌 5. Propiedades principales de la TF

#### Mencionar:

* **Linealidad**:

  $$
  a x_1(t) + b x_2(t) \;\; \longleftrightarrow \;\; aX_1(\omega) + bX_2(\omega)
  $$
* **Simetría** en $\omega$.
* **Dualidad**: si $x(t) \leftrightarrow X(\omega)$, entonces $X(t) \leftrightarrow 2\pi x(-\omega)$.
* **Diferenciación**:

  * En el tiempo: derivar ↔ multiplicar por $j\omega$ en frecuencia.
  * En frecuencia: derivar ↔ multiplicar por $-jt$ en tiempo.

#### Demostrar:

* **Desplazamiento en el tiempo**:

  $$
  x(t-t_0) \;\; \longleftrightarrow \;\; e^{-j\omega t_0} X(\omega)
  $$
* **Convolución en el tiempo**:

  $$
  x(t)*h(t) \;\; \longleftrightarrow \;\; X(\omega) H(\omega)
  $$

  (→ base para análisis de sistemas SLIT).

---

### 📌 6. Aplicaciones

1. **Respuesta en frecuencia – Filtros**

   * Ejemplo: filtro paso bajo ideal.
   * La salida en frecuencia se obtiene multiplicando el espectro de la entrada por la respuesta en frecuencia $H(\omega)$.
   * Ejemplo práctico: entrada $x(t) = \cos(5t) + \sin(20t)$. El filtro elimina la componente de 20 y deja la de 5.

2. **Modulación y Enventanado**

   * Multiplicar una señal infinita por una **ventana** (ej. rectangular) → espectro modificado.
   * Ejemplo: coseno enventanado → sus impulsos espectrales se transforman en sinc (SenC).

3. **Muestreo periódico (intro al Teorema del Muestreo)**

   * Muestrear una señal equivale a multiplicarla por un tren de deltas en tiempo.
   * En frecuencia: el espectro se replica cada múltiplo de la frecuencia de muestreo $F_m$.
   * Condición de Nyquist:

     $$
     F_m > 2 f_{\text{máx}}
     $$

     para evitar **aliasing**.

---

### 📌 7. Ejemplos de uso

* Ejercicios de transformadas (ej. rectángulo ↔ sinc).
* Filtro paso bajo (demostración con convolución).
* Señales muestreadas y condición de muestreo.
* Enventanado de cosenos → espectro con sinc.

---

# ✅ **Conclusión del A5**

* Extiende Fourier a **señales no periódicas** con la **Transformada de Fourier**.
* Define el **par de transformadas** y sus condiciones de validez.
* Presenta las **propiedades de la TF** (mencionar y demostrar).
* Muestra aplicaciones clave:

  * **Filtros paso bajo**.
  * **Modulación y enventanado**.
  * **Muestreo periódico** (intro al teorema del muestreo, que se completa en A7).

---

👉 Con esto, A5 cierra la parte de **Fourier en TC**.
El paso siguiente (A6 y A7) ya es **Fourier en TD, TDF, muestreo y filtros FIR** → temas del **final**.

---

¿Querés que te arme ahora un **cuadro comparativo A4 vs A5** (Serie vs Transformada de Fourier) para que tengas claro qué cambia entre señales periódicas y no periódicas?
