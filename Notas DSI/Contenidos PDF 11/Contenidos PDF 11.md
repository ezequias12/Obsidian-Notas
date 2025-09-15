# Arquitectura de Microservicios (pág. 4 a 12)

## 1. ¿Qué es la Arquitectura de Microservicios?

- Es un **estilo arquitectónico** donde una aplicación se construye como un conjunto de **componentes pequeños, autónomos y poco acoplados**.
- Cada módulo es un **servicio independiente**, que se puede desarrollar, desplegar y mantener por separado.
- Cada servicio **tiene su propia base de datos y lógica de negocio**.
    - Esto genera cierta **redundancia de datos**, pero permite **bajo acoplamiento** (cada servicio evoluciona solo).

🔹 **Ejemplo práctico**:

En un sistema de e-commerce:

- Servicio de **Usuarios** (gestiona registro y login).
- Servicio de **Productos** (gestiona el catálogo).
- Servicio de **Pedidos** (gestiona compras).
- Servicio de **Pagos** (integra con tarjetas o billeteras).
    
    Cada servicio tiene su **propia base de datos** y puede ser desarrollado en distintos lenguajes (Java, Node, Python).
    

![[/image 22.png|image 22.png]]

---

## 2. Diferencia con el Monolito

- En un **sistema monolítico**, toda la lógica de negocio, procesos y datos están empaquetados en un único bloque.
- En **microservicios**, cada parte funciona de forma aislada.

|Aspecto|Monolítico|Microservicios|
|---|---|---|
|Código|Un único código fuente grande|Muchos proyectos/repos separados|
|Procesos|Una aplicación que contiene todo|Varios procesos independientes|
|Infraestructura|Un único despliegue|Cada servicio se despliega por separado|

🔹 **Ejemplo Netflix**:

- Monolítico → todo el código (usuarios, películas, cobros) en una sola app.
- Microservicios → más de **700 servicios**:
    - Uno recomienda películas.
    - Otro cobra la suscripción.
    - Otro gestiona el historial de visualizaciones.
    - Otro maneja los subtítulos.
        
        Si falla el servicio de “subtítulos”, no cae toda la plataforma.
        

![[/image 1 2.png|image 1 2.png]]

![[/image 2 3.png|image 2 3.png]]

---

## 3. Comunicación entre Microservicios

- Cada microservicio expone su funcionalidad a través de una **API (Application Programming Interface)**.
- Lo común es usar **REST** (HTTP) o **mensajería ligera** (RabbitMQ, Kafka, etc.).
- La idea es que cada servicio **solo haga una tarea específica**, pero pueda **interactuar** con otros servicios cuando haga falta.

🔹 **Ejemplo**:

Cuando un usuario compra un producto:

1. El microservicio de **Pedidos** crea la orden.
2. Llama al microservicio de **Pagos** para cobrar.
3. Si el pago es exitoso, avisa al microservicio de **Envíos** para programar la entrega.

![[/image 3 3.png|image 3 3.png]]

---

## 4. ¿Qué significa “Micro”?

- No se refiere literalmente al tamaño en líneas de código.
- Puede ser un servicio pequeño (unas pocas funciones) o bastante grande (mantenido por un equipo entero).
- Lo importante es que cada servicio:
    1. **Tenga una única responsabilidad clara.**
    2. Pueda **desarrollarse y desplegarse de forma independiente**.

---

## 5. Características comunes de la Arquitectura de Microservicios

1. **Composición vía servicios**:
    - En lugar de librerías internas, los módulos se publican como servicios accesibles por APIs.
    - Se busca que las APIs sean de **grano grueso** (operaciones significativas, no métodos internos).
    - Favorece modularidad.
2. **Organización en torno a capacidades de negocio**:
    - Se dividen los servicios según procesos reales del negocio (pedidos, pagos, clientes) y no según capas técnicas.
3. **Productos, no proyectos**:
    - El enfoque es que cada equipo sea dueño de un “producto/servicio” **durante todo su ciclo de vida**, no solo durante el desarrollo.
    - Ejemplo: el equipo de **Pagos** se encarga del servicio de pagos en todas sus versiones, desde la primera hasta las futuras.
4. **Terminales inteligentes y tuberías bobas**:
    - La lógica se deja en los microservicios (“inteligentes”).
    - La red o middleware solo transporta mensajes (“tubería boba”).
    - Ejemplo: en lugar de usar un **bus centralizado muy complejo**, se prefiere que los microservicios se comuniquen con mensajes simples (ej. HTTP + JSON).
5. **Administración descentralizada**:
    - No hay un único “jefe” tecnológico.
    - Cada equipo puede elegir el mejor stack para su microservicio (lenguaje, framework, BD).
    - Eso sí, deben respetar los contratos de las APIs.
6. **Gestión de datos descentralizada**:
    - Cada servicio maneja su propia base de datos (**persistencia independiente**).
    - Se evita compartir una única base de datos entre todos.
    - Cuando se necesita consistencia, se usan patrones como **eventual consistency** o **operaciones compensatorias**.
7. **Automatización de la infraestructura**:
    - CI/CD, despliegue automatizado, testing automático, monitoreo constante.
    - Herramientas típicas: Docker, Kubernetes, Jenkins, GitHub Actions.
8. **Diseño para fallas**:
    - Se asume que las fallas ocurren.
    - Se diseñan mecanismos para detectarlas, aislarlas y recuperarse rápido.
    - Ejemplo: Circuit Breaker, Retry, Monitoreo en tiempo real.

---

📌 **Hasta acá (pág. 4 a 12) se cubren los fundamentos básicos de microservicios: definición, comparación con monolito, forma de comunicación, características principales.**

---

¿Querés que a esta parte le agregue también un **mapa conceptual** con flechas tipo “Monolito vs Microservicios” + “Características clave” para que lo tengas visual y resumido?