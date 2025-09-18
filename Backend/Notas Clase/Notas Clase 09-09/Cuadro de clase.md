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
![](../../../img/Pasted%20image%2020250910175651.png)

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

# Apuntes de los que toma conceptos
### 🔹 **Apunte 03 – Maven**

- Qué es el `pom.xml`.
    
- Cómo se gestionan dependencias como `hibernate-core`, `jakarta.persistence` o el driver de H2/Postgres.
    
- Explica cómo Maven descarga y configura todo lo que ves en el diagrama.
    

---

### 🔹 **Apunte 12 – JDBC**

- Explica qué es JDBC, qué son `Connection`, `Statement`, `ResultSet`.
    
- Los distintos tipos de drivers JDBC.
    
- Uso de H2 embebido (en memoria o archivo) y H2 servidor.
    
- Cómo se conecta la app vía JDBC a motores reales (ej. PostgreSQL).
    
- O sea, la parte **JDBC API → DBMS (H2, PostgreSQL)** del cuadro.
    

---

### 🔹 **Apunte 13 – JPA**

- Qué es JPA y qué resuelve (problema objeto-relacional).
    
- Rol de Hibernate como implementación de JPA.
    
- Uso de `persistence.xml`, anotaciones como `@Entity`, `@Id`, etc.
    
- Cómo se traduce Java ↔ SQL (lo que está en el recuadro rojo del diagrama).
    
- O sea, la parte **Mi App (objetos) → ORM (JPA/Hibernate)**.
    

---

### 🔹 **Apunte 02 – Java**

- Explica la **plataforma Java** y la **JVM**, que es lo que en el cuadro aparece como las cajitas de JVM donde corre H2 o PostgreSQL.
    
- Relación entre JDK, JVM y librerías.
    

---

✅ En resumen:

- **Estructura del proyecto y dependencias** → Apunte 03 (Maven).
    
- **Base teórica de la JVM y librerías** → Apunte 02 (Java).
    
- **Conexión de bajo nivel a la DB (JDBC)** → Apunte 12.
    
- **ORM con JPA/Hibernate** → Apunte 13.
    

---
