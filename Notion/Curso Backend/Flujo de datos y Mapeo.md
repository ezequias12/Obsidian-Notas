## Rol que cumple cada Clase (archivo) que definimos

---

### 1. `Usuario.java` (Modelo)

Este archivo representa la **entidad** que mapea la tabla `usuarios` en la base de datos. Es una clase POJO (Plain Old Java Object) que utiliza anotaciones de JPA para definir cómo se relaciona con la base de datos.

### Aportes:

- Define la estructura de la tabla `usuarios` en la base de datos.
- Cada atributo de la clase corresponde a una columna en la tabla.
- Utiliza anotaciones como `@Entity` y `@Table` para indicar que es una entidad JPA.

### Ejemplo:

```Java
@Entity
@Table(name = "usuarios")
public class Usuario {
    @Id
    @Column(name = "id")
    private Long id;

    @Column(name = "nombre")
    private String nombre;

    @Column(name = "apellido")
    private String apellido;

    // Otros atributos...
}
```

- `**@Entity**`: Marca la clase como una entidad JPA.
- `**@Table(name = "usuarios")**`: Especifica que esta clase está asociada a la tabla `usuarios`.
- `**@Id**`: Indica que el campo `id` es la clave primaria.

Esta clase es utilizada en las consultas y operaciones de persistencia para interactuar con la tabla `usuarios`.

---

### 2. `UsuarioDao.java` (Interface DAO)

Es una **interfaz** que define los métodos que deben implementarse para interactuar con la base de datos. No contiene lógica, solo declara qué métodos deben existir.

### Aportes:

- Define el contrato que debe cumplir cualquier clase que implemente esta interfaz.
- Obliga a implementar métodos como `getUsuarios()` y `eliminar(Long id)`.

### Ejemplo:

```Java
public interface UsuarioDao {
    List<Usuario> getUsuarios();
    void eliminar(Long id);
}
```

- `**List<Usuario> getUsuarios()**`: Declara un método para obtener una lista de usuarios.
- `**void eliminar(Long id)**`: Declara un método para eliminar un usuario por su ID.

Esta interfaz asegura que cualquier clase que la implemente tendrá estos métodos, promoviendo consistencia.

---

### 3. `UsuarioDaoImp.java` (Implementación DAO)

Es la clase que **implementa** la interfaz `UsuarioDao`. Aquí se escribe la lógica para interactuar con la base de datos utilizando JPA y el `EntityManager`.

### Aportes:

- Implementa los métodos definidos en `UsuarioDao`.
- Utiliza `EntityManager` para realizar operaciones de persistencia.

### Ejemplo:

```Java
@Repository
@Transactional
public class UsuarioDaoImp implements UsuarioDao {
    @PersistenceContext
    EntityManager entityManager;

    @Override
    public List<Usuario> getUsuarios() {
        String query = "FROM Usuario";
        return entityManager.createQuery(query).getResultList();
    }

    @Override
    public void eliminar(Long id) {
        Usuario usuario = entityManager.find(Usuario.class, id);
        entityManager.remove(usuario);
    }
}
```

- `**@Repository**`: Marca esta clase como un componente de acceso a datos (DAO).
- `**@Transactional**`: Asegura que las operaciones de base de datos se ejecuten dentro de una transacción.
- `**EntityManager**`: Es el objeto que permite interactuar con la base de datos (consultas, inserciones, eliminaciones, etc.).

Esta clase implementa los métodos `getUsuarios()` y `eliminar(Long id)` definidos en `UsuarioDao`.

---

### 4. `UsuarioController.java` (Controlador)

Es el controlador REST que expone los endpoints de la API. Utiliza la capa DAO para realizar operaciones y devolver respuestas HTTP.

### Aportes:

- Define los endpoints de la API.
- Utiliza `UsuarioDao` para interactuar con la base de datos.
- Maneja las solicitudes HTTP y devuelve respuestas.

### Ejemplo:

```Java
@RestController
public class UsuarioController {
    @Autowired
    private UsuarioDao usuarioDao;

    @RequestMapping(value = "api/usuarios")
    public List<Usuario> getUsuarios() {
        return usuarioDao.getUsuarios();
    }

    @RequestMapping(value = "api/usuarios/{id}", method = RequestMethod.DELETE)
    public void eliminar(@PathVariable Long id) {
        usuarioDao.eliminar(id);
    }
}
```

- `**@RestController**`: Marca esta clase como un controlador REST.
- `**@Autowired**`: Inyecta automáticamente una instancia de `UsuarioDao`.
- `**@RequestMapping**`: Define los endpoints de la API.

Por ejemplo:

- El endpoint `GET /api/usuarios` llama al método `getUsuarios()` de `UsuarioDao` para obtener la lista de usuarios.
- El endpoint `DELETE /api/usuarios/{id}` llama al método `eliminar(Long id)` para eliminar un usuario.

---

### Flujo Completo

1. **Solicitud HTTP**:
    - Un cliente (como un navegador o Postman) realiza una solicitud HTTP a un endpoint definido en `UsuarioController`.
2. **Controlador (**`**UsuarioController**`**)**:
    - El controlador recibe la solicitud y llama a los métodos de `UsuarioDao` para realizar la operación correspondiente.
3. **DAO (**`**UsuarioDaoImp**`**)**:
    - La implementación del DAO utiliza `EntityManager` para interactuar con la base de datos.
    - Por ejemplo, ejecuta una consulta para obtener usuarios o elimina un registro.
4. **Modelo (**`**Usuario**`**)**:
    - Los resultados de la base de datos se mapean a objetos de la clase `Usuario`.
    - Estos objetos se devuelven al controlador, que los envía como respuesta al cliente.

---

### Resumen de Roles

- `**Usuario**`: Representa la tabla `usuarios` en la base de datos.
- `**UsuarioDao**`: Define los métodos que deben implementarse para interactuar con la base de datos.
- `**UsuarioDaoImp**`: Implementa la lógica de acceso a datos utilizando JPA.
- `**UsuarioController**`: Expone los endpoints de la API y utiliza la capa DAO para realizar operaciones.

Este diseño sigue el patrón **MVC (Modelo-Vista-Controlador)**, donde:

- El **Modelo** es `Usuario`.
- La **Vista** es la respuesta JSON generada por el controlador.
- El **Controlador** es `UsuarioController`.

Este flujo asegura una separación clara de responsabilidades y facilita el mantenimiento del código.