# Paso 0 — Plan de juego (breve)

1. Elegir 2–3 microservicios con alta cohesión y bajo acoplamiento.
2. Definir recursos y subrecursos + estados y acciones especiales.
3. Esbozar rutas REST (CRUD + operaciones de estado + dashboard).
4. Borrador de requests/responses y códigos HTTP.
5. Esqueleto de proyecto Spring Boot por servicio (controllers con comentarios).

# Paso 1 — Propuesta de microservicios (3 MS)

En criollo: separamos lo que **cambia junto** y evitamos que varios servicios “toquen la misma tabla”. Los archivos (fotos, afiches, portadas) van a un **Media Service** para no mezclar binarios con dominio.

**A) Festivales Service (core del dominio)**

* Recurso: `festivales` (con estados: `borrador | publicado | cancelado`).
* Subrecursos: `obras` (pertenecen a un festival), `publicaciones` (novedades del festival).
* Endpoints de estado: publicar/cancelar.
* Dashboard/resumen por festival.

**B) Artistas Service**

* Recurso: `artistas`.
* Asociación de artistas a obras se hace **desde Festivales Service** (la obra vive allí), pero llamando a Artistas por ID (evitamos transacciones distribuidas; aceptamos **consistencia eventual**: puede tardar en verse el cambio en listados).

**C) Media Service (archivos)**

* Recurso: `multimedia` (subida/consulta/descarga de archivos: portada, afiches, fotos, adjuntos).
* La BD guarda solo **ruta + metadatos**; el binario vive en FS/nube. Los otros servicios guardan el `mediaId`/URL.

> Por qué así: la guía pide agrupar endpoints coherentes, evitar compartir base de datos entre MS, y aceptar consistencia eventual en integraciones (menos acoplamiento, mejor escalabilidad).&#x20;

# Paso 2 — Rutas (borrador alineado a la guía)

Prefijo obligatorio `/api` y versionado opcional (uso `/api/v1`). Plurales, jerarquías cortas, filtros por query params.

**Festivales Service**

* `GET /api/v1/festivales?page=1&size=20&sort=fechaInicio,desc`
* `POST /api/v1/festivales` (crea en `borrador`)
* `GET /api/v1/festivales/{festivalId}`
* `PUT /api/v1/festivales/{festivalId}` (actualiza datos)
* `POST /api/v1/festivales/{festivalId}/publicar`
* `POST /api/v1/festivales/{festivalId}/cancelar`
* `DELETE /api/v1/festivales/{festivalId}` (baja lógica si aplicara)
* Obras (subrecurso):

  * `GET /api/v1/festivales/{festivalId}/obras`
  * `POST /api/v1/festivales/{festivalId}/obras`
  * `GET /api/v1/obras/{obraId}`
  * `PUT /api/v1/obras/{obraId}`
  * `DELETE /api/v1/obras/{obraId}`
  * Asociación artistas (operación específica):

    * `POST /api/v1/obras/{obraId}/artistas` (body: `{ "artistaIds": [...] }`)
    * `DELETE /api/v1/obras/{obraId}/artistas/{artistaId}`
* Publicaciones:

  * `GET /api/v1/festivales/{festivalId}/publicaciones`
  * `POST /api/v1/festivales/{festivalId}/publicaciones`
  * `PUT /api/v1/publicaciones/{publicacionId}`
  * `DELETE /api/v1/publicaciones/{publicacionId}`
* Dashboard:

  * `GET /api/v1/festivales/{festivalId}/resumen` (obras, artistas asociados, publicaciones, estado)

**Artistas Service**

* `GET /api/v1/artistas?nombre=…&nacionalidad=…&page=…`
* `POST /api/v1/artistas`
* `GET /api/v1/artistas/{artistaId}`
* `PUT /api/v1/artistas/{artistaId}`
* `DELETE /api/v1/artistas/{artistaId}`
* (La foto se vincula vía `mediaId` retornado por Media Service.)

**Media Service**

* `POST /api/v1/multimedia/uploads` (multipart/form-data) → responde `mediaId`, `url`, `tipo`, `tamaño`
* `GET /api/v1/multimedia/{mediaId}` (metadatos)
* `GET /api/v1/multimedia/{mediaId}/download` (descarga protegida)
* (Opcional) `DELETE /api/v1/multimedia/{mediaId}`

# Paso 3 — Requests/Responses + HTTP (ejemplo mínimo)

**POST /festivales** → `201 Created`
Request (JSON):

```json
{
  "nombre": "Festival de Invierno",
  "descripcion": "Edición 2026",
  "fechaInicio": "2026-07-10",
  "fechaFin": "2026-07-20",
  "ubicacion": "Córdoba, AR",
  "portadaMediaId": null
}
```

Response `201` (Location: `/api/v1/festivales/123`):

```json
{
  "id": 123,
  "estado": "borrador",
  "nombre": "Festival de Invierno",
  "portadaMediaId": null
}
```

Errores típicos: `400` (fechas inválidas), `409` (superposición de fechas si aplica reglas), `422` (violación de regla negocio), `500` (error interno).

**POST /festivales/{id}/publicar** → `200 OK` o `409 Conflict` si ya está publicado/cancelado.
**POST /obras/{obraId}/artistas** → `204 No Content` si asoció OK; `404` si obra o artista no existen; `409` si duplicado.

# Paso 4 — Código base (Spring Boot) — Festivales Service

> Objetivo: darte un **esqueleto muy comentado** para entender responsabilidades. No implemento lógica de negocio todavía (vamos por iteraciones).

```java
// src/main/java/com.ejemplo.festivales/api/FestivalController.java
package com.ejemplo.festivales.api;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.net.URI;
import java.util.List;

/**
 * Controlador REST de Festivales.
 * En criollo: "traduce" HTTP <-> capa de servicio (reglas de negocio).
 * Mantiene las rutas limpias y delega la lógica.
 */
@RestController
@RequestMapping("/api/v1/festivales")
public class FestivalController {

    private final FestivalService service;

    public FestivalController(FestivalService service) {
        this.service = service;
    }

    // GET /api/v1/festivales?page=..&size=..&sort=..
    @GetMapping
    public ResponseEntity<List<FestivalDTO>> listar(
            @RequestParam(defaultValue = "1") int page,
            @RequestParam(defaultValue = "20") int size,
            @RequestParam(defaultValue = "fechaInicio,desc") String sort) {
        // En una iteración siguiente, devolvé PageDTO con total, etc.
        return ResponseEntity.ok(service.listar(page, size, sort));
    }

    // POST /api/v1/festivales (crea en estado "borrador")
    @PostMapping
    public ResponseEntity<FestivalDTO> crear(@RequestBody CrearFestivalRequest req) {
        FestivalDTO creado = service.crear(req);
        // Location: URL del recurso creado, buena práctica REST
        URI location = URI.create("/api/v1/festivales/" + creado.id());
        return ResponseEntity.created(location).body(creado);
    }

    // GET /api/v1/festivales/{id}
    @GetMapping("/{id}")
    public ResponseEntity<FestivalDTO> obtener(@PathVariable Long id) {
        return ResponseEntity.ok(service.obtener(id));
    }

    // PUT /api/v1/festivales/{id}
    @PutMapping("/{id}")
    public ResponseEntity<FestivalDTO> actualizar(@PathVariable Long id, @RequestBody ActualizarFestivalRequest req) {
        return ResponseEntity.ok(service.actualizar(id, req));
    }

    // POST /api/v1/festivales/{id}/publicar
    @PostMapping("/{id}/publicar")
    public ResponseEntity<FestivalDTO> publicar(@PathVariable Long id) {
        return ResponseEntity.ok(service.publicar(id)); // 409 si no se puede (regla de negocio)
    }

    // POST /api/v1/festivales/{id}/cancelar
    @PostMapping("/{id}/cancelar")
    public ResponseEntity<FestivalDTO> cancelar(@PathVariable Long id) {
        return ResponseEntity.ok(service.cancelar(id));
    }

    // GET /api/v1/festivales/{id}/resumen
    @GetMapping("/{id}/resumen")
    public ResponseEntity<ResumenFestivalDTO> resumen(@PathVariable Long id) {
        return ResponseEntity.ok(service.resumen(id)); // obras, #artistas, #publicaciones, estado
    }
}
```

```java
// src/main/java/com.ejemplo.festivales/api/ObraController.java
package com.ejemplo.festivales.api;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.net.URI;
import java.util.List;

/**
 * Subrecurso Obras: viven "debajo" de Festivales.
 * Notá dos estilos de ruta:
 *  - Colección anidada: /festivales/{festivalId}/obras
 *  - Item global por id: /obras/{obraId} (acceso directo por ID)
 */
@RestController
public class ObraController {

    private final ObraService service;

    public ObraController(ObraService service) { this.service = service; }

    @GetMapping("/api/v1/festivales/{festivalId}/obras")
    public ResponseEntity<List<ObraDTO>> listar(@PathVariable Long festivalId) {
        return ResponseEntity.ok(service.listarPorFestival(festivalId));
    }

    @PostMapping("/api/v1/festivales/{festivalId}/obras")
    public ResponseEntity<ObraDTO> crear(@PathVariable Long festivalId, @RequestBody CrearObraRequest req) {
        ObraDTO creada = service.crear(festivalId, req);
        URI location = URI.create("/api/v1/obras/" + creada.id());
        return ResponseEntity.created(location).body(creada);
    }

    @GetMapping("/api/v1/obras/{obraId}")
    public ResponseEntity<ObraDTO> obtener(@PathVariable Long obraId) {
        return ResponseEntity.ok(service.obtener(obraId));
    }

    @PutMapping("/api/v1/obras/{obraId}")
    public ResponseEntity<ObraDTO> actualizar(@PathVariable Long obraId, @RequestBody ActualizarObraRequest req) {
        return ResponseEntity.ok(service.actualizar(obraId, req));
    }

    @DeleteMapping("/api/v1/obras/{obraId}")
    public ResponseEntity<Void> eliminar(@PathVariable Long obraId) {
        service.eliminar(obraId);
        return ResponseEntity.noContent().build();
    }

    // Asociación específica: POST /obras/{obraId}/artistas
    @PostMapping("/api/v1/obras/{obraId}/artistas")
    public ResponseEntity<Void> asociarArtistas(@PathVariable Long obraId, @RequestBody AsociarArtistasRequest req) {
        service.asociarArtistas(obraId, req.artistaIds());
        return ResponseEntity.noContent().build(); // idempotente: duplicados -> 204 igual o 409 según diseño
    }

    @DeleteMapping("/api/v1/obras/{obraId}/artistas/{artistaId}")
    public ResponseEntity<Void> desasociarArtista(@PathVariable Long obraId, @PathVariable Long artistaId) {
        service.desasociarArtista(obraId, artistaId);
        return ResponseEntity.noContent().build();
    }
}
```

```java
// DTOs como records (Java 21) — simples y claros
package com.ejemplo.festivales.api;
import java.time.LocalDate;
import java.util.List;

public record FestivalDTO(Long id, String nombre, String descripcion,
                          LocalDate fechaInicio, LocalDate fechaFin,
                          String ubicacion, String estado, Long portadaMediaId) {}

public record CrearFestivalRequest(String nombre, String descripcion,
                                   LocalDate fechaInicio, LocalDate fechaFin,
                                   String ubicacion, Long portadaMediaId) {}

public record ActualizarFestivalRequest(String nombre, String descripcion,
                                       LocalDate fechaInicio, LocalDate fechaFin,
                                       String ubicacion, Long portadaMediaId) {}

public record ResumenFestivalDTO(Long festivalId, int cantidadObras, int cantidadArtistas,
                                 int cantidadPublicaciones, String estado) {}

public record ObraDTO(Long id, Long festivalId, String titulo, String descripcion,
                      String genero, Integer duracionMin, Long aficheMediaId) {}

public record CrearObraRequest(String titulo, String descripcion,
                               String genero, Integer duracionMin, Long aficheMediaId) {}

public record ActualizarObraRequest(String titulo, String descripcion,
                                    String genero, Integer duracionMin, Long aficheMediaId) {}

public record AsociarArtistasRequest(List<Long> artistaIds) {}
```

> Nota rápida de arquitectura: cada MS tendría su módulo (Maven/Gradle) y su DB propia. El **Festivales Service** guarda `portadaMediaId`/`aficheMediaId` (referencia al Media Service), no el archivo. El **Media Service** expone upload/descarga. El **Artistas Service** gestiona artistas y sus fotos por `mediaId`.

---

¿Seguimos con **esta división de 3 microservicios** (Festivales, Artistas, Media) o preferís **2 microservicios** (uno “Core” con Festivales+Obras+Publicaciones+Artistas y otro Media)? Elegí una opción y avanzamos al **Paso 5**: listado completo de endpoints con ejemplos de request/response y códigos HTTP por cada caso, y te voy armando los otros controladores igual de comentados.
