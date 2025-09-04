# Apunte 04 - Sintaxis

# 1. Tipos de datos

Java es **fuertemente tipado**: cada variable tiene un tipo fijo.

| Tipo | Tamaño aprox. | Ejemplo | Notas útiles |
| --- | --- | --- | --- |
| `byte` | 1 byte | `byte b = 100;` | Enteros chicos (–128 a 127) |
| `short` | 2 bytes | `short s = 2000;` | Enteros medianos |
| `int` | 4 bytes | `int edad = 20;` | Enteros “default” |
| `long` | 8 bytes | `long pobl = 8_000_000_000L;` | Lleva **L** si es literal grande |
| `float` | 4 bytes | `float x = 2.5f;` | Lleva **f**; menos precisión |
| `double` | 8 bytes | `double pi = 3.14159;` | Decimales “default” |
| `char` | 2 bytes | `char letra = 'A';` | Un carácter (comillas simples) |
| `boolean` | 1 byte | `boolean activo = true;` | Solo `true` o `false` (no hay “truthy”) |
| `String` | — | `String nombre = "Ana";` | **Clase**, no primitivo (comillas dobles) |

Tips rápidos:

- **Literales**: enteros sin sufijo son `int`; decimales sin sufijo son `double`. Poné `f` para `float`, `L` para `long`.
- **`String`** empieza con mayúscula porque es una **clase**.

# 2. Variables e identificadores

- **Case sensitive**: `contador` ≠ `Contador`.
- **No** pueden empezar con número ni ser palabra reservada.
- **Convención**: `camelCase` para variables (`sueldoBruto`, `esValido`).
- Ejemplos:
    
    ```java
    int x1;
    float x2, x3;
    char c;
    String nombreCompleto;
    
    ```
    

# 3. Asignación y operadores

### Aritméticos

`+  -  *  /  %` (resto)

- **División entre `int`** trunca decimales:
    
    ```java
    int a = 5, b = 2;
    int c = a / b; // 2 (NO 2.5)
    double d = a / (double)b; // 2.5
    
    ```
    
- **Promoción de tipos** (regla rápida): el resultado “salta” al **más grande** entre los operandos:
    
    `int < long < float < double`.
    

### Contadores y acumuladores

```java
a = a + 1;   a += 1;   a++;   ++a;
b = b - 1;   b -= 1;   b--;   --b;

suma = suma + valor;   suma += valor;
total = total * 2;     total *= 2;

```

**Pre vs post** (clave cuando lo usás en una expresión):

```java
int x = 5;
int y = x++; // y=5, x=6  (primero usa, después suma)
int z = ++x; // x=7, z=7  (primero suma, después usa)

```

# 4. Conversión de tipos (casting) e inferencia

### Inferencia de literales

- `3.5` es `double`. Para `float`: `3.5f`.
- `12345678901` puede requerir `L` si se pasa del rango de `int`: `12345678901L`.

### Widening (implícito) y narrowing (explícito)

- **De menor a mayor**: automático.
    
    ```java
    int a = 10;
    double b = a; // OK
    
    ```
    
- **De mayor a menor**: **casting** (podés perder datos).
    
    ```java
    double d = 3.14;
    int i = (int) d; // 3 (pierde decimales)
    
    int grande = 123456;
    short chico = (short) grande; // cuidado: desborda
    
    ```
    

### `var` (Java 10+)

- Deja que el compilador infera **solo en variables locales**:
    
    ```java
    var edad = 25;      // int
    var nombre = "Ana"; // String
    
    ```
    
- **No** cambia de tipo después. Usalo cuando el tipo es obvio.

# 5. Relacionales, lógicos y comparaciones “de verdad”

| Operador | Significado |
| --- | --- |
| `==` | Igualdad |
| `!=` | Distinto |
| `<`, `>`, `<=`, `>=` | Comparaciones numéricas |
| `&&` | AND (y lógico) |
| ` |  |
| `!` | NOT (negación) |

En Java, las condiciones **solo aceptan `boolean`** (no existe “0 es falso, distinto de 0 es verdadero”).

**Strings**: compará con `.equals`, **no** con `==`:

```java
String a = "hola", b = "hola";
System.out.println(a.equals(b)); // true
System.out.println(a == b);      // NO usar para lógica de igualdad

```

# 6. Salida por pantalla (y concatenación)

```java
System.out.print("Hola");      // sin salto
System.out.println(" Mundo");  // con salto
System.out.println("Edad: " + edad); // concatena

```

# 7. Entrada de datos con `Scanner`

### Teclado (`System.in`)

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);

System.out.print("Edad: ");
int edad = sc.nextInt();

sc.nextLine(); // LIMPIA el salto pendiente si después vas a usar nextLine()

System.out.print("Nombre: ");
String nombre = sc.nextLine();

// sc.close(); // Evitalo si vas a seguir usando System.in en el programa

```

**Métodos útiles**: `nextInt()`, `nextDouble()`, `nextBoolean()`, `next()`, `nextLine()`.

⚠️ **Gotcha clásico**: después de `nextInt()` queda un “Enter” en el buffer. Si luego usás `nextLine()`, **consumí** ese Enter con un `sc.nextLine()` “de limpieza”.

### Leer archivo de texto

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class App {
  public static void main(String[] args) throws FileNotFoundException {
    File f = new File("datos.txt");
    try (Scanner s = new Scanner(f)) {     // try-with-resources lo cierra solo
      while (s.hasNextInt()) {
        System.out.println(s.nextInt());
      }
    }
  }
}

```

# 8. Condicionales (if/else), `switch` y ternario

### If / else (doble y simple)

```java
if (nota >= 6) {
  System.out.println("Aprobado");
} else {
  System.out.println("Desaprobado");
}

if (edad >= 18)
  System.out.println("Mayor"); // una sola instrucción: llaves opcionales (igual, RECOMIENDO poner llaves)

```

### Condicional múltiple: `switch`

### Forma clásica

```java
switch (opcion) {
  case 1:
    System.out.println("Alta");
    break;
  case 2:
    System.out.println("Baja");
    break;
  default:
    System.out.println("Otra");
}

```

- **Fallthrough**: si olvidás `break`, “cae” a los siguientes `case`.

### Forma moderna (Java 14+): flecha `>`

Más corto y sin `break`:

```java
String dia = "LUNES";
switch (dia) {
  case "LUNES", "MIERCOLES", "VIERNES" -> System.out.println("Cursado");
  case "SABADO", "DOMINGO"             -> System.out.println("Finde");
  default                               -> System.out.println("Normal");
}

```

### Operador ternario (cuando solo decidís un valor)

```java
String resultado = (nota >= 6) ? "Aprobado" : "Desaprobado";

```

Úsalo si **ambas ramas** asignan **una misma variable**. Si hay lógica más grande, mejor `if/else`.

# 9. Bucles: `for`, `while`, `do-while` (+ `break` / `continue`)

### `for` (contado, 0..N)

```java
System.out.print("{");
for (int i = 1; i <= 5; i++) {
  System.out.print(i);
  if (i < 5) System.out.print(", ");
}
System.out.println("}"); // {1, 2, 3, 4, 5}

```

### `while` (0..N, chequea antes)

```java
int cont = 0;
while (cont < 5) {
  cont++;
  System.out.println("Vuelta: " + cont);
}

```

### `do-while` (1..N, siempre una vuelta)

```java
int intento = 0;
do {
  intento++;
  System.out.println("Intento " + intento);
} while (intento < 3);

```

### `break` y `continue`

```java
for (int i = 1; i <= 10; i++) {
  if (i % 2 == 0) continue; // salta pares
  System.out.print(i);
  if (i == 7) break;        // corta en 7
  System.out.print(", ");
}
// salida: 1, 3, 5, 7

```

# 10. Novedades útiles de Java moderno

### `switch` con `>` (Java 14+)

Ya lo viste: **más seguro** (sin `break`) y se pueden agrupar casos con comas.

### Bloques de texto `"""` (Java 15+)

Cadenas multilínea legibles (ideal para HTML/JSON/SQL):

```java
String html = """
  <html>
    <body>
      <h1>Bienvenidos</h1>
    </body>
  </html>
  """;
System.out.println(html);

```

# 11. Buenas prácticas rápidas

- **Inicializá** variables antes de usarlas.
- **Nombres descriptivos** en `camelCase` (`totalNeto`, `esMayorDeEdad`).
- **Constantes** con `final` y MAYÚSCULAS: `final double PI = 3.14159;`
- **Usá llaves `{}`** aunque haya una sola línea (evita bugs tontos).
- **`equals` para `String`** (no `==`).
- Evitá mezclar `nextInt()` y `nextLine()` sin limpiar el buffer.
- **Evita cerrar `System.in`** (deja vivo el `Scanner` si vas a seguir leyendo).
- Para archivos/recursos, **try-with-resources**.
- Comentá **el porqué**, no lo obvio.