# 1) Programación funcional en Java (qué es y para qué me sirve)

**Idea base:** además de variables con datos (`int`, `String`, objetos), podés tener **variables que guardan “comportamientos”** (funciones). Esas funciones se pueden **pasar** a otros métodos o **devolver** como resultado. A los métodos que **reciben** o **devuelven** funciones se los llama _de orden superior_.

**En criollo:** igual que guardás `42` en una variable, ahora guardás “la lógica de sumar” en una variable —y después **la ejecutás**.

**Ejemplo mínimo (sin magia):**

```Java
// 1) Defino el “tipo” de función que quiero pasar (ver §2)
@FunctionalInterface
interface Operacion {
    float calcular(float a, float b);
}

// 2) Uso: guardo comportamientos en variables
Operacion suma  = (a, b) -> a + b; // aca tenemos una lambda que
// implementa calcular de la interfaz funcional Operacion
Operacion resta = (a, b) -> a - b;

// 3) Paso una función como parámetro
static float aplicar(Operacion op, float x, float y) {
    return op.calcular(x, y);
}

// 4) Ejecutar
float r1 = aplicar(suma, 10, 5);   // 15.0
float r2 = aplicar(resta, 10, 5);  // 5.0
```

**Cuándo conviene:** cuando tenés “qué hacer” variable (filtros, transformaciones, validaciones), o cuando querés **desacoplar** la lógica concreta del “esqueleto” del algoritmo.

**Importante:** Para entender el resto hace falta saber bien que es lambda y para que lo usamos, aunque luego se explica al detalle en la seccion 3.

En Java, una **lambda** es una **forma compacta de escribir una función anónima**, es decir, un pedacito de código que se puede guardar en una variable o pasar como parámetro.
Siempre implementa **la única operación abstracta** de una **interfaz funcional**.

**Sintaxis básica:**

```java
(parametros) -> expresión
(parametros) -> { bloque }
```

---

## 2) Interfaces funcionales y `@FunctionalInterface`

Java no tiene “funciones sueltas”, así que **representa una función con una interfaz de un solo método** (SAM: _Single Abstract Method_). A eso se le llama **interfaz funcional**. La anotación `@FunctionalInterface` es opcional pero recomendable: el compilador te avisa si, por error, agregaste otro método abstracto.

Osea, una interfaz funcional es una interfaz que tiene **exactamente un método abstracto**. Puede tener otros métodos `default` o `static`, pero solo uno sin cuerpo.
Ese metodo abstracto define la **firma** de la función (qué parámetros recibe y qué devuelve).

En el caso de la calculadora, en vez de hacer una clase para cada operacion de la misma, definimos una interfaz funcional `Operacion` con un solo metodo `calcular`. Luego podemos crear multiples implementaciones de esa interfaz usando lambdas o referencias a métodos.

**Ejemplo clásico de calculadora:**

```Java
@FunctionalInterface
interface Operacion {
    float calcular(float a, float b);
}

// Múltiples “estrategias” que respetan la misma firma:
Operacion mult = (a, b) -> a * b;
Operacion div  = (a, b) -> {
    if (b == 0) throw new ArithmeticException("No se puede dividir");
    return a / b;
};
// Osea, calcular nos brinda la "forma" de la función (dos floats, devuelve float) y
// nosotros luego vamos definiendo distintas operaciones que calzan con esa forma.
// estas operaciones las definimos mediante lambdas o referencias a métodos.
```

- La **interfaz funcional** define la “forma” (`int, int -> int`).
- Cada **lambda** aporta la lógica concreta (`a+b`, `a-b`).

**Regla mental:** _la interfaz funcional define la “forma” de la función_ (qué parámetros y qué devuelve). Cualquier lambda o referencia a método que “calce” esa forma, sirve.

---

## 3) Lambdas y Method References (formas compactas de escribir funciones)

**Lambdas**: `(parámetros) -> expresión` o `(parámetros) -> { bloque }`.

- Si hay **una sola expresión**, no hace falta `return` ni `{}`.
- Los **tipos** de parámetros casi siempre se **infieren**.

```Java
Operacion suma = (a, b) -> a + b;  // asi lo escribimos normalmente

// tambien se puede escribir asi, pero es mas verboso y no es comun:
Operacion resta = (float a, float b) -> { // con tipos explícitos y bloque
    return a - b;
};
```

**Method references**: cuando ya existe un método con **misma firma** que la interfaz funcional, podés apuntarlo directo:

Se usan para referenciar métodos existentes sin necesidad de escribir una lambda completa. Son más concisas y mejoran la legibilidad cuando el método ya está definido.
Es una forma más corta de escribir un lambda cuando el lambda solo llama a un método existente.
Usa la sintaxis `Clase::método` o `objeto::método`.

👉 En criollo: es un atajo para no tener que escribir (x -> algo.hacer(x)).

- Estático: `Clase::metodoEstatico`
- Instancia: `objeto::metodo`
- “De tipo” (instancia desconocida): `Tipo::metodo`
- Constructor: `Tipo::new`

en vez de escribir: `list.forEach(x -> System.out.println(x));`
podés escribir: `list.forEach(System.out::println);`

```Java
// Ejemplos típicos en streams:
List<String> nombres = List.of("Ana", "Juan", "Pedro");
nombres.forEach(System.out::println);         // Consumer<String>
List<Integer> longitudes = nombres.stream()
    .map(String::length)                      // Function<String,Integer>
    .toList();
```

**Cuándo usar qué:** Lambda cuando la lógica es breve y personalizada. Method reference cuando ya existe el método y querés máxima legibilidad.

---

## 4) Interfaces funcionales listas para usar (`java.util.function`)

Estas interfaces serian las "interfaces funcionales estándar" que Java provee en el paquete `java.util.function`. Son muy útiles porque cubren la mayoría de los casos comunes y evitan que tengas que definir tus propias interfaces funcionales cada vez que necesitas pasar una función como parámetro.

Cuando escribís un `stream().algo(...)`, **ese `algo` espera una interfaz funcional** como parámetro.

Ejemplo con `.filter(...)`:

```java
List<String> nombres = List.of("Ana", "Juan", "Pedro");

nombres.stream()
       .filter(n -> n.length() > 3) // acá va un Predicate<String>
       .forEach(System.out::println);
```

* `.filter(...)` espera un `Predicate<T>` (es decir, una función `T -> boolean`).
* El lambda `n -> n.length() > 3` **es la implementación concreta** de ese `Predicate<String>`.
* Entonces **vos no llamás directamente a un método de la interfaz**, sino que la API de Streams **usa tu lambda como el método de esa interfaz**.


No crees una interfaz cada vez. Usá las estándar:

- **Supplier<T>**: no recibe nada, devuelve un `T`. Ej: `Stream.generate(supplier)`.
- **Consumer<T>**: recibe `T`, no devuelve nada. Ej: `forEach`.
- **Predicate<T>**: recibe `T`, devuelve `boolean`. Ej: `filter`.
- **Function<T,R>**: transforma `T` en `R`. Ej: `map`.
- **UnaryOperator<T>**: `T -> T` (caso especial de Function). Ej: `map` con salida mismo tipo.
- **BinaryOperator<T>**: `(T,T) -> T` (combinar/acumular).

**Elegí por firma:** si necesitás “filtrar”, usá `Predicate<T>`; si necesitás “transformar”, `Function<T,R>`; si vas a “consumir”, `Consumer<T>`; si “producir” sin entrada, `Supplier<T>`.

En criollo

Las interfaces funcionales de `java.util.function` son contratos ya listos que dicen: “quiero una función de tal forma” (ej: recibe T, devuelve boolean).
Los métodos de Streams (`map`, `filter`, `forEach`, etc.) están escritos para recibir justo esas interfaces.

Cuando vos ponés un lambda o un method reference, lo que hacés es darle a la API de Streams la función que cumple el contrato de la interfaz.

---

## 5) Streams: qué son y cómo crearlos (fuentes)

**Qué es un Stream:** un **pipeline** (conducto) por el que pasan elementos. Vos declarás **qué** transformaciones querés (filtros, mapeos, orden, etc.) y recién se ejecutan cuando pedís un **resultado final** (operación terminal). **No** es una colección; **no guarda** datos.

**Fuentes típicas:**

```Java
// 1) Colecciones
Stream<String> s1 = List.of("a","b","c").stream();

// 2) Arreglos
Stream<Integer> s2 = Arrays.stream(new Integer[]{1,2,3});

// 3) Valores fijos
Stream<String> s3 = Stream.of("x","y","z");

// 4) Infinitos (¡usá limit!)
Stream<Integer> nat = Stream.iterate(0, n -> n + 1).limit(10);
Stream<Double> rands = Stream.generate(Math::random).limit(5);

// 5) Archivos (cada línea es un elemento)
try (Stream<String> lineas = java.nio.file.Files.lines(java.nio.file.Path.of("data.txt"))) {
    lineas.forEach(System.out::println);
}
```

**Imagen mental:** una **cascada**. Cada piedra (operación) transforma el flujo, y al final cae al lago (resultado).

---

## 6) Operaciones útiles (intermedias / terminales) + collectors + reduce + streams primitivos

## 6.1 Intermedias (encadenables, devuelven stream)

- `filter(Predicate)`: quedate con los que cumplen.
- `map(Function)`: transformá cada elemento (podés cambiar el tipo).
- `distinct()`: eliminá duplicados (usa `equals`/`hashCode`).
- `sorted()` / `sorted(Comparator)`: ordená natural o por criterio.
- `limit(n)` / `skip(n)`: recortar el flujo.

```Java
List<String> datos = List.of("ana", "juan", "ana", "pedro");
List<Integer> res = datos.stream()
    .filter(s -> s.length() > 3)   // "juan", "pedro"
    .distinct()                    // elimina repetidos
    .map(String::length)           // 4, 5
    .sorted()                      // 4, 5
    .toList();                     // [4, 5]
```

## 6.2 Terminales (cierran el stream)

- `forEach(Consumer)`: visitar (efecto lateral).
- `collect(...)`: recolectar (ver abajo).
- `count()`: contar.
- `min/max(Comparator)`: extremos.
- `findFirst()/findAny()`: uno (envueltos en `Optional`).
- `anyMatch/allMatch/noneMatch(Predicate)`: chequear condiciones.

```Java
long cuantos = datos.stream().filter(s -> s.startsWith("a")).count();
Optional<String> primero = datos.stream().findFirst();
boolean hayLargo = datos.stream().anyMatch(s -> s.length() >= 5);
```

## 6.3 Collectors (la “cocina” del resultado)

Se usan con `collect(Collectors.algo(...))`.

- **A listas/sets**:

  ```Java
  List<String> lista = datos.stream().toList(); // Java 16+
  // o:
  List<String> lista2 = datos.stream().collect(Collectors.toList());
  Set<String> set = datos.stream().collect(Collectors.toSet());
  ```

- **Unir strings**:

  ```Java
  String csv = datos.stream().collect(Collectors.joining(", "));
  ```

- **Agrupar**:

  ```Java
  // Map<Longitud, Lista de palabras con esa longitud>
  Map<Integer, List<String>> porLong = datos.stream()
      .collect(Collectors.groupingBy(String::length));

  // Conteo por grupo
  Map<Integer, Long> conteo = datos.stream()
      .collect(Collectors.groupingBy(String::length, Collectors.counting()));

  // Transformación dentro del grupo (downstream)
  Map<Integer, Set<Character>> letrasIniciales = datos.stream()
      .collect(Collectors.groupingBy(
          String::length,
          Collectors.mapping(s -> s.charAt(0), Collectors.toSet())
      ));
  ```

- **Particionar (true/false)**:

  ```Java
  Map<Boolean, List<String>> porA = datos.stream()
      .collect(Collectors.partitioningBy(s -> s.startsWith("a")));
  ```

- **A Map con manejo de duplicados y tipo de mapa**:

  ```Java
  // Clave: palabra, Valor: longitud. Si se repite clave, quedarse con la más larga
  Map<String, Integer> mapa = datos.stream().collect(
      Collectors.toMap(
          s -> s,                 // clave
          String::length,         // valor
          Integer::max,           // cómo resolver conflicto de clave
          LinkedHashMap::new      // tipo de mapa (opcional)
      )
  );
  ```

- **Estadísticas en una pasada**:

  ```Java
  IntSummaryStatistics stats = datos.stream()
      .mapToInt(String::length)
      .summaryStatistics();
  // stats.getCount(), getMin(), getAverage(), getMax(), getSum()
  ```

## 6.4 `reduce` (aplastar todo a un valor)

Formas típicas:

```Java
int suma = List.of(1,2,3,4).stream().reduce(0, (acc, x) -> acc + x);
// identidad 0, acumulador (acc, x) -> nuevoAcc
```

Con binario y combinador (cuando paralelizás o combinás parciales):

```Java
int suma2 = List.of(1,2,3,4).stream()
    .reduce(0, Integer::sum); // referencia a método
```

**Cuándo** `**reduce**` **vs** `**collect**`**:**

- `reduce`: obtener **un escalar** (número, string único).
- `collect`: construir **estructuras** (listas, mapas, sets, estadísticas).

## 6.5 Streams **primitivos**: `IntStream`, `LongStream`, `DoubleStream`

Evitan _boxing/unboxing_ y agregan terminales útiles (`sum`, `average`, `summaryStatistics`).

```Java
double promedio = List.of(10, 20, 30).stream().mapToInt(x -> x).average().orElse(0);
IntSummaryStatistics st = List.of(10, 20, 30).stream().mapToInt(x -> x).summaryStatistics();
```

**Cuándo usarlos:** pipelines **numéricos largos** o sensibles a rendimiento. Si trabajás con objetos, priorizá claridad y convertí a primitivo **solo** donde te rinde.

---

## 7) Buenas prácticas, anti-patrones y tips (lo que evita bugs)

- **Laziness y un-solo-uso:** nada corre hasta que hacés una **terminal**. Y un stream **no se puede reusar**:

  ```Java
  Stream<String> s = datos.stream();
  long c = s.count();
  // s.forEach(...); // ERROR: stream ya consumido
  ```

- **Evitá efectos colaterales** dentro del pipeline (`forEach` que modifica listas externas, acumulaciones manuales). Mejor **recolectá** o usá `reduce/collect`.
- `**peek**` **es para debug**, no para lógica (puede no ejecutarse si no hay terminal, y confunde).
- **Orden importa**: `distinct().sorted()` no es igual que `sorted().distinct()` (pensá en costo/semántica).
- **Archivos:** usá `try-with-resources` para cerrar el stream de líneas; definí charset si hace falta. Hacé el pipeline **adentro** del `try`.
- `**flatMap**`: aplana listas de listas (muy útil).

  ```Java
  List<List<String>> frases = List.of(
      List.of("hola", "mundo"),
      List.of("buen", "día")
  );
  List<String> tokens = frases.stream()
      .flatMap(List::stream)
      .toList(); // ["hola","mundo","buen","día"]
  ```

- **Cuándo NO usar streams:** si un `for` simple es **más claro** (por ejemplo, lógica con `break/continue` compleja), si **necesitás mutar** estructuras compartidas, o si estás haciendo **micro-loops muy chicos** donde el stream solo agrega ruido.

---

## Mini-bloques prácticos (copiá y probá)

**A) Aleatorios y métricas:**

```Java
List<Integer> nums = new java.util.Random().ints(100, 1, 1001).boxed().toList();
int max  = nums.stream().mapToInt(Integer::intValue).max().orElse(Integer.MIN_VALUE);
int min  = nums.stream().mapToInt(Integer::intValue).min().orElse(Integer.MAX_VALUE);
double avg = nums.stream().mapToInt(Integer::intValue).average().orElse(0);
System.out.println("min=" + min + ", max=" + max + ", avg=" + avg);
```

**B) Texto → tokens → conteo de palabras (agrupación):**

```Java
String texto = "hola mundo hola java streams mundo hola";
java.util.Map<String, Long> frec = java.util.Arrays.stream(texto.split("\\s+"))
    .collect(java.util.stream.Collectors.groupingBy(s -> s, java.util.stream.Collectors.counting()));
System.out.println(frec); // {hola=3, mundo=2, java=1, streams=1}
```

**C) Integración con tu modelo bancario (de los apuntes 8/9):**

```Java
List<Cuenta> cuentas = java.util.List.of(
    new Cuenta(1, 1200),
    new Inversion(2, 2000, 5.0f),
    new Corriente(3, 500, false),
    new Inversion(4, 1500, 3.0f),
    new Corriente(5, 0, true)
);

// saldo promedio (todas)
double prom = cuentas.stream().mapToDouble(Cuenta::getSaldo).average().orElse(0);

// saldo total SOLO de inversiones
double totalInv = cuentas.stream()
    .filter(c -> c instanceof Inversion)
    .mapToDouble(Cuenta::getSaldo)
    .sum();

// cuentas agrupadas por tipo concreto
java.util.Map<Class<?>, java.util.List<Cuenta>> porTipo = cuentas.stream()
    .collect(java.util.stream.Collectors.groupingBy(Object::getClass));

System.out.println("Promedio=" + prom + ", Total inversiones=" + totalInv);
System.out.println("Por tipo=" + porTipo);
```

---

## Qué tenés que grabarte (resumen express)

- **Funcional en Java** = funciones como datos (vía **interfaces funcionales** + **lambdas** + **method references**).
- **Streams** describen **qué** hacer; **se ejecutan** con una terminal; **no** se reusan.
- **Operaciones**: `filter/map/distinct/sorted/limit` (intermedias) + `collect/count/min/max/forEach/find*` (terminales).
- **Collectors**: `toList`, `joining`, `groupingBy`, `partitioningBy`, `toMap`, `summaryStatistics`.
- `**reduce**`: aplastar a un valor (número, string).
- **Primitivos (**`**IntStream**` **& cia.)**: rendimiento en pipelines numéricos.
- **Buenas prácticas**: sin side-effects, `peek` solo debug, `try-with-resources` con archivos, `flatMap` para aplanar.

---
