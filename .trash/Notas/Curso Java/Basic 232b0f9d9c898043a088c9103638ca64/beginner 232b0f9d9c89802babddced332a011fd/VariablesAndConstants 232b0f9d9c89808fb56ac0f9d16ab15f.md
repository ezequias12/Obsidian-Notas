# VariablesAndConstants

```java
package basic.c01_beginner;

/*
Clase 15 - Variables y constantes
Vídeo: https://youtu.be/JOAqpdM36wI?t=5294
*/

public class VariablesAndConstants {

    public static void main(String[] args) {

        // Variables

        String name = "Brais";
        System.out.println(name);

        name = "MoureDev";
        System.out.println(name);

        // name = 37; Error (no podemos cambiar el tipo de dato)

        int age = 37;
        System.out.println(age);

        var email = "mouredev@gmail.com"; //
        System.out.println(email);

        var year = 2025;
        System.out.println(year);

        // Constantes
        // Seria como una String normal pero le agrego el modificador final antes
        final String EMAIL = "mouredev@gmail.com";
        // EMAIL = "brais@gmail.com"; Es constante
        System.out.println(EMAIL); // Para las constantes usamos generalmente MAYUSCULA asi se entiende mas rapido que es una constante

    }
}

```