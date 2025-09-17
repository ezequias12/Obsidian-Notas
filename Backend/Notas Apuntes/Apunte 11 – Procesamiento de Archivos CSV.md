# 1) Qué es un CSV y por qué nos importa

Un **CSV** es un archivo de texto con **filas** y **columnas** separadas por un **delimitador** (`,` o `;`). Es súper común para intercambiar datos entre sistemas, pero trae desafíos: **comillas**, **espacios en blanco**, **escapes**, **acentos/charset** (UTF-8) y **orden de columnas**【】. En la cursada lo necesitás porque en el **parcial individual** vas a **procesar un CSV para cargar datos a BD**, así que es tema clave.

# 2) Lectura básica (sin dependencias): `Scanner` + `split`

La versión “mínima” es leer **línea por línea** y **dividir** por el delimitador. Todo con el **JDK** (sin libs externas)【】.
Consiste en leer el archivo línea por línea y luego usar el método `split()` de `String` para dividir cada línea por el delimitador.

```Java
Path csv = Path.of("personas.csv");
try (Scanner sc = new Scanner(Files.newBufferedReader(csv))) {
    while (sc.hasNextLine()) {
        String[] col = sc.nextLine().split(";");
        // col[0] → documento, col[1] → nombre, col[2] → apellido, col[3] → edad
    }
}
```

Esto te da un `String[]` por fila, que después podés **mapear** a objetos (por ej., un constructor que reciba `String[]`)【】.

**Cuándo usarlo:** archivos simples, delimitadores predecibles, sin comillas complicadas.

---

# 3) Lectura robusta con `BufferedReader` (+ charset y cierre seguro)

Cuando querés **controlar el charset** (UTF-8), manejar **excepciones** bien y asegurar **cierre automático**, usá `BufferedReader` con **try-with-resources**【】.

```Java
Path archivo = Path.of("personas.csv");
try (BufferedReader br = Files.newBufferedReader(archivo, StandardCharsets.UTF_8)) {
    String linea;
    while ((linea = br.readLine()) != null) {
        String[] col = linea.split(";");
        // procesar col...
    }
}
```

---

# 4) OpenCSV – cuando el CSV se pone “real”

Si el archivo trae **comillas, escapes, delimitadores variables** o querés limpiar casos raros sin reinventar la rueda, entrá con **OpenCSV**【】. Agregás la dependencia Maven:

```XML
<dependency>
  <groupId>com.opencsv</groupId>
  <artifactId>opencsv</artifactId>
  <version>5.9</version>
</dependency>
```

【】

### 4.1 `CSVReader` (por índice de columna)

Leés filas ya separadas/limpias y accedés por **índice**【】:

```Java
try (CSVReader reader = new CSVReader(Files.newBufferedReader(Path.of("personas.csv")))) {
    String[] fila;
    while ((fila = reader.readNext()) != null) {
        // fila[0] doc, fila[1] nombre, fila[2] apellido, fila[3] edad
    }
}
```

**Pro:** te resuelve los casos problemáticos.

**Contra:** dependés del **orden** de columnas【】.

### 4.2 `CSVReaderHeaderAware` (por nombre de columna)

Si tu CSV tiene **encabezado** (primera fila con nombres), leés cada fila como `**Map<String,String>**` y accedés por **nombre** (¡mucho más legible y resistente a cambios de orden!)【】:

```Java
try (CSVReaderHeaderAware reader =
        new CSVReaderHeaderAware(Files.newBufferedReader(Path.of("personas.csv")))) {
    Map<String, String> fila;
    while ((fila = reader.readMap()) != null) {
        String nombre = fila.get("nombre");
        String apellido = fila.get("apellido");
    }
}
```

### 4.3 `CsvToBean` + POJOs con Lombok (mapeo tipado)

La forma más OO: **anotás** tu clase y OpenCSV crea **objetos** directamente (fuerte tipado, limpio)【】【】:

```Java
@Data @NoArgsConstructor
public class Persona {
    @CsvBindByName(column = "documento") private Integer documento;
    @CsvBindByName(column = "nombre")    private String  nombre;
    @CsvBindByName(column = "apellido")  private String  apellido;
    @CsvBindByName(column = "edad")      private Integer edad;
}

try (Reader r = Files.newBufferedReader(Path.of("personas.csv"))) {
    List<Persona> personas = new CsvToBeanBuilder<Persona>(r)
        .withType(Persona.class)
        .withSeparator(';')
        .withIgnoreLeadingWhiteSpace(true)
        .build()
        .parse();
}
```

OpenCSV **vincula columnas ↔ atributos** gracias a las anotaciones, y te entrega `List<Persona>` lista para usar【】.

---

# 5) Streams & Collectors sobre la lista (consultas y “analytics” rápidos)

Ya con `List<Persona>` podés hacer consultas **declarativas**:

- **Promedio de edad** (stream primitivo para evitar boxing)【】:

```Java
double promedio = personas.stream().mapToInt(Persona::getEdad).average().orElse(0);
```

- **Resumen** (count, min, avg, max, sum) con `summaryStatistics`【】:

```Java
IntSummaryStatistics stats = personas.stream().mapToInt(Persona::getEdad).summaryStatistics();
```

- **Top 3 mayores** (orden desc)【】:

```Java
List<Persona> top3 = personas.stream()
    .sorted(Comparator.comparing(Persona::getEdad).reversed())
    .limit(3)
    .toList();
```

- **Filtrar y proyectar** (nombres de >21)【】:

```Java
List<String> nombresMayores = personas.stream()
    .filter(p -> p.getEdad() != null && p.getEdad() > 21)
    .map(Persona::getNombre)
    .toList();
```

- **Agrupar y contar** (por apellido)【】:

```Java
Map<String, Long> conteoPorApellido = personas.stream()
    .collect(Collectors.groupingBy(Persona::getApellido, Collectors.counting()));
```

- **Promedio de edad por apellido**【】:

```Java
Map<String, Double> promPorApellido = personas.stream()
    .collect(Collectors.groupingBy(Persona::getApellido,
             Collectors.averagingInt(Persona::getEdad)));
```

- **A** `**Map**` **con manejo de duplicados** (clave: documento; quedarse con el de mayor edad) y **preservar orden** con `LinkedHashMap`【】:

```Java
Map<Integer, Persona> porDocumento = personas.stream()
    .filter(p -> p.getDocumento() != null)
    .collect(Collectors.toMap(
        Persona::getDocumento,
        p -> p,
        (p1, p2) -> (p1.getEdad() >= p2.getEdad() ? p1 : p2),
        LinkedHashMap::new
    ));
```

- **Join de nombres** (a un solo string)【】:

```Java
String listado = personas.stream().map(Persona::getNombre)
    .collect(Collectors.joining(", ", "[", "]"));
```

---

# 6) Buenas prácticas (para que no duela en producción)

- **try-with-resources** siempre (Reader/BufferedReader/lines): evita archivos abiertos y fugas【】.
- `**trim()**` **+ validaciones** antes de parsear números: evitás `NumberFormatException` (control de nulos)【】.
- **Sin side-effects en streams**: preferí recolectar (`collect`) a estructuras nuevas【】.
- **Elegí bien la estructura**: `List` (orden y duplicados), `Set` (sin duplicados), `LinkedHashMap` (preserva orden) según lo que necesites【】.
- **No sobreusar Streams**: si un `for` simple es más claro, usalo. Streams suman cuando **mejoran legibilidad**【】.

---

# 7) Mini “end-to-end” listo para copiar

**CSV de ejemplo (**`**personas.csv**`**)**

```Plain
documento;nombre;apellido;edad
1;Ana;Pérez;22
2;Juan;Gómez;31
3;Mia;López;19
3;Mia;López;25
```

**POJO + carga + consultas**

```Java
import com.opencsv.bean.CsvBindByName;
import com.opencsv.bean.CsvToBeanBuilder;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Reader;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.*;
import java.util.stream.Collectors;

@Data @NoArgsConstructor
class Persona {
    @CsvBindByName(column="documento") private Integer documento;
    @CsvBindByName(column="nombre")    private String  nombre;
    @CsvBindByName(column="apellido")  private String  apellido;
    @CsvBindByName(column="edad")      private Integer edad;
}

public class AppCSV {
    public static void main(String[] args) throws Exception {
        List<Persona> personas;
        try (Reader r = Files.newBufferedReader(Path.of("personas.csv"))) {
            personas = new CsvToBeanBuilder<Persona>(r)
                    .withType(Persona.class)
                    .withSeparator(';')
                    .withIgnoreLeadingWhiteSpace(true)
                    .build()
                    .parse(); // List<Persona>
        }

        double promedio = personas.stream().mapToInt(Persona::getEdad).average().orElse(0);
        Map<String, Long> conteo = personas.stream()
                .collect(Collectors.groupingBy(Persona::getApellido, Collectors.counting()));
        List<Persona> top3 = personas.stream()
                .sorted(Comparator.comparing(Persona::getEdad).reversed())
                .limit(3).toList();
        Map<Integer, Persona> porDoc = personas.stream()
                .filter(p -> p.getDocumento() != null)
                .collect(Collectors.toMap(
                        Persona::getDocumento, p -> p,
                        (p1, p2) -> (p1.getEdad() >= p2.getEdad() ? p1 : p2),
                        LinkedHashMap::new
                ));

        System.out.println("Promedio edad: " + promedio);
        System.out.println("Conteo por apellido: " + conteo);
        System.out.println("Top3 por edad: " + top3);
        System.out.println("Mapa por documento: " + porDoc);
    }
}
```

(La configuración `CsvToBean` y las consultas están tomadas del enfoque del apunte)【】【】.

---

## En criollo, lo esencial

1. Si el CSV es “facilito”: `Scanner`/`BufferedReader` + `split`.
2. Si pinta más serio: **OpenCSV** (`CSVReader` o **por header** con `CSVReaderHeaderAware`).
3. Si querés **clases** y tipado: `**CsvToBean**` **+ anotaciones**.
4. Una vez cargado → **Streams/Collectors** para sacar métricas, agrupar, ordenar, mapear.
5. **Charset, try-with-resources, validaciones** y **estructura correcta** = te salvan en producción.