Para la definicion de una clase en java, la clase debe ser el fichero contenedor

Osea si quiero definir una clase persona, creo un archivo nuevo llamado Person.java

```Java
public class Persona {
    // Atributos
    String nombre;
    int edad;

		// Constructor
    public Person(String name, int age, String id) {
        this.name = nombre;
        this.age = edad; 
    }

    // Métodos
    void saludar() {
        System.out.println("Hola, soy " + nombre + " y tengo " + edad + " anios");
    }
}
```

Clase `Classes` — Uso de Objetos y Acceso a Atributos

La clase `Classes` contiene el método `main()` y muestra cómo trabajar con objetos de la clase `Person`.

### Crear objetos

```Java
var person = new Person("Brais", -38, "123456789A");
```

Al crear una instancia de `Person`, se debe pasar nombre, edad e ID. Si la edad no es válida, el setter lo informará.

### Acceso a atributos

- No se puede modificar `age` directamente porque es privado.
- Se accede a través de `setAge()` y `getAge()`.
- `name` es `protected`: se puede modificar dentro del mismo paquete (pero idealmente también debería encapsularse).
- `id` es `final` y `private`: solo lectura mediante `getId()`.

### Ejemplo de ejecución

```Java
person.sayHello();
person.name = "Brais Moure";
System.out.println(person.name);
System.out.println(person.getId());

person.setAge(38);
System.out.println(person.getAge());

var person2 = new Person("MoureDev", 18, "123456789B");
person2.sayHello();
```

Esto demuestra:

- Cómo modificar atributos si están accesibles.
- Cómo llamar métodos de instancia.
- Cómo crear múltiples objetos con distintos valores.

---

## Código completo

```Java
package basic.c08_oop;

public class Classes {

    public static void main(String[] args) {

        // ahora obligatoriamente tengo que pasarle nombre, edad e id para instanciar la clase Person
        var person = new Person("Brais", -38, "123456789A");

        // person.name = "Brais"; // no puedo acceder directamente al atributo name porque es protected, para eso creamos los getters y setters
        // person.age = 38;

        person.sayHello();

        person.name = "Brais Moure";
        System.out.println(person.name);

        // person.id = "123456789A";

        System.out.println(person.getId());

        person.setAge(38);
        System.out.println(person.getAge());

        var person2 = new Person("MoureDev", 18, "123456789B");
        person2.sayHello();
    }
}
```

---

---

# Person.java

El constructor se utiliza para inicializar los atributos de la clase. Me obliga a que al momento de instanciar la clase Person, debo pasarle un nombre, una edad y un id

Clase `Person` — Definición y Encapsulamiento

### Atributos

- `protected String name`: accesible desde la misma clase, subclases y otras clases del mismo paquete.
- `private int age`: solo accesible dentro de la clase `Person`.
- `final private String id`: es una constante (no puede modificarse una vez asignada) y solo se puede acceder desde la propia clase.

### Constructor

```Java
public Person(String name, int age, String id)
```

Este constructor obliga a proporcionar todos los datos necesarios para crear una persona válida. Se usa `this` para diferenciar los atributos de los parámetros. Además:

- Se usa `this.setAge(age)` en lugar de `this.age = age` para validar la edad desde el principio.
- El atributo `id` se inicializa una única vez gracias a `final`.

### Métodos

```Java
public void sayHello()
```

Imprime un saludo con todos los atributos de la persona.

### Métodos de acceso (getters y setters)

Los atributos privados (`age`, `id`) no se pueden leer ni modificar directamente desde afuera. Para eso se crean:

- `getAge()` y `getId()` para acceder a los valores.
- `setAge(int age)` para modificar la edad (solo si es válida).

Esto asegura el control sobre cómo se accede o modifica el estado del objeto.

---

## Código completo

```Java
package basic.c08_oop;

public class Person {

    // ATRIBUTOS
    protected String name; // protected permite el acceso desde la propia clase y sus subclases
    // todoo dentro del mismo paquete (carpeta)
    // si intentamos acceder desde otro paquete no nos va a dejar
    private int age; // private permite el acceso solo desde la propia clase
    // no podemos modificarlo desde fuera de la clase Person
    final private String id; // final significa que este atributo no se puede modificar una vez asignado
    // si aparte esta privado, tampoco se va a poder modificar desde fuera de la clase Person


    // CONSTRUCTOR
    // El constructor se utiliza para inicializar los atributos de la clase
    // me obliga a que al momento de instanciar la clase Person, debo pasarle un nombre, una edad y un id
    public Person(String name, int age, String id) {
        // tengo que establecer una relacion entre los parametros y los atributos de la clase
        // this hace referencia interna a la clase

        // SE PUEDE HACER TAMBIÉN
        // name = name;
        // age = age;
        // PERO ES BUENA PRACTICA HACERLO ASI:
        this.name = name;
        this.setAge(age); // aca acordarse de llamar al metodo nuevo, no setear directamente (this.age = age;)
        this.id = id;
    }

    // METODOS
    public void sayHello() {
        System.out.println("Hola, soy " + name + ", tengo " + age + " años, y mi id es " + id + ".");
    }

    // METODOS DE ACCESO (GETTERS Y SETTERS)
    // Como antes pusimos el atributo age como private, no podemos acceder a él directamente desde fuera de la clase, no podemos ni siquiera imprimirlo. aunque lo que realmente buscamos es que no se pueda modificar.
    // Por eso creamos metodos para realizar esas operaciones (modificacion y acceso)

    // Getter
    public int getAge() {
        return age;
    }

    public String getId() {
        return id;
    }

    // Setter
    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        } else {
            System.out.println("Edad no válida");
        }
    }
}
```