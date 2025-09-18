
# Primera idea general

### Division de microservicios
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

**Microservicios propuestos por el profe**
1. Microservicio Multimedia, relacionado con 
    -> Publicaciones
    -> Osea tengo un MS de PublicacionesYMultimedia
    -> Lo que pasa aca es que las publicaciones en el dominio son Post, por lo que 

2. FestivalesYObras

3. Artistas

# Segunda idea general

**Swagger**
Lo que nos va a ayudar Swagger es a documentar la API, y a tener una idea clara de como va a ser la comunicacion entre los microservicios.
Nos va a permitir definir los recursos, los metodos HTTP, los parametros, las respuestas, etc.
Nos va a permitir tambien probar la API de manera interactiva.

No va a hacer falta escribir en openApi, Swagger nos va a generar el archivo a partir de las anotaciones en el codigo.
Lo va a extraer de nuestro codigo.


**Docker con PostgreSQL**
-> El Tp va a tener que ser desplegado en un contenedor Docker


