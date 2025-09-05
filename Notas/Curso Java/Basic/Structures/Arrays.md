```Java
package basic.c05_structures;

public class Arrays {

    public static void main(String[] args) {

        // Declaración y creación
        int[] numbers = new int[3]; // si le hago un .length me da 3
        // para crearlo con numeros ya dentro
        // int[] numbers = {1, 2, 3};
        System.out.println(numbers);

        String[] names = {"Brais", "Moure", "mouredev"};
        System.out.println(names); // no me imprime el contenido

        // Acceso
        // con el indice podemos imprimir el contenido
        System.out.println(numbers[0]); // imprime 0, porque reserva el valor con 0
        System.out.println(names[0]);

        System.out.println((new String[3])[0]); // imprime null, reserva los valores con null

        // Modificación
        numbers[0] = 1;
        numbers[1] = 10;
        System.out.println(numbers[0]); // imprime 1
        System.out.println(numbers[1]); // imprime 10

        // numbers[3] = 2; Error

        System.out.println(names[2]); // imprime mouredev
        names[2] = "mouredev@gmail.com"; // cambiamos el valor
        System.out.println(names[2]); // imprime mouredev@gmail.com

        System.out.println(names.length); // imprime 3
        names[2] = null; // cambiamos el valor
        System.out.println(names[2]); // imprime null
        System.out.println(names.length); // sigue imprimiento 3 porque al null lo cuenta como un valor mas

        // numbers[2] = null; Error  -> no se puede poner null en un array de enteros

        boolean[] booleans = new boolean[5];
        System.out.println(booleans[4]);
    }
}
```