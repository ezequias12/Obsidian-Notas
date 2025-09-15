
### A3 – Sistemas, Propiedades, SLIT y Convolución

**Unidad 1 – Introducción a las Señales y Sistemas (continuación de clase 1)**

#### SISTEMAS

* Definición y Conexión entre los sistemas
* Propiedades: **Memoria, Invertibilidad, Causalidad, Estabilidad, Linealidad, Invarianza en el tiempo**

**Unidad 2 – Sistemas Lineales Invariantes en el Tiempo (S.L.I.T.)**

* Importancia de la descomposición de una señal cualquiera en **combinación lineal de señales básicas**.
  → Interpretación del principio de superposición.
* Representación de señales continuas en términos de impulsos en representación incremental desplazados en el tiempo continuo.
  → “Escudriñamiento del impulso” en tiempo continuo (**Aproximación incremental**).
* Integral de convolución aplicada a la señal aproximada.
  → Límite cuando Δ → 0.

---

### **Propiedades de los Sistemas**

**Consideramos:**

* t → instante de tiempo t
* t - 1 → tiempo anterior
* t + 1 → tiempo posterior

**1. Sistemas con memoria / sin memoria:**

* Un sistema es **sin memoria** si la salida en un instante de tiempo *t* depende únicamente de la entrada en el mismo instante de tiempo *t*.

**2. Invertibilidad:**

* Para que un sistema sea **invertible**, a cada entrada diferente le corresponde una salida diferente.
* De esta forma, a partir de la salida se puede identificar la entrada.
* Se cumple que:

  * A la señal de salida *y(t)* le aplicamos otro sistema (llamado **Sistema Inverso**), el cual arroja como salida la entrada original *x(t)*.
![[Pasted image 20250915130636.png]]
---
**Propiedades de los Sistemas**

**3. Causalidad:**

* Un sistema es **causal** si los valores de la salida en un instante de tiempo *t* dependen de los valores de la entrada en el mismo instante de tiempo *t* y/o en tiempos anteriores.

**Ejemplos:**

* **Causal:**

  $$
  y(t) = x(t) - x(t-1)
  $$

* **No causal:**

  $$
  y(t) = x(t) - x(t+1)
  $$

*Nota:* un sistema no causal depende de valores futuros de la entrada.

Entonces: Si es causal es con memoria

---
