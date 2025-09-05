## Primera parte

Explica como funciona java y como ejecutarlo manualmente, sin usar un IDE (osea el IntelliJ)

Entras a la carpeta donde esta el archivo desde la terminal y ejecutas `javac Programa.java`

y eso te genera el archivo `Programa.class`

### 1. ¿Qué es el Programa.class?

Cuando compilás tu archivo `Programa.java` con el compilador de Java (`javac`), se genera un nuevo archivo llamado:

```Plain
Programa.class
```

### 2. ¿Qué contiene este archivo .class?

Contiene el **bytecode** de tu programa.

### 3. ¿Qué es el bytecode?

El **bytecode** es un código intermedio. No es el código fuente que escribiste en Java, ni tampoco es código máquina directo que entienda el procesador. Es un conjunto de instrucciones especiales que entiende la **Máquina Virtual de Java (JVM)**.

### 4. ¿Por qué se usa bytecode en Java?

Porque permite que tu programa sea **multiplataforma**. Es decir:

- Escribís y compilás tu código en una computadora (Windows, Linux, Mac).
- Generás el bytecode (`.class`).
- Ese bytecode se puede ejecutar en cualquier sistema operativo que tenga instalada la JVM, sin necesidad de recompilarlo.

### 5. ¿Cómo se ejecuta el bytecode?

La **JVM (Java Virtual Machine)** lee ese archivo `.class` y lo interpreta o compila just-in-time (JIT) para el sistema operativo y procesador donde esté corriendo.

---

Todo esto de arriba lo hace automáticamente el intellij Idea

  

---

![[/image 2.png|image 2.png]]

### 1. **Escribimos el código fuente en** `**HolaMundo.java**`

- Es un archivo de texto que contiene código Java.
- Lo escribís vos, con un editor o en un IDE como IntelliJ.

> Este archivo contiene la clase HolaMundo y el método main que es el punto de inicio del programa.

### 2. **Compilación con** `**javac**` **(compilador de Java)**

> Comando:

```Shell
javac HolaMundo.java
```

- El compilador verifica si hay errores de sintaxis.
- Si hay errores, te los muestra y no genera nada.
- Si está todo bien, genera un archivo:

```Plain
HolaMundo.class
```

✔️ Este `.class` **es el bytecode**, un lenguaje intermedio que no depende del sistema operativo.

Lenguaje intermedio entre el codigo en java puro y el codigo que entiende la maquina.

### 3. **¿Qué es el bytecode?**

- Es el código que **no es Java puro ni código de máquina**, sino un código intermedio.
- Es lo que **entiende la JVM (Máquina Virtual de Java)**.

> Es portable: lo podés ejecutar en cualquier sistema operativo que tenga una JVM, sin volver a compilar.

### 4. **¿Qué es la JVM? ¿Y qué es el JRE?**

### ✅ **JVM (Java Virtual Machine) —> “Podria verse como un interprete” segund el profe**

- Es el **programa que ejecuta el bytecode** (`.class`).
- Traduce el bytecode a instrucciones de tu sistema operativo y procesador **en tiempo de ejecución** (JIT - Just In Time Compiler).

> **La JVM es una parte del JRE.**

### ✅ **JRE (Java Runtime Environment)**

- Es el **entorno necesario para ejecutar** programas Java.
- Contiene:
    - La JVM (que interpreta/ejecuta el bytecode)
    - Librerías estándar de Java
    - Herramientas básicas para ejecución

> Si solo querés ejecutar un programa .class, con tener la JRE alcanza.
> 
> No necesitás el compilador `javac`.

### 5. **¿Qué es el JDK?**

> JDK = Java Development Kit

- Incluye todo lo que tiene el JRE **+ herramientas para programar y compilar**.
- Es lo que necesitás si querés **escribir y compilar programas Java**.

### 6. **Ejecución del programa**

> Comando:

```Shell
java HolaMundo
```

- Esto llama a la **JVM** incluida en la **JRE**, que:
    - Busca el archivo `HolaMundo.class`
    - Lee su bytecode
    - Lo ejecuta en tu sistema operativo

> Escribís tu programa en HolaMundo.javaLo compilás con javac → genera HolaMundo.class (bytecode)Ese .class se puede ejecutar en cualquier sistema operativo con una JRE. La JRE contiene la JVM, que es quien ejecuta el .class

> La JVM es el programa que ejecuta el bytecode. La JRE es el entorno completo que incluye la JVM y lo necesario para ejecutar programas Java. El JDK incluye todo lo del JRE más las herramientas para escribir y compilar código.

---

[[Notas clase]]