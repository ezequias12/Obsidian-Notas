```JavaScript
        String cadena = "Hola ";
        cadena += "mundo";

        System.out.println(cadena);

        // ejemplo de error
        int numero = 12;
        short otroNumero;

        // otroNumero = numero;  -> no se puede porque short es más pequeño que int
        otroNumero = (short) numero; // se puede hacer un cast
```

  

El cast lo que hace es que:

“Sé que este valor es de un tipo más grande o distinto, pero quiero tratarlo como si fuera de este otro tipo”.

### ¿Por qué es necesario aquí?

- `**int**` ocupa 4 bytes y puede almacenar números desde **2,147,483,648** hasta **2,147,483,647**.
- `**short**` ocupa 2 bytes y va de **32,768** a **32,767**.
- El compilador **no garantiza** que el número que tenés en `int` entre en un `short` sin perder datos.
- Para evitar errores “silenciosos”, Java te obliga a **hacer un cast explícito**: `(short) numero`.

### 🔍 Qué hace el cast

- Toma el valor binario de `numero`.
- Lo “recorta” a lo que cabe en un `short` (los 2 bytes menos significativos).
- Si el número es demasiado grande para un `short`, **se desborda** y el valor cambia.

---

```Java
 float elDecimal = 1.2f; // se debe poner la f al final para indicar que es un float
        
        var elSegundoNumero = 2.5; // var infiere el tipo de dato, en este caso double
```