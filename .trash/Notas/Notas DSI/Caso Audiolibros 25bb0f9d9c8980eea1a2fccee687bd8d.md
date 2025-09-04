# Caso Audiolibros

## Descomposición de dominio

![image.png](Caso%20Audiolibros%2025bb0f9d9c8980eea1a2fccee687bd8d/image.png)

# Receta rápida (para cualquier ejercicio)

1. **Core**: lo que **entrega valor** al cliente y/o **genera ingreso directo**. Si se cae, el negocio no opera.

Algo que diferencia a mi negocio

1. **Soporte**: **maestros/configuración** que el Core usa para funcionar. Aporta reglas pero **no** entrega valor por sí solo.
2. **Genérico**: capacidades **transversales y commodity** (podrías tercerizarlas sin perder identidad).

---

# Uno por uno (caso audiolibros)

### Core (corazón del negocio)

- La profe aclaro que audiolibro puede estar en Core (No seria un error tan grave)

Analizar los dos Beneficios, el del usuario y el del negocio

negocio → Suscripciones

Usuario → Posibilidad de escuchar audiolibros y tener una biblioteca de los mismos

**1) Suscripciones**

- Qué es: vínculo “usuario + plan + ciclo de cobro + estado”.
- Por qué Core: **monetiza** (define quién paga y está habilitado). Coordina renovaciones, cortes de servicio, beneficios. Sin esto, **no hay negocio**.
- Señales: afecta ingresos, tiene reglas críticas (vencimientos, moras), orquesta otros servicios.

**2) Biblioteca de suscriptor**

- Qué es: catálogo **personalizado** de lo que cada usuario puede usar.
- Por qué Core: es **la entrega del valor** (qué puede consumir gracias a su suscripción). Implementa “derechos de acceso” (entitlements).
- Señales: depende de Suscripciones, se usa en cada sesión del cliente.

**3) Escucha de audiolibro**

- Qué es: reproducción/streaming, progreso, DRM, controles.
- Por qué Core: es **la experiencia principal**. Si esto falla, no hay servicio.
- Señales: métricas clave (tiempo de escucha, retención), requisitos de performance/latencia.

### Soporte (habilita al Core)

**4) Audiolibro (catálogo de contenido)**

- Qué es: metadatos, archivos, secciones.
- Por qué Soporte: **alimenta** a la Biblioteca/Escucha, pero no genera valor por sí solo; podrías gestionarlo con un CMS externo.
- Ojo: si tu diferencial de negocio fuera “curación/recomendación de catálogo”, podría subir a **Core**.

**5) Membresías (tipos de plan)**

- Qué es: planes, precios, límites, vigencias.
- Por qué Soporte: es **maestro/configuración** que **Suscripciones** usa. Cambia políticas, no entrega valor solo.
- Señales: se edita de forma administrativa; impacta reglas de Suscripciones pero no ejecuta el flujo central.

### Genéricos (transversales/commodity)

**6) Usuarios**

- Identidad/autenticación/autorización.
- Genérico: podrías usar un IdP (OAuth/Keycloak/Auth0). Está en **casi cualquier sistema**.

**7) Cobro con tarjeta de crédito**

- Integración con PSP (Mercado Pago, Stripe).
- Genérico: crítico, sí, pero **no diferencial**; se externaliza sin perder identidad de negocio.

**8) Notificaciones push** y **9) Notificación mail**

- Envío de mensajes.
- Genérico: se reemplaza por servicios (Firebase, SES, etc.). **Transversal** a muchos dominios.

---

# Cómo aplicarlo en otros casos (paso a paso)

1. **Escribí la propuesta de valor** en una frase (“vendemos acceso ilimitado a X por suscripción”).
2. **Marcá la transacción/flujo que cobra o entrega valor** → eso es **Core**.
3. **Listá maestros/configuración** que ese flujo usa (planes, catálogos, tarifas) → **Soporte**.
4. **Separá lo transversal/commodity** (usuarios, pagos, notificaciones, logging) → **Genérico**.
5. **Ajustá por diferenciadores**: si algo es tu ventaja competitiva (p. ej., recomendación), puede pasar a **Core**.

Si querés, probamos con otro dominio que tengas y lo clasificamos juntos con esta receta.

### Otra regla - Importante

La clave es que este tipo de diagrama no lo sacás de la galera: lo armas mirando los casos de uso y preguntándote:

¿Esto es lo que diferencia mi negocio (core)?
¿Esto lo necesito pero no me da ventaja (soporte)?
¿Esto es commodity, algo que casi todos hacen igual (genérico)?

---

---

# Vista Funcional

## Porque ABMC de Membresia y no de Catalogo

### Regla corta

Elegí como ejemplo **el ABMC más simple** que a la vez **obliga** a decidir cosas importantes de la arquitectura (RNF) sin meter ruido extra del dominio. En este caso, eso es **Membresía**.

### Señales claras para elegir **Membresía**

- **Toca RNF clave de entrada**: administración web simple (CRUD), soporte de navegador y despliegue en cloud.
- **Tiene “vigencias” (desde/hasta)**: te fuerza a decidir cómo versionar datos y evitar solapamientos → buena para fijar modelo y validaciones.
- **Conecta con procesos transversales**: pagos/renovaciones y notificaciones. O sea, desde ese CRUD ya “salpica” a otros servicios.

### ¿Por qué **no** Catálogo para el primer ABMC?

- Trae **ruido de multimedia/jerarquías** (imágenes, audios, secciones) que **no** aporta a las primeras decisiones arquitectónicas y te distrae.
- Lo **relevante** del catálogo (búsqueda/consumo desde la app) igual se cubre en otros casos de uso más adelante.

### Checklist en 10 segundos

Si estas 3 preguntas dan “sí”, elige **Membresía**:

1. ¿Es un CRUD web sencillo que me hace definir capas, acceso a datos y validaciones básicas? **Sí**
2. ¿Incluye **vigencias** o reglas temporales que ejerciten el modelo? **Sí**
3. ¿Dispara otros procesos (pagos/notificaciones) que muestren integración? **Sí**

Si, en cambio, tu CRUD arrastra multimedia/jerarquías y no agrega decisiones arquitectónicas nuevas, **no** es el mejor “representativo” para la vista funcional inicial (ahí cae Catálogo).

---