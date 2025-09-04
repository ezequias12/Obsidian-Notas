```Java
package basic.c03_strings;

public class Strings {

    public static void main(String[] args) {
        // Declaración

        String name = "Brais"; // si pusiera [var name = "Brais"] tambien funcionaria, se toma como que el tipo de variable es String por las comillas

        // como String es una clase podemos instanciarla como un objeto de la clase
        // lo toma como "redundante" pero se puede hacer
        var surname = new String("Moure");

        // Operaciones básicas

        // Concatenación
        System.out.println(name + " " + surname);


        // Longitud
        System.out.println(name.length()); // imprime 5

        // Obtener carácter
        System.out.println(name.charAt(name.length() - 1)); // imprime s
        // System.out.println(name.charAt(5)); Error ya que va del 0 al 4

        // Subcadena
        System.out.println(name.substring(2)); // imprime ais
        System.out.println(name.substring(1, 3)); // imprime ra, excluyendo el 3

        // Mayúsculas y minúsculas
        System.out.println(name.toUpperCase()); // imprime BRAIS
        System.out.println(name.toLowerCase()); // imprime brais

        System.out.println(name); // imprime Brais

        // Comprobar si contiene
        System.out.println("Hola, Java".contains("Brais")); // imprime false
        System.out.println("Hola, Java".toUpperCase().contains("AVA")); // imprime true

        // Comparación
        System.out.println(name.equals("Brais")); // imprime true
        System.out.println(name.equals("brais")); // imprime false
        System.out.println(name.equalsIgnoreCase("brais")); // imprime true
        // ignora mayusculas y minusculas

        // == vs. equals

        var a = "Brais";
        var b = "Brais";
        var c = new String("Brais"); // se podria crear normalmente con var c = "Brais";

        // el == compara el objeto como tal
        System.out.println(a == b); // imprime true porque son el mismo objeto
        System.out.println(a == c); // imprime false porque son dos objetos diferentes
        System.out.println(a.equals(c)); // imprime true porque compara el contenido

        // Trim
        // elimina los espacios al principio y al final
        System.out.println(" Hola, me llamo Brais ".trim()); // imprime "Hola, me llamo Brais"

        // Replace
        // reemplaza una cadena por otra
        System.out.println(" Hola, me llamo Brais ".replace("Brais", "Moure")); // imprime " Hola, me llamo Moure "

        System.out.println(" Hola, me llamo Brais ".replace(" ", "ESPACIO"));
        // imprime ESPACIOHola,ESPACIOmeESPACIOllamoESPACIOBraisESPACIO

        // Format
        // permite formatear cadenas de texto con variables
        var age = 37;
        System.out.println(String.format("Hola, %s. Tengo %d.", name, age));
        // imprime Hola, Brais. Tengo 37.
    }
}
```