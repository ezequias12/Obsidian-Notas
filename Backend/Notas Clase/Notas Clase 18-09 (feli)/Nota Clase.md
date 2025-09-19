
# Primera idea general

## Division de microservicios

Tenemos que entender bien como separar la aplicacion en 3 microservicios. uno va a ser un apigateway
Cosas basicas como que cada microservicio tiene una base de datos, aunque podemos conectar todos a una sola y no va a haber tanto problema, la idea seria que este todo en la misma

### Comunicaciones entre microservicios

La comunicacion por peticion rest entre cada ms va a ser mucho mas lenta que la comunicacion de cada ms a la BD
tenemos que saber **balancear** bien la carga de cada ms, y que no se sature uno solo

**Que es un recurso?**
-> Un recurso es un servicio que ofrece un microservicio a otro microservicio
-> la diferencia con Entidad es que, una entidad es un objeto que tiene su propia identidad y ciclo de vida, mientras que un recurso es una representación de datos o funcionalidad que puede ser accedida a través de una API.
-> En sintesis, es un Json. un Json que tiene una estructura definida y que representa un objeto o una coleccion de objetos.

-> el recurso se expone y puede componerse de partes, por ejemplo, un recurso "Usuario" puede tener sub-recursos como "Perfil", "Configuraciones", etc.
-> Cada recurso tiene un **Nombre**, que es la URI
    -> **que se diferencia URI con URL?**
    -> URI (Identificador Uniforme de Recursos) es un identificador genérico que puede ser una URL o un URN.
    Tengo solo RECURSOS
    -> URL (Localizador Uniforme de Recursos) es un tipo específico de URI que proporciona
    Tengo SERVIDOR + RECURSOS

**Storage? a que nos referimos?**
-> Almacenamiento de datos, puede ser en bases de datos, sistemas de archivos, etc.
-> Cada microservicio puede tener su propio almacenamiento, pero deben ser capaces de comunicarse entre sí para acceder a los datos que necesitan.

**Como nombramos los recursos?**
-> Usamos sustantivos en plural, por ejemplo, /usuarios, /productos, /ordenes
-> Usamos Cabecera y detalle. como decir DetalleFactura
-> Si tenemos /facturas, podemos tener /facturas/{id}/detalles

vamos a tener
/festivales/{festivalId}/obras
/festivales/{festivalId}/publicaciones
/festivales/{festivalId}/resumen
/obras/{obraId}/artistas
/artistas/{artistaId}
/multimedia/{mediaId}/download
/multimedia/{mediaId}

Lo que propone la actividad es armar la estructura completa

### Microservicios propuestos por el profe

1. Microservicio Multimedia, relacionado con
    -> Publicaciones
    -> Osea tengo un MS de PublicacionesYMultimedia
    -> Lo que pasa aca es que las publicaciones en el dominio son Post, por lo que

2. FestivalesYObras

3. Artistas

## Segunda idea general

**Swagger**
Lo que nos va a ayudar Swagger es a documentar la API, y a tener una idea clara de como va a ser la comunicacion entre los microservicios.
Nos va a permitir definir los recursos, los metodos HTTP, los parametros, las respuestas, etc.
Nos va a permitir tambien probar la API de manera interactiva.

No va a hacer falta escribir en openApi, Swagger nos va a generar el archivo a partir de las anotaciones en el codigo.
Lo va a extraer de nuestro codigo.

**Docker con PostgreSQL**
-> El Tp va a tener que ser desplegado en un contenedor Docker.

## Pautas fundamentales para entender Apunte 15

### Spring framework vs Spring boot

**Cual es la principal diferencia?**
-> Spring framework es un framework de desarrollo de aplicaciones Java, mientras que Spring Boot es una herramienta que facilita la configuracion y el despliegue de aplicaciones Spring.

-> Recordadndo que hacia Express en Javascript aca, Spring hace lo mismo en Java

En Express (JavaScript), defines rutas y controladores usando funciones como app.get, app.post, etc., para manejar peticiones HTTP y enviar respuestas.

En Spring (Java), el equivalente es usar anotaciones en clases y métodos, como @RestController, @RequestMapping, @GetMapping, @PostMapping, etc. Cada método en el controlador recibe la petición, procesa la lógica (usualmente delegando a un servicio) y retorna una respuesta HTTP.

La idea es que voy a dejar de crear Instancias de Objetos con New. esas instancais las va a instalar el contenedor de Spring

La principal Funcion es ser un PATRON DE INYECCION DE DEPENDENCIAS

En Spring, vos dejás de crear instancias con new en tu código.

Esas instancias (objetos) las crea, inicializa y mantiene el contenedor de Spring (el IoC Container).

La función principal del contenedor es implementar el patrón de Inversión de Control (IoC) a través de la Inyección de Dependencias (DI)

🔹 Spring Framework

* Es el **framework base** (desde 2003).
* Te da herramientas para:

  * **Inyección de dependencias (IoC / DI)** → desacoplar tu código.
  * Manejo de beans (objetos gestionados).
  * Módulos: Spring Data, Spring Security, Spring MVC, etc.
* Ventajas: súper flexible, modular, estándar de facto en Java.
* Desventaja: la configuración a mano era **larga y pesada** (XML enormes, beans declarados a mano, config detallada).

Spring Framework es **la caja de herramientas completa**. Vos elegís qué usar y lo configurás a mano.

🔹 Spring Boot

* Apareció en 2014 como **extensión de Spring Framework**.
* Objetivo: **simplificar** el arranque de proyectos.
* Lo que te da:

  * **Autoconfiguración** → detecta qué tenés en el classpath y configura beans por vos.
  * **Starters** → dependencias listas para usar (`spring-boot-starter-web`, `spring-boot-starter-data-jpa`, etc.).
  * **Servidor embebido** (Tomcat/Jetty) → tu app se ejecuta con `main()` sin desplegar un WAR en un servidor externo.
  * **Convenciones > configuración** → vos solo sobreescribís lo que quieras personalizar.
  * Integración natural con **Spring Data JPA, REST, Security**, etc.

Spring Boot es como un **Spring con piloto automático** → vos te enfocás en la lógica, no en la configuración.

**La primera tarea a llevar a cabo**
-> Con Spring Boot, crear un proyecto base con las dependencias necesarias (web, JPA, PostgreSQL, etc.) y una estructura inicial de paquetes (controladores, servicios, repositorios, modelos).
-> Armar una Aplicacion con el contenedor de Spring plano de consola (No Spring Web, sino el cor)
-> Dentro implementar SCOPE (Ver en el apunte)
-> Dentro de ese ejemplo tenemos los dos Scope basicos, Singleton y Prototype

### Que hace el Spring Initializr?
-> Nos genera un proyecto base con las dependencias que le indiquemos.
-> Le tenemos que indicar el lenguaje (Java), la version de Spring Boot, las dependencias (Web, JPA, PostgreSQL, etc.), el nombre del proyecto, el paquete base, etc.
-> Nos genera un proyecto zip que podemos importar en nuestro IDE 
-> Nos genera la estructura de paquetes basica (controladores, servicios, repositorios, modelos)

-> En el caso de Spring Core, no nos va a generar nada, vamos a tener que crear la estructura de paquetes a mano

Entonces, aldescomprimir el archivo vamos a tener un proyecto de maven. 
Dentro en las dependencias del pom.xml vamos a tener las dependencias que le indicamos al Spring Initializr
El contenedor de inyeccion de dependencias 

**Luego tenemos que agregar en las dependencias el codigo que tenemos en el ejemplo de Scope**

-> Siguiendo, agregamos la dependecia Web (Creo que lo teniamos que agregar antes de crear el Initializer pero no comprendi bien)

-> El zip te da el directorio con el proyecto maven dentro

-> Luego tenemos que agregar un Controller (Siguiendo el ejemplo de Scope)
-> Luego el Hola Mundo 

El initializer resuelve lo del pom.xml
Lo que No resuelve es lo del Swagger todavia

LLEGAR HASTA EL HOLA MUNDO PARA LA PROXIMA CLASE