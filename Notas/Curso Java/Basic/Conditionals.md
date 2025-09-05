```Java
package basic.c04_conditionals;

public class Conditionals {

    public static void main(String[] args) {

        // Condicionales

        // if, else if, else

        var age = 20;

        System.out.println(age >= 18);

        if (age > 18) {
            System.out.println("El usuario es mayor de edad");
        } else if (age == 18) {
            System.out.println("El usuario acaba de cumplir 18");
        } else {
            System.out.println("El usuario es menor de edad");
        }

        // switch

        var day = 5;

        switch (day) {
            case 1:
                System.out.println("Lunes");
                break;
            case 2:
                System.out.println("Martes");
                break;
            case 3:
                System.out.println("Miércoles");
                break;
            case 4:
                System.out.println("Jueves");
                break;
            case 5:
                System.out.println("Viernes");
                break;
            case 6:
                System.out.println("Sábado");
                break;
            case 7:
                System.out.println("Domingo");
                break;
            default:
                System.out.println("No es lunes, martes o miércoles");
        }
    }
}
```