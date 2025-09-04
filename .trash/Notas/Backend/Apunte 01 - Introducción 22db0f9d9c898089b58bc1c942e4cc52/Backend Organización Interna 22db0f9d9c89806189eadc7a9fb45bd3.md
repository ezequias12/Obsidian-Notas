# Backend Organización Interna

### 🎯 **¿Qué son las arquitecturas en backend?**

Las **arquitecturas de software** son **formas de organizar y estructurar tu aplicación** para que sea más mantenible, escalable y fácil de desarrollar.

No se trata solamente de “separar el código en carpetas”, sino de **cómo se distribuyen los componentes y cómo se comunican**.

---

### 🏛️ **Principales arquitecturas del apunte explicadas con claridad**

### 1. **Arquitectura Monolítica**

- **Qué es**: Toda la aplicación está unida en un único proyecto/paquete ejecutable.
- **Ejemplo práctico**:
    
    Imagina que tienes un proyecto Java donde están:
    
    - Las rutas (controllers)
    - La lógica de negocio (services)
    - El acceso a datos (repositories)
    
    … todo eso en un mismo servidor y proceso.
    
- 🔧 **No significa un solo script** como en un `.py` o `.js`, sino que todo se ejecuta junto, aunque internamente esté bien organizado en capas y carpetas.
- ✅ **Ventajas**: fácil de desarrollar y probar localmente. Se organiza mejor la conexión con la base de datos.
- ❌ **Desventajas**: si crece mucho es difícil de mantener o escalar partes individuales. Para cambiar algo debes redeplegar toda la aplicación.

Todo corre en una sola memoria ram

---

### 2. **Arquitectura de Microservicios**

- **Qué es**: Separás la aplicación en varios **servicios pequeños e independientes**.
- Cada microservicio:
    - Tiene su propia base de datos (en muchos casos).
    - Se despliega por separado.
    - Se comunica con otros microservicios por **API REST**, colas de mensajes u otros mecanismos.
- **Ejemplo práctico**:
    - Un microservicio solo para usuarios.
    - Otro microservicio solo para pagos.
    - Otro microservicio solo para productos.
- ✅ **Ventajas**: escalas solo la parte que necesita más recursos. Si un servicio falla, no cae todo.
- ❌ **Desventajas**: más complejidad en infraestructura y en la comunicación entre servicios.

---

### 3. **Arquitectura Hexagonal (Ports & Adapters)**

- **Qué es**: Un modelo de diseño donde:
    - Tu **núcleo de aplicación (dominio)** no sabe nada del mundo exterior.
    - Se conecta con el exterior (bases de datos, APIs externas) a través de **puertos (interfaces)** y **adaptadores (implementaciones)**.
- **Ejemplo práctico**:
    - Defines una interfaz `RepositorioUsuarios`.
    - El código de tu dominio usa esa interfaz sin importar si es implementada con Hibernate, con JDBC o con un mock para testing.
- ✅ **Ventajas**: testeable, desacoplada de tecnologías externas.
- ❌ **Desventajas**: más diseño y boilerplate inicial, no siempre necesario en proyectos pequeños.

---

### 4. **Arquitectura SOA (Service Oriented Architecture)**

- **Qué es**: Similar a microservicios, pero tradicionalmente implementado dentro de aplicaciones monolíticas grandes con servicios expuestos para integrarse con otros sistemas.
- **Diferencia con microservicios**:
    - SOA es más sobre **servicios compartidos y reusables en toda la organización**.
    - Microservicios es sobre **dividir una app en pequeños servicios deployeables independientemente**.

---

### Resumen

✅ Las arquitecturas **no son formas de escribir un script único**, sino **formas de estructurar la aplicación a nivel de componentes, proyectos y despliegues**.

✅ Monolítico → todo junto en un solo deployable.

✅ Microservicios → separado en múltiples servicios pequeños.

✅ Hexagonal → enfoque de diseño limpio para desacoplar tu dominio.

✅ SOA → servicios compartidos a nivel organizacional.

---

### En detalle

[Microservicios ](Backend%20Organizaci%C3%B3n%20Interna%2022db0f9d9c89806189eadc7a9fb45bd3/Microservicios%2022db0f9d9c89809284adefc9970a0211.md)

[Esquema Arquitectura Microservicios](Backend%20Organizaci%C3%B3n%20Interna%2022db0f9d9c89806189eadc7a9fb45bd3/Esquema%20Arquitectura%20Microservicios%2022db0f9d9c898019bb17c56bbeaff9fa.md)