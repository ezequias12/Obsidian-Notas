# Resumen del Apunte 11 – Procesamiento de Archivos CSV

## 1) Qué es un CSV y por qué nos importa

- Un **CSV** es un archivo de texto que organiza datos en **filas** y **columnas**, utilizando un **delimitador** (comúnmente `,` o `;`) para separar los valores.
- Es un formato estándar para el **intercambio de datos** entre diferentes sistemas.
- **Desafíos comunes:** manejo de comillas, espacios en blanco, caracteres de escape, codificación de caracteres (especialmente **UTF-8** para acentos y caracteres especiales), y el orden de las columnas.
- **Importancia en la cursada:** Es un tema **clave** porque en el **parcial individual** se requerirá **procesar un CSV para cargar datos a una Base de Datos**.

---

## 2) Lectura básica (sin dependencias): `Scanner` + `split`

- Es la forma más **simple y directa** de leer un CSV usando solo el **JDK** (Java Development Kit), sin librerías externas.
- Consiste en leer el archivo **línea por línea** y luego usar el método `split()` de `String` para dividir cada línea por el delimitador.
- **Ejemplo de uso:**

  ```Java
  Path csv = Path.of("personas.csv");
  try (Scanner sc = new Scanner(Files.newBufferedReader(csv))) {
      while (sc.hasNextLine()) {
          String[] col = sc.nextLine().split(";");
          // col[0] → documento, col[1] → nombre, etc.
      }
  }
  ```

- El resultado es un `String[]` por cada fila, que luego se puede **mapear a objetos**.
- **Cuándo usarlo:** Para archivos CSV **simples**, con delimitadores **predecibles** y **sin comillas** o escapes complejos.

---

## 3) Lectura robusta con `BufferedReader` (+ charset y cierre seguro)

BufferedReader es una alternativa a Scanner que ofrece más **control** sobre la lectura del archivo. Se usa igual que Scanner, pero con algunas ventajas:

- Ofrece un mayor **control** sobre la lectura, permitiendo especificar el **charset** (como `StandardCharsets.UTF_8`) y asegurando el **cierre automático** del archivo mediante `try-with-resources`.
- Es preferible cuando se necesita manejar la codificación de caracteres o se busca una gestión de recursos más segura.
- **Ejemplo de uso:**

  ```Java
  Path archivo = Path.of("personas.csv");
  try (BufferedReader br = Files.newBufferedReader(archivo, StandardCharsets.UTF_8)) {
      String linea;
      while ((linea = br.readLine()) != null) {
          String[] col = linea.split(";");
          // procesar col[]
      }
  }
  ```

---

## 4) OpenCSV – cuando el CSV se pone “real”

- OpenCSV es una **librería externa** especializada en el manejo de archivos CSV.
- Es la librería recomendada para manejar archivos CSV con **comillas, escapes, delimitadores variables** y otras complejidades sin tener que implementar la lógica desde cero.
- **Dependencia Maven:**

  ```XML
  <dependency>
    <groupId>com.opencsv</groupId>
    <artifactId>opencsv</artifactId>
    <version>5.9</version>
  </dependency>
  ```

### 4.1 `CSVReader` (por índice de columna)

- Lee las filas ya **separadas y limpias** por OpenCSV.
- Se accede a los datos de cada columna por su **índice** (ej. `fila[0]`).
- **Pro:** Resuelve los casos problemáticos de formato CSV.
- **Contra:** La lectura **depende del orden** de las columnas en el archivo.
- **Ejemplo de uso:**

  ```Java
  try (CSVReader reader = new CSVReader(Files.newBufferedReader(Path.of("personas.csv")))) {
      String[] fila;
      while ((fila = reader.readNext()) != null) {
          // fila[0] doc, fila[1] nombre, etc.
      }
  }
  ```

### 4.2 `CSVReaderHeaderAware` (por nombre de columna)

- Ideal para CSVs que incluyen una **fila de encabezado** (headers).
- Cada fila se lee como un `Map<String, String>`, permitiendo acceder a los datos por el **nombre de la columna** (ej. `fila.get("nombre")`).
- **Ventaja:** Mucho más **legible** y **resistente a cambios en el orden** de las columnas.
- **Ejemplo de uso:**

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

- La forma más **orientada a objetos** y robusta.
- Permite **mapear directamente** las filas del CSV a **objetos POJO** (Plain Old Java Objects) fuertemente tipados.
- Se utilizan **anotaciones** de OpenCSV (`@CsvBindByName`) en los atributos del POJO para vincularlos con las columnas del CSV.
- **Lombok** (`@Data`, `@NoArgsConstructor`) se usa para reducir el boilerplate (getters, setters, constructores).
- **Resultado:** Una `List<TuClase>` lista para usar.
- **Ejemplo de POJO:**

  ```Java
  @Data @NoArgsConstructor
  public class Persona {
      @CsvBindByName(column = "documento") private Integer documento;
      @CsvBindByName(column = "nombre")    private String  nombre;
      @CsvBindByName(column = "apellido")  private String  apellido;
      @CsvBindByName(column = "edad")      private Integer edad;
  }
  ```

- **Ejemplo de uso:**

  ```Java
  try (Reader r = Files.newBufferedReader(Path.of("personas.csv"))) {
      List<Persona> personas = new CsvToBeanBuilder<Persona>(r)
          .withType(Persona.class)
          .withSeparator(';')
          .withIgnoreLeadingWhiteSpace(true)
          .build()
          .parse(); // List<Persona>
  }
  ```

---

## 5) Streams & Collectors sobre la lista (consultas y “analytics” rápidos)

Una vez que los datos del CSV se han cargado en una `List<TuClase>` (por ejemplo, `List<Persona>`), se pueden usar las **API de Streams y Collectors** (vistas en el Apunte 10) para realizar consultas, transformaciones y análisis de forma declarativa y eficiente.

- **Promedio de edad:**

  ```Java
  double promedio = personas.stream().mapToInt(Persona::getEdad).average().orElse(0);
  ```

- **Resumen estadístico** (`count`, `min`, `avg`, `max`, `sum`):

  ```Java

  IntSummaryStatistics stats = personas.stream().mapToInt(Persona::getEdad).summaryStatistics();
  ```

- **Top 3 mayores por edad:**

  ```Java

  List<Persona> top3 = personas.stream()
      .sorted(Comparator.comparing(Persona::getEdad).reversed())
      .limit(3)
      .toList();
  ```

- **Filtrar y proyectar** (ej. nombres de personas mayores de 21):

  ```Java
  List<String> nombresMayores = personas.stream()
      .filter(p -> p.getEdad() != null && p.getEdad() > 21)
      .map(Persona::getNombre)
      .toList();
  ```

- **Agrupar y contar** (ej. por apellido):

  ```Java
  Map<String, Long> conteoPorApellido = personas.stream()
      .collect(Collectors.groupingBy(Persona::getApellido, Collectors.counting()));
  ```

- **Promedio de edad por apellido:**

  ```Java
  Map<String, Double> promPorApellido = personas.stream()
      .collect(Collectors.groupingBy(Persona::getApellido,
               Collectors.averagingInt(Persona::getEdad)));
  ```

- **A `Map` con manejo de duplicados y preservación de orden** (ej. clave: documento; quedarse con el de mayor edad si hay duplicados; usar `LinkedHashMap`):

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

- **Unir nombres a un solo string:**

  ```Java
  String listado = personas.stream().map(Persona::getNombre)
      .collect(Collectors.joining(", ", "[", "]"));
  ```

---

## 6) Buenas prácticas

- **`try-with-resources` siempre:** Fundamental para asegurar el cierre automático de recursos (Readers, BufferedReaders, Streams de líneas de archivos) y evitar fugas de memoria o archivos bloqueados.
- **`trim()` + validaciones:** Antes de intentar parsear `String` a tipos numéricos (como `Integer` o `Double`), es crucial usar `trim()` para eliminar espacios en blanco y realizar validaciones para manejar valores nulos o no numéricos, previniendo `NumberFormatException`.
- **Sin side-effects en streams:** Evitar modificar estructuras de datos externas dentro de un pipeline de stream. Es mejor usar `collect` para construir nuevas estructuras de datos inmutables.
- **Elegir la estructura de datos correcta:**
  - `List`: Si el orden y los duplicados son importantes.
  - `Set`: Si se necesitan elementos únicos.
  - `LinkedHashMap`: Si se necesita un mapa que preserve el orden de inserción.
- **No sobreusar Streams:** Aunque potentes, los Streams no siempre son la mejor solución. Si un bucle `for` tradicional es más claro o más eficiente para una lógica específica (ej. con `break`/`continue` complejos), úsalo. Los Streams son valiosos cuando **mejoran la legibilidad** y la expresividad del código.

---

## En criollo, lo esencial

1. **CSV simple:** `Scanner` o `BufferedReader` + `split()`.
2. **CSV complejo:** **OpenCSV** (`CSVReader` o `CSVReaderHeaderAware`).
3. **Mapeo a objetos:** `**CsvToBean**` **+ anotaciones** en tus POJOs.
4. **Análisis de datos:** Una vez cargados, usa **Streams y Collectors** para consultas, agrupaciones y métricas.
5. **Robustez:** Siempre usa `try-with-resources`, valida los datos, maneja el `charset` (UTF-8) y elige la estructura de datos adecuada.

---

