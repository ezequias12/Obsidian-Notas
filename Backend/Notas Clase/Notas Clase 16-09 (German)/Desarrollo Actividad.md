# Planteamiento general de que hay que hacer

## Pasos a grandes rasgos para la Actividad

1. **Analizar el CSV**

   * Ver qué datos contiene (columnas).
   * Crear una clase Java (`Persona`, `Producto`, lo que corresponda) con atributos que representen esas columnas.

2. **Leer el CSV → objetos Java**

   * Con `Scanner/split` (básico) o con **OpenCSV** (`CsvToBean` con anotaciones).
   * Guardar los objetos en una `List<Entidad>` (colecciones).

3. **Procesar con Streams (opcional, consultas)**

   * Calcular estadísticas, agrupar, ordenar.
   * Ej: promedio, conteo por categoría, top N.

4. **Persistir los datos**

   * Opción A: con **JDBC** → usar `PreparedStatement` e insertar cada objeto.
   * Opción B (más moderno): con **JPA/Hibernate** → mapear la clase como `@Entity` y persistir con `EntityManager`.

5. **(Opcional) Exponer como API REST**

   * Si la actividad lo pide, usar Spring Boot para levantar endpoints `/entidades`.
   * Métodos: `GET` para listar, `POST` para agregar, etc.

6. **Buenas prácticas**

   * Usar `try-with-resources` para manejar archivos y conexiones.
   * Cerrar siempre `ResultSet → Statement → Connection`.
   * Evitar concatenar strings en SQL → usar `PreparedStatement` o JPA.
