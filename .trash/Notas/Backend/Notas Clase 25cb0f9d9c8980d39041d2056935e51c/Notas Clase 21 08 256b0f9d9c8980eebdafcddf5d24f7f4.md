# Notas Clase 21/08

## Dependencias necesarias

### Lombok

Se crean mediante una anotacion al principio de todo

Es una librería que elimina el código repetitivo (boilerplate) en Java.

En vez de escribir a mano getters, setters, constructores, `toString()`, `equals()` o `hashCode()`, podés usar **anotaciones** y Lombok los genera automáticamente en compilación.

---

### ⚙️ Configuración

En **Maven** (`pom.xml`):

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.30</version> <!-- la última estable -->
    <scope>provided</scope>
</dependency>

```

👉 Además, necesitás activar el **annotation processing** en tu IDE (Eclipse, IntelliJ o VSCode).

En IntelliJ, por ejemplo:

`File > Settings > Build, Execution, Deployment > Compiler > Annotation Processors > Enable`.

---

### 🛠️ Anotaciones principales

- `@Getter` → genera automáticamente todos los getters.
- `@Setter` → genera automáticamente todos los setters.
- `@ToString` → genera el método `toString()`.
- `@EqualsAndHashCode` → genera `equals()` y `hashCode()`.
- `@AllArgsConstructor` → genera constructor con todos los atributos.
- `@NoArgsConstructor` → genera constructor vacío.
- `@Data` → combinación de `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode` y `@RequiredArgsConstructor`.

---

### 📌 Ejemplo sin Lombok

```java
public class Persona {
    private String nombre;
    private int edad;

    public Persona() {}

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public int getEdad() { return edad; }
    public void setEdad(int edad) { this.edad = edad; }

    @Override
    public String toString() {
        return "Persona{nombre='" + nombre + "', edad=" + edad + "}";
    }
}

```

---

### 📌 Ejemplo con Lombok

```java
import lombok.Data;

@Data
public class Persona {
    private String nombre;
    private int edad;
}

```

👉 Automáticamente genera:

(Es decir, en vez de escribir cada anotacion una por una, se puede poner @Data y que genere todos)

- `getNombre()`, `setNombre()`,
- `getEdad()`, `setEdad()`,
- `toString()`, `equals()`, `hashCode()`,
- `constructor vacío` y `constructor con parámetros requeridos`.

---

Lombok es como un **atajo** para no tener que copiar-pegar siempre el mismo código. Le ponés una “etiqueta” (anotación) arriba de la clase y él se encarga del trabajo sucio.