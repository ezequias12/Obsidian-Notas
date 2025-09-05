## Parcial 

Tenemos que saber por ejemplo si nos preguntan que es la arquitectura de software, saber de donde viene  
No van a ser preguntas literales de diseño pero si tenemos que tener idea

Saber masomenos que es el diseño arquitectonico (ver cuadro U2)

Contexto general de U2 (Cuadro redondo inicial de datosm proc manuales, formas de E/S, interaccion H-M, procesos) 

para poder definir la unidad 3

### Diseño Arquitectonico

- Que es, porque → Documentacion de arquitectura
- Rol/quienes
- Como/Proc

  

## Proceso de diseño de arquitectura de software → Importante

### Determinar los requerimientos arquitectonicos

- identificar los requerimientos que me van a servir para la arquitectura
- requerimientos no funcionales que vienen del mdelo de req, y determinosi me van a servir para la arquitectura, para tomar desiciones arquitectonicas
- de los casos de uso identifico tambien, cuales son significativos para la arquitectura
- identifico cual es el impacto, viendo la proridad de cada requerimiento, prioridad alta, media o baja → que tanto afecta a las desiciones arquitectonicas
- Para establecer las funcionalidades (caso de uso) sobre las q voy a tomar decisiones arquitectónicas, un punto a considerar es a que funcionalidades afecta cada uno de los req no funcionales.

### Ejemplos muy especificos y propios → ejemplo que muestre el primer paso del diseño arquitectonico

Crear una pagina web, viendo los req no funcionales digamos. que la pagina sea responsive, que se vea en web y mobile, etc

Describir la empresa y el problema que tiene

- Despues decimos, analizando encontramos que el req no funcional es → crear una app web responsive
- El diseño arquitectonico sera, crear una estructura con tal condicion. Osea la solucion de diseño

Describimos el negocio, identificamos el req no funciona, vemos la desicion arquitectonica que sera la solucion → la prioridad que tendra

y luego se construyen las vistas

—> si no aclara explicitamente, esta bien con detallar un solo req no funcional (supongo que uno grande e importante, de prioridad alta)

  

### En el diseño arquitectonico

Identifico las Vistas, el documento de la arquitectura → donde la principal es la vista de caso de uso

  

### Patron vs Estilo arquitectonico → Depende del autor

- Estilo arquitectonico → ej, Hexagonal o Layered es sobre todo el software
- Patrones → un patron de microservicios es algo particular, desicion particular

  

### Preguntas generales que nos puede hacer

- Explicar toda la parte de diseño
- nos va a dar un diagrama completo capaz y nos pide explicar
- relacionar todo, de donde viene cada parte y a donde lleva
- Parte de CENTRADO EN LA ARQUITECTURA importante
- ejemplos de todo, chicos
- Ejemplos donde tengamos q armar 3 casos de uso cosas asi seguro
- Explicar todo el proceso
- Porque necesito validar la arquitectura? → corroborar que todo funcione antes de hacer el sistema, puedo tener que cancelar el proyecto por costos
- Cuando debo tomar desiciones para definir la arquitectura → CONCEPTO DE CONFLICTOS ARQUITECTONICOS (Pagina 109 resumen)

Requerimientos que se contradicen, algo asi. Debo hacer un analisis para ver soluciones que satisfagan ambos requisitos sin tener consecuencias

Entender la clasificacion de los requerimientos y como se resuelve el conflicto

Ej → escalabilidad y seguridad, Que es mas importante? depende en que contexto este y que me conviene mas —— Pag 109 resumen mas detallado

  

### Vistas Arquitectonicas

- saber que diagrama se usa en cada vista

  

### Validar la Arquitectura

Escenarios y prototipos

- Escenario es plantear situaciones hipoteticas y ver si estan cubiertos, analizar si no impacta en otros req funcionales, ver que todo coordine

es mas barato el escenario

- Prototipo, es costruir un pedazo de software pequeño para hacer pruebas especificas y pequeñas

  

## Elegir Framework de Arquitectura → Distribuir componentes

- Documentar la arquitectura
- Vistas

  

### Arquitectura de aplicacion

- Monolitica
- Microservicios

  

### Estilos y/o Patrones (embebidos vs platonicos)

- Spageti
- Layered
    - Particion tecnica
    - Particion dominios
- Hexagonal
- -
- onion
- Cliente/servidor
    - 2 capas
    - 3 capas
    - n - capas
    - master - slave
    - p2p
        - puros
        - hibridos
- Para aplucaciones web
    - mcv
    - mvp
    - mvm
    - Front controller
- SDA
- EDA
    - topologia broker
        - publisher and suscribe
        - broker
        - messagin
    - topologia mediator
        - Process cordinator

  

### Patrones que vemos

- De descomposicion
    - Descomoposicion por subdominios
- De datos
    - 1 bd por servicio
- De infraestructura de aplicacion
    - API gateway
    - Backend for frontend
- De infraestructura
    - Single service per host
    - Multiple sevice per host
    
---

hacer un modelo para los ejemplos
estilo arquitectónico hexagonal 
estilo arquitectónico layered
Arq de microservicios 
arq monolitica
proceso de diseño arquitectonico 
modelo 4+1 de vistas arq 
rol de arquitecto y roles asociados 
doc de arquitectura
patrones de microservicios 
arq dirigidas x eventos
arq distribuidas
arq orientada a servicios va pero no lo dio 
arq c/s 
patrones de arq monoliticas
diferencia entre patrón y estilo no va a preguntar, a lo sumo platónicos vs embebidos. si toca pregunta a qué son los patrones hablar eso, y que hable de estilo está bien
explique el ejemplo de arquitectura hexagonal, dibujar hexagonal y explicar todo
no espera que nos sepamos bien de memoria los 5 patrones (broker y ellos)
dar ejemplo y también tenemos que escribir
backend x frontend, la diferencia es la independencia en si que se les da

---

## Preguntas

ESTILO EN ARQUITECTURA DE CAPAS (LAYERED)


1)GRUPO A: Que es un estilo arquitectonico? Diferencia entre patron y estilo arquitectonico.Nombre la etapa de proceso del diseño arquitectonico en donde se llava a cabo la decision de de dicho framework. Explique el estilo hexagonal con un ejemplo propio.

2)GRUPO B: Explique que es y como se estructura el estilo arquitectónico Layered. Nombre 3 ventajas y desventajas. De un ejemplo de un sistema que tenga como RNF crítico la seguridad.

3)GRUPO C:

 4)GRUPO D: Defina la arquitectura monolítica y mencione dos de sus características principales. Describa un escenario donde la adopción de esta arquitectura sería la solución más adecuada explicando el porqué de esta elección basándose en las ventajas que ofrece.

La arquitectura monolitica es un estilo arquitectonico de software donde toda la aplicacion se empaqueta y despliega como una unica unidad. Puede estar organizada en capas internas pero al momento del despliegue es un solo artefacto indivisible. 
Como carateristicas principales podemos mencionar: 
Unidad de despliegue: Todas las funcionalidades estan contenindas en un unico procesos/servidor. No cuenta con dependencias externas.
Simplicidad inicial: Resulta fácil de desarrollar probar y poner en marcha, ya que no requiere coordinar multiples servicios ni infraestructuras distribuidas.

Por ejemplo, se quiere desarrollar un software para un almacén o maxikiosco. Podemos usar una arquitectura monolítica cuando recién comenzamos un nuevo proyecto de software y no queremos complejizar esta. Utilizar monolítico es una buena opción para comenzar un software, para luego ir pensando en otros estilos mas sofisticados. 


5)GRUPO E: Teniendo en cuenta el proceso de diseño arquitectonico. Por que se dice que es un proceso instinsecomente iterativo? Desarrolle cada paso del mismo, teniendo en cuenta entradas y salida del proceso, y diagramas UML 2.0 que se involucran en el mismo

6)GRUPO F: Explique el modelo 4+1, desarrollando las relaciones que existen entre cada una de las vistas. De un ejemplo en el cual un sistema no se encuentren presentes todas las vistas juntas.

7)GRUPO H: Por que es importante el documento de arquitectura? Que deberia incluir para cumplir con su objetivo? Planteé un dominio en donde se vea reflejado la diferencia entre un producto documentado y uno no documentado.

8)Grupo I:
9)GRUPO J:

10)GRUPO K:

---
Genial, Ezequias 🙌. Te armo todas las respuestas organizadas grupo por grupo. Voy a usar lo que vimos en tus apuntes y resúmenes para que quede con el **tono UTN FRC** y preparado para el parcial.

---

# 📌 Respuestas a las preguntas de Diseño Arquitectónico

---

## **1) GRUPO A**

### a) ¿Qué es un estilo arquitectónico?

- Es una **forma general de organizar un sistema** de software, un conjunto de principios que guían la división en componentes y su interacción.
    
- Ejemplo de estilos: **en capas (Layered)**, **cliente-servidor**, **hexagonal**, **microservicios**, **P2P**.
    

### b) Diferencia entre patrón y estilo

- **Estilo**: define la **estructura global** del sistema (macro nivel). Ejemplo: “en capas”.
    
- **Patrón**: es una **solución recurrente y particular** a un problema específico dentro de un estilo. Ejemplo: dentro de microservicios aplicar el **API Gateway**.
    

👉 Regla práctica:

- El **estilo** organiza el sistema completo.
    
- El **patrón** resuelve problemas locales dentro de ese estilo.
    

### c) ¿En qué etapa del proceso se decide?

- Durante el **Proceso de Diseño Arquitectónico**, en el paso de **“Elección del modelo arquitectónico / framework”**.
    

### d) Explique estilo Hexagonal (Ports & Adapters)

- Separa el **núcleo del negocio (dominio)** de las **interfaces externas**.
    
- Se accede al núcleo mediante **puertos**, y cada puerto se conecta a un **adaptador** (ej: BD, APIs externas, UI).
    
- Ventaja: el core no depende de tecnologías externas.
    

👉 **Ejemplo propio**:  
Un sistema de gestión de bicis de alquiler (BiciRent):

- El **núcleo** tiene las reglas de negocio (validar tarifas, registrar alquiler).
    
- Un **puerto** expone esa lógica.
    
- **Adaptadores**:
    
    - uno conecta con la base de datos (PostgreSQL),
        
    - otro con la app móvil,
        
    - otro con una API de pagos (MercadoPago).  
        Si mañana cambio la BD por MySQL, solo modifico ese adaptador.
        

---

## **2) GRUPO B**

### a) Estilo arquitectónico Layered (en capas)

- Organiza el sistema en **capas jerárquicas**, cada una con responsabilidades bien definidas.
    
- Ejemplo clásico:
    
    1. **Presentación** (UI).
        
    2. **Aplicación / Lógica de negocio**.
        
    3. **Persistencia** (acceso a datos).
        
    4. **Infraestructura** (red, SO, frameworks).
        

### b) Ventajas

1. Alta mantenibilidad (modularidad).
    
2. Reutilización de capas (ej: lógica de negocio para web y móvil).
    
3. Bajo acoplamiento → cada capa depende solo de la inferior.
    

### c) Desventajas

1. Performance reducida (demasiados pasos entre capas).
    
2. Rigidez al cambiar el flujo (difícil “saltar capas”).
    
3. Puede volverse un diseño burocrático y sobrecargado.
    

### d) Ejemplo con RNF crítico = **Seguridad**

Un **Homebanking**.

- RNF crítico: confidencialidad de datos y autenticación fuerte.
    
- Arquitectura en capas permite aislar seguridad en capa de **infraestructura** (encriptación, firewalls, autenticación) y mantener el negocio protegido.
    

---

## **4) GRUPO D** ✅ (ya tenés)

Lo dejaste muy bien 👌. Solo agrego el cierre:

- Es ideal en sistemas **pequeños/medianos**, cuando importa la **simplicidad inicial** más que la escalabilidad.
    
- Ejemplo: sistema de facturación de kiosco → conviene monolito porque es más barato y rápido.
    

---

## **5) GRUPO E**

### ¿Por qué es iterativo?

Porque no se define todo en una sola pasada. Se **revisan y ajustan decisiones** a medida que entendemos mejor los requerimientos y limitaciones tecnológicas.

### Pasos del proceso (con entradas/salidas y UML):

1. **Identificación de requerimientos arquitectónicos**
    
    - Entrada: modelo de requerimientos (casos de uso + RNF).
        
    - Salida: lista de requerimientos arquitectónicos priorizados.
        
    - UML: **Casos de uso**.
        
2. **Elección del modelo arquitectónico**
    
    - Entrada: requerimientos + restricciones tecnológicas.
        
    - Salida: estilo arquitectónico elegido (capas, hexagonal, microservicios, etc.).
        
    - UML: **Diagramas de componentes**.
        
3. **Diseño de vistas arquitectónicas**
    
    - Entrada: modelo arquitectónico.
        
    - Salida: vistas 4+1 (casos de uso, diseño, procesos, implementación, despliegue).
        
    - UML: diagramas de clases, secuencia, despliegue.
        
4. **Validación de la arquitectura**
    
    - Entrada: vistas + requerimientos.
        
    - Salida: escenarios evaluados, prototipos probados.
        
    - UML: diagramas de actividades, secuencia.
        

---

## **6) GRUPO F – Modelo 4+1**

- Propuesto por Kruchten. Representa la arquitectura mediante **5 vistas**:
    
    1. **Vista lógica** (diseño: clases, objetos).
        
    2. **Vista de procesos** (concurrencia, comunicación).
        
    3. **Vista de desarrollo** (componentes, módulos).
        
    4. **Vista física / despliegue** (nodos de hardware).
        
    5. **+1 Vista de casos de uso** (hilos conductores que validan las otras).
        

👉 Relaciones:

- Los **casos de uso** son el eje → validan que las otras 4 vistas estén alineadas.
    
- Lógica = qué hace; Procesos = cómo fluye; Desarrollo = cómo se construye; Despliegue = dónde vive.
    

👉 Ejemplo donde no estén todas:  
Un **sistema embebido** simple (ej: microcontrolador en una cafetera inteligente).

- Tiene vista de despliegue (el hardware) y de lógica (código de control).
    
- Pero no necesita vista de procesos (no hay concurrencia compleja) ni de casos de uso elaborados.
    

---

## **7) GRUPO H – Documento de arquitectura**

### Importancia

- Es la **fuente de verdad** para todo el equipo.
    
- Comunica las decisiones, justifica los trade-offs, permite validar y mantener la arquitectura.
    

### Qué debe incluir

1. Introducción y contexto.
    
2. Requerimientos arquitectónicos.
    
3. Decisiones de estilo y patrones.
    
4. Vistas arquitectónicas (4+1).
    
5. Estrategias de validación (escenarios, prototipos).
    
6. Restricciones tecnológicas.
    

### Ejemplo de dominio

- **Con documento**: un sistema de salud con historia clínica digital → el documento asegura que todos los módulos (pacientes, recetas, seguridad) están alineados y cumplen normas legales (confidencialidad).
    
- **Sin documento**: cada dev implementa a su manera → inconsistencias, duplicación de datos, riesgos de seguridad.
    

---
