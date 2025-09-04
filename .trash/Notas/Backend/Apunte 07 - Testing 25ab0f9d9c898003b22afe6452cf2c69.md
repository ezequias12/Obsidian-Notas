# Apunte 07 - Testing

## 1. 🔹 ¿Qué es el testing?

Cuando programás, tenés que **verificar** que tu código funciona bien.

- Para eso existen las **pruebas unitarias (unit tests)** → pequeños tests que prueban partes concretas de tu código (métodos).
- Se hacen con **JUnit**, la librería más usada en Java.

👉 Ventaja:

- Si después cambiás el código, podés correr los tests para ver si algo “se rompió”.

📌 En criollo: es como tener un **tester automático** que revisa que tu programa haga lo esperado.

---

## 2. 🔹 JUnit y Maven

En proyectos Maven, se agrega la dependencia:

```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>

```

- `scope=test` → significa que la librería **solo se usa para probar**, no forma parte del código final.

---

## 3. 🔹 Estructura del proyecto

- `src/main/java` → tu código real.
- `src/test/java` → tus pruebas unitarias.

Ejemplo:

```
hola-mundo/
├── src/
│   ├── main/java/    (App.java, clases del proyecto)
│   └── test/java/    (AppTest.java, clases de prueba)

```

---

## 4. 🔹 Crear un Test en JUnit

Un test es una clase con métodos anotados con `@Test`.

Ejemplo:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculadoraTest {

    public int sumar(int a, int b) {
        return a + b;
    }

    @Test
    public void testSuma() {
        int resultado = sumar(2, 3);
        assertEquals(5, resultado);   // comprueba que 2+3 = 5
    }
}

```

👉 `assertEquals(esperado, obtenido)` → valida que el resultado sea el esperado.

---

## 5. 🔹 Anotaciones útiles en JUnit

- `@BeforeEach` → corre antes de cada test (ej. inicializar objetos).
- `@AfterEach` → corre después de cada test.
- `@BeforeAll` / `@AfterAll` → corren una sola vez antes o después de todos los tests.
- `@Disabled` → desactiva un test temporalmente.
- `@Timeout` → falla si el test tarda demasiado.

Ejemplo:

```java
@BeforeEach
public void setup() {
    calc = new Calculadora();
}

```

---

## 6. 🔹 Assertions (comprobaciones)

Métodos de la clase `Assertions` para verificar resultados:

```java
assertTrue(condición);              // espera true
assertFalse(condición);             // espera false
assertEquals(esperado, obtenido);   // compara valores
assertNotEquals(esperado, obtenido);
assertThrows(Exception.class, () -> { /* código */ });
assertDoesNotThrow(() -> { /* código */ });
assertNull(objeto);
assertNotNull(objeto);

```

---

## 7. 🔹 Ejecutar tests

- Desde **IntelliJ/Eclipse**: botón derecho → *Run All Tests*.
- Desde **Maven**:

```bash
mvn test

```

Esto ejecuta todos los tests y te muestra un reporte (qué pasó, qué falló).

---

## 8. 🔹 Mocking (simular dependencias)

A veces un objeto depende de otro (ej: un **Servicio** usa un **Repositorio** que trae datos).

Para probar el Servicio sin depender del repositorio real, creamos un **mock** (un reemplazo de prueba).

Ejemplo:

```java
class RepositorioMock extends RepositorioAlumnos {
    private List<Alumno> lista;

    public void setLista(List<Alumno> l) { this.lista = l; }

    @Override
    public List<Alumno> listar() { return lista; }
}

public class ServicioAlumnosTest {
    private ServicioAlumnos servicio;
    private RepositorioMock repoMock;

    @BeforeEach
    public void setup() {
        repoMock = new RepositorioMock();
        servicio = new ServicioAlumnos(repoMock);
    }

    @Test
    public void testAlumnoExistente() {
        Alumno esperado = new Alumno("Ana", 1);
        repoMock.setLista(List.of(esperado));
        assertEquals(esperado, servicio.obtenerAlumno(1));
    }
}

```

👉 Así controlás qué devuelve el repo y podés probar al Servicio sin conectarte a nada real.

---

## 9. 🔹 Ejemplo completo: Clase Fracción

Se testean cosas como:

- Que el denominador no sea 0.
- Que `valorReal()` devuelva bien el decimal.
- Que `simplificar()` reduzca correctamente.
- Que `toString()` muestre bien la fracción.

Ejemplo:

```java
@Test
public void testValorReal() {
    Fraccion f = new Fraccion(2, 4);
    assertEquals(0.5, f.valorReal(), 0.0001);
}

@Test
public void testDenominadorCero() {
    assertThrows(IllegalArgumentException.class, () -> {
        new Fraccion(3, 0);
    });
}

```

---

# 🎯 Conclusiones

- **Testing unitario** → pruebas pequeñas que validan tu código.
- **JUnit** → librería más usada, integrada con Maven.
- **Assertions** → comparaciones para validar resultados.
- **Mocking** → simular dependencias para testear aislado.
- **Buenas prácticas**:
    - 1 método = 1 responsabilidad → más fácil de testear.
    - Tests claros y automáticos → ahorrás tiempo y evitás errores invisibles.