# Sección 2.1 - Fundamentos

![image.png](Secci%C3%B3n%202%201%20-%20Fundamentos%20223b0f9d9c8980588f99f209fddffb55/image.png)

## Que es el control de calidad✅

- Que tan bien cumple con las necesidades y expectativas del cliente
- Funcionalidad : hace lo que se espera que haga
- Diseño: Intuitivo estético fácil de usar
- Fiabilidad: Funciona de manera correcta en distintos contextos (ej, diferentes dispositivos o conexión a internet)
- Durabilidad
- Precio

## Assurance✅

- garantia -> Cumple con tus expectativas
-> Funciona correctamente
-> No presenta problemas

## Software testing✅

- Garantizar la calidad de un producto antes de que llegue a los usuarios finales

## Objetivo principal✅

- Prevenir los errores en el desarrollo y en produccion (cuando llegue a los clientes)

## Características claves del QA✅

- Es un proceso preventivo
- Enfocado en procesos
- Asegura cofianza en el producto

## Diferencias entre QA y Testing✅

QA ->Proceso Preventivo asegura la calidad  testing->Actividad para encontrar defectos
QA-> Procesos estandares  Testing->En el producto final
QA->Mejora el ciclo de desarrollo  Testing->Verifica si el producto cumple con los requisitos

## QA Mindset (Mentalidad de garantia de Calidad)✅

- Prevenir antes de corregir (identificar problemas desde los requisitos, procesos)
- Atencion al detalle
- Pensamiento critico
- Enfoque colaborativon (trabajo en equipo, esfuerzo compartido entre todos)
- Mentalidad de mejora continua (siempre buscar formas para optimizar procesos)

## Como desarrollar esta mentalidad? ✅

- Comprender el negocio y al usaurio final (conocer al producto y que espera el usuario final)
- Adoptar estandares de calidad (familiarizarnos con estandares de calidad)
- Debemos hacer preguntas
- Ser proactivo
- Usar herramientas y metricas

## White box testing - Pruebas de caja blanca✅

- Enfoque de pruebas donde se prueba el codigo fuente

verifica el flujo logico del programa

## Black Box testing - pruebas de caja negra ✅

- Funcionalidad sin conocer el codigo fuente

Equipo que testea y que puede trabajar cerca del equipo de desarrollo como tambien puede que ni lo conozca

## Gray Box Testing - Pruebas de caja gris✅

- Acceso parcial al sistema
- centradas en las entradas y salidas en algunos aspectos internos del sistema
Ejemplos.
Inicio de sesion -> despues de 3 intentos se bloquea
Revisar la base de datos: Usuario Bloqueado hora de bloqueo: 12:00:12

lo que agrega el gray box testing es que puedo entrar a la base de datos y reviso la hora de bloqueo del usuario para ver si funciona efectivamente

## Test Oracles✅

- Son fuentes de verdad que nos permiten determinar si los resultados son correctos o no.
- El oracle define cual debe ser el comportamiento.

Ejemplo.
Busqueda de OpenAI en google, el primer resultado debe ser un enlace que lleve a [openai.com](http://openai.com/)

## ¿Qué es Test Prioritization?✅

- Es una estrategia en la que asignas un orden a las pruebas
- Un orden en funcion de su importancia, impacto, urgencia
- Te ayuda que los casos mas valiosos se ejecuten primero

## Objetivos principales de Test Prioritization✅

- Ejecutar pruebas con mayor impacto
- Detectar errores criticos
- Maximizar el valor de las pruebas

## Criterios para priorizar pruebas✅

- Frecuencia de uso
- Impacto del error
- Probabilidad de fallo
- Requisitos del cliente
- Dependencias

Ejemplo:
Probando la funcionalidad de google buscar

- una barra de busqueda
- resultados con enlaces y titulos
- filtros como imagenes, videos, etc.

Listar casos de prueba

- probar buscar textos siemple
- probar filtros
- probar resultados patrocinados
- probar sugerencias de busqueda
- probar casso de limite

Luego armariamos un orden de que nos parece lo mas importante para probar primero 

1 casos criticos

- buscar texto simple
- buscar filtros

2 casos alto impacto

- resultados patrocinados
- sugerencias de buqueda

3 casos menor probabilidad

- probar casos de limite (textos largos)