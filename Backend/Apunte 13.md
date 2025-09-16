¡Vamos con todo! Te dejo un **resumen completo y didáctico del Apunte 13 (JPA)**, listo para copiar/pegar. La idea es que lo puedas entender “en criollo” y usarlo como machete.

---

# 1) ¿Qué es JPA y por qué conviene (o no)?

**JPA** (Jakarta Persistence API) es un **estándar de mapeo objeto–relacional (ORM)** en Java. La idea: **trabajás con objetos** (entidades) y el **ORM traduce** esas operaciones a **SQL** por vos. Ventajas: menos SQL repetitivo, portabilidad entre motores, modelo de dominio centralizado y limpio; ojo que, al ocultar SQL, **podés generar consultas poco eficientes** si no sabés lo que hace debajo (y en casos complejos vas a preferir **SQL nativo**).
JPA es el **estándar** y **Hibernate** la implementación más usada; con anotaciones e interfaces de JPA definís el contrato y **Hibernate ejecuta** el SQL real.

Mini contexto histórico: de Entity Beans (EJB 2.x) se pasó a JPA para simplificar; hoy es **Jakarta Persistence** y sigue evolucionando.

---

# 2) Los 3 ejes que tenés que dominar

El apunte organiza JPA en **3 frentes** que vas a tocar siempre:

1. **Configuración** (conexión a BD, proveedor, transacciones, propiedades)
2. **Mapeo** (cómo tu clase y sus atributos/relaciones caen a la tabla y columnas)
3. **Administración y Consultas** (EntityManager, estados de entidad, CRUD, JPQL).

En criollo: primero **prendo el motor**, después **le enseño el mapa** y al final **manejo** (consulto/guardo/borro).

---

# 3) Configuración (persistence.xml) — H2 para practicar, Postgres para “real”

En `META-INF/persistence.xml` definís la **unidad de persistencia**: proveedor (Hibernate), datos JDBC, **dialecto**, **estrategia DDL** y logging.
Claves:

* **provider**: clase del proveedor (Hibernate).
* **jdbc driver/url/user/pass**.
* **hibernate.dialect**: guía a Hibernate para emitir SQL compatible.
* **hibernate.hbm2ddl.auto**: `none/validate/update/create/create-drop` (en dev: `create-drop` con H2; en real: evitar `create`)
* **transaction-type**: `RESOURCE_LOCAL` (vos manejás `EntityTransaction`) o `JTA` (lo maneja el contenedor).

**Ejemplo H2 (labs):** `create-drop`, `show_sql=true` para ver lo que hace.
**Ejemplo Postgres (cuando pases a motor real):** suele usarse `validate` o `update` (en producción no `create`).
Tips: `DB_CLOSE_DELAY=-1` mantiene la DB H2 viva; en producción considerá **pool de conexiones** y **cerrar el `EntityManagerFactory`** al finalizar.

---

# 4) Mapeo: de tabla ↔ entidad (anotaciones básicas y relaciones)

Reglas base:

* Cada **tabla** ↔ **clase** con `@Entity` (y opcional `@Table` si el nombre difiere).
* Cada **columna** ↔ **atributo** (con `@Column` si necesitás cambiar nombre o restricciones).
* **PK** con `@Id` (+ `@GeneratedValue` si la genera la BD).
* **FKs** con relaciones: `@ManyToOne`, `@OneToMany`, `@ManyToMany`.

**Requisitos mínimos de una entidad**: `@Entity`, **constructor por defecto**, tener **@Id**, clase pública (no final) y no hacer `static/transient` en campos persistentes.
Si el nombre de tabla/columna no coincide, anotá `@Table(name="...")` / `@Column(name="...")`.

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

**Métodos frecuentes (qué hacen de verdad):**

* `persist(e)`: **INSERT** si es NEW; pasa a Managed. Falla si la PK ya existe.
* `merge(e)`: **devuelve una instancia Managed nueva** con los cambios (la original sigue Detached); puede **insertar** si no existía (upsert manual).
* `remove(e)`: borra solo si está **Managed** (si está Detached, hacé `em.remove(em.merge(e))`).
* `detach(e)`: saca del contexto; cambios posteriores **no** se sincronizan.
* `refresh(e)`: pisa en memoria con lo de la BD (perdés cambios locales no sincronizados).
* `flush()`: **empuja** SQL pendiente sin cerrar la transacción (sirve para ver errores antes).
* `find(C,id)`: devuelve **Managed** o `null` y **respeta la caché de primer nivel**; tocar relaciones LAZY puede disparar carga.

**Caché de primer nivel:** el `find` primero mira el contexto; si ya está, **no pega la BD**; por eso puede devolverte el mismo objeto Managed.

**Obtener EM / EMF:**

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("MiUnidad");
EntityManager em = emf.createEntityManager();
```

En servidores/ Spring, suele inyectarse (`@PersistenceContext`) y lo maneja el contenedor.

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
  Usuario u = em.find(Usuario.class, id); // Managed o null
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
  Usuario u = em.find(Usuario.class, id);
  em.remove(u);
  em.getTransaction().commit();
  ```

Todo esto con **EntityManager** y **transacción** (`begin/commit`).

---

# 7) JPQL bien usado (consultas “a objetos”, no a tablas)

**JPQL** consulta **entidades y relaciones** (no tablas); Hibernate la **traduce a SQL** del motor.
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

  .

* **Consultar y entender LAZY**:

  ```java
  Album found = em.find(Album.class, album.getId());
  System.out.println(found.getTracks().size()); // puede disparar un SELECT (LAZY)
  ```

  .

* **Evitar N+1**: preferí **fetch join** cuando querés traer una colección asociada en la misma consulta (pero filtrada/paginada con cuidado).

---

# 9) Checklist mental rápido

* **Config**: `persistence.xml` con driver/URL/credenciales, `dialect`, `hbm2ddl.auto` (H2: `create-drop`; Postgres: `validate/update`), `show_sql` en dev, `RESOURCE_LOCAL` en labs.
* **Mapeo**: `@Entity/@Id/@GeneratedValue`, `@Table/@Column` si difieren, `@ManyToOne`/`@OneToMany`/`@ManyToMany`. **LAZY por defecto**; dinero con `BigDecimal`; validaciones donde suma.
* **EM & ciclo de vida**: saber **NEW/MANAGED/DETACHED/REMOVED** y qué hace **persist/merge/remove/detach/refresh/flush/find** (y la caché de 1er nivel).
* **CRUD**: siempre con **transacción** (begin/commit).
* **JPQL**: alias, rutas (`a.artist.name`), parámetros nombrados, **fetch join** para evitar N+1, **DTO projection** para respuestas livianas, **paginado**.

---

¿Querés que te arme **3 ejercicios guiados** (con solución) para:

1. mapear 2 entidades con N→1 y persistir sample data,
2. escribir 3 JPQL (búsqueda, join, fetch join + paginado),
3. practicar `merge/detach/refresh` y ver el efecto?
