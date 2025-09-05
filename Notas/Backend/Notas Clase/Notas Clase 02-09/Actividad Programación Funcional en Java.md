### **1. Programación Funcional en Java**

# **Guía — Menú Funcional + ApplicationContext (Singleton) + [Streams](https://uv.frc.utn.edu.ar/mod/url/view.php?id=523558) (Java 21)**

  

> Importante: Esta guía es conceptual. No damos el código de cada clase; definimos qué debe hacer cada componente y los criterios de aceptación. Al final de cada etapa dejamos solo el App.java como punto de partida de ejecución. El objetivo es que implementemos nosotros cada pieza sin copiar/pegar.

---

## **Bootstrap del proyecto Maven (Java 21)**

### **1 ¿Por qué Maven + Java 21?**

- Usamos **Maven** para gestionar ciclo de vida (compilar, empaquetar, ejecutar) y dependencias.
- Definimos **Java 21** con la propiedad `maven.compiler.release` para compilar con el _target_ correcto y evitar inconsistencias de bytecode.
- Sumamos el **exec-maven-plugin** para ejecutar la app con `mvn exec:java` directamente.

### **2 Creación del proyecto**

```Bash
mvn -q archetype:generate \
  -DgroupId=utnfc.isi.back \
  -DartifactId=menu-funcional \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
cd menu-funcional
```

### **3** `**pom.xml**` **comentado (mínimo suficiente)**

- `maven.compiler.release=21`: fija versión de lenguaje/bytecode de salida.
- `exec-maven-plugin`: indica la **clase principal** a ejecutar.
- `maven-jar-plugin` (opcional): permite empaquetar un **jar ejecutable** con `Main-Class` en el manifest.

```XML
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>utnfc.isi.back</groupId>
  <artifactId>menu-funcional</artifactId>
  <version>1.0.0</version>
  <properties>
    <maven.compiler.release>21</maven.compiler.release>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
  <dependencies>
    <!-- Trabajamos solo con el JDK -->
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>3.5.1</version>
        <configuration>
          <mainClass>utnfc.isi.back.App</mainClass>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.4.2</version>
        <configuration>
          <archive>
            <manifest>
              <mainClass>utnfc.isi.back.App</mainClass>
            </manifest>
          </archive>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

### **Estructura de paquetes sugerida**

```Bash
utnfc
└── isi
    └── back
        ├── App.java
        ├── AppStreams.java
        ├── app
        │   ├── Actions.java
        │   ├── DataSeeder.java
        │   └── Persona.java
        └── menu
            ├── ApplicationContext.java
            ├── FuncAction.java
            ├── Menu.java
            └── MenuOption.java
```

### **Comandos útiles**

```Bash
mvn -q exec:java                # ejecutar
mvn -q -DskipTests package      # empaquetar
java -jar target/menu-funcional-1.0.0.jar
```

---

## **Etapa 1 — Menú funcional (contratos y esqueleto)**

### **1.1 Contexto**

Escencialmente vamos a reahacer el parcial 4 de AED pero con java y al más alto nivel de programción que java nos permite.

Queremos un **menú de consola** que muestre opciones numeradas y ejecute una **acción** por opción. En esta etapa **no** habrá estado compartido; solo nos concentramos en la **arquitectura del menú** y el **contrato funcional** de las acciones.

Queremos que todo el funcionamiento del Menú sea genérico e independiente de cada programa en particular, es decir que con solo armar la lista de acciones podamos aprovechar el código de implementación del Menú en cualquier programa que lo necesitemos.

### **1.2 Componentes que debemos implementar (nombres y responsabilidades)**

1. `**FuncAction**` **(interfaz funcional)**
    - Rol: representar una **acción ejecutable** del menú.
    - Firma: `void run()` (sin parámetros en esta etapa).
    - Criterio de aceptación: debe permitir usar **lambdas** y **method references**.
2. `**MenuOption**` **(record/clase de opción)**
    - Rol: encapsular una opción de menú `(código, etiqueta, acción)`.
    - Atributos: `int code`, `String label`, `FuncAction action`.
    - Criterio de aceptación: inmutable y con acceso a sus campos.
3. `**Menu**` **(control del flujo)**
    - Rol: mostrar opciones, leer entrada, resolver la acción y ejecutarla.
    - Requisitos:
        - Mostrar todas las opciones ordenadas por `code`.
        - Aceptar `0` para salir.
        - Validar entradas no numéricas sin romper el programa.
        - Buscar la opción por `code` y, si no existe, informar "Opción inválida".
    - Criterio de aceptación: debe permitir inyectar una lista de `MenuOption` en el constructor.

### **1.3 Reglas de interacción**

- El menú se repite hasta que elijamos `0`.
- No hay estado global. Cada acción es **auto-contenida** (por ahora es completamente idependiente y solo imprime algo).

### **1.4** `**App.java**` **de referencia (único código de esta etapa)**

> Este archivo sirve para probar que nuestro menú y contratos funcionan. Si este main corre, la etapa está cumplida.

```Java
package utnfc.isi.back.menu;

import java.util.List;

public class App {
    public static void main(String[] args) {
        var opciones = List.of(
            new MenuOption(1, "Hola mundo", () -> System.out.println("¡Hola!")),
            new MenuOption(2, "Mostrar hora", () -> System.out.println(java.time.LocalTime.now()))
        );
        new Menu("Menú Funcional — Etapa 1", opciones).run();
    }
}
```

---

## **Etapa 2 — ApplicationContext (Singleton) estilo** _**key–value store**_

### **2.1 Contexto**

Ahora necesitamos **estado compartido** entre acciones (ej.: un `Scanner`, datasets en memoria, configuraciones). Diseñamos un `**ApplicationContext**` tipo diccionario **global** controlado (patrón **Singleton**).

### **2.1.1 Fundamentos y flujo de uso**

**¿Por qué** `**ApplicationContext**` **como Singleton?**

- Necesitamos **una única instancia** de contexto que actúe como _fuente de verdad_ para toda la app: menú y opciones deben ver **el mismo estado** (mismo `Scanner`, mismos datasets, misma configuración).
- Evitamos pasar referencias del contexto por todos lados (boilerplate) o recurrir a **variables estáticas globales** (poco encapsuladas y difíciles de testear).
- El patrón **Singleton** con _lazy holder_ garantiza **creación diferida**, **thread‑safety** por carga de clase y una API clara: `ApplicationContext.getInstance()`.
- Es una introducción didáctica al concepto de **contexto de aplicación** que luego veremos en frameworks como Spring.

**Alternativas (y por qué no ahora):**

- **Pasar el contexto por parámetros** en todos los métodos: es explícito, pero engorroso (propaga la referencia por muchos niveles y endurece firmas).
- **Framework DI/IoC (p. ej., Spring)**: ideal en proyectos grandes; acá sería **sobre‑ingeniería** para una app de consola de práctica.

**Aspectos de diseño que cuidamos:**

- **Encapsulación** del _key–value store_ (no exponemos el `Map` crudo).
- **API tipada** con `get(key, Class<T>)` para centralizar _casts_ y fallar temprano si el tipo no coincide.
- **Operaciones con semántica clara**: `put` (upset), `set` (reemplazo estricto), `remove`, `contains`.
- **Thread‑safety**: `ConcurrentHashMap` + singleton con _holder_ (si mañana agregamos hilos).

**Flujo de uso (simulado):**

```Bash
App.main()
  └─ ctx = ApplicationContext.getInstance()
  └─ new Menu(...).run(ctx)
       ├─ ctx.put("in", new Scanner(System.in))   // menú registra el scanner
       ├─ mostrar opciones
       ├─ leer opción (ej: 3 → "Top N por edad")
       └─ acción.run(ctx)
            ├─ List<Persona> personas = ctx.get("personas", List.class)
            ├─ personas.stream() ... (consultas conStreams)
            ├─ ctx.set("personas", personasActualizadas)
            └─ imprimir resultados
```

### **2.2 Componentes y responsabilidades**

1. `**ApplicationContext**` **(Singleton)**
    - Rol: almacenar y compartir objetos por **clave** (String) en un **Map** interno.
    - Requisitos mínimos de API:
        - `put(String key, Object value)` → inserta/actualiza un valor.
        - `get(String key)` → recupera el valor bruto.
        - `<T> T get(String key, Class<T> type)` → recupera con _cast_ centralizado.
        - `remove(String key)` / `contains(String key)`.
        - `set(String key, Object newValue)` → **reemplaza solo si la clave existe** (si no, lanza excepción).
    - Semántica esperada:
        - `put` hace **upset** (inserta o actualiza).
        - `set` es **estricto** (falla si la clave no existe).
    - Criterio de aceptación: implementación **thread-safe** usando `ConcurrentHashMap` y **Singleon** con _lazy holder_.
2. **Ajuste del contrato funcional** (`FuncAction`)
    - Nueva firma: `void run(ApplicationContext ctx)`.
    - Criterio de aceptación: todas las acciones reciben el contexto.
3. `**Menu**` **(versión con contexto)**
    - Debe **inyectar** el `Scanner` en el contexto con la clave `"in"` al inicio y cerrarlo al final.
    - Debe invocar `action.run(ctx)` al ejecutar cada opción.

### **2.3 Reglas de interacción**

- Las acciones pueden recuperar objetos desde el contexto: `ctx.get("in", Scanner.class)`.
- El contexto es **único** (`ApplicationContext.getInstance()`).

### **2.4** `**App.java**` **de referencia**

> Este main prueba que el menú con contexto funciona.

```Java
package utnfc.isi.back.menu;

import java.util.List;

public class App {
    public static void main(String[] args) {
        var ctx = ApplicationContext.getInstance();
        ctx.put("instrucciones", "Demo etapa 2");

        var opciones = List.of(
            new MenuOption(1, "Hola mundo", c -> System.out.println("¡Hola!")),
            new MenuOption(2, "Mostrar hora", c -> System.out.println(java.time.LocalTime.now()))
        );
        new Menu("Menú Funcional — Etapa 2", opciones).run(ctx);
    }
}
```

---

## **Etapa 3 — Datos en memoria + [Streams](https://uv.frc.utn.edu.ar/mod/url/view.php?id=523558) (acciones reales)**

### **3.1 Contexto**

Con el menú y el contexto listos, incorporamos un **modelo** (`Persona`) y un **generador** de datos en memoria (`DataSeeder`). A partir de estos datos, vamos a implementar una serie de requerimientos que vamos a denominar acciones con **[Streams](https://uv.frc.utn.edu.ar/mod/url/view.php?id=523558)/Collectors**.

Se espera entonces, que el programa al inicio, genere la base de personas para trabajar, y sobre esta base de personas almacenadas en una lista en el contexto, realice ciertas acciones definidas en los requerimientos de cada opción de menu.

Para la implementación de las acciones se requiere el uso de [Streams](https://uv.frc.utn.edu.ar/mod/url/view.php?id=523558) en todos los casos posibles.

### **3.2 Componentes y responsabilidades**

1. `**Persona**` **(modelo de dominio)**
    - Atributos: `documento`, `nombre`, `apellido`, `edad`, `ciudad`.
    - Recomendación: usar **Lombok** (`@Data`, `@AllArgsConstructor`, `@NoArgsConstructor`).
2. `**DataSeeder**` **(generador de datos)**
    - Rol: crear `List<Persona>` con datos aleatorios **razonables**.
    - Método de clase `generar(cantidad)` que recibe la cantidad de personas a generar y retorna la lista de personas con datos aleatorios
    - Criterios: rango de edad 18–65, ciudades/nombres/ apellidos desde arrays base.
3. `**Actions**` **(acciones de menú)**
    - Implementar métodos de instancia con firma `void metodo(ApplicationContext ctx)`.
    - Acciones mínimas (usando [Streams](https://uv.frc.utn.edu.ar/mod/url/view.php?id=523558)):
    - **Listar**
        - Requisito: mostrar por pantalla la cantidad total de elementos y los primeros 20.
        - Criterio de aceptación: si hay más de 20, debe indicarse con un mensaje que solo se muestran los primeros.
    - **Buscar**
        - Requisito: recibir un texto y filtrar coincidencias en `nombre` o `apellido`.
        - Criterio de aceptación: mostrar la cantidad de coincidencias encontradas y listar hasta 20 resultados.
    - **Top N por edad** (desc)
        - Requisito: solicitar al usuario un número `N` y mostrar las `N` personas de mayor edad.
        - Criterio de aceptación: validar entrada numérica; ordenar de mayor a menor edad; mostrar resultados con formato uniforme.
    - **Conteo por ciudad**
        - Requisito: agrupar personas por `ciudad` y contar cuántas hay en cada una.
        - Criterio de aceptación: mostrar el listado de ciudades ordenado de mayor a menor cantidad, con ciudad y número de personas.
    - **Estadísticas de edad**
        - Requisito: calcular `min`, `max`, `avg` y `count` sobre el campo `edad`.
        - Criterio de aceptación: mostrar todos los valores en una sola línea clara (`count, min, avg, max`).

### **3.3 Reglas de interacción**

- Debemos **sembrar** el dataset en el contexto con la clave `"personas"`.
- El `Scanner` está disponible en `"in"` (cargado por `Menu`).
- No usamos archivos CSV en esta etapa (todo en memoria).

### **3.4** `**App.java**` **de referencia**

> Este main prueba el flujo completo con datos y acciones.

```Java
package utnfc.isi.back.menu;

import java.util.*;

public class App {
    public static void main(String[] args) {
        var ctx = ApplicationContext.getInstance();
        ctx.put("personas", DataSeeder.generar(200));

        var actions = new Actions();
        var opciones = List.of(
            new MenuOption(1, "Listar (20 primeras)", actions::listar),
            new MenuOption(2, "Buscar por texto",    actions::buscar),
            new MenuOption(3, "Top N por edad",      actions::topEdades),
            new MenuOption(4, "Conteo por ciudad",   actions::conteoPorCiudad),
            new MenuOption(5, "Estadísticas de edad",actions::estadisticasEdad)
        );
        new Menu("Menú Funcional — Etapa 3", opciones).run(ctx);
    }
}
```

---

## **Apéndice — Convenciones y claves del contexto**

- Claves sugeridas: `"in"` (Scanner), `"personas"` (List), `"config"` (Map<String,Object> si hiciera falta).
- Evitar colisiones de claves: usar prefijos si sumamos módulos (p. ej., `"personas.dataset"`).
- `get(key, Class<T>)` centraliza los _casts_ y mejora legibilidad.
- `set(key, value)` sirve para reemplazos estrictos; si queremos upsert, usamos `put`.

> Con esto, tenemos la columna vertebral de una app de consola. Es en general un parcial 4 de AED pero trabajado a alto nivel y en lenguaje Java 😜.

---

---

# Como se ve el codigo hasta Etapa 2

## 1) `ApplicationContext.java`

**Ruta:** `src/main/java/utnfc/isi/back/menu/ApplicationContext.java`

```Java
package utnfc.isi.back.menu;

import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

/**
 * ApplicationContext (Singleton)
 * - Guarda objetos "compartidos" por clave (String).
 * - Thread-safe con ConcurrentHashMap.
 * - Acceso global controlado con getInstance() (lazy holder).
 *
 * Usos típicos:
 *   ctx.put("in", scanner);
 *   Scanner in = ctx.get("in", Scanner.class);
 *   ctx.put("personas", List.of(...));
 */
public final class ApplicationContext {

    // Almacenamiento interno (key-value). Concurrent para seguridad básica en hilos.
    private final Map<String, Object> store = new ConcurrentHashMap<>();

    // Constructor privado → nadie puede crear otra instancia
    private ApplicationContext() {}

    // Lazy holder: crea la instancia cuando se usa por primera vez (thread-safe por carga de clase)
    private static class Holder {
        private static final ApplicationContext INSTANCE = new ApplicationContext();
    }

    /** Punto de acceso global */
    public static ApplicationContext getInstance() {
        return Holder.INSTANCE;
    }

    /** Inserta o actualiza (upsert) */
    public void put(String key, Object value) {
        store.put(key, value);
    }

    /** Obtiene el valor "crudo" (puede ser null) */
    public Object get(String key) {
        return store.get(key);
    }

    /**
     * Obtiene con tipo (hace el cast centralizado y falla temprano si no coincide).
     * Si la clave no existe, retorna null.
     */
    public <T> T get(String key, Class<T> type) {
        Object v = store.get(key);
        if (v == null) return null;
        if (!type.isInstance(v)) {
            throw new ClassCastException("La clave '" + key + "' contiene " +
                    v.getClass().getName() + " y no " + type.getName());
        }
        return type.cast(v);
    }

    /** Reemplaza solo si la clave ya existe; si no existe, lanza excepción. */
    public void set(String key, Object newValue) {
        if (!store.containsKey(key)) {
            throw new IllegalStateException("No existe la clave '" + key + "' para set()");
        }
        store.put(key, newValue);
    }

    /** ¿Existe la clave? */
    public boolean contains(String key) {
        return store.containsKey(key);
    }

    /** Quita la clave y devuelve el valor previo (o null si no estaba) */
    public Object remove(String key) {
        return store.remove(key);
    }
}
```

---

## 2) `FuncAction.java` (actualizado)

**Ruta:** `src/main/java/utnfc/isi/back/menu/FuncAction.java`

```Java
package utnfc.isi.back.menu;

/**
 * Interfaz funcional para acciones de menú (con contexto).
 * Ahora cada acción recibe el ApplicationContext.
 */
@FunctionalInterface
public interface FuncAction {
    void run(ApplicationContext ctx);
}
```

---

## 3) `Menu.java` (versión con contexto)

**Ruta:** `src/main/java/utnfc/isi/back/menu/Menu.java`

```Java
package utnfc.isi.back.menu;

import java.util.*;
import java.util.stream.Collectors;

/**
 * Menú de consola — Etapa 2
 * - Igual que Etapa 1, pero ahora trabaja con ApplicationContext.
 * - Inyecta un Scanner en el contexto con clave "in" al inicio y lo cierra al final.
 * - Ejecuta las acciones con action.run(ctx).
 */
public class Menu {
    private final String title;
    private final List<MenuOption> options;

    public Menu(String title, List<MenuOption> options) {
        this.title = Objects.requireNonNull(title, "title no puede ser null");
        this.options = new ArrayList<>(Objects.requireNonNull(options, "options no puede ser null"));
    }

    /** Azúcar sintáctica: si no te pasan ctx, usamos el singleton por defecto. */
    public void run() {
        run(ApplicationContext.getInstance());
    }

    /** Versión "real" con contexto explícito. */
    public void run(ApplicationContext ctx) {
        // Preparamos una vista ORDENADA una sola vez
        final List<MenuOption> ordenadas = options.stream()
                .sorted(Comparator.comparingInt(MenuOption::code))
                .collect(Collectors.toList());

        // Creamos el Scanner y lo registramos en el contexto como "in"
        try (Scanner in = new Scanner(System.in)) {
            ctx.put("in", in); // queda disponible para las acciones

            while (true) {
                // Pantalla
                System.out.println();
                System.out.println(title);
                for (MenuOption o : ordenadas) {
                    System.out.printf("%d) %s%n", o.code(), o.label());
                }
                System.out.println("0) Salir");

                // Leer entrada
                System.out.print("Elegí una opción (0 para salir): ");
                String linea = in.nextLine().trim();

                // Validar número
                int codeElegido;
                try {
                    codeElegido = Integer.parseInt(linea);
                } catch (NumberFormatException e) {
                    System.out.println("⚠ Entrada no numérica. Probá de nuevo.\n");
                    continue;
                }

                // Salida
                if (codeElegido == 0) {
                    System.out.println("Hasta luego 👋");
                    break;
                }

                // Buscar y ejecutar
                Optional<MenuOption> elegida = ordenadas.stream()
                        .filter(o -> o.code() == codeElegido)
                        .findFirst();

                if (elegida.isPresent()) {
                    try {
                        elegida.get().action().run(ctx); // ahora pasa el contexto
                    } catch (Exception ex) {
                        System.out.println("⚠ Error al ejecutar la opción: " + ex.getMessage());
                    }
                } else {
                    System.out.println("⚠ Opción inválida.\n");
                }
            }
        } finally {
            // Limpieza opcional del contexto (evita dejar "in" colgado si querés ser prolijo)
            ctx.remove("in");
        }
    }
}
```

---

## 4) `App.java` (para probar Etapa 2)

**Ruta:** `src/main/java/utnfc/isi/back/App.java`

```Java
package utnfc.isi.back;

import utnfc.isi.back.menu.ApplicationContext;
import utnfc.isi.back.menu.Menu;
import utnfc.isi.back.menu.MenuOption;

import java.time.LocalTime;
import java.util.List;

/**
 * App de prueba para Etapa 2:
 * - Obtiene el contexto singleton
 * - Carga una clave de demo
 * - Arma el menú con acciones que reciben ctx (aunque no lo usen aún)
 */
public class App {
    public static void main(String[] args) {
        var ctx = ApplicationContext.getInstance();
        ctx.put("instrucciones", "Demo etapa 2"); // ejemplo de estado compartido

        var opciones = List.of(
            new MenuOption(1, "Hola mundo", c -> System.out.println("¡Hola!")),
            new MenuOption(2, "Mostrar hora", c -> System.out.println(LocalTime.now()))
        );

        new Menu("Menú Funcional — Etapa 2", opciones).run(ctx);
    }
}
```

---

---

# Como queda la Etapa 3

## 1) `Persona` (modelo)

**Ruta:** `src/main/java/utnfc/isi/back/app/Persona.java`

```Java
package utnfc.isi.back.app;

/**
 * Usamos record (Java 21) para un POJO inmutable y compacto.
 * Campos pedidos: documento, nombre, apellido, edad, ciudad.
 */
public record Persona(String documento, String nombre, String apellido, int edad, String ciudad) {}
```

---

## 2) `DataSeeder` (genera lista en memoria)

**Ruta:** `src/main/java/utnfc/isi/back/app/DataSeeder.java`

```Java
package utnfc.isi.back.app;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.ThreadLocalRandom;

/**
 * Genera datos "creíbles" en memoria para jugar con Streams.
 * - Edades entre 18 y 65
 * - Nombres/apellidos/ciudades de arreglos base
 * - Documento simple (string numérica)
 */
public class DataSeeder {

    private static final String[] NOMBRES = {
            "Ana","Luis","María","Juan","Sofía","Pablo","Lucía","Carlos","Valentina","Mateo",
            "Ezequiel","Agustina","Camila","Julián","Florencia","Martín","Ramiro","Micaela"
    };

    private static final String[] APELLIDOS = {
            "Gómez","Fernández","Rodríguez","López","Martínez","Pérez","Sánchez","Díaz","Alvarez","Romero",
            "Suárez","Páez","Vega","Rossi","Luna","Molina","Castro","Herrera"
    };

    private static final String[] CIUDADES = {
            "Córdoba","CABA","Rosario","Mendoza","La Plata","San Miguel de Tucumán","Salta","Neuquén"
    };

    /**
     * Genera "cantidad" personas con datos aleatorios.
     */
    public static List<Persona> generar(int cantidad) {
        var rnd = ThreadLocalRandom.current();
        List<Persona> personas = new ArrayList<>(cantidad);

        for (int i = 0; i < cantidad; i++) {
            String nombre   = NOMBRES[rnd.nextInt(NOMBRES.length)];
            String apellido = APELLIDOS[rnd.nextInt(APELLIDOS.length)];
            String ciudad   = CIUDADES[rnd.nextInt(CIUDADES.length)];
            int    edad     = rnd.nextInt(18, 66); // [18,65]
            String documento = String.valueOf(30_000_000 + rnd.nextInt(0, 20_000_000)); // DNI-like

            personas.add(new Persona(documento, nombre, apellido, edad, ciudad));
        }
        return personas;
    }
}
```

---

## 3) `Actions` (acciones del menú con Streams)

**Ruta:** `src/main/java/utnfc/isi/back/app/Actions.java`

```Java
package utnfc.isi.back.app;

import utnfc.isi.back.menu.ApplicationContext;

import java.util.Comparator;
import java.util.IntSummaryStatistics;
import java.util.List;
import java.util.Locale;
import java.util.Map;
import java.util.Scanner;
import java.util.stream.Collectors;

/**
 * Acciones del menú (todas reciben ApplicationContext).
 * Claves usadas:
 *   - "personas" : List<Persona>
 *   - "in"       : Scanner (inyectado por Menu)
 */
public class Actions {

    // Helper para obtener la lista desde el contexto, con chequeo de tipo
    @SuppressWarnings("unchecked")
    private List<Persona> personas(ApplicationContext ctx) {
        Object v = ctx.get("personas");
        if (v == null) throw new IllegalStateException("No hay datos: ctx['personas'] es null");
        if (!(v instanceof List<?> l) || (l.stream().findFirst().orElse(null) instanceof Persona) == false) {
            // Nota: chequeo sencillo; podrías hacer validación más estricta si querés
            throw new ClassCastException("ctx['personas'] no es List<Persona>");
        }
        return (List<Persona>) v;
    }

    // Helper para pedir Scanner
    private Scanner in(ApplicationContext ctx) {
        Scanner in = ctx.get("in", Scanner.class);
        if (in == null) throw new IllegalStateException("No hay Scanner: ctx['in'] es null");
        return in;
    }

    /**
     * 1) Listar: muestra cantidad total y las primeras 20 personas.
     */
    public void listar(ApplicationContext ctx) {
        List<Persona> personas = personas(ctx);

        System.out.println("\nTotal: " + personas.size());
        personas.stream()
                .limit(20)
                .forEach(p -> System.out.printf("- %s %s (%d) — %s — DNI %s%n",
                        p.nombre(), p.apellido(), p.edad(), p.ciudad(), p.documento()));

        if (personas.size() > 20) {
            System.out.println("(Se muestran solo las primeras 20)");
        }
    }

    /**
     * 2) Buscar por texto en nombre o apellido (case-insensitive), listar hasta 20 resultados.
     */
    public void buscar(ApplicationContext ctx) {
        Scanner in = in(ctx);
        List<Persona> personas = personas(ctx);

        System.out.print("\nTexto a buscar (nombre/apellido): ");
        String q = in.nextLine().trim().toLowerCase(Locale.ROOT);

        List<Persona> halladas = personas.stream()
                .filter(p -> p.nombre().toLowerCase(Locale.ROOT).contains(q)
                          || p.apellido().toLowerCase(Locale.ROOT).contains(q))
                .limit(20)
                .toList();

        System.out.println("Coincidencias: " + halladas.size());
        halladas.forEach(p -> System.out.printf("- %s %s (%d) — %s%n",
                p.nombre(), p.apellido(), p.edad(), p.ciudad()));

        if (halladas.size() == 20) {
            System.out.println("(Se muestran como máximo 20 resultados)");
        }
    }

    /**
     * 3) Top N por edad (desc). Pide N por teclado y valida entrada.
     */
    public void topEdades(ApplicationContext ctx) {
        Scanner in = in(ctx);
        List<Persona> personas = personas(ctx);

        System.out.print("\nIngrese N (cantidad a listar): ");
        String linea = in.nextLine().trim();
        int N;
        try {
            N = Integer.parseInt(linea);
            if (N <= 0) {
                System.out.println("N debe ser > 0");
                return;
            }
        } catch (NumberFormatException e) {
            System.out.println("Entrada no numérica.");
            return;
        }

        personas.stream()
                .sorted(Comparator.comparingInt(Persona::edad).reversed())
                .limit(N)
                .forEach(p -> System.out.printf("- %s %s — %d años — %s%n",
                        p.nombre(), p.apellido(), p.edad(), p.ciudad()));
    }

    /**
     * 4) Conteo por ciudad: agrupar y ordenar desc por cantidad.
     */
    public void conteoPorCiudad(ApplicationContext ctx) {
        List<Persona> personas = personas(ctx);

        Map<String, Long> conteo = personas.stream()
                .collect(Collectors.groupingBy(Persona::ciudad, Collectors.counting()));

        System.out.println();
        conteo.entrySet().stream()
                .sorted(Map.Entry.<String, Long>comparingByValue().reversed())
                .forEach(e -> System.out.printf("- %s: %d%n", e.getKey(), e.getValue()));
    }

    /**
     * 5) Estadísticas de edad: count, min, avg, max.
     */
    public void estadisticasEdad(ApplicationContext ctx) {
        List<Persona> personas = personas(ctx);

        IntSummaryStatistics stats = personas.stream()
                .mapToInt(Persona::edad)
                .summaryStatistics();

        System.out.printf("\ncount=%d, min=%d, avg=%.2f, max=%d%n",
                stats.getCount(), stats.getMin(), stats.getAverage(), stats.getMax());
    }
}
```

---

## 4) `App.java` (sembrar datos + menú con acciones)

**Ruta:** `src/main/java/utnfc/isi/back/App.java`

```Java
package utnfc.isi.back;

import utnfc.isi.back.app.Actions;
import utnfc.isi.back.app.DataSeeder;
import utnfc.isi.back.menu.ApplicationContext;
import utnfc.isi.back.menu.Menu;
import utnfc.isi.back.menu.MenuOption;

import java.util.List;

/**
 * Etapa 3:
 * - Genera dataset en memoria y lo guarda en ctx["personas"]
 * - Construye el menú con acciones reales (Streams)
 */
public class App {
    public static void main(String[] args) {
        var ctx = ApplicationContext.getInstance();

        // Sembramos 200 personas (podés cambiar la cantidad)
        ctx.put("personas", DataSeeder.generar(200));

        var actions = new Actions();
        var opciones = List.of(
                new MenuOption(1, "Listar (20 primeras)", actions::listar),
                new MenuOption(2, "Buscar por texto",    actions::buscar),
                new MenuOption(3, "Top N por edad",      actions::topEdades),
                new MenuOption(4, "Conteo por ciudad",   actions::conteoPorCiudad),
                new MenuOption(5, "Estadísticas de edad",actions::estadisticasEdad)
        );

        new Menu("Menú Funcional — Etapa 3", opciones).run(ctx);
    }
}
```

---
