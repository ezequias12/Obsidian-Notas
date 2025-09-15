 Unidad 3: Análisis de Fourier en Tiempo Continuo.
 
---

# 📘 **Resumen A4 – Serie de Fourier**

### 📌 1. Repaso de números complejos

* Formas de representación:

  * **Cartesiana**: $z = a + jb$.
  * **Polar**: $z = |z| e^{j\theta}$.
  * **Euler**: $e^{j\theta} = \cos\theta + j\sin\theta$.
* Identidades útiles:

  $$
  \cos \theta = \tfrac{1}{2}(e^{j\theta}+e^{-j\theta}), \quad 
  \sin \theta = \tfrac{1}{2j}(e^{j\theta}-e^{-j\theta})
  $$

---

### 📌 2. Tercera señal básica: exponencial compleja

$$
x(t) = c e^{at}
$$

* Casos:

  1. **Exponencial real** → $a$ real, crecimiento o decrecimiento.
  2. **Exponencial compleja general** → $a = r + j\omega_0$.

     $$
     x(t) = |c| e^{rt} e^{j(\theta + \omega_0 t)}
     $$

     * Parte real e imaginaria: senoidales amortiguadas.
     * Si $r>0$ → creciente, si $r<0$ → decreciente.
  3. **Exponencial imaginaria pura** → $a = j\omega_0$.

     * Es **periódica**, de frecuencia $\omega_0$.

---

### 📌 3. Periodicidad y frecuencia fundamental

* Una señal es periódica si:

  $$
  x(t) = x(t+T), \quad \forall t
  $$

* Para $e^{j\omega_0 t}$, el período es:

  $$
  T_0 = \frac{2\pi}{\omega_0}
  $$

* Frecuencia fundamental:

  $$
  \omega_0 = \frac{2\pi}{T_0}
  $$

* Exponenciales complejas armónicas: aquellas cuyas frecuencias son múltiplos enteros de $\omega_0$:

  $$
  e^{jk\omega_0 t}, \quad k \in \mathbb{Z}
  $$

---

### 📌 4. Serie de Fourier

Permite representar señales **periódicas** como combinación lineal de exponenciales armónicas:

$$
x(t) = \sum_{k=-\infty}^{\infty} a_k e^{jk\omega_0 t}
$$

* Donde:

  * $a_k$ = coeficientes de Fourier.
  * $\omega_0$ = frecuencia fundamental.
  * $T_0$ = período fundamental.

---

### 📌 5. Cálculo de los coeficientes $a_k$

1. Multiplicar por $e^{-jn\omega_0 t}$ y **integrar en un período** $T_0$.
2. Resultado:

   $$
   a_k = \frac{1}{T_0} \int_{T_0} x(t) e^{-jk\omega_0 t} \, dt
   $$
3. Casos:

   * $k = n$: integral distinta de cero.
   * $k \neq n$: integral = 0 (ortogonalidad).

---

### 📌 6. Tipos de ejercicios con Serie de Fourier

* **Tipo 1**: señal ya expresada como serie → identificar armónicas y coeficientes.
* **Tipo 2**: señal dada como senos/cosenos → usar fórmulas de Euler y pasar a forma exponencial.
* **Tipo 3**: señal dada gráficamente → determinar $T_0$ y calcular coeficientes.
* **Tipo 4**: señal analítica (ej. funciones a trozos) → resolver con integral.

---

### 📌 7. Convergencia de la Serie de Fourier

* **Condiciones de Dirichlet** (garantizan convergencia):

  1. $x(t)$ debe ser **absolutamente integrable** en un período.
  2. En cada período debe tener un número **finito de máximos y mínimos**.
  3. En cada período debe haber un número **finito de discontinuidades**.

* Si estas condiciones no se cumplen:

  * Puede ocurrir que la serie diverja.
  * O que los coeficientes sean finitos, pero la serie no represente a la señal original en todos los puntos.

---

# ✅ **Conclusión del A4**

* Presenta las **exponenciales complejas** como base fundamental.
* Define **periodicidad, frecuencia fundamental y armónicas**.
* Introduce la **Serie de Fourier en TC**, fórmula de los coeficientes y distintos métodos para calcularlos.
* Establece las **condiciones de Dirichlet** para asegurar convergencia.

---

¿Querés que en el próximo paso, cuando hagamos el **A5**, te muestre ya cómo la **Transformada de Fourier se deduce de la Serie** (o sea, el puente entre A4 y A5)?
