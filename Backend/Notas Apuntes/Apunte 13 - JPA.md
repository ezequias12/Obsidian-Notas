
## 1) ¿Qué es JPA y por qué conviene (o no)?

*   **JPA (Jakarta Persistence API)** es un **estándar de mapeo objeto-relacional (ORM)** en Java.
*   **Idea principal:** Permite a los desarrolladores trabajar con **objetos (entidades)** en su código Java, y el ORM se encarga de **traducir** esas operaciones a **SQL** para interactuar con la base de datos.
*   **Ventajas:**
    *   Reduce la cantidad de **SQL repetitivo** que hay que escribir.
    *   Ofrece **portabilidad** entre diferentes motores de base de datos (cambias la configuración, no el código).
    *   Centraliza y limpia el **modelo de dominio** en objetos Java.
*   **Desventajas/Consideraciones:**
    *   Al ocultar el SQL, puede llevar a generar **consultas poco eficientes** si no se entiende lo que el ORM hace por debajo.
    *   En casos complejos o para optimizaciones muy específicas, a veces es necesario recurrir a **SQL nativo**.
*   **JPA vs. Hibernate:** JPA es el **estándar** (la especificación, las interfaces y anotaciones), mientras que **Hibernate** es la **implementación más usada** de ese estándar. Con JPA se define "qué" hacer, y Hibernate se encarga de "cómo" ejecutar el SQL real.
*   **Contexto histórico:** Evolucionó de los complejos Entity Beans (EJB 2.x) a una API más simple, hoy conocida como Jakarta Persistence.

---

## 2) Los 3 ejes que tenés que dominar

El apunte estructura el aprendizaje de JPA en tres áreas fundamentales:

1.  **Configuración:** Cómo se conecta la aplicación a la base de datos, qué proveedor ORM se usa, cómo se manejan las transacciones y otras propiedades.
2.  **Mapeo:** Cómo se define la correspondencia entre las clases Java (entidades) y las tablas de la base de datos, incluyendo atributos y relaciones.
3.  **Administración y Consultas:** Cómo se gestionan las entidades (CRUD) y cómo se realizan consultas utilizando el `EntityManager` y JPQL.

**Analogía:** Primero se **prende el motor** (configuración), luego se **le enseña el mapa** (mapeo) y finalmente se **maneja** (administración y consultas).


---

# 3) Configuración (`persistence.xml`) — H2 para practicar, Postgres para “real”

*   La configuración principal se define en el archivo `META-INF/persistence.xml`.
*   Aquí se especifica la **unidad de persistencia**, incluyendo:
    *   **`provider`:** La clase del proveedor ORM (ej. Hibernate).
    *   **Datos JDBC:** `jdbc driver/url/user/pass` (similar a JDBC).
    *   **`hibernate.dialect`:** Indica a Hibernate el tipo de base de datos para generar SQL compatible.
    *   **`hibernate.hbm2ddl.auto`:** Estrategia para la gestión del esquema de la base de datos:
        *   `none`: No hace nada.
        *   `validate`: Valida que el esquema de la BD coincida con las entidades.
        *   `update`: Actualiza el esquema de la BD (agrega columnas, etc.).
        *   `create`: Crea el esquema desde cero.
        *   `create-drop`: Crea el esquema y lo elimina al cerrar el `EntityManagerFactory`.
        *   **Recomendaciones:** En desarrollo con H2, `create-drop` es útil. En producción, **evitar `create` y `create-drop`**, usando `validate` o `update` con precaución.
    *   **`transaction-type`:**
        *   `RESOURCE_LOCAL`: Las transacciones son manejadas por el desarrollador (`EntityTransaction`). Común en aplicaciones standalone.
        *   `JTA`: Las transacciones son manejadas por un contenedor (ej. servidor de aplicaciones).
*   **Ejemplo H2 (labs):** Se suele usar `create-drop` y `show_sql=true` para ver el SQL generado.
*   **Ejemplo Postgres (producción):** `validate` o `update`.
*   **Tips:** `DB_CLOSE_DELAY=-1` para H2 en memoria; en producción, usar **pool de conexiones** y cerrar el `EntityManagerFactory` al finalizar la aplicación.

---

# 4) Mapeo: de tabla ↔ entidad (anotaciones básicas y relaciones)

Define cómo las clases Java se corresponden con las tablas de la base de datos.

*   **Clase ↔ Tabla:**
    *   `@Entity`: Marca una clase como una entidad JPA.
    *   `@Table(name="...")`: Opcional, si el nombre de la tabla difiere del nombre de la clase.
*   **Atributo ↔ Columna:**
    *   `@Column(name="...", length=..., nullable=...)`: Opcional, para configurar el nombre, longitud, nulabilidad de la columna.
*   **Clave Primaria (PK):**
    *   `@Id`: Marca el atributo como la clave primaria.
    *   `@GeneratedValue(strategy=...)`: Indica que la PK es generada por la base de datos (ej. `IDENTITY` para autoincrementales).
*   **Requisitos mínimos de una entidad:**
    *   `@Entity`
    *   **Constructor por defecto** (sin argumentos, público o protegido).
    *   Tener un atributo `@Id`.
    *   Clase pública y no `final`.
    *   No usar `static` o `transient` en campos persistentes.
*   **Relaciones (FKs):**
    *   `@ManyToOne`: Muchos a uno (ej. muchos `Track` a un `Album`).
    *   `@OneToMany`: Uno a muchos (ej. un `Album` a muchos `Track`). **Usar con cuidado** por el problema N+1 y cargas grandes.
    *   `@ManyToMany`: Muchos a muchos (ej. muchos `Playlist` a muchos `Track`). En la práctica, a menudo se modela la **tabla intermedia como una entidad** para añadir atributos.
    *   `@JoinColumn(name="...")`: Especifica la columna de la clave foránea.
    *   `fetch = FetchType.LAZY`: **Buena práctica** para cargar las relaciones solo cuando se acceden, evitando cargas innecesarias (problema N+1).
*   **Ejemplo de `Artist` y `Track`:** (Ver el código en el apunte para las anotaciones `@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@Column`, `@ManyToOne`, `@JoinColumn`, `fetch = FetchType.LAZY`).
*   **Buenas prácticas de mapeo:**
    *   Alinear nombres/tipos con el esquema de la BD.
    *   Usar `BigDecimal` para valores monetarios.
    *   Aplicar **Bean Validation** (ej. `@NotNull`) en los atributos.
    *   `fetch = LAZY` por defecto y controlar la carga con JPQL.


**Ejemplo simple (Artist / Track con N→1):**

```java
@Entity
@Table(name = "ARTIST")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
@EqualsAndHashCode(of = "id")
public class Artist {
  @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
  @Column(name = "ARTIST_ID")
  private Integer id;

  @Column(name = "NAME", length = 160, nullable = false)
  private String name;
}
```

```java
@Entity
@Table(name = "TRACK")
@Data @NoArgsConstructor @AllArgsConstructor @Builder
@EqualsAndHashCode(of = "id")
public class Track {
  @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
  @Column(name = "TRACK_ID")
  private Integer id;

  @Column(name = "NAME", nullable = false) private String name;

  @ManyToOne(fetch = FetchType.LAZY, optional = false)
  @JoinColumn(name = "ALBUM_ID", nullable = false)
  @ToString.Exclude
  private Album album;

  @ManyToOne(fetch = FetchType.LAZY) @JoinColumn(name = "GENRE_ID")
  @ToString.Exclude
  private Genre genre;

  @ManyToOne(fetch = FetchType.LAZY, optional = false)
  @JoinColumn(name = "MEDIA_TYPE_ID", nullable = false)
  @ToString.Exclude
  private MediaType mediaType;

  @Column(name = "MILLISECONDS", nullable = false) private Integer milliseconds;
  @Column(name = "UNIT_PRICE", nullable = false) private BigDecimal unitPrice;
}
```

**Uno→Muchos (1→N):** existe pero **usalo con cuidado**: puede disparar cargas grandes y el famoso **N+1**. Mejor **consultas específicas** con JPQL si la colección puede ser grande; ojo con `equals/hashCode` y con `cascade=ALL` que puede hacer inserts/updates/eliminaciones en masa sin querer.

**Muchos↔Muchos (M↔M):** se puede con `@ManyToMany` + `@JoinTable`, pero **en práctica conviene modelar la tabla intermedia como entidad** (p. ej. `PlaylistTrack`) para guardar metadatos (orden, fecha).

**Buenas prácticas de mapeo:** alinear nombres/tipos con el esquema; **`BigDecimal`** para dinero; poner **Bean Validation** (p. ej. `@NotNull`) donde corresponda; **`fetch = LAZY`** por defecto y controlar la carga con JPQL/joins.

---

# 5) EntityManager y ciclo de vida de las entidades (¡clave!)

El **EntityManager** gestiona un **contexto de persistencia** con entidades **Managed**; dentro de ese contexto, los cambios a objetos **se detectan** y se sincronizan (en `commit` o `flush`).

**Estados:**

* **NEW (Transient)**: objeto nuevo, **no** está en la BD ni en el contexto. `persist()` lo vuelve **Managed**.
* **MANAGED (Persistente)**: está dentro del contexto; cambios se sincronizan en `commit/flush`; `remove()` lo marca **Removed**; `detach()` lo pasa a **Detached**.
* **DETACHED**: estuvo Managed pero ya no (cerraste EM, fin de tx, o `detach()`). Cambios **no** se sincronizan; `merge()` crea **una copia Managed** con esos cambios.
* **REMOVED**: marcada para borrar; al `commit` hace **DELETE**.

*   **Métodos frecuentes del `EntityManager`:**
    *   `persist(e)`: Guarda un objeto **NEW** en la BD (INSERT) y lo hace **Managed**.
    *   `merge(e)`: Toma un objeto **Detached** (o NEW), aplica sus cambios a una **nueva instancia Managed** y la devuelve. Puede insertar si no existía.
    *   `remove(e)`: Marca un objeto **Managed** para ser borrado (DELETE).
    *   `detach(e)`: Desvincula un objeto **Managed** del contexto, pasándolo a **Detached**.
    *   `refresh(e)`: Recarga un objeto **Managed** desde la BD, sobrescribiendo cualquier cambio local no sincronizado.
    *   `flush()`: Sincroniza los cambios pendientes del contexto con la BD (ejecuta SQL) sin finalizar la transacción.
    *   `find(C, id)`: Busca una entidad por su PK. Primero mira la **caché de primer nivel**; si no está, va a la BD. Devuelve **Managed** o `null`.
*   **Caché de primer nivel:** El `EntityManager` mantiene una caché interna. Si se pide un objeto por `find` y ya está en la caché, se devuelve la misma instancia sin ir a la BD.
*   **Obtener `EntityManager` / `EntityManagerFactory`:**

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("MiUnidad");
EntityManager em = emf.createEntityManager();
```

En entornos como Spring, el `EntityManager` suele ser inyectado (`@PersistenceContext`) y gestionado por el contenedor.

---

# 6) CRUD “de todos los días”

* **Create**:

  ```java
  em.getTransaction().begin();
  em.persist(nuevoUsuario);
  em.getTransaction().commit();
  ```
* **Retrieve**:

  ```java
  Usuario u = em.find(Usuario.class, id); // devuelve Managed o null
  ```
* **Update** (si está Detached):

  ```java
  em.getTransaction().begin();
  Usuario m = em.merge(uDetached); // m es Managed
  em.getTransaction().commit();
  ```
* **Delete**:

  ```java
  em.getTransaction().begin();
  Usuario u = em.find(Usuario.class, id); //primero se busca para que sea managed
  em.remove(u);
  em.getTransaction().commit();
  ```

Todo esto con **EntityManager** y **transacción** (`begin/commit`).

---

# 7) JPQL bien usado (consultas “a objetos”, no a tablas)

**JPQL (Jakarta Persistence Query Language)** es un lenguaje de consulta orientado a **entidades y sus relaciones** (no a tablas SQL). Hibernate lo traduce a SQL nativo.
Claves: alias (`from Album a`), navegar relaciones (`a.artist.name`), **parámetros nombrados** (`:q`), proyecciones (entidad completa, campos, **DTO con constructor**).

**Básica:**

```jpql
select a from Album a
  where a.title like :title
  order by a.id
```

Podés omitir `select` si pedís la entidad completa: `from Album a`.

**Joins (navegación por relaciones):**

* `join` (inner): `select a from Album a join a.artist ar`
* `left join`: `select t from Track t left join t.genre g`
* `join` con filtro: `... where a.id = :id`.

**Fetch join** (evitar N+1):

```jpql
select distinct a
from Album a left join fetch a.tracks
where a.id = :id
```

Trae la colección asociada **en la misma consulta**; usar con cuidado si es enorme.

**Paginado y parámetros:**

```java
List<Album> page = em.createQuery(
  "select a from Album a where a.title like :q order by a.id", Album.class)
  .setParameter("q", "%black%")
  .setFirstResult(0)      // offset
  .setMaxResults(10)      // tamaño de página
  .getResultList();
```

`getSingleResult()` puede tirar `NoResult` o `NonUnique`; **mejor** `getResultList()` y chequear vacío.

**Proyecciones útiles:** a entidad, a escalares (`List<Object[]>`), o **DTO** con “constructor expression”:

```jpql
select new utnfc.back.isi.jpa.dto.AlbumSummary(a.id, a.title, ar.name)
from Album a join a.artist ar
where a.title like :q
```

y el DTO:

```java
public record AlbumSummary(Integer id, String title, String artistName) {}
```

.

---

# 8) Mini recetas típicas (con buenas prácticas)

* **Persistir respetando N→1** (primero “lado uno”, luego “muchos” que referencian):

  ```java
  em.getTransaction().begin();
  Artist artist = Artist.builder().name("AC/DC").build(); em.persist(artist);
  Album album = Album.builder().title("Back in Black").artist(artist).build(); em.persist(album);
  // ...
  em.getTransaction().commit();
  ```

* **Consultar y entender LAZY**: Acceder a una colección o entidad marcada como `LAZY` puede disparar una nueva consulta a la BD en el momento del acceso.

  ```java
  Album found = em.find(Album.class, album.getId());
  System.out.println(found.getTracks().size()); // puede disparar un SELECT (LAZY)
  ```

* **Evitar N+1**: preferí **fetch join** cuando querés traer una colección asociada en la misma consulta (pero filtrada/paginada con cuidado).

---

# 9) Checklist mental rápido

* **Config**: `persistence.xml` con driver/URL/credenciales, `dialect`, `hbm2ddl.auto` (H2: `create-drop`; Postgres: `validate/update`), `show_sql` en dev, `RESOURCE_LOCAL` en labs.
* **Mapeo**: `@Entity/@Id/@GeneratedValue`, `@Table/@Column` si difieren, `@ManyToOne`/`@OneToMany`/`@ManyToMany`. **LAZY por defecto**; dinero con `BigDecimal`; validaciones donde suma.
* **EM & ciclo de vida**: saber **NEW/MANAGED/DETACHED/REMOVED** y qué hace **persist/merge/remove/detach/refresh/flush/find** (y la caché de 1er nivel).
* **CRUD**: siempre con **transacción** (begin/commit).
* **JPQL**: alias, rutas (`a.artist.name`), parámetros nombrados, **fetch join** para evitar N+1, **DTO projection** para respuestas livianas, **paginado**.

---
---
# Importante para entender y conectado con el cuadro de clase

La nota [[Cuadro de clase]] da una pista muy importante: **no necesitas memorizar cómo programar cada línea de JDBC**, sino **entender qué hace y por qué es la base de todo lo que viene después** (como JPA/Hibernate).

Pensemos en JDBC como las "tuberías" que conectan tu aplicación Java con la base de datos. Es el nivel más fundamental.

**Conexión entre JDBC y JPA:**
*   **JDBC es el "cómo" de bajo nivel:** Es la API directa para enviar SQL a la base de datos. Piensa en ello como las "tuberías" o el "lenguaje de máquina" para hablar con la BD.
*   **JPA es el "qué" de alto nivel:** Te permite trabajar con objetos Java y JPA (a través de su implementación como Hibernate) se encarga de generar y ejecutar el SQL de JDBC por ti. Es una capa de abstracción que simplifica el desarrollo.
### 1. ¿Para qué sirve JDBC? (La idea principal)

Imagina que tu programa Java necesita guardar o leer información (usuarios, productos, etc.). Esa información vive en una **base de datos relacional** (como PostgreSQL o H2).

**JDBC (Java Database Connectivity)** es simplemente el **puente** o el "traductor" estándar que permite a tu código Java enviar comandos SQL a la base de datos y recibir las respuestas.

Sin JDBC, tendrías que escribir código específico para cada motor de base de datos, lo cual sería un caos. JDBC te da una API única para hablar con casi cualquier base de datos.

### 2. Los 5 Pasos Clave para Usar JDBC (El "Hola Mundo")

Tu apunte lo resume perfectamente. Hacer una consulta con JDBC siempre sigue estos 5 pasos. Piénsalo como hacer una llamada telefónica:

1.  **Cargar el Driver (Marcar el número correcto)**: Antes, tenías que decirle a Java "¡Oye, voy a hablar con una base de datos H2, carga las herramientas para H2!". Hoy, como dice la nota, esto es casi siempre automático si tienes el `.jar` (el driver) en tu proyecto.
2.  **Abrir la Conexión (Establecer la llamada)**: Usas `DriverManager.getConnection(...)` para conectarte a la base de datos. Le pasas la "dirección" (la URL JDBC), un usuario y una contraseña.
    ```java
    // "Hola, quiero conectarme a una base de datos H2 en memoria llamada chinook"
    Connection conn = DriverManager.getConnection("jdbc:h2:mem:chinook", "sa", "");
    ```
3.  **Crear una Sentencia (Preparar tu mensaje)**: Una vez conectado, necesitas un "mensajero" para llevar tu consulta SQL. Ese mensajero es un `Statement` o, mejor aún, un `PreparedStatement`.
4.  **Ejecutar la Consulta y Leer Resultados (Enviar el mensaje y escuchar la respuesta)**: Le das tu SQL al mensajero y lo envías.
    *   Si es una consulta (`SELECT`), usas `executeQuery()` y te devuelve un `ResultSet` (una especie de tabla con los resultados) que recorres fila por fila con `rs.next()`.
    *   Si es una modificación (`INSERT`, `UPDATE`, `DELETE`), usas `executeUpdate()`.
5.  **Cerrar los Recursos (Colgar la llamada)**: ¡Esto es crucial! Tienes que cerrar la conexión, el statement y el resultset para liberar recursos. La forma moderna y segura de hacerlo es con `try-with-resources`, que los cierra automáticamente por ti.

### 3. `Statement` vs. `PreparedStatement` (La diferencia más importante)

Esta es quizás la lección más práctica del apunte.

*   **`Statement`**: Es como escribir tu consulta SQL en un papel y dársela al mensajero. Es simple, pero peligroso si incluyes datos que vienen de un usuario.
    *   **El Peligro (Inyección SQL)**: Si un usuario en un campo de texto escribe `'; DROP TABLE users; --`, y tú simplemente pegas eso en tu consulta, ¡podría borrarte la tabla!

*   **`PreparedStatement`**: Es como darle al mensajero una plantilla con espacios en blanco (`?`) y luego decirle qué poner en cada espacio.
    ```java
    // Plantilla: "SELECT * FROM TRACK WHERE ALBUM_ID = ?"
    String sql = "SELECT * FROM TRACK WHERE ALBUM_ID = ?";
    PreparedStatement ps = conn.prepareStatement(sql);
    // Lleno el espacio en blanco de forma segura:
    ps.setInt(1, 5); // El primer '?' es el valor 5
    ```
    **Ventajas Clave**:
    1.  **Seguridad**: **Evita la inyección SQL**. Trata los datos del usuario como datos, no como parte del comando SQL.
    2.  **Eficiencia**: La base de datos puede "pre-compilar" la plantilla y reutilizarla, haciéndola más rápida si la ejecutas muchas veces con diferentes valores.

> **Regla de oro de tu apunte**: ¿Tu consulta necesita parámetros (especialmente si vienen del usuario)? **Usa siempre `PreparedStatement`**.

### 4. Transacciones (Hacer varias cosas como si fueran una sola)

Imagina una transferencia bancaria:
1.  Restar $100 de la cuenta A.
2.  Sumar $100 a la cuenta B.

¿Qué pasa si el paso 1 funciona pero el sistema se cae antes del paso 2? ¡El dinero desaparece!

Una **transacción** agrupa estas dos operaciones. O se hacen las dos con éxito (`commit()`), o si algo falla, se deshace todo (`rollback()`) y se vuelve al estado inicial.

Por defecto, JDBC está en modo `autocommit=true` (cada operación se confirma al instante). Para una transacción, debes:
1.  Desactivarlo: `conn.setAutoCommit(false);`
2.  Hacer tus operaciones (tus `executeUpdate`).
3.  Si todo fue bien: `conn.commit();`
4.  Si algo falló: `conn.rollback();`

### 5. Pool de Conexiones (No llamar a la base de datos cada vez)

Abrir y cerrar una conexión a la base de datos es un proceso lento y costoso (consume tiempo y recursos).

En una aplicación real con muchos usuarios, no puedes crear una nueva conexión para cada petición. Sería un desastre.

**Solución**: Un **Pool de Conexiones** (como HikariCP).
*   Es una "piscina" de conexiones ya abiertas y listas para usar.
*   Cuando tu código necesita una conexión, la "pide prestada" del pool.
*   Cuando terminas, en lugar de cerrarla, la "devuelves" al pool para que otro la use.

Esto hace que tu aplicación sea muchísimo más rápida y eficiente.

### En Síntesis (Lo que te tienes que llevar del apunte)

*   **JDBC** es la API base de Java para hablar con bases de datos usando SQL.
*   El flujo es: **Conectar -> Preparar SQL -> Ejecutar -> Leer -> Cerrar**.
*   **`PreparedStatement` es tu mejor amigo** para la seguridad y el rendimiento.
*   Las **Transacciones** garantizan que un

#### Sources:

- [[Apunte 12 - JDBC]]
- [[Apunte 13]]
- [[Cuadro de clase]]
- [[Apunte 10 - Programación funcional y API de Streams]]
- [[Notas Clase 26-08 - Actividad ListaArray]]
