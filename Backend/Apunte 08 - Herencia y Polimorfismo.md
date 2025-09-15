## 1. 🔹 Herencia en Java

La **herencia** es cuando una clase (subclase o derivada) “toma prestado” lo que otra clase (superclase o base) ya tiene.

- Reutiliza atributos y métodos.
- Permite modelar relaciones “es un…”.

📌 En criollo: si `Perro` **es un** `Animal`, entonces `Perro` hereda de `Animal`.

### Ejemplo básico:

```Java
// Clase base
public class Animal {
    public void comer() {
        System.out.println("Estoy comiendo...");
    }
}

// Clase derivada
public class Perro extends Animal {
    public void ladrar() {
        System.out.println("Guau guau!");
    }
}
```

Uso:

```Java
Perro p = new Perro();
p.comer();   // método heredado de Animal
p.ladrar();  // método propio de Perro
```

---

## 2. 🔹 Herencia simple vs múltiple

- **Java solo soporta herencia simple** → una clase hereda de una sola superclase.
- Para combinar más, se usan **interfaces** (se ven después).

---

## 3. 🔹 `extends` y clase `Object`

- Para heredar se usa `extends`.
- Si no ponés `extends`, tu clase hereda automáticamente de `Object`.
- Por eso todas las clases en Java tienen métodos como `toString()`, `equals()`, `hashCode()`.

---

## 4. 🔹 Constructores en herencia

Cuando una clase hija se crea, primero se ejecuta el constructor de la clase padre.

- Si no lo llamás, Java mete `super()` automáticamente.

Ejemplo:

```Java
public class Cuenta {
    private int numero;
    private double saldo;

    public Cuenta(int numero, double saldo) {
        this.numero = numero;
        this.saldo = saldo;
    }
}

public class Corriente extends Cuenta {
    private boolean descubierto;

    public Corriente(int num, double sal, boolean desc) {
        super(num, sal);      // llama al constructor de Cuenta
        this.descubierto = desc;
    }
}
```

---

## 5. 🔹 Sobrecarga vs Redefinición

- **Sobrecarga (overloading):** mismo nombre, diferentes parámetros.
- **Redefinición (overriding):** la subclase cambia el comportamiento de un método heredado.

Ejemplo de redefinición:

```Java
public class Cuenta {
    public String toString() {
        return "Cuenta genérica";
    }
}

public class Inversion extends Cuenta {
    @Override
    public String toString() {
        return "Cuenta de inversión con intereses";
    }
}
```

---

## 6. 🔹 Polimorfismo

El **polimorfismo** significa que una referencia de la clase base puede apuntar a un objeto de cualquier subclase.

- Esto permite tratar objetos distintos de forma uniforme.

Ejemplo:

```Java
Cuenta a = new Cuenta(1, 1000);
Cuenta b = new Inversion(2, 2000, 2.5f);
Cuenta c = new Corriente(3, 1500, true);

System.out.println(a.toString()); // "Cuenta genérica"
System.out.println(b.toString()); // "Cuenta de inversión..."
System.out.println(c.toString()); // "Cuenta corriente..."
```

👉 La JVM elige en tiempo de ejecución qué método ejecutar (esto se llama _dynamic binding_).

---

## 7. 🔹 Limitaciones del polimorfismo

- Si la referencia es del tipo de la clase padre, **solo puede llamar métodos que existen en la clase padre**.
- Para acceder a métodos específicos de la subclase → tenés que usar _casting_.

Ejemplo:

```Java
Cuenta x = new Inversion(4, 5000, 3.0f);

// x.actualizar();   // ❌ error de compilación (no existe en Cuenta)

Inversion y = (Inversion) x;   // casting explícito
y.actualizar();                // ✅ ahora sí
```

---

## 8. 🔹 `instanceof` y `getClass()`

Sirven para identificar de qué clase real es un objeto:

```Java
if (x instanceof Inversion) {
    System.out.println("Es una cuenta de inversión");
}

if (x.getClass() == Inversion.class) {
    System.out.println("Confirmado con getClass");
}
```

---

## 9. 🔹 Arrays polimórficos

Podés tener arrays de referencias de la clase base y llenarlos con objetos de distintas subclases.

Ejemplo:

```Java
Cuenta[] cuentas = new Cuenta[3];
cuentas[0] = new Inversion(1, 1000, 2.0f);
cuentas[1] = new Corriente(2, 200, false);
cuentas[2] = new Cuenta(3, 500);

for (Cuenta cta : cuentas) {
    System.out.println(cta);  // usa el toString correspondiente
}
```

---

## 10. 🔹 Clases Wrapper y Autoboxing

En Java, los tipos primitivos (`int`, `float`, etc.) tienen equivalentes “objetos” llamados **wrappers** (`Integer`, `Float`, etc.).

Esto permite usarlos en colecciones y aprovechar polimorfismo.

Ejemplo:

```Java
Integer i1 = 23;   // autoboxing: int → Integer
int i2 = i1;       // unboxing: Integer → int
```

---

## 11. 🔹 Modificadores `static`, `final` y `abstract`

- **static** → miembro de clase (compartido por todos los objetos).
- **final** → no se puede modificar/heredar.
- **abstract** → clase o método incompleto, que las subclases deben implementar.

Ejemplo rápido:

```Java
abstract class Figura {
    public abstract double area();
}

class Circulo extends Figura {
    private double r;
    public Circulo(double r) { this.r = r; }
    @Override
    public double area() { return Math.PI * r * r; }
}
```

---

# 🎯 Conclusiones

- **Herencia** → reusar código: “un Perro es un Animal”.
- **Redefinición** → cambiar comportamiento heredado.
- **Polimorfismo** → una referencia puede apuntar a objetos de distintas subclases.
- **Casting** → necesario si querés usar métodos de la subclase.
- **Wrappers y autoboxing** → tipos primitivos como objetos.
- **static, final, abstract** → modificadores clave para clases y métodos.

📌 En criollo:

- La herencia es como **heredar ADN** → una subclase trae todo lo de la clase base.
- El polimorfismo es como **tener un control remoto universal** → con el mismo control (`Cuenta`) podés manejar distintas teles (`Inversion`, `Corriente`), y cada una responde a su manera.

---

# 🏦 Ejemplo: Modelo Bancario con Herencia y Polimorfismo

---

## 📌 Clase base: `Cuenta`

```Java
public class Cuenta {
    private int numero;
    private double saldo;

    // Constructor
    public Cuenta(int numero, double saldo) {
        this.numero = numero;
        this.saldo = saldo;
    }

    // Métodos comunes
    public void depositar(double monto) {
        saldo += monto;
    }

    public void retirar(double monto) {
        if (monto <= saldo) {
            saldo -= monto;
        } else {
            System.out.println("Fondos insuficientes en la cuenta " + numero);
        }
    }

    // Getters
    public int getNumero() {
        return numero;
    }

    public double getSaldo() {
        return saldo;
    }

    // toString (puede ser redefinido por las subclases)
    @Override
    public String toString() {
        return "Cuenta N°" + numero + " - Saldo: $" + saldo;
    }
}
```

---

## 📌 Subclase: `Inversion`

```Java
public class Inversion extends Cuenta {
    private float tasaInteres; // % de interés anual

    public Inversion(int numero, double saldo, float tasaInteres) {
        super(numero, saldo); // invoca al constructor de Cuenta
        this.tasaInteres = tasaInteres;
    }

    // Método propio
    public void actualizar() {
        double interes = getSaldo() * (tasaInteres / 100);
        depositar(interes);
    }

    @Override
    public String toString() {
        return "Cuenta Inversión N°" + getNumero() +
               " - Saldo: $" + getSaldo() +
               " - Tasa: " + tasaInteres + "%";
    }
}
```

---

## 📌 Subclase: `Corriente`

```Java
public class Corriente extends Cuenta {
    private boolean descubierto; // ¿permite saldo negativo?

    public Corriente(int numero, double saldo, boolean descubierto) {
        super(numero, saldo);
        this.descubierto = descubierto;
    }

    @Override
    public void retirar(double monto) {
        if (descubierto || monto <= getSaldo()) {
            // usamos super para no reescribir la lógica del padre
            super.retirar(monto);
        } else {
            System.out.println("No se puede retirar: descubierto no habilitado.");
        }
    }

    @Override
    public String toString() {
        return "Cuenta Corriente N°" + getNumero() +
               " - Saldo: $" + getSaldo() +
               " - Descubierto: " + (descubierto ? "Sí" : "No");
    }
}
```

---

## 📌 Clase principal: `App`

```Java
public class App {
    public static void main(String[] args) {
        // Polimorfismo: todas son "Cuenta"
        Cuenta[] cuentas = new Cuenta[3];
        cuentas[0] = new Cuenta(1, 1000);
        cuentas[1] = new Inversion(2, 2000, 5.0f);
        cuentas[2] = new Corriente(3, 500, false);

        // Recorrer y mostrar
        for (Cuenta c : cuentas) {
            System.out.println(c); // invoca el toString adecuado
        }

        System.out.println("\n--- Operaciones ---");
        cuentas[0].depositar(200);
        cuentas[1].retirar(500);
        cuentas[2].retirar(600); // debería fallar (no hay descubierto)

        // Polimorfismo en acción con casting
        if (cuentas[1] instanceof Inversion) {
            Inversion inv = (Inversion) cuentas[1];
            inv.actualizar(); // método propio de Inversion
        }

        System.out.println("\n--- Después de operaciones ---");
        for (Cuenta c : cuentas) {
            System.out.println(c);
        }
    }
}
```

---

## 📌 Ejecución esperada

```Plain
Cuenta N°1 - Saldo: $1000.0
Cuenta Inversión N°2 - Saldo: $2000.0 - Tasa: 5.0%
Cuenta Corriente N°3 - Saldo: $500.0 - Descubierto: No

--- Operaciones ---
Fondos insuficientes en la cuenta 3

--- Después de operaciones ---
Cuenta N°1 - Saldo: $1200.0
Cuenta Inversión N°2 - Saldo: $1575.0 - Tasa: 5.0%
Cuenta Corriente N°3 - Saldo: $500.0 - Descubierto: No
```

---

📌 En criollo:

- `Cuenta` es la base con lo común.
- `Inversion` agrega interés.
- `Corriente` redefine cómo se retira (según descubierto).
- En el `App` usamos **polimorfismo**: todas son `Cuenta`, pero cada una responde distinto a `toString()` y `retirar()`.