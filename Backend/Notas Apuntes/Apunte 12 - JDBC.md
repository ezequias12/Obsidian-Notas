

# 1) ¿Para qué BD y qué es JDBC?

* Las **bases de datos relacionales** te dan persistencia, integridad, concurrencia, seguridad y consultas eficientes; la BD se encarga de esas “cosas difíciles” y vos te concentrás en el código de negocio. En este bloque aprendemos **cómo Java se conecta** a una BD usando **JDBC**, la API de más bajo nivel en Java para hablar con la base (después vendrán JPA/Hibernate/Spring Data, que usan JDBC por debajo). &#x20;

---

# 2) Motores que usamos (H2 y PostgreSQL) y URLs típicas

* **Elegimos H2** para arrancar (rápido, embebido, cero instalación) y **PostgreSQL** para “modo real” (robusto, multiusuario, transacciones avanzadas). Es un camino natural: empezar simple y subir la vara.&#x20;

* **H2 (embebido vs. servidor)**

  * *Embebido*: vive dentro de tu JVM; ideal para labs y tests (`jdbc:h2:mem:testdb` o `jdbc:h2:file:./data/testdb`) &#x20;
  * *Servidor*: corre aparte y vos te conectás por TCP (más parecido a producción). &#x20;

* **PostgreSQL (productivo)**: URL típica `jdbc:postgresql://localhost:5432/miBD`. Ventajas: robustez, roles/schemas, extensiones; desventajas: más complejidad de instalación (suele ir en Docker). &#x20;

* **Patrones de URL** (para que copies y pegues):

  * H2 memoria: `jdbc:h2:mem:chinook`
  * H2 archivo: `jdbc:h2:file:./data/chinook.mv.db`
  * H2 TCP: `jdbc:h2:tcp://<host>[:<puerto>]/[ruta/]nombreBD`
  * PostgreSQL: `jdbc:postgresql://<host>:<puerto>/<bd>`&#x20;

> Tip H2: podés agregar `DB_CLOSE_DELAY=-1` para que la base en memoria **no se destruya** al cerrar la última conexión (queda viva mientras la JVM esté viva).&#x20;

---

# 3) JDBC en 5 pasos (Hola Mundo)

**La idea**: cargar driver (hoy opcional), abrir conexión, ejecutar SQL, leer resultados, cerrar todo y manejar errores.

1. **(Hoy opcional)** Registrar driver

   * Con JDBC 4+ los drivers se cargan solos vía SPI si el .jar está en el classpath. Antes se hacía `Class.forName(...)`.&#x20;
   * Ejemplo clásico (solo si hace falta):

     ```java
     Class.forName("org.h2.Driver");
     ```



2. **Conexión** (DriverManager):

   ```java
   Connection conn = DriverManager.getConnection("jdbc:h2:mem:chinook", "sa", "");
   ```

   También podés pasar usuario/clave en la URL o un `Properties`. (Hay ejemplos equivalentes para MySQL).  &#x20;

3. **Sentencia + Resultados**

   * Con **Statement**:

     ```java
     try (Statement st = conn.createStatement();
          ResultSet rs = st.executeQuery("SELECT ...")) {
       while (rs.next()) {
         int id = rs.getInt(1);
         String name = rs.getString("NAME");
       }
     }
     ```

     Métodos: `executeQuery` (SELECT), `executeUpdate` (INSERT/UPDATE/DELETE), `execute` (genérico). El `ResultSet` se recorre con `next()` y se obtienen valores por índice o nombre.  &#x20;

4. **Cerrar recursos** con **try-with-resources** (Connection/Statement/ResultSet son `AutoCloseable`). Evitás fugas.&#x20;

5. **Errores**

   * JDBC lanza `SQLException`. Si además hacés I/O (por ej. cargar script de init), también `IOException`. Manejalas.&#x20;

---

# 4) Statement vs **PreparedStatement** vs **CallableStatement**

* **PreparedStatement** = SQL **con parámetros** (`?`). Ventajas: te **evita inyección SQL** y tipás los parámetros (`setInt`, `setString`, etc.). Ideal para cualquier entrada de usuario.

  ```java
  String sql = "SELECT TRACK_ID, NAME FROM TRACK WHERE ALBUM_ID = ? AND MILLISECONDS > ?";
  try (PreparedStatement ps = conn.prepareStatement(sql)) {
      ps.setInt(1, 5);
      ps.setInt(2, 180_000);
      try (ResultSet rs = ps.executeQuery()) { /* ... */ }
  }
  ```

  &#x20;

* **Batch** con `PreparedStatement`: insertar/actualizar **muchas filas** rápido.

  ```java
  String ins = "INSERT INTO PLAY_LIST_TRACK (PLAY_LIST_ID, TRACK_ID) VALUES (?, ?)";
  try (PreparedStatement ps = conn.prepareStatement(ins)) {
    for (var trackId : trackIds) {
      ps.setInt(1, playlistId);
      ps.setInt(2, trackId);
      ps.addBatch();
    }
    int[] counts = ps.executeBatch();
  }
  ```



* **Generated Keys** (IDs autogenerados):

  ```java
  try (PreparedStatement ps = conn.prepareStatement(ins, Statement.RETURN_GENERATED_KEYS)) {
    ps.executeUpdate();
    try (ResultSet keys = ps.getGeneratedKeys()) { if (keys.next()) { int id = keys.getInt(1); } }
  }
  ```



* **CallableStatement**: para procedimientos/funciones almacenadas (cuando el motor lo permite).

  ```java
  try (CallableStatement cs = conn.prepareCall("{ ? = call GET_TOTAL_SALES_BY_CUSTOMER(?) }")) { ... }
  ```



* **Regla práctica**: ¿Hay parámetros? **PreparedStatement**. ¿Procedimiento/función? **CallableStatement**. ¿DDL o query ad-hoc sin parámetros? **Statement**.&#x20;

---

# 5) Transacciones y autocommit (lo justo y necesario)

* Para **múltiples operaciones coherentes** (por ej., insertar encabezado + sus detalles), **apagá autocommit**, hacé las operaciones y **cerrá con `commit()`**; si algo falla, **`rollback()`**. Regla simple del día a día.&#x20;

> Mini–tip: transacciones **cortas**. Nada de hacer I/O pesado “adentro” de la transacción.&#x20;

---

# 6) Buenas prácticas y anti-patrones

**Buenas:**

* **Siempre** `PreparedStatement` para entradas de usuario (inyección).
* **try-with-resources** para cerrar todo.
* **Batch** para cargas masivas.
* Manejá `NULL` explícitamente (`ps.setNull(idx, Types.INTEGER)`).&#x20;

**Evitar:**

* Concatenar strings con datos del usuario (`"... WHERE NAME = '" + userInput + "'"`) → inyección.
* Reutilizar `Statement` para lógica repetitiva con parámetros → usá `PreparedStatement`.
* Abrir/cerrar conexión **por cada fila** → pensá en **pooling** y batch.&#x20;

---

# 7) Pool de conexiones (HikariCP) — cuando tu app atiende muchas requests

**El problema**: abrir conexión cuesta (handshake, auth, memoria, sesión). Una conexión única = cuello de botella; una por operación = overhead y latencia. **Solución: un pool** (conjunto de conexiones “listas” que se prestan y devuelven). Usás `DataSource` en lugar de `DriverManager`. Al “cerrar” la conexión, **vuelve al pool** (no se destruye). Beneficios: latencias estables, mejor throughput y control de concurrencia.  &#x20;

**Parámetros clave (conceptos generales):**

* `maximumPoolSize`: máx. conexiones simultáneas (la **concurrencia real** hacia la BD).
* `minimumIdle`: conexiones “calientes” en reposo (absorber picos).
* `connectionTimeout`: cuánto esperar por una conexión libre (si salta, el pool está corto o hay fugas/consultas lentas).
* `idleTimeout`: cuánta inactividad antes de cerrar conexiones ociosas.
* `maxLifetime`: vida máxima de una conexión (reciclaje).
* `keepaliveTime` y `leakDetectionThreshold`: salud y detección de fugas.  &#x20;

**Buenas prácticas con pool**:

* Siempre `try-with-resources` y cerrar en orden **ResultSet → Statement → Connection**.
* No compartir `Connection/Statement` entre hilos; nada de `static`.
* Transacciones cortas; nada de operaciones largas “prestando” una conexión.
* Monitorear métricas del pool (préstamos, esperas, timeouts). &#x20;

**Ejemplo mini con HikariCP** (patrón de uso):

```java
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:postgresql://localhost:5432/sakila");
config.setUsername("appuser");
config.setPassword("apppass");
config.setMaximumPoolSize(10);
DataSource ds = new HikariDataSource(config);

try (Connection conn = ds.getConnection();
     PreparedStatement ps = conn.prepareStatement(
       "SELECT film_id, title FROM film WHERE rating = ? LIMIT 10")) {
  ps.setString(1, "PG");
  try (ResultSet rs = ps.executeQuery()) { /* ... */ }
} // vuelve al pool
((HikariDataSource) ds).close(); // cerrar al final de la app
```

&#x20; &#x20;

**Spring Boot (mapa mental)**: lo mismo de arriba se traduce a propiedades con prefijo `spring.datasource.hikari.*` (`maximum-pool-size`, `minimum-idle`, `connection-timeout`, `idle-timeout`, `max-lifetime`, etc.). &#x20;

**Dimensionar el pool (checklist express)**:

* Medí latencias típicas (p50/p95) y throughput esperado.
* Usá **Little’s Law** como brújula: concurrencia ≈ throughput × tiempo\_en\_BD.
* Ajustá tamaños y mirá métricas (si hay `connectionTimeout`, falta pool o hay consultas lentas). &#x20;

---

# 8) En sintesis

* **JDBC** = abrir una conexión, mandar SQL, leer filas, cerrar prolijo.
* **PreparedStatement** para todo lo parametrizable (seguridad + performance). **Batch** para volumen. **Generated Keys** para IDs. &#x20;
* **Transacciones**: apagá autocommit cuando tengas que garantizar coherencia entre varias operaciones. **Commit/Rollback** rápido.&#x20;
* **Pool**: en apps concurrentes, es **obligatorio** para no matar la BD ni la latencia. Configuralo con criterio y medí.&#x20;
