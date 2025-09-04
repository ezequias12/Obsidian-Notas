# AccessModifiers

Clase `AccessModifiers` — Modificadores de Acceso y Encapsulamiento

### Modificadores de acceso en Java:

- `public`: se puede acceder desde cualquier clase.
- `private`: solo accesible dentro de la misma clase.
- `protected`: accesible desde la clase, subclases, y otras clases del mismo paquete.
- `(default)`: si no se especifica ningún modificador, el acceso es solo dentro del mismo paquete (también llamado “package-private”).

### Encapsulamiento

Es una de las bases de la POO. Permite:

- Proteger los atributos y que solo puedan ser modificados a través de métodos específicos.
- Controlar cómo se accede a los datos.

### Getters y Setters

- **Getters** permiten obtener el valor de un atributo privado.
- **Setters** permiten modificar el valor, usualmente con validaciones.

```java
package basic.c08_oop;

public class AccessModifiers {

    public static void main(String[] args) {

        // Encapsulamiento

        // - Modificadores de acceso
        // public - Acceso desde cualquier parte del programa
        // private - Acceso solo desde la propia clase
        // protected - Acceso desde la propia clase y sus subclases
        // (default) - Acceso solo desde el mismo paquete (carpeta)

        // getters - permiten acceder a los atributos privados de una clase
        // setters - permiten modificar los atributos privados de una clase

    }
}

```