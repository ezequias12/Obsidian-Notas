## 1) ¿Qué es una API (web) y por qué nos importa HTTP?

Una **API** es un contrato para que dos aplicaciones se comuniquen. En web hablamos de **APIs REST** que usan **HTTP** como “carretera”: vos exponés recursos y otros sistemas los consumen mandando pedidos (requests) y recibiendo respuestas (responses) por la red. Esto permite integrar frontends, móviles u otros backends de forma predecible y desacoplada .
HTTP nació para **entregar contenido estático** (archivos) y evolucionó para **servir datos generados dinámicamente**, o sea, no necesariamente “un archivo físico” sino **datos que el servidor produce según la solicitud**. Hoy lo usamos para todo tipo de recursos, no solo páginas HTML .

---

## 2) Recursos y URIs (cómo “nombramos” lo que se expone)

Un **recurso** es “algo con identidad” (usuarios, pedidos, productos, etc.). En la web lo identificamos con una **URI**, que combina el host con un **path** distintivo. La gracia es que la URI **no tiene por qué apuntar a un archivo**: puede representar “el usuario 123” o “la colección de productos filtrados”. El servidor decide qué hacer al recibir la solicitud a esa URI .

Ejemplos de URIs de recursos:

* `/api/v1/users/123` → el usuario 123
* `/api/v1/products?category=books&minPrice=1000` → productos filtrados

---

## 3) Verbos HTTP, CRUD e **idempotencia** (qué hace cada método y por qué)

Para interactuar con recursos usamos **métodos HTTP**. La idea es que el **verbo** cuente la **intención**:

* **GET**: leer (seguro e idempotente → repetirlo no cambia el estado)
* **POST**: crear (no idempotente → dos POST iguales pueden crear dos recursos)
* **PUT**: **reemplazar** el recurso completo (idempotente)
* **PATCH**: **actualizar parcialmente** (no idempotente)
* **DELETE**: eliminar (idempotente: borrar dos veces debería tener efecto solo la primera)  .

Este mapeo de verbo→intención ayuda a que cliente y servidor **hablen el mismo idioma** y que tu API sea predecible al aplicar **CRUD** (Crear, Leer, Actualizar, Eliminar).

---

## 4) Requests y Responses (códigos, headers y cuerpo)

Cada respuesta HTTP trae **un código de estado** (200, 404, 500, etc.), **cabeceras** (metadatos: tipo de contenido, fecha, cookies, etc.) y un **cuerpo** (los datos “en sí”: JSON, HTML, imagen, etc.). Los códigos se agrupan por familia:
**1xx** informativos, **2xx** éxito (ej.: 200 OK), **3xx** redirecciones, **4xx** error del cliente (ej.: 404 Not Found), **5xx** error del servidor (ej.: 500)   .
Si necesitás devolver **binarios** (imágenes, PDFs) y tenés restricciones, podés **codificarlos en Base64** para llevarlos como texto en el cuerpo (o, mejor, servirlos como binario directo cuando sea posible) .

**Códigos comunes bien usados (mini tabla mental):**

* 200 OK: lectura o actualización “con contenido”
* 201 Created: creación OK (ideal incluir **Location** con la URI del nuevo recurso)&#x20;
* 204 No Content: operación OK sin cuerpo (PUT/PATCH/DELETE)&#x20;
* 400/422: error de datos de entrada (malformado/reglas de negocio) &#x20;
* 401/403: falta autenticación / falta permiso&#x20;
* 404: recurso no encontrado&#x20;
* 500: error interno del servidor .
  El apunte incluye una lista resumida para memorizar los más usados .

---

## 5) Diseño de endpoints: recursos, rutas y **versionado**

Al pensar tus endpoints, arrancá por **nombrar recursos**: “users”, “products”, “orders”. Luego asigná **acciones** según los verbos (GET/POST/PUT/PATCH/DELETE). Mantené los paths **sustantivos y en plural**, y usá **parámetros de ruta** para IDs (`/users/{id}`) y **query params** para filtros/paginación (p. ej. `?page=2&size=20`) .

Ejemplo e-commerce (mini guía de estructura):

* **Usuarios**: `GET /api/v1/users/{id}`, `POST /api/v1/users`, `PUT /api/v1/users/{id}`, `DELETE /api/v1/users/{id}`, `GET /api/v1/users`
* **Productos**: `GET /api/v1/products`, `GET /api/v1/products/{id}`, `POST /api/v1/products`, `PUT /api/v1/products/{id}`, `DELETE /api/v1/products/{id}`
* **Pedidos**: idem, según tu dominio  .

Para **versionar**, usá un prefijo como **`/v1`** en la URL para introducir cambios sin romper clientes existentes. Acompañá con **HTTPS** (siempre) y una estrategia clara de **autenticación/autorización** (roles/permisos) .

---

## 6) Paginación, filtros y búsquedas (evitar respuestas gigantes)

En colecciones grandes, **paginá** (offset/limit o page/size) para no devolver miles de filas en un solo response. Permití **filtros y términos de búsqueda** via query params para que el cliente recupere **solo lo necesario**, mejorando rendimiento y usabilidad .

Ejemplos:

* `GET /products?page=2&size=20`
* `GET /orders?status=SHIPPED&from=2025-01-01&to=2025-01-31`

---

## 7) Manejo de errores (consistente y útil)

Definí un **esquema claro** de errores: **código HTTP** correcto + **cuerpo JSON** con `code`, `message`, `details` (si aplica). Devolvé **mensajes explicativos**; al cliente le sirve saber **qué** está mal y **cómo** corregirlo. Mantené **consistencia** en todos los endpoints (mismo formato, mismas reglas)  .

Ejemplo:

```json
{
  "code": "USR_422",
  "message": "El email no tiene un formato válido",
  "details": { "field": "email" }
}
```

---

## 8) Documentación con **OpenAPI/Swagger** (contrato vivo)

A medida que tu API crece, documentarla bien **no es opcional**. Usamos **OpenAPI (OAS)** para describir endpoints, parámetros, modelos, respuestas, seguridad, etc., en **YAML/JSON**. Esa especificación es **la fuente de verdad**: desde ahí se genera **documentación interactiva**, **mocks** y hasta **código cliente/servidor**  .

Herramientas clave:

* **Swagger Editor**: escribir/validar OAS en el navegador y ver la doc al instante. Pasos: entrar al editor, cargar o crear el YAML/JSON, definir **info**, **paths** (endpoints y métodos), **components/schemas** (modelos), exportar o generar esqueleto de código   .
* **Swagger UI**: documentación navegable y “Try it out” para probar la API desde el navegador .
* **Swagger Codegen** y plataformas como **Stoplight/Apicurio/Postman/Insomnia** para colaborar, testear y generar clientes/servidores .

Estructura OAS (a vuelo de pájaro): `openapi` (versión), `info` (metadatos), `servers` (entornos), `paths` (endpoints y **parameters**/**responses**), `components` (**schemas** y **securitySchemes**), `security`, `tags`, `externalDocs`   .

---

## 9) Buenas prácticas (checklist mental)

* **Usá el verbo correcto** según la intención y respetá la **idempotencia** (GET/PUT/DELETE sí; POST/PATCH no) .
* **Status codes bien elegidos** y **mensajes útiles** en el cuerpo. En **POST** exitoso devolvé **201** y si podés **Location** a la nueva URI .
* **Versioná** tus endpoints (`/v1`) y **siempre HTTPS**; sumá **AuthN/AuthZ** acorde (roles, permisos) .
* **Paginá y filtrá** colecciones; no devuelvas “todo” si puede ser enorme .
* **Documentá con OpenAPI**, mantené el contrato **actualizado** y compartilo con el equipo (Swagger UI/Editor) .
* **Consistencia**: mismo formato de error, convenciones de nombres (plural, snake/camel según el equipo), y respuestas previsibles en toda la API (evita sorpresas) .

---

## 10) Mini ejemplos rápidos

**Crear usuario (POST)** → devuelve 201 y Location

```
POST /api/v1/users
Content-Type: application/json

{"name":"Ana","email":"ana@example.com"}
```

Respuesta:

```
201 Created
Location: /api/v1/users/123
{"id":123,"name":"Ana","email":"ana@example.com"}
```

(201 + URI del creado)&#x20;

**Actualizar total (PUT)** vs **parcial (PATCH)**

```
PUT /api/v1/users/123
{"name":"Ana María","email":"ana@example.com"}  // reemplaza completo
```

```
PATCH /api/v1/users/123
{"name":"Ana María"}  // solo cambia ese campo
```

(Idempotencia: PUT sí, PATCH no)&#x20;

**Paginación y filtros**

```
GET /api/v1/products?page=2&size=20&category=books
```

(Paginá y filtrá para respuestas livianas)&#x20;

---

### Lo que tenés que llevarte (en criollo)

* Una **API REST** es un contrato HTTP para manipular **recursos** vía **URIs** y **métodos** claros.
* Elegí bien **verbo** y **código**; devolvé **mensajes útiles** y **estructura consistente**.
* **Paginá/filtrá**; **versioná**; **seguridad** siempre.
* Documentá con **OpenAPI/Swagger** y usalo como contrato vivo del equipo.

¿Seguimos con el **Apunte 15** con el mismo estilo?
