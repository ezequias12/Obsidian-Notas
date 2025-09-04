# Maps

Un **Map** en Java es una estructura que guarda **pares clave-valor**.

Cada **clave es única**, y se usa para acceder a su valor asociado de forma rápida.

HashMap  → Sin orden

```java
package basic.c05_structures;

import java.util.HashMap;

public class Maps {

    public static void main(String[] args) {

        // Declaración y creación
        // cambia la forma de definir, no es como lists y sets

        HashMap<String, String> names = new HashMap<>();
        // Para asociar a un nombre un email, hago String, String
        var numbers = new HashMap<Integer, String>();
        // para asociar a un numero un nombre, hago Integer, String

        // Tamaño

        System.out.println(names.size());

        // Inserción

        names.put("Brais", "brais@gmail.com"); // es un par de valores
        names.put("Moure", "moure@gmail.com");
        names.put("MoureDev", "mouredev@gmail.com");
        System.out.println(names.size());
        System.out.println(names);
        // imprime {MoureDev=mouredev@gmail.com, Moure=moure@gmail.com, Brais=brais@gmail.com}
        // 3 elementos donde cada uno se conforma de una clave y un valor

        // Acceso
        // permite acceder a los valores mediante la clave
        System.out.println(names.get("Moure"));
        System.out.println(names.get("Dev"));

        // Verificación

        System.out.println(names.containsKey("Moure")); // verifica si la clave existe
        System.out.println(names.containsKey("Dev"));

        System.out.println(names.containsValue("moure@gmail.com")); // verifica si el valor existe

        // Eliminación
        // se puede eliminar tambien con clave y valor juntos para mas seguridad
        System.out.println(names.remove("Moure"));
        System.out.println(names.remove("MoureDev"));
        System.out.println(names);

        // Limpieza

        names.clear();
        System.out.println(names);

        // Modificación

        names.put("Brais", "brais@gmail.com");
        System.out.println(names);

        names.put("Brais", "braismouredev@gmail.com");
        // si guardamos una clave repetida pero cambiamos el valor, si se guarda
        System.out.println(names);

        names.replace("Moure", "mouredev@gmail.com"); // Reemplaza el valor si existe
        System.out.println(names);

        names.putIfAbsent("Moure", "mouredev@gmail.com"); // Solo lo añade si no existe
        System.out.println(names);

        // Otras operaciones

        System.out.println(names.isEmpty()); // devuelve true si esta vacio
        var values = names.values(); // creo una variable solo con los valores
        System.out.println(values);
     }
}

```