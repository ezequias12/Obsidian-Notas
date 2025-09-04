```Java
package basic.c01_beginner;

/*
Clase 16 - Tipos de datos primitivos
Vídeo: https://youtu.be/JOAqpdM36wI?t=6211
*/

public class DataTypes {

    public static void main(String[] args) {

        // Tipos de datos primitivos

        int myInt = 37; // entero
        System.out.println(myInt);

        double myDouble = 1.77; // decimal
        System.out.println(myDouble);

        // float, long, byte

        char myChar = 'a'; // solo con comilla simple
        System.out.println(myChar);

        boolean myBoolean = true;
        myBoolean = false;
        System.out.println(myBoolean);

        String myString = "Hola, Java"; // en realidad es un objeto (clase String)
        System.out.println(myString);

        // Tipo de dato en tiempo de compilación
        // no tenemos un TypeOff como en otros lenguajes
        // System.out.println(myBoolean) seria el normal

        System.out.println(myString.getClass().getSimpleName());
        // obtenemos los datos de la clase con getClass() y luego el nombre de la clase con getSimpleName()
    }
}
```