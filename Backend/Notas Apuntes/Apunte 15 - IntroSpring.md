# Introducción a Spring Framework

![Diagrama introductorio de Spring Framework](../../img/Pasted%20image%2020250917235408.png)

## 1) ¿Qué es Spring y por qué lo usa “todo el mundo” en Java?

**Spring** es un framework de código abierto que te acelera el desarrollo de apps Java, sobre todo web y APIs REST. No viene en la biblioteca estándar, pero en la práctica es un estándar de facto en la comunidad porque simplifica muchísimo el trabajo del día a día. Además trae un ecosistema enorme: acceso a datos (JDBC/JPA), seguridad, transacciones e integración con otros frameworks (Hibernate, Thymeleaf, JUnit, etc.).

Idea clave en criollo: **menos código repetitivo, más foco en la lógica del negocio**.

> Contexto moderno: Spring fue evolucionando (3.x → 6.x). Hoy conviven el modelo “clásico” y el **reactivo** (WebFlux) para apps no bloqueantes. A partir de Spring 6/Boot 3, Java 17 es requisito y Jakarta reemplaza a `javax.*`.

---

## 2) El **contenedor de Spring**, los **beans** y la **Inyección de Dependencias (DI)**

Spring tiene un **contenedor** que **crea** tus objetos (beans), **los conecta** entre sí y **maneja su ciclo de vida**. El corazón del framework es la **Inversión de Control** + **Inyección de Dependencias (IoC/DI)**: *vos declarás “qué necesitás”, Spring te lo inyecta; no andás haciendo `new` por todos lados*.

**IoC** -> (Inversión de Control) significa que el control de creación y gestión de objetos lo maneja Spring, no vos.
**DI** -> (Inyección de Dependencias) es el patrón que permite pasar las dependencias (objetos que una clase necesita) desde afuera, en lugar de que la clase las cree por sí misma.

Osea lo que busca Spring es que en una implementacion normal no tengas que estar creando instancias de las clases que necesitas, sino que Spring lo haga por vos y te las inyecte.
Con estas Instancias nos referimos a los objetos que se crean a partir de una clase.

* **Beans**: son los objetos “administrados” por Spring; se pueden definir por XML o por anotaciones en POJOs (lo más común hoy). 
* **¿Por qué DI?** Porque lográs **código desacoplado y testeable**: cambiás implementaciones sin tocar la clase que las usa, y en tests podés inyectar mocks.

Ejemplo “anti-DI” (acopla fuerte):

Aca creamos una clase GeneradorPlaylist que tiene dentro un atributo que va a ser del tipo BuscadorCanciones, con el nombre buscadorCanciones.

Osea, **GeneradorPlaylist** tiene un atributo que es de tipo **BuscadorCanciones**. Esto significa que cada objeto **GeneradorPlaylist** tendrá su propio objeto **BuscadorCanciones** asociado.

Luego esta el constructor de la clase, GeneradorPlaylist 
->dentro del constructor es donde se inicializa el atributo buscadorCanciones.
new BuscadorCanciones(): Aquí es donde se crea una nueva instancia (un nuevo objeto) de la clase BuscadorCanciones.
this.buscadorCanciones = ...: Este nuevo objeto BuscadorCanciones se asigna al atributo buscadorCanciones del objeto GeneradorPlaylist que se está creando en ese momento.


```java
public class GeneradorPlaylist {
    private BuscadorCanciones buscadorCanciones;
    public GeneradorPlaylist() {
        this.buscadorCanciones = new BuscadorCanciones(); // Acoplamiento
    }
}
```

El problema con la línea **this.buscadorCanciones** = new **BuscadorCanciones()**; es que **GeneradorPlaylist** es directamente responsable de crear su **BuscadorCanciones**. Esto genera un fuerte acoplamiento:

* Si en el futuro quieres usar una versión diferente de **BuscadorCanciones** (por ejemplo, una para pruebas que no acceda a una base de datos real), tendrías que modificar el código de **GeneradorPlaylist**.
* **GeneradorPlaylist** está "atada" a una implementación específica de **BuscadorCanciones**.

La Inyección de Dependencias (DI), que Spring promueve, resuelve esto. En lugar de que **GeneradorPlaylist** cree su **BuscadorCanciones**, simplemente declara que lo necesita (como en el ejemplo del constructor con DI):

```java
public class GeneradorPlaylist {
    private final BuscadorCanciones buscador;
    public GeneradorPlaylist(BuscadorCanciones buscador) {
        this.buscador = buscador;
    }
}
```

En este segundo ejemplo, **GeneradorPlaylist** ya no se preocupa por cómo se crea **BuscadorCanciones**, solo sabe que lo necesita y que alguien (Spring) se lo proporcionará. Esto hace que el código sea más flexible, modular y fácil de probar.

---

## 3) **Spring vs Spring Boot** (qué agrega Boot)

Antes, armar un proyecto Spring requería **muchísima configuración** (XMLs, wiring, servidor, dependencias). **Spring Boot** aparece para **auto-configurar** lo común, darte **starters** (packs de dependencias), **servidor embebido** (Tomcat por defecto) y ayudarte a salir andando rápido.

En una tabla simple:

* Configuración: **Spring** manual / **Boot** auto-configurado.
* Arranque: **Spring** gradual / **Boot** “proyecto listo” (starter).
* Servidor: **Spring** lo configurás / **Boot** ya trae Tomcat embebido.
* Dependencias: **Spring** a mano / **Boot** con *starters*.

---

## 4) Crear un proyecto con **Spring Initializr** (pasos y qué elegir)

Entrás a **start.spring.io** y elegís:

* **Maven o Gradle**, lenguaje (Java en nuestro caso), **versión de Spring Boot** (preferí la **estable más nueva**).
* **Group** y **Artifact** (definen los paquetes), empaquetado **JAR/WAR** y la **versión de JDK**. Podés marcar dependencias frecuentes: *Spring Web*, *DevTools*, *Lombok*, etc. Bajás el ZIP y lo abrís en tu IDE.

---

## 5) “Hola Mundo” API y **servidor embebido**

Al ejecutar el proyecto Boot, se **levanta un servidor** (Tomcat) en el **puerto 8080**. El log dice algo como:
`Tomcat started on port(s): 8080 (http) ...`.
Si entrás a `http://localhost:8080` y no programaste endpoints, verás un mensaje de error **de la app**, señal de que el servidor está vivo.

* **Cambiar el puerto**: `server.port=9090` en `application.properties` (o YAML).
* **Actuator** (opcional): agrega endpoints de monitoreo como `/actuator/health` y `/actuator/info`.

---

## 6) **Rutas y controladores** (tu primera API REST)

Un **endpoint** es una URL + verbo HTTP para interactuar con un recurso (GET/POST/PUT/DELETE). En Spring se programan en **controllers** con anotaciones como `@RestController`, `@GetMapping`, etc..

Ejemplo: **GET /saludo** que devuelve “Hola, mundo”:

```java
@RestController
public class HolaMundoController {
  @GetMapping("/saludo")
  public String saludar() { return "Hola, mundo"; }
}
```

Esto lo corrés y probás en `http://localhost:8080/saludo`.

---

## 7) Pasar datos al endpoint: **Path Variables**, **Query Params** y **Request Body**

**a) Path Variable**: datos “metidos” en la ruta (obligatorios).
`/hola/{nombre}` → `@PathVariable String nombre`

```java
@GetMapping("/hola/{nombre}")
public String saludar(@PathVariable String nombre) {
  return "¡Hola, " + nombre + "!";
}
```

Si llamás `/hola/Maria`, responde “¡Hola, Maria!”. La idea es declarar la parte variable entre llaves y Spring la inyecta en el parámetro del método.

**b) Query String**: datos opcionales después de `?` (orden intercambiable).
`/saludar?nombre=John` → `@RequestParam("nombre") String nombre`

```java
@GetMapping("/saludar")
public String saludarConParam(@RequestParam("nombre") String nombre) {
  return "Hola " + nombre;
}
```

Trae el parámetro de la URL y permite `required`, `defaultValue`, etc..

**c) Request Body (POST/PUT)**: el **cuerpo** de la petición (generalmente JSON) se mapea a un objeto Java con `@RequestBody`. Útil para crear/editar recursos.

> Tip de prueba: para endpoints que **no son GET**, usá Postman/Bruno y armá requests con body JSON.

---

## 8) **Scopes de Beans** (ciclo de vida/alcance)

Spring define **cómo** y **cuándo** se crea un bean:

* **singleton** (default): una única instancia para toda la app.
* **prototype**: una nueva instancia cada vez que se pide.
* **request**: una por **request HTTP** (web).
* **session**: una por **sesión HTTP** (web).

Ejemplo didáctico (contadores en distintos scopes, expuestos por endpoints):

* `GET /visit/singleton` suma un contador **global**;
* `GET /visit/prototype` arranca desde cero para cada instancia;
* `GET /visit/request` empieza en cero por **cada request**;
* `GET /visit/session` mantiene el conteo por **sesión HTTP**.

Esto te deja **ver** la diferencia haciendo varias llamadas desde el navegador o Postman.

---

## 9) Arquitectura en capas (cómo ordenar el proyecto)

Para que el proyecto escale y se entienda, se suele separar en capas:

* **Controllers**: reciben requests y devuelven respuestas.
* **Services**: lógica de negocio.
* **Repositories**: acceso a datos (guardar/buscar/actualizar/eliminar).
* **Models**: entidades del dominio (lo que representás en la base).
* (Opcional) **DTOs**: objetos para **entrar/salir** por la API.

Beneficio: **separación de responsabilidades** y **mantenimiento** más fácil.

---

## 10) Mini-proyecto “Personas” (ejemplo de cierre)

Se propone una API REST para gestionar **Personas** (id, nombre, fechaNacimiento, esExtranjero) con endpoints:

* **POST /personas** (crea)
* **GET /personas** (lista)
* **GET /personas/{id}** (detalle)

Se persiste **en memoria** con un `HashMap<Integer, Persona>` (patrón **Singleton** para compartir el almacén). La doc se puede ver con **Swagger** en `/swagger-ui.html`.

---

## 11) BONUS: arrancar una app de consola con Boot (inyección en consola)

También podés usar Boot fuera del contexto web: definís componentes con `@Component`, arrancás la app y ejecutás lógica con `CommandLineRunner` (inyección por constructor o con `@Autowired`). Es útil para jobs, migraciones, seeds, etc.

---

## 12) En síntesis (lo que te llevás)

* **Spring** te da el contenedor (IoC/DI) para **desacoplar** y **testear mejor**.
* **Spring Boot** te quita fricción: **autoconfig**, **starters**, **Tomcat embebido**, proyecto andando en minutos.
* **Endpoints** con `@RestController` + `@Get/PostMapping`. Parámetros por **path** (`@PathVariable`), **query** (`@RequestParam`) y **body** (`@RequestBody`).
* **Servidor**: Tomcat en 8080 por defecto; cambiás con `server.port` y podés habilitar **Actuator** para monitoreo.
* **Scopes**: `singleton`, `prototype`, `request`, `session` para manejar **ciclo de vida** de beans.
* **Capas**: controllers ↔ services ↔ repositories ↔ models (+ DTOs), separación clara y mantenible.

Si querés, te armo enseguida un **mini proyecto base** con:

1. `HelloController` (`/saludo`, path y query param),
2. `PersonasController` con DTO y un repo en memoria,
3. `application.properties` con `server.port=9090`,
4. y Actuator habilitado para `/actuator/health`.
