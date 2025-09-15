[[Clases]]

[[AccessModifiers]]

[[Inheritance]]

[[Polymorphism]]

[[Abstraction & Interfaces]]

[[Composition]]

## 1. Atributos, Constructores y Encapsulamiento

Una clase contiene atributos (propiedades), constructores (métodos especiales que se ejecutan al crear el objeto) y métodos para operar sobre los datos.

El encapsulamiento protege los datos internos del objeto haciendo que sólo se acceda a ellos a través de métodos (getters y setters).

Ejemplo: clase `Person` con atributos `name`, `age`, `id`, y constructor con validaciones.

Se usa `private` para proteger atributos y `this` para diferenciarlos de los parámetros.

---

## 2. Modificadores de Acceso

|Modificador|Accesible desde...|
|---|---|
|public|cualquier clase|
|private|solo la misma clase|
|protected|misma clase, subclases, o mismo paquete|
|(default)|solo dentro del mismo paquete|

Los **getters y setters** se usan para leer o modificar atributos privados de forma controlada.

---

## 3. Herencia

La herencia permite que una clase hija herede atributos y métodos de una clase padre. Se declara con `extends`.

Relación “es un”:

```Java
class Dog extends Animal
```

Una subclase puede:

- Usar métodos heredados.
- Sobrescribir métodos (`@Override`).
- Agregar nuevos métodos propios.

---

## 4. Polimorfismo

Permite que un método o clase se comporte de distintas maneras:

### a) Por herencia (sobrescritura)

Una subclase redefine un método de la superclase:

```Java
@Override
public void sound() {
    System.out.println("Guau");
}
```

### b) Por sobrecarga (overloading)

Una clase define varios métodos con el mismo nombre pero distinta firma:

```Java
int sum(int a, int b)
double sum(double a, double b)
```

---

## 5. Abstracción

Oculta la implementación y expone solo lo esencial.

Se logra mediante clases abstractas y métodos abstractos (`abstract`), que obligan a las subclases a definir su propio comportamiento.

```Java
abstract class Animal {
    public abstract void sound();
    public void sleep() {
        System.out.println("Duerme");
    }
}
```

---

## 6. Interfaces

Una interfaz es un contrato. Define qué métodos debe implementar una clase, pero no cómo.

```Java
interface Flying {
    void fly();
}
```

Una clase puede implementar múltiples interfaces:

```Java
class Bird extends Animal implements Flying {
    public void fly() { ... }
}
```

---

## 7. Composición

Representa una relación “tiene un”. Una clase contiene a otra como atributo.

```Java
class Car {
    private Engine engine = new Engine();
    public void on() {
        engine.on();
    }
}
```

A diferencia de la herencia, **no hay dependencia jerárquica**, sino estructural.

---