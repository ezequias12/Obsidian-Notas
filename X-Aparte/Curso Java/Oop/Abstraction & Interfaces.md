### Clase `Abstraction` — Abstracción e Interfaces en Java

Su objetivo es **ocultar la implementación** y mostrar únicamente lo esencial.

Permite definir métodos sin detallar cómo funcionan, dejando que las clases hijas lo implementen más adelante.

---

## Clases abstractas

Una clase abstracta se define con `abstract class`. No se puede instanciar directamente, pero **puede tener tanto métodos concretos como abstractos**.

```Java
public abstract class Animal {
    public abstract void sound(); // sin implementación
    public void sleep() {         // implementación concreta
        System.out.println("El animal está durmiendo");
    }
}
```

### Uso:

- No se puede hacer `new Animal()` porque es abstracta.
- Las clases que hereden de `Animal` deben implementar los métodos abstractos obligatoriamente (como `sound()`).
- Pueden sobrescribir también métodos concretos como `sleep()` si lo desean.

### Ejemplo:

```Java
public class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Guau");
    }

    @Override
    public void sleep() {
        System.out.println("El perro está durmiendo");
    }
}
```

---

## Interfaces

Una **interfaz** define un conjunto de métodos que una clase debe implementar.

Es como un **contrato**: la clase que lo implementa se obliga a definir los métodos.

```Java
public interface Flying {
    void fly();
}
```

- No tiene atributos ni lógica.
- Todas las clases que implementan una interfaz deben implementar **todos** sus métodos.

### Combinando clase abstracta e interfaz

Una clase puede **extender de una clase abstracta** y **a la vez implementar una interfaz**:

```Java
public class Bird extends Animal implements Flying {
    @Override
    public void sound() {
        System.out.println("Pio pio");
    }

    @Override
    public void fly() {
        System.out.println("El pájaro vuela");
    }
}
```

Esto permite:

- Compartir comportamiento común mediante la herencia (`Animal`)
- Reutilizar funcionalidades específicas mediante interfaces (`Flying`)

## Conclusión

|Concepto|Clase abstracta|Interface|
|---|---|---|
|Instanciación|❌ No se puede instanciar directamente|❌ No se puede instanciar|
|Métodos|Puede tener abstractos y concretos|Solo métodos abstractos (hasta Java 8)|
|Herencia|Una clase solo puede extender **una** clase|Puede implementar **múltiples** interfaces|
|Propósito|Reutilizar lógica común y definir contratos base|Definir contratos (capacidad)|

---

```Java
package basic.c08_oop;

public class Abstraction {

    public static void main(String[] args) {

        // Abstracción
        // ocultar la implementacion y mostrar solo lo esencial
        // podemos definir comportamientos sin obligar a escribir el detalle de como funciona hasta que se necesite

        // - Clase abstracta

        // no podemos instancia ahora Animal porque es abstracta
        // var animal = new Animal(); // Error: Cannot instantiate the type Animal
        // animal.sleep();

        var dog = new Dog();
        dog.sleep();
        dog.sound();

        var cat = new Cat();
        cat.sleep();
        cat.sound();

        // - Interface
    }

    // - Clase abstracta
    // con el abstract class ya pasa a ser abstracta
    // nos permite definir métodos abstractos (sin implementación) y métodos concretos (con implementación)

    public static abstract class Animal {

        // se pueden definir metodos abstractos (sin implementacion)
        // sirven para que las clases hijas implementen su propia version, pero siempre extendiendo de la clase padre
        public abstract void sound();

        // el metodo sleep esta siendo ocultado
        // yo se que Animal tiene este metodo pero no lo puedo usar directamente
        // solo lo puedo usar cuando lo herede
        public void sleep() {
            System.out.println("El animal está durmiendo");
        }
    }

    // ahora sound() va a tener que ser implementado por las clases hijas obligatoriamente
    public static class Dog extends Animal {

        @Override
        public void sound() {
            System.out.println("Guau");
        }

        @Override
        public void sleep() {
            System.out.println("El perro está durmiendo");
        }
    }

    public static class Cat extends Animal {

        @Override
        public void sound() {
            System.out.println("Miau");
        }
    }

    // --------------------------------------------------------------------------------------------------
    // -- Interface --
    // contrato que define metodos que debe tener una clase, pero no como implementarlos

    public interface Flying {

        void fly();
    }

    // reutilizamos Flying en las clases que lo necesiten

    // ahora cuando las clases implementen Flying, deben implementar el metodo fly() obligatoriamente

    public static class Bird extends Animal implements Flying {

        @Override
        public void sound() {
            System.out.println("Pio pio");
        }

        @Override
        public void fly() {
            System.out.println("El pájaro vuela");
        }
    }

    public static class Bat extends Animal implements Flying {

        @Override
        public void sound() {
            System.out.println("Soy batman!");
        }

        @Override
        public void fly() {
            System.out.println("El murciélago vuela");
        }
    }
}
```