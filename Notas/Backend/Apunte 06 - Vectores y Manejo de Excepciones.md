## 1. 🔹 Arreglos (Arrays)

Un **array** en Java es una **colección de valores del mismo tipo** almacenados en posiciones numeradas (**índices**).

- Los índices empiezan en `0`.
- Los arrays en Java son **objetos** → se crean con `new`.

### Declaración

```Java
int[] v;             // Declaro referencia
v = new int[6];      // Creo un array de 6 enteros, inicializados en 0
```

👉 `v[0]` es la 1° casilla, `v[5]` la última.

### Carga y recorrido con `for`

```Java
for(int i = 0; i < v.length; i++) {
    v[i] = i * 2;   // asigno valores
    System.out.println("v[" + i + "] = " + v[i]);
}
```

📌 `.length` devuelve el tamaño del array.

---

## 2. 🔹 Matrices (Arrays bidimensionales)

Un **array bidimensional** es una tabla (filas y columnas).

### Ejemplo:

```Java
int[][] m = new int[3][4];  // 3 filas, 4 columnas
m[1][2] = 7;                // fila 1, columna 2
```

### Recorrido con bucles anidados:

```Java
for(int i = 0; i < m.length; i++) {           // filas
    for(int j = 0; j < m[i].length; j++) {    // columnas
        m[i][j] = i + j;
        System.out.print(m[i][j] + " ");
    }
    System.out.println();
}
```

👉 El acceso es siempre con **dos índices**: `m[fila][columna]`.

---

## 3. 🔹 Manejo de Excepciones

En Java, los errores en ejecución se manejan como **objetos especiales llamados excepciones**.

Ejemplos típicos:

- `ArithmeticException` → dividir por cero.
- `ArrayIndexOutOfBoundsException` → acceder fuera de rango del array.
- `NullPointerException` → usar un objeto `null`.

👉 Las excepciones derivan de la clase `Throwable`.

---

## 4. 🔹 Tipos de excepciones

- **Checked (chequeadas)** → el compilador obliga a manejarlas (ej: `IOException`).
- **Unchecked (no chequeadas)** → opcional manejarlas (ej: `NullPointerException`).

---

## 5. 🔹 Try – Catch – Finally

Para **capturar y manejar** excepciones usamos bloques `try` y `catch`.

### Sintaxis:

```Java
try {
    // Código que puede fallar
} catch (TipoDeExcepcion e) {
    // Qué hacer si ocurre el error
} finally {
    // Se ejecuta siempre (haya o no error)
}
```

---

### Ejemplo práctico:

```Java
import java.util.Scanner;

public class EjemploExcepcion {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        try {
            System.out.print("Ingrese un número entero: ");
            int num = sc.nextInt();   // Si pongo "hola" → error
            System.out.println("Número ingresado: " + num);
        } catch (Exception e) {
            System.out.println("Error: no ingresaste un número válido.");
        } finally {
            System.out.println("Fin del programa.");
        }
    }
}
```

---

## 6. 🔹 Varios `catch`

Un `try` puede tener **varios** `**catch**`, uno para cada tipo de excepción:

```Java
try {
    int[] v = new int[3];
    v[5] = 10;   // fuera de rango
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Índice inválido.");
} catch (Exception e) {
    System.out.println("Otro error: " + e.getMessage());
}
```

---

## 7. 🔹 Orden de `catch`

- Los `catch` van de lo más **específico** a lo más **general**.
- Ejemplo: primero `ArrayIndexOutOfBoundsException`, después `Exception`.

---

# 🎯 Conclusiones

- **Array (vector):** colección lineal, tamaño fijo, índices desde 0.
- **Matriz:** array de arrays (tabla de filas y columnas).
- **Excepciones:** objetos que representan errores en tiempo de ejecución.
- **Try/Catch:** mecanismo para capturar errores y evitar que el programa explote.
- **Finally:** bloque opcional que se ejecuta siempre.

📌 En criollo:

- Los **arrays** son como cajones numerados → siempre del mismo tipo y tamaño fijo.
- Las **excepciones** son como alarmas → si algo falla, se dispara y podés atraparla para que el programa no se caiga.