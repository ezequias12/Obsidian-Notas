## Como se conecta a la BD y que haría si fuera una BD local en vez de en una URL

---

La conexión a la base de datos en tu proyecto se configura y gestiona automáticamente por Spring Boot gracias a las propiedades definidas en el archivo `application.properties` y el uso de JPA (Java Persistence API). Aquí te detallo cómo se realiza la conexión y cómo interactúan los diferentes componentes:

---

### 1. **Archivo** `**application.properties**`

Este archivo contiene la configuración necesaria para que Spring Boot se conecte a la base de datos. En tu caso, estás utilizando una base de datos MySQL alojada en una URL.

### Configuración:

```Plain
spring.datasource.url=jdbc:mysql://localhost/cursojava?useSSL=false
spring.datasource.username=root
spring.datasource.password=
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

- `**spring.datasource.url**`: Especifica la URL de conexión a la base de datos. En este caso, `localhost` indica que la base de datos está en tu máquina local, y `cursojava` es el nombre de la base de datos.
- `**spring.datasource.username**` **y** `**spring.datasource.password**`: Credenciales para acceder a la base de datos.
- `**spring.datasource.driver-class-name**`: Especifica el driver JDBC que se utiliza para conectarse a MySQL (`com.mysql.cj.jdbc.Driver`).

Spring Boot utiliza esta configuración para crear automáticamente un `DataSource` (fuente de datos) que se encarga de gestionar la conexión a la base de datos.

---

### 2. `**EntityManager**` **y JPA**

El `EntityManager` es el componente que interactúa directamente con la base de datos. En tu proyecto, se utiliza en la clase `UsuarioDaoImp`.

### Ejemplo:

```Java
@PersistenceContext
EntityManager entityManager;
```

- `**@PersistenceContext**`: Inyecta automáticamente el `EntityManager` configurado por Spring Boot.
- `**entityManager.createQuery()**`: Permite ejecutar consultas JPQL (Java Persistence Query Language) para interactuar con la base de datos.
- `**entityManager.find()**` **y** `**entityManager.remove()**`: Métodos para buscar y eliminar registros en la base de datos.

El `EntityManager` utiliza la configuración del `DataSource` para conectarse a la base de datos y ejecutar las operaciones.

---

### 3. **Flujo de conexión**

1. **Configuración**: Spring Boot lee las propiedades del archivo `application.properties` y configura automáticamente el `DataSource` y el `EntityManager`.
2. **Interacción**: Cuando llamas a un método en `UsuarioDaoImp` (por ejemplo, `getUsuarios()`), el `EntityManager` utiliza el `DataSource` para conectarse a la base de datos y ejecutar la consulta.
3. **Resultados**: Los datos obtenidos de la base de datos se mapean a objetos de la clase `Usuario` gracias a JPA.

---

### 4. **Base de datos física en un archivo**

Si quisieras usar una base de datos que se almacena en un archivo (como SQLite), el proceso sería similar, pero cambiarías el driver y la URL de conexión.

### Ejemplo con SQLite:

1. **Dependencia en** `**pom.xml**`:
    
    ```XML
    <dependency>
        <groupId>org.xerial</groupId>
        <artifactId>sqlite-jdbc</artifactId>
        <version>3.36.0.3</version>
    </dependency>
    ```
    
2. **Configuración en** `**application.properties**`:
    
    ```Plain
    spring.datasource.url=jdbc:sqlite:/ruta/a/tu/archivo.db
    spring.datasource.driver-class-name=org.sqlite.JDBC
    spring.datasource.username=
    spring.datasource.password=
    ```
    
3. **Flujo**:
    - Spring Boot configurará automáticamente el `DataSource` para conectarse al archivo SQLite.
    - El resto del código (como el uso de `EntityManager`) no cambiará.

---

### Resumen

- La conexión a la base de datos en tu proyecto se configura en `application.properties`.
- Spring Boot utiliza esta configuración para crear un `DataSource` y un `EntityManager`.
- Para una base de datos física en un archivo, solo necesitas cambiar el driver y la URL de conexión. El resto del flujo permanece igual.