# Notas Clase 26/08 - Actividad ListaArray

# Actividad de Clase - Síntesis del enunciado

---

# Paso 1 — ListaArray básica

- Creá la clase `ListaArray` con:
    - `Object[] elementos` (array interno), `int cantidad`.
    - `add(Object e)`, `size()`, y `toString()` mostrando solo los `cantidad` elementos.
- Agregá `get(int index)` con control de límites: si está fuera de rango, lanzá `IndexOutOfBoundsException`.
- Prueba mínima (en `main`): crear lista, hacer `add("uno")`, `add("dos")`, `add("tres")`, y verificar `get(1)` y el `toString`.

# Paso 2 — Iterador “propio” (no estándar)

- Sumá en la clase:
    - `reiniciarIterador()`, `haySiguiente()`, `siguiente()`.
- Implementá con un `int actual` interno:
    - `reiniciarIterador()` → `actual = 0`
    - `haySiguiente()` → `actual < size()`
    - `siguiente()` → `elementos[actual++]`, y si no hay siguiente: `NoSuchElementException`.
- Prueba mínima: el `while (lista.haySiguiente()) { System.out.println(lista.siguiente()); }` del paso 1.

# Paso 3 — Implementar `Iterable`

- Hacé que `ListaArray` implemente la interfaz propia de java `Iterable`.
- Creá **clase interna** `ListaArrayIterator` que implemente `Iterator` con `int pos = 0`, `hasNext()`, `next()` (y `NoSuchElementException` si corresponde).
- `iterator()` debe **retornar una instancia nueva** de ese iterador.
- Esto habilita:
    - Uso de `Iterator it = miLista.iterator(); while (it.hasNext()) {...}` y
    - `for (Object elem : miLista) { ... }`.
- Detalle esperado (resumen): `pos < size()` en `hasNext`, `elementos[pos++]` en `next`, excepción si no hay más.

# Paso 4 — Extender `AbstractList`

- Hacé que `ListaArray` **herede de `AbstractList`**.
- Implementá mínimos obligatorios:
    - `get(int index)` (ya lo tenés),
    - `size()`.
    - (Opcional: `add(Object)` para contrato `List` más completo).
- Beneficios: compatibilidad con APIs que esperan `List` (p.ej., `Collections.sort`), polimorfismo y validaciones con `instanceof List`.
- Nota: El paso te introduce formalmente al framework de colecciones.

# Tests / Validación (si el proyecto incluye tests)

- Ejecutá los tests **de a uno** (descomentando), fijate qué falla, implementá lo faltante y volvé a correr.

---

---

# DESARROLLO

## PASO 1 Y 2

## 🔹 Paso 1 – Clase básica con almacenamiento en array

La idea inicial fue **simular una lista dinámica** usando un **array fijo** de Java.

Como los arrays no pueden crecer, inventamos un mecanismo para “agrandarlo” copiando a uno más grande cuando se llena.

### Estructura:

1. **Atributos**
    - `Object[] elementos`: el array interno donde se guardan los datos.
        
        Usamos `Object` porque nos deja guardar *cualquier tipo de objeto*.
        
    - `int count`: un contador de cuántos elementos reales tiene la lista (ya que el array puede tener espacio de sobra).
2. **Constructores**
    - Uno que recibe `capacidadInicial` y crea el array de ese tamaño.
    - Uno sin parámetros que por defecto crea un array de tamaño 4 (`this(4)`).
3. **Métodos**
    - `add(Object e)`: agrega un elemento al final.
        - Si hay espacio, lo pone en la posición `count`.
        - Si no hay, duplica la capacidad del array, copia lo viejo, y recién ahí lo agrega.
        - Luego incrementa `count`.
    - `size()`: devuelve cuántos elementos hay cargados (el valor de `count`).
    - `get(int index)`: devuelve el elemento en la posición pedida, verificando que esté dentro de los límites.
    - `toString()`: convierte la lista en un string legible tipo `[uno, dos, tres]`.

👉 Hasta acá, tenés una lista funcional: podés agregar, consultar, y mostrar.

---

## 🔹 Paso 2 – Iterador “casero”

Hasta acá solo podías recorrer con un `for` y `get(i)`.

El segundo paso fue agregar un **mecanismo de iteración manual**, para recorrer la lista sin usar índices directamente.

### ¿Qué hicimos?

1. Creamos un nuevo atributo interno:
    - `int actual`: sirve como puntero a la posición en la que estamos durante la iteración.
2. Agregamos 3 métodos:
    - `reiniciarIterador()`: pone `actual = 0`, o sea, empezamos desde el primer elemento.
    - `haySiguiente()`: devuelve `true` si todavía hay elementos por recorrer.
    - `siguiente()`: devuelve el elemento en la posición actual y avanza (`actual++`).
        - Si no hay más elementos, lanza una excepción `NoSuchElementException`.

### Ejemplo de uso:

```java
lista.reiniciarIterador();
while (lista.haySiguiente()) {
    System.out.println(lista.siguiente());
}

```

📌 Esto nos permitió simular cómo funcionan los iteradores, pero todavía es una solución **artesanal y limitada**: solo un iterador activo a la vez.

## Codigo hasta el paso 1 y 2

```java
package com.ejemplo;

public class ListaArray {

    // atributos

    // Array interno donde se almacenan los elementos
    // se usa Objet para guardar basicamente cualquier tipo de dato
    private Object[] elementos;

    // Contador de elementos q tiene la lista
    // (para comparar tamanio del array con el numero de elementos)
    private int count;

    // Constructores ----

    // constructor con alguna capacidad inicial
    // ej new ListaArray(10) -> capacidad inicial 10
    public ListaArray(int capacidadInicial) {
        elementos = new Object[capacidadInicial];
        count = 0;
    }

    // constructor por defecto (con capacidad inicial 4)
    public ListaArray() {
        //Forma de reutilizar otro constructor de la misma clase
        // es como que llame de nuevo al constructor con parametro
        this(4);
    }

    // metodo para agregar un elemento al final de la lista
    public void add(Object e) {
        // 1. verificar si hay espacio en el array
        if (count == elementos.length) {
            // no hay espacio, agrandamos creando una copia del doble de tamaño
            Object[] nuevo = new Object[elementos.length * 2];

            // copiar los elementos del array viejo al nuevo
            // IMPORTANTE
            // Se usa un for para evitar el metodo System.arraycopy
            for (int i = 0; i < elementos.length; i++) {
                nuevo[i] = elementos[i];
            }
            // ahora elementos apunta al nuevo
            elementos = nuevo;
        }
        // 2. insertar el nuevo elemento en la posicion count
        elementos[count] = e;
        // 3. aumentar el count
        count++;

    }

    // cantidad de elementos cargados en la lista
    public int size() {
        return count;
    }

    // devolver el elemento en la posicion index, para hacer get(indice)
    public Object get(int index) {
        // verificar que el indice sea valido
        if (index < 0 || index >= count) {
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + count);
        }
        return elementos[index];
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("["); // abrimos la lista

        for (int i = 0; i < count; i++) {
            sb.append(elementos[i]); // agrego el elemento
            if (i < count - 1) {
                sb.append(", "); // separador
            }
        }

        sb.append("]");
        return sb.toString();
    }

    // Iterador basico, no estandar todavia
    // Posicion actual del iterador
    private int actual = 0;

    public void reiniciarIterador() {
        actual = 0;
    }

    public boolean haySiguiente() {
        return actual < count;
    }

    public Object siguiente() {
        if (!haySiguiente()) {
            throw new java.util.NoSuchElementException("No hay mas elementos");
        }
        // devuelve el actual y avanza el puntero
        return elementos[actual++];
    }

}

```

---

## PASO 3

Ahora, el **Paso 3** es llevar ese “iterador casero” al **iterador estándar de Java (`Iterator`)** para que tu clase funcione con `for-each`.

## 🔹 `Iterable`

- Es **una interfaz de Java**.
- Significa: *“este objeto puede ser recorrido uno a uno”*.
- Si una clase implementa `Iterable`, el compilador ya entiende que puede usarse en un `for-each`.

👉 Ejemplo:

```java
for (Object e : miLista) {
    ...
}
```

Ese `for-each` solo funciona si la clase implementa `Iterable`.

---

## 🔹 `Iterator`

- Es otra interfaz de Java.
- Representa al **objeto que hace la iteración**.
- Tiene 2 métodos clave:
    - `hasNext()` → ¿queda algún elemento más?
    - `next()` → dame el siguiente elemento y avanzá.

👉 Ejemplo de uso directo:

```java
Iterator<Object> it = miLista.iterator();
while (it.hasNext()) {
    Object elem = it.next();
    System.out.println(elem);
}

```

---

## 🔹 Relación entre ambos

- Una clase que implementa `Iterable` **debe tener un método `iterator()`**.
- Ese método devuelve un objeto que implementa `Iterator`.
- Ese `Iterator` es el que se encarga del recorrido interno.

---

📌 En criollo:

- `Iterable` = “la caja sabe entregarme un iterador si se lo pido”.
- `Iterator` = “el dedo que va pasando uno por uno los elementos de la caja”.

---

Tu `ListaArray` va a implementar `Iterable`, y adentro tendrá una **clase interna** que actúe como `Iterator`.

---

## Contexto para entender mejor

1. **Clase `ListaArray` básica**
    - Internamente tiene un `Object[] elementos` y un `int count`.
    - Puede crecer dinámicamente cuando se llena (duplicando su tamaño).
    - Métodos: `add`, `get`, `size`, `toString`.
2. **Iterador “casero”**
    - Agregaste puntero `actual` con `reiniciarIterador()`, `haySiguiente()` y `siguiente()`.
3. **Soporte real de iteradores de Java**
    - Tu clase ahora **`implements Iterable<Object>`**.
        
        👉 Esto significa que **puede ser usada en un `for-each`**.
        
    - Dentro definiste una **clase interna `IteradorLista` que implementa `Iterator<Object>`**.
        - `hasNext()` → devuelve si quedan elementos.
        - `next()` → devuelve el elemento actual y avanza el cursor.
        - `remove()` → lo dejaste como “no soportado”.

---

### Dudas

- **¿Qué es ese `<Object>`?**
    - En `Iterable<Object>` y `Iterator<Object>`, lo que va entre `< >` se llama **parámetro genérico**.
    - Le indica al compilador “este iterable va a devolver **objetos de tipo Object**”.
    - Como pusiste `Object`, significa que tu lista es **genérica pero muy amplia**: puede devolver cualquier cosa.
    - Si mañana la declarás como `ListaArray<String>`, recién ahí devolvería `Iterator<String>` (eso es lo que veremos con *generics* más adelante).
- **Por qué obligan a implementar `hasNext()` y `next()`**
    - Porque son el **contrato mínimo** de `Iterator`. Sin esos dos métodos, no hay forma de recorrer nada.
    - `remove()` es opcional (la interfaz lo tiene, pero podés dejarlo como no soportado).

---

### 📌 En criollo

- `Iterable<T>` → la clase puede devolver un iterador de elementos de tipo `T`.
- `Iterator<T>` → es el “dedo” que va pasando de uno en uno esos elementos.
- En tu caso usaste `Object`, así que por ahora puede devolver cualquier cosa (más adelante vas a restringirlo con generics).

## Codigo del Paso 3

```java
package com.ejemplo;

import java.util.Iterator;
import java.util.NoSuchElementException;

public class ListaArray implements Iterable<Object>{

    // atributos

    // Array interno donde se almacenan los elementos
    // se usa Objet para guardar basicamente cualquier tipo de dato
    private Object[] elementos;

    // Contador de elementos q tiene la lista
    // (para comparar tamanio del array con el numero de elementos)
    private int count;

    // Constructores ----

    // constructor con alguna capacidad inicial
    // ej new ListaArray(10) -> capacidad inicial 10
    public ListaArray(int capacidadInicial) {
        elementos = new Object[capacidadInicial];
        count = 0;
    }

    // constructor por defecto (con capacidad inicial 4)
    public ListaArray() {
        //Forma de reutilizar otro constructor de la misma clase
        // es como que llame de nuevo al constructor con parametro
        this(4);
    }

    // metodo para agregar un elemento al final de la lista
    public void add(Object e) {
        // 1. verificar si hay espacio en el array
        if (count == elementos.length) {
            // no hay espacio, agrandamos creando una copia del doble de tamaño
            Object[] nuevo = new Object[elementos.length * 2];

            // copiar los elementos del array viejo al nuevo
            // IMPORTANTE
            // Se usa un for para evitar el metodo System.arraycopy
            for (int i = 0; i < elementos.length; i++) {
                nuevo[i] = elementos[i];
            }
            // ahora elementos apunta al nuevo
            elementos = nuevo;
        }
        // 2. insertar el nuevo elemento en la posicion count
        elementos[count] = e;
        // 3. aumentar el count
        count++;

    }

    // cantidad de elementos cargados en la lista
    public int size() {
        return count;
    }

    // devolver el elemento en la posicion index, para hacer get(indice)
    public Object get(int index) {
        // verificar que el indice sea valido
        if (index < 0 || index >= count) {
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + count);
        }
        return elementos[index];
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("["); // abrimos la lista

        for (int i = 0; i < count; i++) {
            sb.append(elementos[i]); // agrego el elemento
            if (i < count - 1) {
                sb.append(", "); // separador
            }
        }

        sb.append("]");
        return sb.toString();
    }

    // Iterador basico, no estandar todavia
    // Posicion actual del iterador
    private int actual = 0;

    public void reiniciarIterador() {
        actual = 0;
    }

    public boolean haySiguiente() {
        return actual < count;
    }

    public Object siguiente() {
        if (!haySiguiente()) {
            throw new java.util.NoSuchElementException("No hay mas elementos");
        }
        // devuelve el actual y avanza el puntero
        return elementos[actual++];
    }

    // Paso 3: Implementar el metodo iterator() de la interfaz Iterable

    // este metodo tiene que devolver un "objeto iterador" nuevo cada vez que se llama
    @Override
    public Iterator<Object> iterator() {
        // devolvemos una nueva instancia de la clase interna
        return new IteradorLista();
    }
    
    // ahora va una clase interna (oculta) que implementa Iterator<Object>.
    // Mantiene su propio puntero independiente de la lista principal
    private class IteradorLista implements Iterator<Object> {
        // puntero del iterador (arranca en 0)
        private int posicion = 0;

        // revisamos si quedan elementos
        @Override
        public boolean hasNext() {
            return posicion < count;
        }

        @Override
        public Object next() {
            if (!hasNext()) {
                throw new NoSuchElementException("No hay mas elementos");
            }
            return elementos[posicion++]; // devuelve el actual y avanza el puntero
        }

        @Override
        public void remove() {
            throw new UnsupportedOperationException("remove no soportado");
        }
    }

}

```

## Codigo en el [App.java](http://App.java) para probar esto

```java
package com.ejemplo;

public class App {
    public static void main(String[] args) {
        // Crear una ListaArray con capacidad inicial 2
        ListaArray lista = new ListaArray(2);

        // Agregar algunos elementos
        lista.add("uno");
        lista.add("dos");
        lista.add("tres"); // acá debería duplicar la capacidad interna

        // Probar size()
        System.out.println("Tamaño actual: " + lista.size()); // debería mostrar 3

        // Probar get()
        System.out.println("Elemento en posición 0: " + lista.get(0)); // "uno"
        System.out.println("Elemento en posición 1: " + lista.get(1)); // "dos"
        System.out.println("Elemento en posición 2: " + lista.get(2)); // "tres"

        // Si probás un índice fuera de rango
        // System.out.println(lista.get(5)); // debería tirar IndexOutOfBoundsException

        System.out.println("Lista completa: " + lista);

        lista.reiniciarIterador();
        while (lista.haySiguiente()) {
            System.out.println("Iterando: " + lista.siguiente());
        }

        System.out.println("For-each sobre ListaArray:");
        for (Object e : lista) {          // gracias a Iterable + Iterator
            System.out.println(" -> " + e);
        }

    }
}

```

## PASO 4