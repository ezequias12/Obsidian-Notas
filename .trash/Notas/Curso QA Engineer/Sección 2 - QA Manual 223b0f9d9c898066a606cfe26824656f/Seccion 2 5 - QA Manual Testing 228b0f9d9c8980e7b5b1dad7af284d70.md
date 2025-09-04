# Seccion 2.5 - QA Manual Testing

![image.png](Seccion%202%205%20-%20QA%20Manual%20Testing%20228b0f9d9c8980e7b5b1dad7af284d70/image.png)

## QA MANUAL

Pruebas manualmente

**Las pruebas manuales (Manual Testing)** son un tipo de prueba de software en la que los casos de prueba se ejecutan manualmente por un tester, sin utilizar herramientas automatizadas. El propósito de las pruebas manuales es identificar los errores, problemas y defectos en la aplicación de software. Las pruebas manuales son la técnica más primitiva de todos los tipos de prueba y ayudan a encontrar errores críticos en la aplicación de software.

Objetivos

- Identificar defectos
- verificar si el sistema cumple lo esperado
- Asegurar la calidad de la interfaz y la usabilidad
- Validar la calidad en general

Tipos de pruebas manuales
+ Pruebas funcionales
+ Pruebas de usabilidad
+ Prueba de exploracion
+ Pruebas de regression
+ Pruebas de compatibilidad
+ Validacion de datos

Pasos del QA Manual Testing
1. Analisar los requisitos
2. Preparar los casos de prueba - (Escenarios escritos para probar algo)
3. Ejecutar los casos de prueba
Verificar que la opcion de teclado virtual funcione en busquedas en bing PASSED
Verificar que la opcion de teclado virtual funcione en busquedas en bing FAILED - Error
Verificar que la opcion de teclado virtual funcione en busquedas en bing PASSED
4. Registro de resultados

5. Revision y seguimiento
Error 1: La tecla enter del teclado virtual no esta funcionando correctamente FIXED

Ventajas de QA Manual
+ Enfoque humano
+ Flexibilidad
+ Costo inicial es bajo
+ Ideal para pruebas exploratorias

Desvantajas del QA Manual
+ Propenso a errores humanos
+ Lento
+ Dificultad para repetir pruebas

Herramientas para QA manual
+ Jira
+ Testrail (gestion de casos de pruebas)
+ Postman
+ BrowserStack

## TDD Test-Driven Development

Desarrollo guiado por pruebas

**El Desarrollo Guiado por Pruebas (TDD)** es un enfoque de desarrollo de software en el que se crean casos de prueba para especificar y validar lo que hará el código. En términos simples, primero se crean y prueban los casos de prueba para cada funcionalidad, y si la prueba falla, se escribe nuevo código para que la prueba pase, manteniendo el código simple y libre de errores.

Principios de TDD
1. Escribe una prueba que falle:
2. Escribe el código mínimo para pasar la prueba:
3. Refactoriza el código:
4. Repite:

Demo.

## Test cases and Scenarios

Describes un flujo que se va a probar

Un **caso de prueba** es un conjunto de pasos y datos diseñados para verificar una funcionalidad específica del software.

Incluye precondiciones, postcondiciones y resultados esperados para comparar con los reales.

Un **escenario de prueba** es una funcionalidad completa que puede ser probada, agrupando varios casos de prueba.

Ayuda a evaluar características positivas y negativas del sistema de manera general.

Caractericas
+ Son amplios
+ descriptivos
+ Para planificar

Test Case
+ Son especificos
+ incluyen pasos claros
+ responde la pregunta ¿Como probamos?

Ejemplo practico de un test case:

- Titulo: Probar que un usuario registro puede agregar un producto en el carrito y completar la compra
- ID: IDTS001
- Precondicion: El usuario este registrado
- Pasos:
    1. Navegar a la pagina principal de [www.supermarket.com](http://www.supermarket.com/)
    2. Seleccionar un producto
    3. Hacer clic en agregar al carrito
    4. ir al carrito
    5. Procesar la compra
    6. Completar detalles de pago
    7. Confirmar pedido
- Datos de prueba: Producto laptop, metodo de pago tarjeta de credito
- Resultado esperado: El pedido se realiza con exito y aparece una confirmacion en la pantalla
podemos descargar la factura

Test case y un test scenario
- Scenario es alto nivel  el test case es especifico
- Scenario identificar que probar Test case describe como probarlo
- Scenario describe la funcionalidad Test case incluye los pasos y datos y resultado

TS1 Verificar que el usuario pueda realizar compras
TS2 Verificar que el usuario pueda anular su compra

## Prueba manual Compatibility

Nos permite verificar que una app funcione en distintos entornos

¿Qué es Compatibility Testing?
+ Es el proceso de comprobr que un software interactua con todos los demas sistemas o herramienta de manera correcta.
+ Sistemas operativos
+ Todos los navegadores
+ Dispositivos (Moviles, tables, pc, laptos)
+ Resoluciones (HD, full HD, 2K, 4K)
+ Versiones de hardware (Procesadores, memoria, almacenamiento, etc.)
+ Redes (Wifi, 4G, 5G, Baja latencia, etc.)
+ Versiona de software relacionadas (Bibliotecas, apis, bases de datos)

Tipos de compatibilidad
+ Forward Com.
+ Backward Com.

Importancia de Compatibility
+ Experiencia de usaurio
+ Mayor alcance de mercado
+ Reduccion de riesgos
+ Cumplimiento de los estandares

## Verificacion vs Validacion

Son conceptos importantes

Verificacion
Verificamos que el proceso del software se esta llevando de acuerdo a los requisitos

- ¿Estamos construyendo el producto correctamente?

Caracteristicas
+ Se enfoca en revisar docuemntos
+ Implica actividades estaticas
+ Detecta errores tempranos

Ejemplo:
Revisar el diseño del software para verificar si cumple con los requisitos minimos
Inspeccion el codigo para asegurar que cumple las guias de estilos

Validacion
Es el proceso de asegurarse cumple con las necesidades del usuario

- ¿Estamos construyendo el producto adecuado?

Caracteristicas
+ enfoca en probar el software en ejecucion
+ ejecutar codigo
+ Verificar que el software cumpla con el cliente

Ejemplo:
Realizar pruebas para verificar si la cal suma correctamente
Probar la interfaz del usaurio que sea usable

## Test Planning

Planificas todo lo refernte  a las pruebas

- Preparar el plan detallado
- Quien va probar
- Como se va a probar
- que herramientas se van a usar
- como se van a ejecutar
- que tiempo va a demorar

¿Porque es importante el plan de pruebas?
+ Define los objetivos de pruebas
+ Evita retrasos y errores
+ Optimiza recursos
+ Miniza riesgos

Pasos para realizar un plan de pruebas
+ Revision de los requisitos
+ Definicion de objetivos
+ Decidir la estrategia
+ Asignacion de recursos
+ Identificas los riesgos
+ Documentacion
+ Aprobacion del test plan