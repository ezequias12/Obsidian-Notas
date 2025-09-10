# Importante

### Apunte 12 
- No es necesario saber tanto de como programar lo que se ve en este apunte (el JDBC). La idea es enteder mejor que se implementa y como.
- No nos vamos a poner a programar literalmente lo que habla en ese apunte.

### Parcial
- CSV con filas repetidas. armar una estructura que logre diferenciar las partes repetidas y almacenar todo en una base de datos
- Para luego devolver consultas

### Repositorio con instructivo para instalar el contenedor

https://labsys.frc.utn.edu.ar/gitlab/backend-app/alumnos/instructivos/contenedor-postgresql

## Cuadro de clase
![[Pasted image 20250910175651.png]]

# Explicacion del cuadro

### 1. **Mi App (tu código Java)**

- Acá está tu código fuente en Java.
    
- También el `pom.xml` (donde Maven descarga dependencias) y el `persistence.xml` (donde configuras JPA/Hibernate).
    
- Vos programás en **objetos** (clases `Cliente`, `Producto`, etc.), no en tablas ni SQL.
    

---

### 2. **ORM (JPA + Hibernate)**

- **JPA**: es la **especificación** (el contrato, las reglas).
    
- **Hibernate**: es la **implementación concreta** más usada.
    

¿Qué hace Hibernate?

- **Carga la configuración** desde `persistence.xml`.
    
- **Interpreta los mapeos** (anotaciones como `@Entity`, `@Column`, `@Id`).
    
- **Traduce**:
    
    - de **Java → SQL** cuando guardás un objeto (`em.persist(cliente)`).
        
    - de **SQL → Java** cuando consultás (te devuelve un objeto `Cliente`).
        

👉 A vos te deja trabajar con objetos, Hibernate se encarga de hablar en SQL con la base.

---

### 3. **JDBC API**

- Es la **capa intermedia estándar de Java** para conectarse a bases de datos.
    
- Hibernate en realidad no habla directo con la DB, sino que usa JDBC.
    
- JDBC maneja:
    
    - `Connection` → abrir conexión.
        
    - `Statement/PreparedStatement` → enviar SQL.
        
    - `ResultSet` → recibir resultados.
        

---

### 4. **Motores de Base de Datos**

El diagrama muestra distintas formas de usar la DB:

- **H2 en memoria (H2 Mem, misma JVM)**
    
    - La DB vive en RAM.
        
    - Súper rápida.
        
    - Ideal para pruebas (cuando se apaga la app, se borra todo).
        
    - URL típica: `jdbc:h2:mem:testdb`.
        
- **H2 en archivo (JVM con H2 DBMS)**
    
    - En vez de RAM, guarda en un archivo local.
        
    - Persiste aunque cierres la app.
        
    - Muy usado en proyectos de prueba.
        
- **H2 en modo servidor (H2 TCP, otra JVM)**
    
    - H2 corre como un proceso aparte.
        
    - La app se conecta por red (TCP).
        
    - Simula un motor real multiusuario.
        
- **PostgreSQL (DBMS real)**
    
    - Ya no está embebido en tu app, es un sistema aparte.
        
    - Se conecta vía **driver JDBC de Postgres** (dependencia en `pom.xml`).
        
    - Acá se usan setups más realistas (Docker, servidor).
        

---

### 5. **Relación con Maven**

- El `pom.xml` define las dependencias:
    
    - `jakarta.persistence-api` → JPA.
        
    - `hibernate-core` → Hibernate.
        
    - `h2` o `postgresql` → el driver JDBC necesario.
        
- Maven baja el **driver** correcto según el motor de DB.
    

---

### 📌 Resumen en criollo

1. Vos escribís código con objetos (`Cliente`, `Pedido`).
    
2. JPA/Hibernate traduce eso a SQL automáticamente.
    
3. Hibernate se apoya en JDBC, que es la API estándar de Java para enviar y recibir consultas.
    
4. Según cómo configures el `persistence.xml`, Hibernate conecta a:
    
    - H2 en memoria → ideal para pruebas rápidas.
        
    - H2 en archivo o servidor → más cercano a producción.
        
    - PostgreSQL → motor real para proyectos serios.
        

---
