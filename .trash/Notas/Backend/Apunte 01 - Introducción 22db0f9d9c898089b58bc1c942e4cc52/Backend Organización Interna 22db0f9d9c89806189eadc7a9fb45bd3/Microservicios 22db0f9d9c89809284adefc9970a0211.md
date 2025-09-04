# Microservicios

### 🎯 **En una aplicación común hoy en día (ejemplo con Spring Boot en Java)**

🔷 Normalmente se usa una **arquitectura monolítica en capas** organizada así:

```
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

```
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

```
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

```
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