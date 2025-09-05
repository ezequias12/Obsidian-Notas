### Clase `Polymorphism` — Polimorfismo en Java

Permite que un mismo método o clase se comporte de diferentes maneras dependiendo del contexto.

En Java, el polimorfismo puede presentarse de dos formas principales:

---

### 🔁 1. Polimorfismo por herencia (sobrescritura)

Este tipo se da cuando una **subclase sobrescribe** un método de la superclase para darle su propio comportamiento.

La firma del método (nombre y parámetros) se mantiene igual, pero el cuerpo cambia.

```Java
public class Animal {
    public void sound() {
        System.out.println("Algún sonido");
    }
}

public class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Guau");
    }
}
```

### ✅ Uso:

```Java
Animal animal = new Animal();
animal.sound(); // imprime "Algún sonido"

Dog dog = new Dog();
dog.sound(); // imprime "Guau"
```

Esto demuestra que el mismo método `sound()` se comporta diferente según el tipo de objeto.

---

### ➕ 2. Polimorfismo por sobrecarga (overloading)

Este tipo se da cuando una misma clase tiene **métodos con el mismo nombre**, pero **con diferente cantidad o tipo de parámetros**.

```Java
public class Calculator {

    public int sum(int a, int b) {
        return a + b;
    }

    public int sum(int a, int b, int c) {
        return a + b + c;
    }

    public double sum(double a, double b) {
        return a + b;
    }
}
```

### ✅ Uso:

```Java
Calculator calculator = new Calculator();

calculator.sum(3, 5);       // llama al método sum(int, int)
calculator.sum(3.2, 5.4);   // llama al método sum(double, double)
calculator.sum(1, 2, 3);    // llama al método sum(int, int, int)
```

Esto es posible porque Java identifica cuál método ejecutar según la cantidad y tipo de parámetros.

No es necesario usar nombres diferentes para cada operación similar: el compilador elige el correcto automáticamente.

---

```Java
package basic.c08_oop;

/*
Clase 63 - Polimorfismo
Vídeo: https://youtu.be/JOAqpdM36wI?t=24505
*/

public class Polymorphism {

    public static void main(String[] args) {


        // Polimorfismo
        // permite que un metodo o una clase se comporte de diferentes maneras según el contexto.

        // - Polimorfismo por herencia (sobrescritura)
        //  Permite que una clase hija sobrescriba un métodoo de la clase padre, proporcionando su propia implementación.
        var animal = new Animal();
        animal.sound();

        var dog = new Dog();
        dog.sound();

        // - Polimorfismo por sobrecarga (sobrecarga de métodos)
        // Permite que una clase tenga varios métodos con el mismo nombre pero diferentes parámetros.
        var calculator = new Calculator();
        System.out.println(calculator.sum(3, 5));
        System.out.println(calculator.sum(3.2, 5.4));
    }

    // - Polimorfismo por herencia (sobrescritura)

    public static class Animal {

        public void sound() {
            System.out.println("Algún sonido");
        }
    }

    public static class Dog extends Animal {

        // aca aplicamos el polimorfismo por herencia, sobrescribiendo el metodo sound de la clase Animal
        @Override
        public void sound() {
            System.out.println("Guau");
        }
    }

    // - Polimorfismo por sobrecarga (sobrecarga de métodos)
    // tiene varios metodos con el mismo nombre (sum) pero al tener diferentes parametros, se comportan de manera diferente.
    // y eso se ve cuando usamos el metodo (dependiendo del parametro que le pasemos, se ejecuta uno u otro metodo)
    public static class Calculator {

        public int sum(int a, int b) {
            return a + b;
        }

        public int sum(int a, int b, int c) {
            return a + b + c;
        }

        public double sum(double a, double b) {
            return a + b;
        }
    }
}
```