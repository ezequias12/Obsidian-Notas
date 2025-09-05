### Clase `Composition` — Composición en Java

La **composición** es un principio de diseño en POO que permite que una clase tenga objetos de otras clases como atributos.

Se usa para representar relaciones del tipo **“tiene un”** o **“está compuesto por”**.

---

## ¿Qué es la composición?

A diferencia de la herencia (que representa una relación “es un”), la composición indica que un objeto **contiene** o **usa** otro.

En el ejemplo:

```Java
public class Car {
    private final Engine engine = new Engine();
}
```

La clase `Car` **tiene un** motor (`Engine`), pero no **es un** motor. Por eso no usamos `extends`.

---

## Comparación: Herencia vs Composición

|Relación|Herencia|Composición|
|---|---|---|
|Expresión|"es un"|"tiene un"|
|Implementación|`extends`|atributo de tipo objeto|
|Ejemplo|`class Dog extends Animal`|`class Car { Engine engine; }`|

---

## Encapsulamiento en la composición

El atributo `engine` está declarado como `private`:

```Java
final private Engine engine = new Engine();
```

Eso significa que no podemos acceder directamente a `car.engine.on()`.

En cambio, se expone el comportamiento deseado (encender el motor) a través de un método intermedio:

```Java
public void on() {
    engine.on();
}
```

Así, desde afuera simplemente se hace:

```Java
car.on();
```

Esto **oculta los detalles internos** y mejora el diseño, siguiendo el principio de **abstracción**.

---

## Conclusión

La composición permite:

- Reutilizar clases sin depender de herencia.
- Diseñar sistemas más flexibles.
- Mejor encapsulamiento: se ocultan detalles de implementación.
- Evita las limitaciones de la herencia única (Java no permite herencia múltiple).

---

```Java
package basic.c08_oop;

public class Composition {

    public static void main(String[] args) {

        // Composición ("tiene un")

        var car = new Car();
        car.on();
        // podria hacer car.engine.on() pero al establecerla como private no podemos (no se puede acceder)
    }

    // En vez de hacer un extend normal (herencia), tenemos la capacidad de componer objetos
    // Herencia -> es un
    // Composición -> tiene un

    // El Car no puede extender de Engine porque no es un tipo de Engine
    // Tenemos que representar que Car tiene un Engine, no que Car es un Engine
    // Osea NO podemos hacer public static class Car extends Engine {}


    public static class Engine {

        public void on() {
            System.out.println("Motor encendido");
        }
    }

    public static class Car {

        // Basicamente instancio un Engine dentro de Car
        final private Engine engine = new Engine();

        // Esto es para poder hacer el llamado directamente car.on()
        public void on() {
            engine.on();
        }
    }
}
```