## Interfaz

Una **interfaz** en Java es un tipo especial de clase que define un conjunto de métodos que una clase debe implementar. Sin embargo, no contiene la implementación de esos métodos, solo sus firmas (nombre, parámetros y tipo de retorno). Es como un contrato que obliga a las clases que la implementan a proporcionar su propia versión de los métodos definidos en la interfaz.

### Características principales:

- **No tiene implementación**: Solo declara métodos, no los define.
- **Polimorfismo**: Permite que diferentes clases implementen la misma interfaz de diferentes maneras.
- **Herencia múltiple**: Una clase puede implementar múltiples interfaces, lo que no es posible con clases normales.
- **Constantes**: Puede contener variables, pero estas son automáticamente `public`, `static` y `final`.

### Ejemplo simple:

```Java
public interface Vehiculo {
    void arrancar(); // Método sin implementación
    void detener();
}

public class Coche implements Vehiculo {
    @Override
    public void arrancar() {
        System.out.println("El coche está arrancando");
    }

    @Override
    public void detener() {
        System.out.println("El coche se ha detenido");
    }
}
```

En este ejemplo, la clase `Coche` está obligada a implementar los métodos `arrancar` y `detener` porque implementa la interfaz `Vehiculo`.

---

  

## Repository y Transactional

`@Repository` y `@Transactional` son anotaciones de Spring Framework que cumplen roles específicos en la arquitectura de una aplicación.

### `@Repository`

- Es una especialización de `@Component`, utilizada para marcar una clase como un componente de acceso a datos (DAO).
- Indica que la clase interactúa con la base de datos, encapsulando la lógica de persistencia.
- Spring la utiliza para detectar automáticamente las clases DAO y registrarlas como beans en el contexto de la aplicación.
- También traduce excepciones específicas de la base de datos a excepciones genéricas de Spring (`DataAccessException`).

### `@Transactional`

- Se utiliza para gestionar transacciones de base de datos.
- Indica que los métodos o clases anotados deben ejecutarse dentro de una transacción.
- Si ocurre una excepción durante la ejecución, la transacción se revierte automáticamente (rollback).
- Es útil para garantizar la consistencia de los datos en operaciones que involucran múltiples pasos.

En tu código:

- `@Repository` marca la clase `UsuarioDaoImp` como un componente DAO.
- `@Transactional` asegura que las operaciones de base de datos realizadas en esta clase se ejecuten dentro de una transacción.