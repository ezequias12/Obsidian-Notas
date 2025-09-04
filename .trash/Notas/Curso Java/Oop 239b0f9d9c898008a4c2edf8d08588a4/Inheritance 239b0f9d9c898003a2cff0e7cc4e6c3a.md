# Inheritance

### `Inheritance` — Herencia en Java

### 🧩 ¿Qué es la herencia?

La **herencia** permite que una clase (subclase) herede atributos y métodos de otra (superclase).

Se usa para modelar relaciones del tipo “es un”: por ejemplo, un `Dog` es un `Animal`.

### 🌳 Jerarquía de clases en el ejemplo

- `Animal` → clase base o superclase
- `Dog`, `Cat` y `Bird` → subclases que heredan de `Animal`

```java
public static class Dog extends Animal { ... }
```

Esto significa que `Dog` hereda automáticamente todo lo que tiene `Animal` (atributos y métodos públicos o protegidos).

### ⚙️ Constructor y uso de `super()`

El constructor de `Animal` requiere un nombre:

```java
public Animal(String name) { this.name = name; }

```

Al heredar, las subclases están **obligadas a llamar** al constructor de la superclase. Esto se hace con `super(...)`.

```java
public Dog(String name, int age) {
    super(name); // llama al constructor de Animal
    this.age = age; // atributo nuevo propio de Dog
}

```

### 🧪 Sobrescritura de métodos (`@Override`)

La clase `Dog` redefine el método `eat()` de la clase `Animal`:

```java
@Override
public void eat() {
    System.out.println("El perro con nombre " + name + " está comiendo.");
}

```

Esto se llama **sobrescritura** (override): la subclase cambia el comportamiento de un método heredado para ajustarlo a su contexto.

### 🐦 Métodos propios de subclase

La clase `Bird` agrega un método nuevo llamado `fly()`, que no existe en `Animal`:

```java
public void fly() {
    System.out.println("Está volando");
}

```

Este método es exclusivo de `Bird`, y no está disponible para `Dog` ni `Cat`.

### 🔁 Instanciación y pruebas en `main()`

El `main()` crea un objeto de cada clase hija y los usa:

```java
var dog = new Dog("Mou", 3);
dog.eat(); // imprime el mensaje propio de Dog

var bird = new Bird("Bou");
bird.eat(); // usa el método de Animal
bird.fly(); // método exclusivo de Bird

```

✅ Si una subclase **no sobreescribe** un método, se usa el de la superclase.

✅ Si una subclase **agrega un método propio**, solo esa subclase puede usarlo.

---

### 🔒 Detalle técnico: métodos `static` y clases internas

Las clases `Animal`, `Dog`, `Cat` y `Bird` están declaradas como `static` porque están dentro del método `main`, que también es `static`. Esto es necesario para poder usarlas sin necesidad de crear una instancia de la clase contenedora (`Inheritance`).

---

```java
package basic.c08_oop;

/*
Clase 61 - Herencia
Vídeo: https://youtu.be/JOAqpdM36wI?t=23334
*/

public class Inheritance {

    public static void main(String[] args) {

        // Herencia ("es un")

        var animal = new Animal("Mi animal");
//        animal.name = "Mi animal";
        animal.eat();

        var dog = new Dog("Mou", 3);
//        dog.name = "Mou";
        dog.eat();

        var cat = new Cat("Cou");
//        cat.name = "Cou";
        cat.eat();

        var bird = new Bird("Bou");
//        bird.name = "Bou";
        bird.eat();
        bird.fly();
    }

    // recordar que deben ser STATIC ya que luego se accede a ellas desde el main (que es estatico)
    public static class Animal {

        String name;

        // cuando usamos un constructor en la clase Padre. estamos estableciendo un Contrato
        // que las clases hijas deben cumplir (osea que las clases hijas tambien tienen que tener un constructor)
        public Animal(String name) {
            this.name = name;
        }

        public void eat() {
            System.out.println("El animal con nombre " + name + " está comiendo.");
        }
    }

    // Dog hereda atributos y métodos de Animal. ademas de agregar los suyos propios
    public static class Dog extends Animal {

        int age;

        public Dog(String name, int age) {
            super(name); // llama al constructor de la clase padre Animal. osea usamos el metodo constructor de Animal para definir name
            this.age = age;
        }

        @Override // hace referencia a que estamos sobreescribiendo el métodoo eat de Animal
        public void eat() {
            System.out.println("El perro con nombre " + name + " está comiendo.");
        }
    }

    public static class Cat extends Animal {

        public Cat(String name) {
            super(name);
        }
    }

    // la funcion fly es propia de Bird, no de Animal
    public static class Bird extends Animal {

        public Bird(String name) {
            super(name);
        }

        public void fly() {
            System.out.println("Está volando");
        }
    }

}

```