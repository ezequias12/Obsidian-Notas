
# Resumen del Apunte 13 - JPA

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

## 3) Configuración (`persistence.xml`) — H2 para practicar, Postgres para “real”

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

## 4) Mapeo: de tabla ↔ entidad (anotaciones básicas y relaciones)

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

---

## 5) EntityManager y ciclo de vida de las entidades (¡clave!)

El **`EntityManager`** es la interfaz central para interactuar con el contexto de persistencia. Gestiona el **ciclo de vida** de las entidades.

*   **Contexto de persistencia:** Un área donde las entidades "Managed" son monitoreadas por el `EntityManager`. Los cambios en estas entidades se detectan y se sincronizan con la BD al hacer `commit` o `flush`.
*   **Estados de una entidad:**
    *   **NEW (Transient):** Objeto recién creado, no asociado a la BD ni al contexto de persistencia.
        *   `persist()` lo convierte a **Managed**.
    *   **MANAGED (Persistente):** El objeto está en el contexto de persistencia y representa una fila en la BD. Los cambios se sincronizan.
        *   `remove()` lo marca como **Removed**.
        *   `detach()` lo convierte a **Detached**.
    *   **DETACHED:** El objeto estuvo Managed pero ya no lo está (ej. el `EntityManager` se cerró, la transacción finalizó, o se llamó a `detach()`). Los cambios **no** se sincronizan automáticamente.
        *   `merge()` crea una **copia Managed** con los cambios del objeto Detached.
    *   **REMOVED:** El objeto está marcado para ser eliminado de la BD. Al `commit`, se ejecuta el `DELETE`.

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

## 6) CRUD “de todos los días”

Todas las operaciones CRUD (Create, Retrieve, Update, Delete) deben realizarse dentro de una **transacción**.

*   **Create:**
    ```java
    em.getTransaction().begin();
    em.persist(nuevoUsuario);
    em.getTransaction().commit();
    ```
*   **Retrieve:**
    ```java
    Usuario u = em.find(Usuario.class, id); // Devuelve Managed o null
    ```
*   **Update (si está Detached):**
    ```java
    em.getTransaction().begin();
    Usuario m = em.merge(uDetached); // m es Managed, uDetached sigue Detached
    em.getTransaction().commit();
    ```
*   **Delete:**
    ```java
    em.getTransaction().begin();
    Usuario u = em.find(Usuario.class, id); // Primero se busca para que sea Managed
    em.remove(u);
    em.getTransaction().commit();
    ```

---

## 7) JPQL bien usado (consultas “a objetos”, no a tablas)

**JPQL (Jakarta Persistence Query Language)** es un lenguaje de consulta orientado a **entidades y sus relaciones** (no a tablas SQL). Hibernate lo traduce a SQL nativo.

*   **Sintaxis:** Usa alias (`from Album a`), permite navegar relaciones (`a.artist.name`), y soporta **parámetros nombrados** (`:q`).
*   **Básica:**
    ```jpql
    select a from Album a where a.title like :title order by a.id
    ```
    Se puede omitir `select a` si se pide la entidad completa: `from Album a`.
*   **Joins:**
    *   `join` (inner join): `select a from Album a join a.artist ar`
    *   `left join`: `select t from Track t left join t.genre g`
*   **Fetch Join (para evitar N+1):**
    ```jpql
    select distinct a from Album a left join fetch a.tracks where a.id = :id
    ```
    Trae la colección asociada (`tracks`) en la **misma consulta** SQL, evitando múltiples consultas posteriores. Usar con cuidado si la colección es muy grande.
*   **Paginado y parámetros:**
    ```java
    List<Album> page = em.createQuery("...", Album.class)
      .setParameter("q", "%black%") // Parámetro nombrado
      .setFirstResult(0)            // Offset
      .setMaxResults(10)            // Tamaño de página
      .getResultList();
    ```
    `getSingleResult()` puede lanzar `NoResultException` o `NonUniqueResultException`; es más seguro usar `getResultList()` y verificar si está vacío.
*   **Proyecciones (DTOs):** Permite seleccionar campos específicos o construir objetos DTO (Data Transfer Object) directamente en la consulta, evitando cargar la entidad completa si no es necesario.
    ```jpql
    select new utnfc.back.isi.jpa.dto.AlbumSummary(a.id, a.title, ar.name)
    from Album a join a.artist ar where a.title like :q
    ```
    Requiere un DTO con un constructor que coincida con los tipos y el orden de los campos seleccionados.

---

## 8) Mini recetas típicas (con buenas prácticas)

*   **Persistir respetando N→1:** Primero se persiste el lado "uno" de la relación, luego el lado "muchos" que lo referencia.
    ```java
    em.getTransaction().begin();
    Artist artist = Artist.builder().name("AC/DC").build(); em.persist(artist);
    Album album = Album.builder().title("Back in Black").artist(artist).build(); em.persist(album);
    em.getTransaction().commit();
    ```
*   **Consultar y entender LAZY:** Acceder a una colección o entidad marcada como `LAZY` puede disparar una nueva consulta a la BD en el momento del acceso.
    ```java
    Album found = em.find(Album.class, album.getId());
    System.out.println(found.getTracks().size()); // Aquí puede dispararse un SELECT adicional
    ```
*   **Evitar N+1:** Usar **fetch join** en JPQL para cargar colecciones asociadas en una sola consulta cuando se sabe que se van a necesitar.

---

## 9) Checklist mental rápido

*   **Configuración:** `persistence.xml` con driver/URL/credenciales, `dialect`, `hbm2ddl.auto` (H2: `create-drop`; Postgres: `validate/update`), `show_sql` en dev, `RESOURCE_LOCAL` en labs.
*   **Mapeo:** `@Entity/@Id/@GeneratedValue`, `@Table/@Column` si difieren, `@ManyToOne`/`@OneToMany`/`@ManyToMany`. **`LAZY` por defecto**; dinero con `BigDecimal`; validaciones donde sumen.
*   **`EntityManager` y ciclo de vida:** Entender los estados **NEW/MANAGED/DETACHED/REMOVED** y la función de `persist/merge/remove/detach/refresh/flush/find` (y la caché de primer nivel).
*   **CRUD:** Siempre dentro de una **transacción** (`begin/commit`).
*   **JPQL:** Alias, navegación de rutas (`a.artist.name`), parámetros nombrados, **fetch join** para evitar N+1, **DTO projection** para respuestas livianas, **paginado**.

---

## Importante para entender y conectado con el cuadro de clase

La nota [[Cuadro de clase]] enfatiza que no se trata de memorizar cada línea de JDBC, sino de **entender su propósito como la base** sobre la que se construyen herramientas como JPA/Hibernate.

**Conexión entre JDBC y JPA:**

*   **JDBC es el "cómo" de bajo nivel:** Es la API directa para enviar SQL a la base de datos. Piensa en ello como las "tuberías" o el "lenguaje de máquina" para hablar con la BD.
*   **JPA es el "qué" de alto nivel:** Te permite trabajar con objetos Java y JPA (a través de su implementación como Hibernate) se encarga de generar y ejecutar el SQL de JDBC por ti. Es una capa de abstracción que simplifica el desarrollo.

**Los 5 pasos de JDBC en el contexto de JPA:**

JPA/Hibernate **realizan internamente** los 5 pasos de JDBC:

1.  **Cargar el Driver:** Hibernate lo hace automáticamente.
2.  **Abrir la Conexión:** Hibernate gestiona las conexiones, a menudo usando un **pool de conexiones** (como HikariCP, visto en el [[Apunte 12 - JDBC]]) para eficiencia.
3.  **Crear una Sentencia (`PreparedStatement`):** Hibernate genera `PreparedStatement`s para todas las operaciones (CRUD, JPQL) para garantizar **seguridad (anti-inyección SQL)** y **rendimiento**.
4.  **Ejecutar la Consulta y Leer Resultados:** Hibernate ejecuta el SQL y mapea automáticamente los `ResultSet` a tus objetos entidad.
5.  **Cerrar los Recursos:** Hibernate se encarga de cerrar `Connection`, `PreparedStatement` y `ResultSet` de forma segura, devolviendo las conexiones al pool.

**La diferencia más importante (`Statement` vs. `PreparedStatement`) en JPA:**

*   JPA/Hibernate **siempre usan `PreparedStatement`** internamente para cualquier operación que involucre parámetros (que es casi todo). Esto significa que, al usar JPA, estás automáticamente protegido contra la inyección SQL y te beneficias de la eficiencia de las consultas precompiladas, sin tener que preocuparte por ello explícitamente como lo harías con JDBC puro.

**Transacciones en JPA:**

*   JPA también maneja transacciones. Cuando usas `em.getTransaction().begin()` y `em.getTransaction().commit()`, JPA está orquestando las transacciones de JDBC por debajo, asegurando la atomicidad de tus operaciones.

**Pool de Conexiones en JPA:**

*   En una aplicación real, JPA/Hibernate se configuran para usar un pool de conexiones (como HikariCP) para gestionar eficientemente el acceso a la base de datos, tal como se explicó en el [[Apunte 12 - JDBC]].

En resumen, JPA es una capa de abstracción que te permite trabajar con objetos, mientras que **JDBC es la base fundamental** que JPA utiliza para comunicarse con la base de datos. Entender JDBC te da una visión clara de lo que JPA está haciendo "bajo el capó".

---

¿Qué te parece este resumen? ¿Hay algo más que quieras añadir o modificar?