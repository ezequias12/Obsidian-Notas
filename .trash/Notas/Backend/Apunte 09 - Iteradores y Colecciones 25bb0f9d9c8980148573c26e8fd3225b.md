# Apunte 09 - Iteradores y Colecciones

## 1. 🔹 ¿Qué son las colecciones?

Hasta ahora usamos **arrays**:

- Tamaño fijo, homogéneos (un solo tipo).
- Poco flexibles (no podés agregar ni quitar fácil).

👉 Las **colecciones** son estructuras de datos dinámicas que permiten almacenar, recorrer y manipular objetos de forma más práctica.

Ejemplos en Java: `ArrayList`, `LinkedList`, `HashSet`, `HashMap`.

📌 En criollo: un array es como una **caja con casilleros numerados fijos**; una colección es como una **bolsa flexible** que se agranda/achica según lo que metas.

---

## 2. 🔹 Implementación propia de lista sobre arrays

El apunte arranca mostrando cómo podés armar tu **propia lista dinámica** usando arrays.

- Internamente usa un array.
- Al llenarse, crea otro más grande y copia los elementos (`System.arraycopy`).
- Controla la cantidad de elementos con un contador (`count`).

Ejemplo simplificado:

```java
public class ListaArray {
    private Object[] elementos;
    private int count;

    public ListaArray(int capacidadInicial) {
        elementos = new Object[capacidadInicial];
        count = 0;
    }

    public void agregar(Object o) {
        if (count == elementos.length) {
            // redimensionar
            Object[] temp = new Object[elementos.length * 2];
            System.arraycopy(elementos, 0, temp, 0, count);
            elementos = temp;
        }
        elementos[count++] = o;
    }

    public Object get(int i) {
        return elementos[i];
    }

    public int size() {
        return count;
    }
}

```

Uso:

```java
ListaArray lista = new ListaArray(2);
lista.agregar("Ana");
lista.agregar("Juan");
lista.agregar("Pedro"); // redimensiona internamente
System.out.println(lista.size()); // 3

```

---

## 3. 🔹 Iteradores

El problema: ¿cómo recorremos una colección sin saber cómo está implementada?

👉 Solución: el **patrón iterador**.

- Define un objeto especial que sabe recorrer la colección.
- Métodos típicos:
    - `iniciar()` → vuelve al principio.
    - `hayMas()` → devuelve si quedan elementos.
    - `getActual()` → obtiene el elemento actual.
    - `siguiente()` → avanza al siguiente.

Ejemplo conceptual:

```java
Iterador it = lista.iterador();
while(it.hayMas()) {
    System.out.println(it.getActual());
    it.siguiente();
}

```

📌 En criollo: el **iterador** es como un “puntero” que va recorriendo tu colección paso a paso, sin que tengas que preocuparte por su estructura interna.

---

## 4. 🔹 Clases internas y anónimas

En la implementación del iterador, muchas veces se usan:

- **Clases internas** → una clase declarada dentro de otra (puede acceder a sus atributos privados).
- **Clases anónimas** → clases sin nombre, creadas en el momento, generalmente para una sola tarea (muy usadas con interfaces).

Ejemplo de clase anónima:

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Soy una clase anónima!");
    }
};

```

---

## 5. 🔹 Generics (clases parametrizadas)

Si tu lista guarda `Object`, después tenés que hacer *casting*:

```java
String nombre = (String) lista.get(0);

```

👉 Para evitar eso existen los **genéricos (Generics)**:

- Te permiten declarar la clase con un tipo de dato parametrizado.
- Así el compilador valida que la colección sea homogénea.

Ejemplo:

```java
public class ListaArray<E> {
    private E[] elementos;
    private int count;

    public void agregar(E elem) { ... }
    public E get(int i) { return elementos[i]; }
}

```

Uso:

```java
ListaArray<String> lista = new ListaArray<>();
lista.agregar("Ana");
String nombre = lista.get(0); // no necesita casting

```

📌 En criollo: los **genéricos** son como poner un cartel en la bolsa diciendo “solo entran manzanas 🍎”, y el compilador te avisa si tratás de meter una pera 🍐.

---

## 6. 🔹 Framework de colecciones de Java

En lugar de reinventar la rueda, Java trae un conjunto muy potente de colecciones en `java.util`.

Principales interfaces:

- **List** → colecciones ordenadas, permiten duplicados (`ArrayList`, `LinkedList`).
- **Set** → colecciones sin duplicados (`HashSet`, `TreeSet`).
- **Map** → pares clave-valor (`HashMap`, `TreeMap`).

Ejemplo con `ArrayList`:

```java
import java.util.ArrayList;

public class Ejemplo {
    public static void main(String[] args) {
        ArrayList<String> nombres = new ArrayList<>();
        nombres.add("Ana");
        nombres.add("Juan");
        nombres.add("Pedro");

        for (String n : nombres) {   // foreach simplificado
            System.out.println(n);
        }
    }
}

```

---

## 7. 🔹 Paquetes e imports

- Las clases en Java están organizadas en **paquetes (packages)**.
- Para usarlas hay que hacer `import`.

Ejemplo:

```java
import java.util.ArrayList;  // solo ArrayList
import java.util.*;          // todo el paquete java.util

```

El único paquete que se importa automáticamente es `java.lang` (ej: `String`, `Math`, `Object`).

---

# 🎯 Conclusiones

- **Arrays** son rígidos, las **colecciones** son dinámicas y más flexibles.
- El **patrón iterador** permite recorrer colecciones sin importar cómo estén implementadas.
- **Clases internas/anónimas** ayudan a encapsular lógica dentro de otra clase.
- **Genéricos (Generics)** evitan el casting y hacen el código más seguro.
- El **framework de colecciones de Java (java.util)** trae listas, sets y mapas listos para usar.

📌 En criollo:

- Los **arrays** son como bandejas con lugares fijos.
- Las **colecciones** son como mochilas flexibles que podés llenar y vaciar a gusto.
- Los **iteradores** son como un dedo que va señalando elemento por elemento.
- Los **genéricos** son la etiqueta que dice qué tipo de cosas admite tu mochila.

---

# Ejemplo Integrador

**Ejemplo integrador** que usa **colecciones + herencia + polimorfismo** con las clases que ya hicimos (`Cuenta`, `Inversion`, `Corriente`).

Podés **copiar/pegar tal cual** en un archivo `AppColecciones.java` (las clases de cuenta son las mismas que ya pegaste antes).

```java
import java.util.*;
import java.util.stream.Collectors;

public class AppColecciones {

    public static void main(String[] args) {
        // 1) Lista polimórfica: una sola lista con distintos tipos de cuentas
        List<Cuenta> cuentas = new ArrayList<>();
        cuentas.add(new Cuenta(1, 1200));
        cuentas.add(new Inversion(2, 2000, 5.0f));
        cuentas.add(new Corriente(3, 500, false));
        cuentas.add(new Inversion(4, 1500, 3.0f));
        cuentas.add(new Corriente(5, 0, true));

        System.out.println("=== LISTA INICIAL ===");
        imprimir(cuentas);

        // 2) Operaciones polimórficas generales
        //    Todas soportan depositar/retirar (definido en la clase base Cuenta)
        cuentas.get(0).depositar(300);  // Cuenta común
        cuentas.get(1).retirar(250);    // Inversión
        cuentas.get(2).retirar(600);    // Corriente sin descubierto -> debería rechazar
        cuentas.get(4).retirar(200);    // Corriente con descubierto -> permite saldo negativo

        // 3) Operación específica de subclase: actualizar() solo en Inversion
        //    Recorremos y casteamos solo las que sean Inversion
        for (Cuenta c : cuentas) {
            if (c instanceof Inversion inv) {
                inv.actualizar(); // suma intereses
            }
        }

        System.out.println("\n=== DESPUÉS DE OPERACIONES ===");
        imprimir(cuentas);

        // 4) Streams: totales, filtros y mapeos (opcional pero súper útil)
        double saldoTotal = cuentas.stream()
                .mapToDouble(Cuenta::getSaldo)
                .sum();

        double saldoInversiones = cuentas.stream()
                .filter(c -> c instanceof Inversion)
                .mapToDouble(Cuenta::getSaldo)
                .sum();

        long cantidadCorrientesConDescubierto = cuentas.stream()
                .filter(c -> c instanceof Corriente co && esDescubierto(co))
                .count();

        System.out.println("\n=== MÉTRICAS ===");
        System.out.println("Saldo total de todas las cuentas: $" + saldoTotal);
        System.out.println("Saldo total SOLO de inversiones: $" + saldoInversiones);
        System.out.println("Cantidad de cuentas corrientes con descubierto: " + cantidadCorrientesConDescubierto);

        // 5) Ordenamientos (Comparator)
        //   a) Por saldo ascendente
        List<Cuenta> porSaldoAsc = new ArrayList<>(cuentas);
        porSaldoAsc.sort(Comparator.comparingDouble(Cuenta::getSaldo));
        System.out.println("\n=== ORDENADAS POR SALDO ASCENDENTE ===");
        imprimir(porSaldoAsc);

        //   b) Por tipo (Cuenta, Corriente, Inversion) y luego por saldo desc
        List<Cuenta> porTipoYSaldoDesc = new ArrayList<>(cuentas);
        porTipoYSaldoDesc.sort(Comparator
                .comparing(AppColecciones::claveTipo)      // primero por tipo
                .thenComparing(Cuenta::getSaldo).reversed() // luego por saldo desc
        );
        System.out.println("\n=== ORDENADAS POR TIPO Y SALDO DESC ===");
        imprimir(porTipoYSaldoDesc);

        // 6) Búsquedas rápidas por número de cuenta con Map
        Map<Integer, Cuenta> porNumero = cuentas.stream()
                .collect(Collectors.toMap(Cuenta::getNumero, c -> c));

        System.out.println("\n=== BÚSQUEDA POR NÚMERO ===");
        int buscar = 4;
        Cuenta encontrada = porNumero.get(buscar);
        System.out.println("Cuenta N°" + buscar + ": " + (encontrada != null ? encontrada : "no existe"));

        // 7) Evitar duplicados con Set (ejemplo simple: números de cuenta)
        Set<Integer> numeros = new HashSet<>();
        for (Cuenta c : cuentas) {
            if (!numeros.add(c.getNumero())) {
                System.out.println("Duplicado detectado: " + c.getNumero());
            }
        }
        // Si no imprime nada, es que no hay duplicados.
    }

    // ---------- Helpers didácticos ----------

    private static void imprimir(List<Cuenta> cuentas) {
        for (Cuenta c : cuentas) {
            System.out.println(c);
        }
    }

    // Para ordenar por tipo: damos una “clave” estable para cada clase
    private static int claveTipo(Cuenta c) {
        if (c instanceof Inversion) return 3;
        if (c instanceof Corriente) return 2;
        return 1; // Cuenta (base)
    }

    // Acceso de solo lectura a "descubierto" (sin modificar tu clase Corriente)
    private static boolean esDescubierto(Corriente c) {
        // Si querés, podés agregar getDescubierto() en Corriente y usarlo directo.
        // Para mantener el ejemplo sin tocar tus clases:
        // asumimos que toString() tiene "Descubierto: Sí/No" y lo leemos (no ideal, solo didáctico).
        return c.toString().contains("Descubierto: Sí");
    }
}

```

### Qué estás viendo (en criollo):

- **List**: guardamos distintos tipos de cuentas en una sola lista (polimorfismo).
- **instanceof**: detectamos `Inversion` para llamar a `actualizar()` (método propio de la subclase).
- **Streams**: sumas, filtros y conteos sin bucles manuales.
- **Comparator**: distintas formas de ordenar (por saldo, por tipo y saldo).
- **Map**: búsqueda directa por número de cuenta (clave → valor).
- **Set**: chequear duplicados de números de cuenta.