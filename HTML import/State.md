 

  [![](HTML%20import/Attachments/read.svg) ![](HTML%20import/Attachments/tea-cup.svg) Autumn SALE ![](HTML%20import/Attachments/mulled-wine.svg) ![](HTML%20import/Attachments/maple-leaf.svg)](https://refactoring.guru/es/store)  

 [](https://refactoring.guru/es/design-patterns/state#checkout)[](https://refactoring.guru/es/design-patterns/state#checkout)

[](https://refactoring.guru/)/ [Patrones de diseño](https://refactoring.guru/es/design-patterns) / [Patrones de comportamiento](https://refactoring.guru/es/design-patterns/behavioral-patterns)

# State

También llamado: Estado

## Propósito

**State** es un patrón de diseño de comportamiento que permite a un objeto alterar su comportamiento cuando su estado interno cambia. Parece como si el objeto cambiara su clase.

![Patrón de diseño State](HTML%20import/Attachments/state-es.png)

## Problema

El patrón State está estrechamente relacionado con el concepto de la _Máquina de estados finitos_ Máquina de estados finitos: [https://refactoring.guru/es/fsm](https://refactoring.guru/es/fsm).

![Máquina de estados finitos](HTML%20import/Attachments/problem1.png)

Máquina de estados finitos.

La idea principal es que, en cualquier momento dado, un programa puede encontrarse en un número _finito_ de _estados_. Dentro de cada estado único, el programa se comporta de forma diferente y puede cambiar de un estado a otro instantáneamente. Sin embargo, dependiendo de un estado actual, el programa puede cambiar o no a otros estados. Estas normas de cambio llamadas _transiciones_ también son finitas y predeterminadas.

También puedes aplicar esta solución a los objetos. Imagina que tienes una clase `Documento`. Un documento puede encontrarse en uno de estos tres estados: `Borrador`, `Moderación` y `Publicado`. El método `publicar` del documento funciona de forma ligeramente distinta en cada estado:

- En `Borrador`, mueve el documento a moderación.
- En `Moderación`, hace público el documento, pero sólo si el usuario actual es un administrador.
- En `Publicado`, no hace nada en absoluto.

![Posibles estados de un objeto de documento](HTML%20import/Attachments/problem2-es.png)

Posibles estados y transiciones de un objeto de documento.

Las máquinas de estado se implementan normalmente con muchos operadores condicionales (`if` o `switch`) que seleccionan el comportamiento adecuado dependiendo del estado actual del objeto. Normalmente, este “estado” es tan solo un grupo de valores de los campos del objeto. Aunque nunca hayas oído hablar de máquinas de estados finitos, probablemente hayas implementado un estado al menos alguna vez. ¿Te suena esta estructura de código?

class Document is
    field state: string
    // ...
    method publish() is
        switch (state)
            "draft":
                state = "moderation"
                break
            "moderation":
                if (currentUser.role == "admin")
                    state = "published"
                break
            "published":
                // No hacer nada.
                break
    // ...

La mayor debilidad de una máquina de estado basada en condicionales se revela una vez que empezamos a añadir más y más estados y comportamientos dependientes de estados a la clase `Documento`. La mayoría de los métodos contendrán condicionales monstruosos que eligen el comportamiento adecuado de un método de acuerdo con el estado actual. Un código así es muy difícil de mantener, porque cualquier cambio en la lógica de transición puede requerir cambiar los condicionales de estado de cada método.

El problema tiende a empeorar con la evolución del proyecto. Es bastante difícil predecir todos los estados y transiciones posibles en la etapa de diseño. Por ello, una máquina de estados esbelta, creada con un grupo limitado de condicionales, puede crecer hasta convertirse en un abotargado desastre con el tiempo.

## Solución

El patrón State sugiere que crees nuevas clases para todos los estados posibles de un objeto y extraigas todos los comportamientos específicos del estado para colocarlos dentro de esas clases.

En lugar de implementar todos los comportamientos por su cuenta, el objeto original, llamado _contexto_, almacena una referencia a uno de los objetos de estado que representa su estado actual y delega todo el trabajo relacionado con el estado a ese objeto.

![Documento delega el trabajo a un objeto de estado](HTML%20import/Attachments/solution-es.png)

Documento delega el trabajo a un objeto de estado.

Para la transición del contexto a otro estado, sustituye el objeto de estado activo por otro objeto que represente ese nuevo estado. Esto sólo es posible si todas las clases de estado siguen la misma interfaz y el propio contexto funciona con esos objetos a través de esa interfaz.

Esta estructura puede resultar similar al patrón [Strategy](https://refactoring.guru/es/design-patterns/strategy), pero hay una diferencia clave. En el patrón State, los estados particulares pueden conocerse entre sí e iniciar transiciones de un estado a otro, mientras que las estrategias casi nunca se conocen.

## Analogía en el mundo real

Los botones e interruptores de tu smartphone se comportan de forma diferente dependiendo del estado actual del dispositivo:

- Cuando el teléfono está desbloqueado, al pulsar botones se ejecutan varias funciones.
- Cuando el teléfono está bloqueado, pulsar un botón desbloquea la pantalla.
- Cuando la batería del teléfono está baja, pulsar un botón muestra la pantalla de carga.

## Estructura

![Estructura del patrón de diseño State](HTML%20import/Attachments/structure-es.png)![Estructura del patrón de diseño State](HTML%20import/Attachments/structure-es-indexed.png)

1. La clase **Contexto** almacena una referencia a uno de los objetos de estado concreto y le delega todo el trabajo específico del estado. El contexto se comunica con el objeto de estado a través de la interfaz de estado. El contexto expone un modificador (_setter_) para pasarle un nuevo objeto de estado.
    
2. La interfaz **Estado** declara los métodos específicos del estado. Estos métodos deben tener sentido para todos los estados concretos, porque no querrás que uno de tus estados tenga métodos inútiles que nunca son invocados.
    
3. Los **Estados Concretos** proporcionan sus propias implementaciones para los métodos específicos del estado. Para evitar la duplicación de código similar a través de varios estados, puedes incluir clases abstractas intermedias que encapsulen algún comportamiento común.
    
    Los objetos de estado pueden almacenar una referencia inversa al objeto de contexto. A través de esta referencia, el estado puede extraer cualquier información requerida del objeto de contexto, así como iniciar transiciones de estado.
    
4. Tanto el estado de contexto como el concreto pueden establecer el nuevo estado del contexto y realizar la transición de estado sustituyendo el objeto de estado vinculado al contexto.
    

## Pseudocódigo

En este ejemplo, el patrón **State** permite a los mismos controles del reproductor de medios comportarse de forma diferente, dependiendo del estado actual de reproducción.

![Ejemplo de estructura del patrón State](HTML%20import/Attachments/example.png)

Ejemplo de cambio del comportamiento de un objeto con objetos de estado.

El objeto principal del reproductor siempre está vinculado a un objeto de estado que realiza la mayor parte del trabajo del reproductor. Algunas acciones sustituyen el objeto de estado actual del reproductor por otro, lo cual cambia la forma en la que el reproductor reacciona a las interacciones del usuario.

// La clase ReproductordeAudio actúa como un contexto. También
// mantiene una referencia a una instancia de una de las clases
// estado que representa el estado actual del reproductor de
// audio.
class AudioPlayer is
    field state: State
    field UI, volume, playlist, currentSong

    constructor AudioPlayer() is
        this.state = new ReadyState(this)

        // El contexto delega la gestión de entradas del usuario
        // a un objeto de estado. Naturalmente, el resultado
        // depende del estado que esté activo ahora, ya que cada
        // estado puede gestionar las entradas de manera
        // diferente.
        UI = new UserInterface()
        UI.lockButton.onClick(this.clickLock)
        UI.playButton.onClick(this.clickPlay)
        UI.nextButton.onClick(this.clickNext)
        UI.prevButton.onClick(this.clickPrevious)

    // Otros objetos deben ser capaces de cambiar el estado
    // activo del reproductor.
    method changeState(state: State) is
        this.state = state

    // Los métodos UI delegan la ejecución al estado activo.
    method clickLock() is
        state.clickLock()
    method clickPlay() is
        state.clickPlay()
    method clickNext() is
        state.clickNext()
    method clickPrevious() is
        state.clickPrevious()

    // Un estado puede invocar algunos métodos del servicio en
    // el contexto.
    method startPlayback() is
        // ...
    method stopPlayback() is
        // ...
    method nextSong() is
        // ...
    method previousSong() is
        // ...
    method fastForward(time) is
        // ...
    method rewind(time) is
        // ...

// La clase estado base declara métodos que todos los estados
// concretos deben implementar, y también proporciona una
// referencia inversa al objeto de contexto asociado con el
// estado. Los estados pueden utilizar la referencia inversa
// para dirigir el contexto a otro estado.
abstract class State is
    protected field player: AudioPlayer

    // El contexto se pasa a sí mismo a través del constructor
    // del estado. Esto puede ayudar al estado a extraer
    // información de contexto útil si la necesita.
    constructor State(player) is
        this.player = player

    abstract method clickLock()
    abstract method clickPlay()
    abstract method clickNext()
    abstract method clickPrevious()

// Los estados concretos implementan varios comportamientos
// asociados a un estado del contexto.
class LockedState extends State is

    // Cuando desbloqueas a un jugador bloqueado, puede asumir
    // uno de dos estados.
    method clickLock() is
        if (player.playing)
            player.changeState(new PlayingState(player))
        else
            player.changeState(new ReadyState(player))

    method clickPlay() is
        // Bloqueado, no hace nada.

    method clickNext() is
        // Bloqueado, no hace nada.

    method clickPrevious() is
        // Bloqueado, no hace nada.

// También pueden disparar transiciones de estado en el
// contexto.
class ReadyState extends State is
    method clickLock() is
        player.changeState(new LockedState(player))

    method clickPlay() is
        player.startPlayback()
        player.changeState(new PlayingState(player))

    method clickNext() is
        player.nextSong()

    method clickPrevious() is
        player.previousSong()

class PlayingState extends State is
    method clickLock() is
        player.changeState(new LockedState(player))

    method clickPlay() is
        player.stopPlayback()
        player.changeState(new ReadyState(player))

    method clickNext() is
        if (event.doubleclick)
            player.nextSong()
        else
            player.fastForward(5)

    method clickPrevious() is
        if (event.doubleclick)
            player.previous()
        else
            player.rewind(5)

## Aplicabilidad

Utiliza el patrón State cuando tengas un objeto que se comporta de forma diferente dependiendo de su estado actual, el número de estados sea enorme y el código específico del estado cambie con frecuencia.

El patrón sugiere que extraigas todo el código específico del estado y lo metas dentro de un grupo de clases específicas. Como resultado, puedes añadir nuevos estados o cambiar los existentes independientemente entre sí, reduciendo el costo de mantenimiento.

Utiliza el patrón cuando tengas una clase contaminada con enormes condicionales que alteran el modo en que se comporta la clase de acuerdo con los valores actuales de los campos de la clase.

El patrón State te permite extraer ramas de esos condicionales a métodos de las clases estado correspondientes. Al hacerlo, también puedes limpiar campos temporales y métodos de ayuda implicados en código específico del estado de fuera de tu clase principal.

Utiliza el patrón State cuando tengas mucho código duplicado por estados similares y transiciones de una máquina de estados basada en condiciones.

El patrón State te permite componer jerarquías de clases de estado y reducir la duplicación, extrayendo el código común y metiéndolo en clases abstractas base.

## Cómo implementarlo

1. Decide qué clase actuará como contexto. Puede ser una clase existente que ya tiene el código dependiente del estado, o una nueva clase, si el código específico del estado está distribuido a lo largo de varias clases.
    
2. Declara la interfaz de estado. Aunque puede replicar todos los métodos declarados en el contexto, céntrate en los que pueden contener comportamientos específicos del estado.
    
3. Para cada estado actual, crea una clase derivada de la interfaz de estado. Después repasa los métodos del contexto y extrae todo el código relacionado con ese estado para meterlo en tu clase recién creada.
    
    Al mover el código a la clase estado, puede que descubras que depende de miembros privados del contexto. Hay varias soluciones alternativas:
    
    - Haz públicos esos campos o métodos.
    - Convierte el comportamiento que estás extrayendo para ponerlo en un método público en el contexto e invócalo desde la clase de estado. Esta forma es desagradable pero rápida y siempre podrás arreglarlo más adelante.
    - Anida las clases de estado en la clase contexto, pero sólo si tu lenguaje de programación soporta clases anidadas.
4. En la clase contexto, añade un campo de referencia del tipo de interfaz de estado y un modificador (_setter_) público que permita sobrescribir el valor de ese campo.
    
5. Vuelve a repasar el método del contexto y sustituye los condicionales de estado vacíos por llamadas a métodos correspondientes del objeto de estado.
    
6. Para cambiar el estado del contexto, crea una instancia de una de las clases de estado y pásala a la clase contexto. Puedes hacer esto dentro de la propia clase contexto, en distintos estados, o en el cliente. Se haga de una forma u otra, la clase se vuelve dependiente de la clase de estado concreto que instancia.
    

## Pros y contras

- _Principio de responsabilidad única_. Organiza el código relacionado con estados particulares en clases separadas.
- _Principio de abierto/cerrado_. Introduce nuevos estados sin cambiar clases de estado existentes o la clase contexto.
- Simplifica el código del contexto eliminando voluminosos condicionales de máquina de estados.

- Aplicar el patrón puede resultar excesivo si una máquina de estados sólo tiene unos pocos estados o raramente cambia.

## Relaciones con otros patrones

- [Bridge](https://refactoring.guru/es/design-patterns/bridge), [State](https://refactoring.guru/es/design-patterns/state), [Strategy](https://refactoring.guru/es/design-patterns/strategy) (y, hasta cierto punto, [Adapter](https://refactoring.guru/es/design-patterns/adapter)) tienen estructuras muy similares. De hecho, todos estos patrones se basan en la composición, que consiste en delegar trabajo a otros objetos. Sin embargo, todos ellos solucionan problemas diferentes. Un patrón no es simplemente una receta para estructurar tu código de una forma específica. También puede comunicar a otros desarrolladores el problema que resuelve.
    
- [State](https://refactoring.guru/es/design-patterns/state) puede considerarse una extensión de [Strategy](https://refactoring.guru/es/design-patterns/strategy). Ambos patrones se basan en la composición: cambian el comportamiento del contexto delegando parte del trabajo a objetos ayudantes. _Strategy_ hace que estos objetos sean completamente independientes y no se conozcan entre sí. Sin embargo, _State_ no restringe las dependencias entre estados concretos, permitiéndoles alterar el estado del contexto a voluntad.
    

## Ejemplos de código

[![State en C#](HTML%20import/Attachments/csharp.svg)](https://refactoring.guru/es/design-patterns/state/csharp/example "State en C#") [![State en C++](HTML%20import/Attachments/cpp.svg)](https://refactoring.guru/es/design-patterns/state/cpp/example "State en C++") [![State en Go](HTML%20import/Attachments/go.svg)](https://refactoring.guru/es/design-patterns/state/go/example "State en Go") [![State en Java](HTML%20import/Attachments/java.svg)](https://refactoring.guru/es/design-patterns/state/java/example "State en Java") [![State en PHP](HTML%20import/Attachments/php.svg)](https://refactoring.guru/es/design-patterns/state/php/example "State en PHP") [![State en Python](HTML%20import/Attachments/python.svg)](https://refactoring.guru/es/design-patterns/state/python/example "State en Python") [![State en Ruby](HTML%20import/Attachments/ruby.svg)](https://refactoring.guru/es/design-patterns/state/ruby/example "State en Ruby") [![State en Rust](HTML%20import/Attachments/rust.svg)](https://refactoring.guru/es/design-patterns/state/rust/example "State en Rust") [![State en Swift](HTML%20import/Attachments/swift.svg)](https://refactoring.guru/es/design-patterns/state/swift/example "State en Swift") [![State en TypeScript](HTML%20import/Attachments/typescript.svg)](https://refactoring.guru/es/design-patterns/state/typescript/example "State en TypeScript")

[![](HTML%20import/Attachments/patterns-book-banner-3.png)](https://refactoring.guru/es/design-patterns/book)

### ¡Apoya nuestro sitio web gratuito y compra el libro!

- 22 patrones de diseño y 8 principios explicados en profundidad
- 436 páginas bien estructuradas, fáciles de leer y libres de tecnicismos
- 225 ilustraciones y diagramas claros y útiles
- Un archivo con ejemplos de código en 11 lenguajes
- Todos los dispositivos soportados: Formatos PDF/EPUB/MOBI/KFX

[Saber más…](https://refactoring.guru/es/design-patterns/book)

#### Leer siguiente

[Strategy](https://refactoring.guru/es/design-patterns/strategy) 

#### Volver

 [Observer](https://refactoring.guru/es/design-patterns/observer)

[![](HTML%20import/Attachments/web-cover-es.png)](https://refactoring.guru/es/design-patterns/book)

Este artículo es parte de nuestro libro eléctronico **Sumérgete en los patrones de diseño**.

[Saber más…](https://refactoring.guru/es/design-patterns/book)

[![Refactoring.Guru](HTML%20import/Attachments/logo-new.png)](https://refactoring.guru/es)

- [Contenido premium](https://refactoring.guru/es/store)
    - [Sumérgete en los  
        patrones de diseño](https://refactoring.guru/es/design-patterns/book)
    - [Refactoring Course](https://refactoring.guru/es/refactoring/course)
- [Patrones de diseño](https://refactoring.guru/es/design-patterns)
    - [Introducción](https://refactoring.guru/es/design-patterns/what-is-pattern)
        - [¿Qué es un patrón de diseño?](https://refactoring.guru/es/design-patterns/what-is-pattern)
        - [Historia de los patrones](https://refactoring.guru/es/design-patterns/history)
        - [¿Por qué debería aprender sobre patrones?](https://refactoring.guru/es/design-patterns/why-learn-patterns)
        - [Crítica de los patrones](https://refactoring.guru/es/design-patterns/criticism)
        - [Clasificación de los patrones](https://refactoring.guru/es/design-patterns/classification)
    - [Catálogo](https://refactoring.guru/es/design-patterns/catalog)
    - [Patrones creacionales](https://refactoring.guru/es/design-patterns/creational-patterns)
        - [Factory Method](https://refactoring.guru/es/design-patterns/factory-method)
        - [Abstract Factory](https://refactoring.guru/es/design-patterns/abstract-factory)
        - [Builder](https://refactoring.guru/es/design-patterns/builder)
        - [Prototype](https://refactoring.guru/es/design-patterns/prototype)
        - [Singleton](https://refactoring.guru/es/design-patterns/singleton)
    - [Patrones estructurales](https://refactoring.guru/es/design-patterns/structural-patterns)
        - [Adapter](https://refactoring.guru/es/design-patterns/adapter)
        - [Bridge](https://refactoring.guru/es/design-patterns/bridge)
        - [Composite](https://refactoring.guru/es/design-patterns/composite)
        - [Decorator](https://refactoring.guru/es/design-patterns/decorator)
        - [Facade](https://refactoring.guru/es/design-patterns/facade)
        - [Flyweight](https://refactoring.guru/es/design-patterns/flyweight)
        - [Proxy](https://refactoring.guru/es/design-patterns/proxy)
    - [Patrones de comportamiento](https://refactoring.guru/es/design-patterns/behavioral-patterns)
        - [Chain of Responsibility](https://refactoring.guru/es/design-patterns/chain-of-responsibility)
        - [Command](https://refactoring.guru/es/design-patterns/command)
        - [Iterator](https://refactoring.guru/es/design-patterns/iterator)
        - [Mediator](https://refactoring.guru/es/design-patterns/mediator)
        - [Memento](https://refactoring.guru/es/design-patterns/memento)
        - [Observer](https://refactoring.guru/es/design-patterns/observer)
        - [State](https://refactoring.guru/es/design-patterns/state)
        - [Strategy](https://refactoring.guru/es/design-patterns/strategy)
        - [Template Method](https://refactoring.guru/es/design-patterns/template-method)
        - [Visitor](https://refactoring.guru/es/design-patterns/visitor)
    - [Ejemplos de código](https://refactoring.guru/es/design-patterns/examples)
        - [C#](https://refactoring.guru/es/design-patterns/csharp)
        - [C++](https://refactoring.guru/es/design-patterns/cpp)
        - [Go](https://refactoring.guru/es/design-patterns/go)
        - [Java](https://refactoring.guru/es/design-patterns/java)
        - [PHP](https://refactoring.guru/es/design-patterns/php)
        - [Python](https://refactoring.guru/es/design-patterns/python)
        - [Ruby](https://refactoring.guru/es/design-patterns/ruby)
        - [Rust](https://refactoring.guru/es/design-patterns/rust)
        - [Swift](https://refactoring.guru/es/design-patterns/swift)
        - [TypeScript](https://refactoring.guru/es/design-patterns/typescript)
- [Refactorización (próximamente)](https://refactoring.guru/es/refactoring)
    - [What is Refactoring](https://refactoring.guru/es/refactoring/what-is-refactoring)
        - [Clean code](https://refactoring.guru/es/refactoring/what-is-refactoring)
        - [Technical debt](https://refactoring.guru/es/refactoring/technical-debt)
        - [When to refactor](https://refactoring.guru/es/refactoring/when)
        - [How to refactor](https://refactoring.guru/es/refactoring/how-to)
    - [Catalog](https://refactoring.guru/es/refactoring/catalog)
    - [Code Smells](https://refactoring.guru/es/refactoring/smells)
        - [Bloaters](https://refactoring.guru/es/refactoring/smells/bloaters)
            - [Long Method](https://refactoring.guru/es/smells/long-method)
            - [Large Class](https://refactoring.guru/es/smells/large-class)
            - [Primitive Obsession](https://refactoring.guru/es/smells/primitive-obsession)
            - [Long Parameter List](https://refactoring.guru/es/smells/long-parameter-list)
            - [Data Clumps](https://refactoring.guru/es/smells/data-clumps)
        - [Object-Orientation Abusers](https://refactoring.guru/es/refactoring/smells/oo-abusers)
            - [Switch Statements](https://refactoring.guru/es/smells/switch-statements)
            - [Temporary Field](https://refactoring.guru/es/smells/temporary-field)
            - [Refused Bequest](https://refactoring.guru/es/smells/refused-bequest)
            - [Alternative Classes with Different Interfaces](https://refactoring.guru/es/smells/alternative-classes-with-different-interfaces)
        - [Change Preventers](https://refactoring.guru/es/refactoring/smells/change-preventers)
            - [Divergent Change](https://refactoring.guru/es/smells/divergent-change)
            - [Shotgun Surgery](https://refactoring.guru/es/smells/shotgun-surgery)
            - [Parallel Inheritance Hierarchies](https://refactoring.guru/es/smells/parallel-inheritance-hierarchies)
        - [Dispensables](https://refactoring.guru/es/refactoring/smells/dispensables)
            - [Comments](https://refactoring.guru/es/smells/comments)
            - [Duplicate Code](https://refactoring.guru/es/smells/duplicate-code)
            - [Lazy Class](https://refactoring.guru/es/smells/lazy-class)
            - [Data Class](https://refactoring.guru/es/smells/data-class)
            - [Dead Code](https://refactoring.guru/es/smells/dead-code)
            - [Speculative Generality](https://refactoring.guru/es/smells/speculative-generality)
        - [Couplers](https://refactoring.guru/es/refactoring/smells/couplers)
            - [Feature Envy](https://refactoring.guru/es/smells/feature-envy)
            - [Inappropriate Intimacy](https://refactoring.guru/es/smells/inappropriate-intimacy)
            - [Message Chains](https://refactoring.guru/es/smells/message-chains)
            - [Middle Man](https://refactoring.guru/es/smells/middle-man)
        - [Other Smells](https://refactoring.guru/es/refactoring/smells/other)
            - [Incomplete Library Class](https://refactoring.guru/es/smells/incomplete-library-class)
    - [Refactorings](https://refactoring.guru/es/refactoring/techniques)
        - [Composing Methods](https://refactoring.guru/es/refactoring/techniques/composing-methods)
            - [Extract Method](https://refactoring.guru/es/extract-method)
            - [Inline Method](https://refactoring.guru/es/inline-method)
            - [Extract Variable](https://refactoring.guru/es/extract-variable)
            - [Inline Temp](https://refactoring.guru/es/inline-temp)
            - [Replace Temp with Query](https://refactoring.guru/es/replace-temp-with-query)
            - [Split Temporary Variable](https://refactoring.guru/es/split-temporary-variable)
            - [Remove Assignments to Parameters](https://refactoring.guru/es/remove-assignments-to-parameters)
            - [Replace Method with Method Object](https://refactoring.guru/es/replace-method-with-method-object)
            - [Substitute Algorithm](https://refactoring.guru/es/substitute-algorithm)
        - [Moving Features between Objects](https://refactoring.guru/es/refactoring/techniques/moving-features-between-objects)
            - [Move Method](https://refactoring.guru/es/move-method)
            - [Move Field](https://refactoring.guru/es/move-field)
            - [Extract Class](https://refactoring.guru/es/extract-class)
            - [Inline Class](https://refactoring.guru/es/inline-class)
            - [Hide Delegate](https://refactoring.guru/es/hide-delegate)
            - [Remove Middle Man](https://refactoring.guru/es/remove-middle-man)
            - [Introduce Foreign Method](https://refactoring.guru/es/introduce-foreign-method)
            - [Introduce Local Extension](https://refactoring.guru/es/introduce-local-extension)
        - [Organizing Data](https://refactoring.guru/es/refactoring/techniques/organizing-data)
            - [Self Encapsulate Field](https://refactoring.guru/es/self-encapsulate-field)
            - [Replace Data Value with Object](https://refactoring.guru/es/replace-data-value-with-object)
            - [Change Value to Reference](https://refactoring.guru/es/change-value-to-reference)
            - [Change Reference to Value](https://refactoring.guru/es/change-reference-to-value)
            - [Replace Array with Object](https://refactoring.guru/es/replace-array-with-object)
            - [Duplicate Observed Data](https://refactoring.guru/es/duplicate-observed-data)
            - [Change Unidirectional Association to Bidirectional](https://refactoring.guru/es/change-unidirectional-association-to-bidirectional)
            - [Change Bidirectional Association to Unidirectional](https://refactoring.guru/es/change-bidirectional-association-to-unidirectional)
            - [Replace Magic Number with Symbolic Constant](https://refactoring.guru/es/replace-magic-number-with-symbolic-constant)
            - [Encapsulate Field](https://refactoring.guru/es/encapsulate-field)
            - [Encapsulate Collection](https://refactoring.guru/es/encapsulate-collection)
            - [Replace Type Code with Class](https://refactoring.guru/es/replace-type-code-with-class)
            - [Replace Type Code with Subclasses](https://refactoring.guru/es/replace-type-code-with-subclasses)
            - [Replace Type Code with State/Strategy](https://refactoring.guru/es/replace-type-code-with-state-strategy)
            - [Replace Subclass with Fields](https://refactoring.guru/es/replace-subclass-with-fields)
        - [Simplifying Conditional Expressions](https://refactoring.guru/es/refactoring/techniques/simplifying-conditional-expressions)
            - [Decompose Conditional](https://refactoring.guru/es/decompose-conditional)
            - [Consolidate Conditional Expression](https://refactoring.guru/es/consolidate-conditional-expression)
            - [Consolidate Duplicate Conditional Fragments](https://refactoring.guru/es/consolidate-duplicate-conditional-fragments)
            - [Remove Control Flag](https://refactoring.guru/es/remove-control-flag)
            - [Replace Nested Conditional with Guard Clauses](https://refactoring.guru/es/replace-nested-conditional-with-guard-clauses)
            - [Replace Conditional with Polymorphism](https://refactoring.guru/es/replace-conditional-with-polymorphism)
            - [Introduce Null Object](https://refactoring.guru/es/introduce-null-object)
            - [Introduce Assertion](https://refactoring.guru/es/introduce-assertion)
        - [Simplifying Method Calls](https://refactoring.guru/es/refactoring/techniques/simplifying-method-calls)
            - [Rename Method](https://refactoring.guru/es/rename-method)
            - [Add Parameter](https://refactoring.guru/es/add-parameter)
            - [Remove Parameter](https://refactoring.guru/es/remove-parameter)
            - [Separate Query from Modifier](https://refactoring.guru/es/separate-query-from-modifier)
            - [Parameterize Method](https://refactoring.guru/es/parameterize-method)
            - [Replace Parameter with Explicit Methods](https://refactoring.guru/es/replace-parameter-with-explicit-methods)
            - [Preserve Whole Object](https://refactoring.guru/es/preserve-whole-object)
            - [Replace Parameter with Method Call](https://refactoring.guru/es/replace-parameter-with-method-call)
            - [Introduce Parameter Object](https://refactoring.guru/es/introduce-parameter-object)
            - [Remove Setting Method](https://refactoring.guru/es/remove-setting-method)
            - [Hide Method](https://refactoring.guru/es/hide-method)
            - [Replace Constructor with Factory Method](https://refactoring.guru/es/replace-constructor-with-factory-method)
            - [Replace Error Code with Exception](https://refactoring.guru/es/replace-error-code-with-exception)
            - [Replace Exception with Test](https://refactoring.guru/es/replace-exception-with-test)
        - [Dealing with Generalization](https://refactoring.guru/es/refactoring/techniques/dealing-with-generalization)
            - [Pull Up Field](https://refactoring.guru/es/pull-up-field)
            - [Pull Up Method](https://refactoring.guru/es/pull-up-method)
            - [Pull Up Constructor Body](https://refactoring.guru/es/pull-up-constructor-body)
            - [Push Down Method](https://refactoring.guru/es/push-down-method)
            - [Push Down Field](https://refactoring.guru/es/push-down-field)
            - [Extract Subclass](https://refactoring.guru/es/extract-subclass)
            - [Extract Superclass](https://refactoring.guru/es/extract-superclass)
            - [Extract Interface](https://refactoring.guru/es/extract-interface)
            - [Collapse Hierarchy](https://refactoring.guru/es/collapse-hierarchy)
            - [Form Template Method](https://refactoring.guru/es/form-template-method)
            - [Replace Inheritance with Delegation](https://refactoring.guru/es/replace-inheritance-with-delegation)
            - [Replace Delegation with Inheritance](https://refactoring.guru/es/replace-delegation-with-inheritance)

[Iniciar sesión](https://refactoring.guru/es/login "Iniciar sesión") [Contáctanos](https://feedback.refactoring.guru/?lang=en "Contáctanos")

[![Refactoring.Guru](./State_files/logo-new-mobile.png)](https://refactoring.guru/es)

[Mi tienda](https://refactoring.guru/es/store)

- Español
    
    [English](https://refactoring.guru/design-patterns/state "English") [Español](https://refactoring.guru/es/design-patterns/state "Español") [Français](https://refactoring.guru/fr/design-patterns/state "Français") [日本語](https://refactoring.guru/ja/design-patterns/state "日本語") [한국어](https://refactoring.guru/ko/design-patterns/state "한국어") [Polski](https://refactoring.guru/pl/design-patterns/state "Polski") [Português Brasileiro](https://refactoring.guru/pt-br/design-patterns/state "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/state "Русский") [Українська](https://refactoring.guru/uk/design-patterns/state "Українська") [中文](https://refactoringguru.cn/design-patterns/state "中文")
    
- [Contáctanos](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Contáctanos")
- [Iniciar sesión](https://refactoring.guru/es/login "Iniciar sesión")

- [Inicio](https://refactoring.guru/es)
- [Refactorización](https://refactoring.guru/es/refactoring)
- [Patrones de diseño](https://refactoring.guru/es/design-patterns)
- [Contenido Premium](https://refactoring.guru/es/store)
- [Curso de Git](https://gitbybit.com/)
- [Foro](https://feedback.refactoring.guru/?lang=en)
- [Contáctanos](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true)

- [](https://www.facebook.com/refactoring.guru)
- [](https://refactoring.guru/es/sendy/form)
- [](https://github.com/RefactoringGuru)

2014-2025 [Refactoring.Guru](https://refactoring.guru/es). Todos los derechos reservados.  
Ilustraciones por [Dmitry Zhart](http://zhart.us/)

- [Términos y condiciones](https://refactoring.guru/es/terms)
- [Política de privacidad](https://refactoring.guru/es/privacy-policy)
- [Política de uso de contenido](https://refactoring.guru/es/content-usage-policy)
- [About us](https://refactoring.guru/es/site-about)

**Ukrainian office:**  
 ![Organization address](HTML%20import/Attachments/fa-building.svg)FOP Olga Skobeleva  
Abolmasova 7  
Kyiv, Ukraine, 02002  
Email: support@refactoring.guru

**Spanish office:**  
 ![Organization address](HTML%20import/Attachments/fa-building.svg)Oleksandr Shvets  
Avda Pamplona 64  
Pamplona, Spain, 31009  
Email: support@refactoring.guru