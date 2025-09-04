# Seccion 2.3.2 - Testing No Funcional

![image.png](Seccion%202%203%202%20-%20Testing%20No%20Funcional%20224b0f9d9c8980f3add0ec1db85ee2ba/image.png)

## Load Testing

Es un tipo de prueba de rendimiento que se realiza para determinar cómo una aplicación maneja una cantidad específica de usuarios.

¿Por qué es importante el Load Testing?
Verificar el rendimiento bajo cargas esperadas.
Identificar cuellos de botella en la infraestructura.
Prevenir fallas durante picos de tráfico.
Asegurar la experiencia del usuario incluso con alta demanda.

Cómo realizar Load Testing

- Identifica los escenarios [www.wikipedia.org](http://www.wikipedia.org/)
- Define la carga esperada | 20

Ejemplo de Software → Apache JMeter 

## Performance Testing (Lo hice con IA)

El Performance Testing o prueba de rendimiento es un tipo de prueba no funcional que evalúa cómo un sistema o aplicación se comporta en términos de capacidad de respuesta, estabilidad, velocidad, escalabilidad y uso de recursos bajo una carga de trabajo específica.

Objetivos principales del Performance Testing:

- Medir tiempos de respuesta y latencia
- Evaluar la capacidad del sistema bajo diferentes condiciones
- Identificar cuellos de botella que afecten el rendimiento
- Determinar la estabilidad bajo cargas sostenidas
- Verificar que el sistema cumple con los requisitos de rendimiento establecidos

Tipos comunes de Performance Testing:

- Load Testing: Evalúa el comportamiento bajo cargas esperadas
- Stress Testing: Prueba el sistema más allá de sus límites normales
- Endurance Testing: Verifica el rendimiento durante periodos prolongados
- Spike Testing: Analiza la respuesta ante aumentos repentinos de carga
- Volume Testing: Evalúa el rendimiento con grandes volúmenes de datos

Herramientas populares para Performance Testing:

- Apache JMeter
- LoadRunner
- Gatling
- Locust
- K6

## Stress Testing

El **Stress Testing** es una prueba que se hace a un sistema para ver cómo funciona bajo condiciones extremas. Consiste en **cargarlo con más usuarios o procesos de lo normal** para ver si falla o se mantiene estable.

Sirve para identificar **límites de capacidad y puntos débiles** antes de que ocurran en producción, así se asegura que el sistema pueda soportar **picos de uso sin colapsar**.

## Security Testing

El Security Testing o prueba de seguridad es un tipo de prueba de software diseñada para identificar vulnerabilidades, amenazas o riesgos en una aplicación o sistema.

Objetivo del Security Testing

- Identificar vulnerabilidades:
- Prevenir accesos no autorizados:
- Proteger datos sensibles:
- Asegurar la continuidad del sistema:
- Cumplir con regulaciones:

Áreas clave del Security Testing

- Confidencialidad:
- Integridad:
- Autenticación:
- Autorización:
- Disponibilidad:
- No repudio:

Tipos de Security Testing

- Vulnerability Scanning:
- Penetration Testing (Pruebas de Penetración):
- Ethical Hacking:
- Risk Assessment:
- Security Auditing:
- Postura de Seguridad:

Herramientas comunes para Security Testing

- OWASP ZAP (Zed Attack Proxy):
- Burp Suite:
- Nmap:
- Metasploit:
- Nikto:
- Wireshark:

Cómo realizar Security Testing

1. Identificar objetivos
2. Analizar posibles amenazas
3. Configurar herramientas
4. Realizar pruebas manuales
5. Evaluar y mitigar riesgos

Demo OWASP ZAP

## ¿Qué es Accessibility Testing?

El Accessibility Testing o prueba de accesibilidad es un tipo de prueba de software diseñada para garantizar que una aplicación, sitio web o sistema sea accesible para todos los usuarios, incluidos aquellos con discapacidades físicas, visuales, auditivas, cognitivas o de otro tipo. Su objetivo es asegurar que las personas con diferentes capacidades puedan interactuar con el sistema sin barreras.

Cómo realizar Accessibility Testing

1. Analizar el alcance:
Identifica las áreas críticas de la aplicación (formularios, navegación, elementos multimedia).
Decide si la prueba será manual, automatizada o una combinación de ambas.
2. Realizar pruebas manuales:
Navega por la aplicación con un lector de pantalla.
Usa solo el teclado para interactuar con los elementos.
Evalúa los mensajes de error, botones y enlaces para garantizar que sean claros y accesibles.
3. Usar herramientas automatizadas:
Ejecuta un análisis de accesibilidad con herramientas como Wave o Axe para identificar problemas de accesibilidad básicos.
4. Validar colores y contraste:
Usa verificadores de contraste para garantizar que los colores sean legibles para personas con daltonismo o visión reducida.
5. Probar tecnologías de asistencia:
Asegúrate de que la aplicación funcione bien con lectores de pantalla y otras tecnologías de asistencia.
6. Generar reportes:
Documenta las vulnerabilidades encontradas y clasifícalas según su severidad (baja, media, alta, crítica).
7. Resolver problemas:
Trabaja con desarrolladores y diseñadores para solucionar los problemas encontrados.