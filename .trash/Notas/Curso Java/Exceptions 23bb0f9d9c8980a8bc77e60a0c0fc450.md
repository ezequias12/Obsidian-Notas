# Exceptions

### Excepciones en Java

## ¿Qué es una excepción?

Una excepción es un evento que interrumpe el flujo normal del programa cuando ocurre un error durante la ejecución. Java permite **capturar, manejar y lanzar** excepciones usando diferentes estructuras.

---

## try-catch

Bloque que **intenta ejecutar** código y, si ocurre una excepción, la **captura** para evitar que el programa se detenga.

```java
try {
    int resultado = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Error: " + e);
}

```

- `try`: contiene el código que puede fallar.
- `catch`: define qué hacer si ocurre una excepción del tipo indicado.
- `e`: contiene información del error.

---

## Múltiples `catch`

Podés encadenar varios `catch` para capturar distintos tipos de errores:

```java
try {
    ...
} catch (ArithmeticException e) {
    ...
} catch (NullPointerException e) {
    ...
} catch (Exception e) {
    ...
}

```

> El catch más general (Exception) debe ir al final.
> 

---

## finally

El bloque `finally` se **ejecuta siempre**, haya o no error.

```java
try {
    ...
} catch (...) {
    ...
} finally {
    System.out.println("Este bloque se ejecuta siempre");
}

```

Sirve para cerrar conexiones, liberar recursos, etc.

---

## throw

La palabra clave `throw` permite **lanzar manualmente** una excepción.

```java
if (edad < 18) {
    throw new IllegalArgumentException("Debes ser mayor de edad");
}

```

Se usa cuando querés **forzar un error** si no se cumple cierta condición.

---

## Métodos que lanzan excepciones

Podés indicar que un método **puede lanzar** una excepción usando `throws`:

```java
public void checkAge(int age) throws IllegalArgumentException {
    ...
}

```

Este método será obligatorio envolverlo en un `try-catch` cuando se llame desde otro lado.

---

## Excepciones personalizadas

Podés crear tus propias excepciones extendiendo una clase existente:

```java
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

```

Y usarlas así:

```java
if (score < 0 || score > 100) {
    throw new CustomException("La puntuación debe estar entre 0 y 100");
}

```

Esto es útil para crear errores más específicos en tus programas y mejorar la legibilidad del código.

---

### Resumen de estructuras clave

| Palabra clave | Función |
| --- | --- |
| `try` | Intenta ejecutar código que puede fallar |
| `catch` | Captura y maneja una excepción |
| `finally` | Siempre se ejecuta, ideal para liberar recursos |
| `throw` | Lanza manualmente una excepción |
| `throws` | Declara que un método puede lanzar una excepción |

---

# ¿Qué es una excepción personalizada?

Una **excepción personalizada** es una clase que vos mismo creás para representar un error específico en tu aplicación, en vez de usar las excepciones genéricas que trae Java como `NullPointerException`, `ArithmeticException`, etc.

Esto te permite **darle un nombre claro** al error y manejarlo de forma diferenciada.

---

## 🧱 Paso 1: Crear la excepción personalizada

📄 **Archivo:** `CustomException.java`

```java
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

```

### ¿Qué está pasando acá?

- Se crea una clase `CustomException` que **extiende** de `Exception` (la clase base para excepciones comprobadas o "checked").
- Al extender `Exception`, estamos creando **una nueva categoría de error** que se comporta como cualquier otra excepción de Java.
- El constructor recibe un mensaje y lo pasa a la clase base con `super(message)`.

---

## 🧱 Paso 2: Usar la excepción personalizada en un método

📄 **Archivo:** `ThrowExample.java`

```java
public void checkScore(int score) throws CustomException {
    if (score < 0 || score > 100) {
        throw new CustomException("La puntuación debe estar entre 0 y 100");
    } else {
        System.out.println("Puntuación válida: " + score);
    }
}

```

### ¿Qué hace este método?

- Recibe un `score` como parámetro.
- Si la puntuación no está en el rango válido (entre 0 y 100), **lanza una excepción personalizada** con un mensaje claro.
- Usa la palabra clave `throw` para lanzar la excepción.
- Declara con `throws CustomException` que este método puede lanzar ese tipo de error. Por eso quien lo llame **está obligado a manejarla con try-catch**.

---

## 🧱 Paso 3: Capturar la excepción personalizada

📄 **Archivo:** `Exceptions.java`

```java
var throwExample = new ThrowExample();

try {
    throwExample.checkScore(450);
} catch (CustomException e) {
    System.out.println("Error revisando la puntuación: "+ e.getMessage());
}

```

### ¿Qué hace esta parte?

- Se crea un objeto `throwExample` de la clase `ThrowExample`.
- Se llama al método `checkScore(450)` con una puntuación inválida.
- Como el valor está fuera del rango, el método lanza una `CustomException`.
- El bloque `try-catch` la captura y muestra el mensaje personalizado que se había definido.

---

## 🔁 Flujo completo (resumen):

1. **Definimos una excepción personalizada** (`CustomException`) en un archivo aparte.
2. En otro archivo (`ThrowExample`), usamos `throw new CustomException(...)` cuando ocurre un error lógico (ej: puntuación fuera de rango).
3. En el archivo principal (`Exceptions.java`), usamos `try-catch` para capturar ese error específico y actuar en consecuencia (mostrar mensaje, registrar, etc).

---

## Checked vs Unchecked - Cuando es obligatorio capturar la excepcion o no

- Si extendés de `Exception`: tu excepción será **checked**, y **debe ser capturada o declarada con throws**.
- Si extendés de `RuntimeException`: tu excepción será **unchecked**, y **no hace falta capturarla obligatoriamente**.

En este caso usamos `Exception`, por eso el método `checkScore()` **declara** con `throws CustomException` y el código que lo usa debe **capturarla con try-catch**.

---

## Exceptions.java

```java
package basic.c09_exceptions;

public class Exceptions {

    public static void main(String[] args) {

        // Manejo de excepciones

        // try catch
        try {
            var result = 10 / 0;
            System.out.println(result);
        } catch (ArithmeticException e) {
            // En la variable "e" se guarda la excepción
            // para captar cualquier tipo de error puedo usar Exception o RuntimeException
            System.out.println("Error: " + e);
        }

        // try con múltiples catch
        try {
            var result = 10 / 5;
            System.out.println(result);

            var name = "Brais";
            name = null;
            System.out.println("Name: " + name.toUpperCase());
        } catch (ArithmeticException e) {
            System.out.println("Cuidado con dividir algo que no puedes: " + e);
        } catch (NullPointerException e) {
            System.out.println("Ha ocurrido un null pointer mítico!");
        } catch (Exception e) {
            System.out.println("Se ha producido un error no esperado");
        }

        // finally
        // Bloque que se ejecuta siempre, haya o no error

        try {
            var result = 10 / 0;
            System.out.println(result);
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e);
        } finally {
            System.out.println("Fin del bloque try-catch");
        }

        // throw
        var throwExample = new ThrowExample();
        try {
            throwExample.checkAge(15);
        } catch (IllegalArgumentException e) {
            // todo - Aquí capturamos la excepción que lanzamos en la función checkAge
            System.out.println("Error revisando la edad: " + e.getMessage());
        }

        // Excepción personalizada
        try {
            throwExample.checkScore(450);
        } catch (CustomException e) {
            System.out.println("Error revisando la puntuación: "+ e.getMessage());
        }

        System.out.println("Fin");
    }
}
```

## TrowExample.java

```java
package basic.c09_exceptions;

public class ThrowExample {

    // Todoo esto no lo hacemos dentro de un public static void main(String[] args) porque
    // queremos que sea reutilizable, por eso lo hacemos dentro de una clase

    // creo directamente una funcion nueva
    // todo - Estamos lanzando una excepcion personalizada que luego captamos en el otro archivo

    public void checkAge(int age) throws IllegalArgumentException {
        if (age < 18) {
            // Puedo personalizar un mensaje de error
            // Podemos personalizar la excepcion que lanza java en algun caso que creemos
            throw new IllegalArgumentException("Tienes que ser mayor de edad");
        } else {
            System.out.println("Es mayor de edad");
        }
    }

    public void checkScore(int score) throws CustomException {
        if (score < 0 || score > 100) {
            throw new CustomException("La puntuación debe estar entre 0 y 100");
        } else {
            System.out.println("Puntuación válida: " + score);
        }
    }
}
```

## CustomException.java

```java
package basic.c09_exceptions;

// La extiendo de una Extension ya existente en el sistema
// Creo mi propia excepcion personalizada
public class CustomException extends Exception {

    public CustomException(String message) {
        super(message);
    }
}
```