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
---
# En Detalle

## Microservicios y su esquema de arquitectura

---
# Microservicios

### 🎯 **En una aplicación común hoy en día (ejemplo con Spring Boot en Java)**

🔷 Normalmente se usa una **arquitectura monolítica en capas** organizada así:

```Plain
java
CopiarEditar
MiAplicacion/
 ├── controllers/   → Las rutas (APIs que reciben las solicitudes HTTP)
 ├── services/      → La lógica de negocio (validaciones, reglas, procesos)
 ├── repositories/  → Acceso a datos (consultas a la base de datos)
 ├── models/        → Clases que representan los datos (entidades, DTOs)
 ├── config/        → Configuraciones generales (seguridad, CORS, DB)
 └── Application.java → Clase principal que arranca todo el servidor

```

✅ Todo eso está en **un solo proyecto, un solo deployable (por ejemplo un .jar en Spring Boot)**.

✅ Se dice que es **monolítico** porque cuando ejecutás `java -jar app.jar` levanta **toda la app completa**: APIs, lógica y acceso a DB.

---

### 📝 **Entonces, por qué decimos “monolítico” si está separado en carpetas?**

🔴 **Monolítico no significa mal organizado.** Significa:

- Todo el código corre en el **mismo proceso / servidor**.
- Si cambias algo en servicios o controllers, debes redeplegar toda la aplicación.
- No podés “levantar” solo la parte de usuarios, o solo la parte de pagos.

---

### 💡 **¿Cómo sería en Microservicios en cambio?**

Si usas **Microservicios**:

➡️ Creas **varios proyectos separados**, cada uno con su propio `Application.java`, su base de datos y sus APIs específicas.

Por ejemplo:

```Plain
mathematica
CopiarEditar
Microservicio-Usuarios/
 ├── controllers/
 ├── services/
 ├── repositories/
 └── Application.java

Microservicio-Pagos/
 ├── controllers/
 ├── services/
 ├── repositories/
 └── Application.java

Microservicio-Productos/
 ├── controllers/
 ├── services/
 ├── repositories/
 └── Application.java

```

✅ Cada microservicio se deploya **independiente en un contenedor o servidor distinto**.

✅ Se comunican entre sí por **APIs REST** o mensajería.

✅ Si cae Pagos, Usuarios sigue funcionando.

---

## Es Decir

💡 **Microservicios → proyectos separados con ejecutables separados, literalmente.**

🔷 Cada microservicio es:

- Un proyecto distinto (por ejemplo, un Spring Boot con su propio `pom.xml` o `build.gradle`).
- Se genera su **propio .jar ejecutable**.
- Tiene su propia base de datos (en muchos casos).
- Se despliega en un servidor, contenedor o puerto distinto.

---

🎯 **Monolítico en cambio → un solo ejecutable (.jar) que contiene todo.**

- Aunque esté bien separado en carpetas (controllers, services, repos), al ejecutar el jar se levanta todo junto.

---

## Como se comunican?

### **Microservicios se comunican igual que frontend y backend:**

✅ **Frontend → Backend:**

- El navegador hace solicitudes HTTP (GET, POST, PUT, DELETE) a endpoints expuestos por el backend.

✅ **Microservicio A → Microservicio B:**

- También hace **solicitudes HTTP a endpoints RESTful** del otro servicio.

---

### 🔗 **Comunicación interna en microservicios**

➡️ Cada microservicio expone su propia API (endpoints).

➡️ Otros microservicios consumen esos endpoints cuando necesitan algo.

---

### 📦 **Ejemplo práctico:**

Tienes:

1. **Microservicio Usuarios**
    - Expone `/api/users`, `/api/users/{id}`, etc.
2. **Microservicio Pedidos**
    - Cuando alguien hace un pedido nuevo, este microservicio necesita verificar que el usuario existe.
    - Entonces hace un **HTTP GET** a `usuarios-service/api/users/{id}` para obtener los datos de ese usuario.

---

### 🔧 **Así se ve conceptualmente:**

```Plain
css
CopiarEditar
[Frontend]
   ↓
[API Gateway (opcional)]
   ↓
[Microservicio Pedidos] ←→ [Microservicio Usuarios]
                               ↑
                           (HTTP REST)

```

✅ **La comunicación entre microservicios se hace igual que un front consulta al back.**

---

## Tipos de comunicacion con Endpoints

Cada servicio expone Endpoints para que se produzca la interaccion entre ellos, pueden ser de dos tipos

### Comunicacion Sincrona

Realiza una solicitud a otro microservicio y espera una respuesta antes de continuar

### Comunicacion Asincrona

Un microservicio envia un mensaje a otro y no espera respuesta inmediata

Ejemplo → Microservicio de Reservas publica un evento “ReservaCreada” en una cola de m,ensajes y el micrservicio de notificaciones puede suscribirse a este evento para enviar una confirmacion al usuario

  

## Diagrama visual:

---

```Plain
bash
CopiarEditar
╔════════════════╗
║   Frontend     ║
║ (React, Angular)║
╚═══════╦════════╝
        │ HTTP (solicitudes REST)
        ▼
╔════════════════╗
║  API Gateway   ║
║ (opcional)     ║
╚══════╦═════════╝
       │
       ├───────────────► [Microservicio Usuarios]
       │                    ├── GET /users
       │                    ├── POST /users
       │                    └── ...
       │
       ├───────────────► [Microservicio Productos]
       │                    ├── GET /products
       │                    ├── POST /products
       │                    └── ...
       │
       └───────────────► [Microservicio Pedidos]
                            ├── GET /orders
                            ├── POST /orders
                            └── ...

🔁 **Comunicación interna entre microservicios**

[Microservicio Pedidos] ───── GET /users/{id} ─────> [Microservicio Usuarios]

[Microservicio Pedidos] ───── GET /products/{id} ─────> [Microservicio Productos]

```

---

✅ **Frontend** hace peticiones al **API Gateway**, que distribuye las solicitudes a cada microservicio según corresponda.

✅ **Microservicios** tienen sus propios endpoints REST.

✅ También se comunican entre sí usando **HTTP REST interno** o **mensajería asíncrona**.


# Esquema Arquitectura Microservicios

![[/image 32.png|image 32.png]]

Representa cómo está **dividida la aplicación en microservicios**, cómo se comunican y qué tecnologías usa.

### 🔍 **Componentes principales explicados en simple**

|**Componente**|**Función práctica en el sistema**|
|---|---|
|**Spring Cloud Config Server**|Guarda archivos de configuración centralizados (YAML) para todos los microservicios. Ej: la DB o puertos no se configuran en cada microservicio, sino aquí.|
|**Git Repo**|Donde se guardan esos archivos YAML versionados (para control de cambios).|
|**Eureka Service Discovery**|Permite registrar y descubrir los microservicios automáticamente (sin saber IPs o puertos exactos). Es como un "buscador de microservicios".|
|**UI + API Gateway**|🔥 **Este es clave**. Es la **puerta de entrada única** de toda la aplicación. ✅ Sirve el frontend (HTML, JS, CSS). ✅ Recibe requests y los redirige al microservicio correspondiente. ✅ Ej: si el usuario consulta veterinarios, lo deriva a `vets-service`.|
|**customers-service**|Microservicio REST API que gestiona **clientes** de la veterinaria (dueños de mascotas).|
|**vets-service**|Microservicio REST API que gestiona **veterinarios**.|
|**visits-service**|Microservicio REST API que gestiona **visitas** o consultas de las mascotas.|
|**Spring Boot Admin Server**|Herramienta para **monitorear** los microservicios (estado, memoria, CPU, etc.).|
|**Zipkin Distributed Logging Server**|Herramienta para **ver los logs distribuidos** de todos los microservicios, muy útil para debugging y seguimiento de requests entre servicios.|

---

### **¿Cómo fluye la comunicación?**

1. **El usuario accede al API Gateway (UI + API Gateway)** desde su navegador.
2. El **API Gateway**:
    - Sirve la **interfaz web** al usuario (HTML, CSS, JS).
    - Recibe las peticiones (ej. ver veterinarios) y las envía al microservicio adecuado (`vets-service`) a través de REST.
3. Cada **microservicio**:
    - Se registra en **Eureka** cuando arranca (le dice “acá estoy” con su IP y puerto).
    - Consulta a **Spring Cloud Config Server** para obtener su configuración.
    - Se conecta a su **base de datos propia** (en la parte inferior de cada microservicio se ve su DB).
4. Los logs de todos los microservicios se envían a **Zipkin** para trazabilidad y monitoreo centralizado.
5. **Spring Boot Admin Server** monitorea el estado de cada microservicio.

---

### **¿El API Gateway conecta con el frontend y el backend?**

✅ **Sí.**

- Desde el **punto de vista del usuario**, el API Gateway es el **único backend que ve**, porque ahí accede al sistema.
- Desde el **punto de vista interno**, el API Gateway **redirecciona requests** al microservicio correspondiente.

💡 **Ejemplo práctico real:**

- Frontend ➔ **API Gateway** ➔ `vets-service` ➔ DB de veterinarios

El **API Gateway no implementa lógica de negocio**, solo distribuye las peticiones y puede aplicar seguridad, logging y balanceo de carga.

---

### **Resumen en una frase**

👉 **El API Gateway es el punto de entrada único al sistema (frontend y backend), y cada microservicio implementa una parte específica de la lógica de negocio.**

---

### 📝 **6. Ejemplo visual resumido**

```Plain
less
CopiarEditar
[Usuario Frontend]
        ↓
   [API Gateway]
   /     |      \
[customers][vets][visits]
   ↓       ↓      ↓
  DB      DB     DB

```

✅ Frontend pide info a Gateway

✅ Gateway reenvía la petición al microservicio

✅ Microservicio procesa y consulta su DB

✅ Respuesta sube hasta el frontend

---

  

## Conceptos

## Yaml

Sirven para la conexion a la bd

**Contienen la configuración de conexión a la base de datos**, además de muchas otras configuraciones necesarias para cada microservicio.

Son archivos de **configuración externa** que contienen variables y propiedades que tu aplicación necesita para funcionar, como:

- **Datos de conexión a la base de datos**:
    - URL de la base de datos
    - Usuario
    - Contraseña
    - Driver (por ejemplo, `org.postgresql.Driver` o `com.mysql.cj.jdbc.Driver`)
- Configuraciones de puertos
- Nombres de servicios
- Timeouts
- Credenciales de APIs externas

### **¿Por qué se usan en microservicios?**

En una arquitectura de microservicios como la de **PetClinic**, cada microservicio necesita **su propia configuración.**

✔️ Para **no hardcodear** (escribir) esas configuraciones dentro del código, se guardan en archivos YAML centralizados en el **Spring Cloud Config Server**.

### **Ejemplo práctico de un archivo** `**application.yml**`

```YAML
yaml
CopiarEditar
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/veterinaria
    username: root
    password: 1234
    driver-class-name: com.mysql.cj.jdbc.Driver

server:
  port: 8081

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/

```

**¿Qué hace este archivo?**

1. Configura la **base de datos** (url, user, password, driver)
2. Define el **puerto** donde correrá este microservicio
3. Configura la conexión al **Eureka Service Discovery**