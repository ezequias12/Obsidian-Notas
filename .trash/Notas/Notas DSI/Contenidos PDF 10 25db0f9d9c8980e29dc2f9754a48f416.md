# Contenidos PDF 10

# 📌 Arquitectura Monolítica

### Definición

La **arquitectura monolítica** es el modelo más tradicional de construcción de software. Se denomina así porque **todo el sistema se desarrolla y despliega como una única unidad**: un solo bloque que contiene todas las funciones necesarias para su funcionamiento.

El término *monolito* proviene de:

- *Mono* → único, uno solo.
- *Lítico* → rígido, pétreo.

En esta arquitectura, los distintos **módulos internos** (presentación, lógica de negocio, acceso a datos, etc.) pueden estar **divididos en capas** dentro del código, pero **al final se empaquetan juntos y se despliegan en un único proceso ejecutable**.

### Características principales

- Todo el sistema se **ejecuta dentro de un solo servidor o contenedor**.
- Se suele tener un único programa que se comunica con:
    1. La interfaz de usuario (generalmente con protocolo REST/HTTP).
    2. La base de datos (a través de SQL u otro lenguaje de consulta).
    3. Dependencias externas (bibliotecas, servicios externos, etc.).
- Aunque internamente haya separación de componentes, **no hay independencia en el despliegue**: si se cambia algo, se recompila y se despliega todo el sistema.

### Ventajas

1. **Facilidad de desarrollo inicial** → muy simple para empezar un proyecto desde cero.
2. **Facilidad de pruebas** → como todo está en un solo lugar, se pueden testear funcionalidades de manera directa.
3. **Escalado inicial sencillo** → basta con poner el mismo ejecutable en varios servidores detrás de un balanceador de carga (*escalado horizontal replicando el monolito*).
4. **Menos puntos de fallo externos** → no depende de múltiples servicios distribuidos, lo que reduce riesgos de comunicación entre componentes.
5. **Performance local** → al estar todo en el mismo proceso, las llamadas son locales (en memoria), más rápidas que llamadas remotas.
6. **Autonomía** → no requiere demasiada infraestructura extra, puede funcionar con un solo servidor.

### Desventajas

1. **Alta complejidad a largo plazo** → a medida que la aplicación crece, el código se vuelve más difícil de entender y mantener.
2. **Falla total** → si un módulo falla, puede comprometer a toda la aplicación.
3. **Dificultad de mantenimiento y depuración** → un cambio en una parte puede afectar a todo el sistema.
4. **Despliegue rígido** → incluso un pequeño cambio requiere recompilar y redeployar todo.
5. **Escalabilidad limitada** → si se necesita escalar solo un módulo (ej. procesamiento de pagos), se debe escalar toda la aplicación completa.
6. **Dependencia tecnológica fuerte** → toda la aplicación suele quedar atada a un stack tecnológico inicial (ej. Java + Spring + SQL Server). Migrar una parte implica migrar todo.

### Ejemplo real

Un **sistema de gestión de inventario** clásico en una pyme:

- Tiene una interfaz web (HTML + JS).
- Una lógica central con reglas de negocio (ventas, compras, stock).
- Acceso a una base de datos SQL.

Todo se despliega como un único archivo **.war** (en Java) o un único **.exe** (en C#). Para agregar un pequeño cambio, hay que recompilar todo el proyecto y volver a instalarlo en el servidor.

### Evolución

Muchas aplicaciones nacen como monolíticas porque es **la forma más simple y económica de empezar**. Sin embargo, cuando el negocio crece y se requieren nuevas características, escalabilidad o equipos de desarrollo más grandes, surgen los problemas:

- El monolito se vuelve **difícil de escalar por partes**.
- **El tiempo de despliegue aumenta**.
- Los cambios en un módulo afectan a otros inesperadamente.

En ese punto, suele evaluarse una migración hacia **arquitecturas más modulares o distribuidas** (por ejemplo, microservicios).

### 🔎 Información complementaria

- **Cuándo conviene usar monolito**:
    - Proyectos pequeños o medianos.
    - Startups en etapa temprana que necesitan validar una idea rápido.
    - Equipos reducidos de desarrollo.
- **Cuándo deja de convenir**:
    - Cuando la aplicación empieza a crecer demasiado en funcionalidad.
    - Cuando distintos equipos necesitan trabajar en paralelo sin pisarse.
    - Cuando los tiempos de despliegue y prueba son cada vez más largos.

Sintesis

> La arquitectura monolítica empaqueta todos los módulos de un sistema en un único programa. Es simple de desarrollar y desplegar, con buen rendimiento local, pero se vuelve difícil de mantener y escalar a medida que crece. Conviene para sistemas chicos o en etapas iniciales, pero suele migrarse a arquitecturas distribuidas cuando el sistema escala.
> 

---

# 📌 Estilos vs Patrones Arquitectónicos

### 1. ¿Qué es un Estilo Arquitectónico?

Un **estilo arquitectónico** es una **familia de arquitecturas** que comparten características comunes y principios de organización.

Define un **conjunto de restricciones y lineamientos generales** sobre:

- Cómo se organizan los componentes.
- Cómo se comunican entre ellos.
- Cuáles son los roles de cada uno.

👉 Los estilos son **macro-estructuras** que guían el diseño.

Se enfocan en la **estructura global del sistema**.

**Ejemplos de estilos arquitectónicos**:

- Monolítico.
- Cliente-Servidor.
- En Capas (Layered).
- Microservicios.
- Pipe & Filter (tuberías y filtros).
- Peer-to-Peer.

📌 Se puede pensar a los estilos como **“plantillas conceptuales”** que guían cómo organizar un sistema.

---

### 2. ¿Qué es un Patrón Arquitectónico?

Un **patrón arquitectónico** es una **solución probada y recurrente** a un problema de diseño arquitectónico.

Mientras que el estilo es **macro-estructural**, el patrón es **más concreto** y se centra en **resolver un problema específico dentro de esa arquitectura**.

👉 Los patrones nacen de la práctica: problemas reales que se resolvieron de forma repetida y que luego se documentaron.

**Ejemplos de patrones arquitectónicos**:

- Modelo-Vista-Controlador (MVC).
- Broker.
- Master-Slave (Maestro-Esclavo).
- Microkernel.
- Pipes & Filters aplicado en procesamiento de datos.

📌 Se puede pensar a los patrones como **“recetas” o “tácticas”** dentro de un estilo o de un contexto concreto.

---

### 3. Diferencias clave entre Estilos y Patrones

| Aspecto | Estilos Arquitectónicos | Patrones Arquitectónicos |
| --- | --- | --- |
| **Nivel de abstracción** | Más general (macroestructura del sistema). | Más específico (resuelve problemas puntuales). |
| **Origen** | Académico, teórico, conceptual. | Práctico, basado en experiencias reales. |
| **Enfoque** | Cómo organizar los **componentes globalmente**. | Cómo resolver un **problema recurrente** en la organización. |
| **Ejemplos** | Cliente-Servidor, Monolítico, En Capas, Microservicios. | MVC, Master-Slave, Broker, Microkernel. |
| **Analogía** | “Estilo” es como elegir la **forma de la casa** (departamento, chalet, edificio). | “Patrón” es como resolver **detalles constructivos** (escalera caracol, doble vidrio, ventilación cruzada). |

---

### 4. Relación entre Estilos y Patrones

- Un **patrón puede estar embebido en un estilo**. Ejemplo: en un sistema con estilo Cliente-Servidor puede aplicarse el patrón **Broker** para mediar entre clientes y servidores.
- Varios patrones pueden convivir dentro de un estilo. Ejemplo: en una arquitectura en capas se pueden aplicar **MVC** para separar presentación, lógica y datos.
- Mientras los estilos son más **“platónicos”** (definiciones generales), los patrones son más **“prácticos”** (casos de uso y soluciones concretas).

---

### 5. Ejemplo aplicado

Supongamos que queremos construir un **sistema de e-commerce**:

- Elegimos un **estilo arquitectónico en capas**:
    - Capa de presentación (UI web y mobile).
    - Capa de negocio (reglas de compras, pagos, envíos).
    - Capa de datos (SQL).
- Dentro de ese estilo aplicamos patrones:
    - **MVC** en la capa de presentación.
    - **Repository** en la capa de datos.
    - **Controlador** (GRASP) en la capa de negocio.

---

# 📌 Platónicos vs. Embebidos (en Estilos/Patrones)

En el apunte se hace una aclaración:

- Algunos estilos arquitectónicos se consideran **“platónicos”** → son modelos ideales, conceptuales, usados principalmente con fines académicos o como guía teórica. Ejemplo: *Pipe & Filter puro*, donde cada filtro es independiente y no guarda estado.
- Otros son **“embebidos”** → surgen en la práctica real, muchas veces como combinaciones de varios estilos o adaptaciones pragmáticas. Ejemplo: un sistema que dice ser “en capas” pero permite que la capa de presentación consulte directamente la base de datos (mezcla de estilos).

👉 Lo importante:

- **Platónicos**: ideales, usados para enseñar o modelar en abstracto.
- **Embebidos**: mezclas o adaptaciones reales, que no siempre respetan al 100% el estilo puro.

---

# 📌 Arquitectura “Spaghetti”

La **arquitectura Spaghetti** es un *anti-patrón* de arquitectura.

- Se llama así porque el código termina siendo una **mezcla desordenada** de módulos, funciones y dependencias, sin separación clara de responsabilidades.
- Surge cuando el sistema crece sin una estructura definida → los componentes quedan fuertemente acoplados y con alta dependencia circular.

### Características

- Ausencia de capas o modularidad.
- Código difícil de leer y mantener.
- Cambios pequeños pueden romper muchas partes.
- Muy baja cohesión y altísimo acoplamiento.

### Consecuencias

- Mantenimiento casi imposible.
- Refactorizaciones costosas.
- Evolución del sistema bloqueada (se tiende a reemplazarlo entero en lugar de modificarlo).

👉 Es lo opuesto a una arquitectura bien definida (monolítica en capas, hexagonal, etc.). El nombre es una metáfora visual: el flujo del programa se enreda como un plato de fideos.

---

# 📌 Arquitectura en Capas (Layered Architecture)

### Definición

La arquitectura en capas organiza un sistema de software en **niveles horizontales**, cada uno con responsabilidades bien definidas.

Cada capa **ofrece servicios a la capa superior** y **consume los de la capa inferior**, generando una separación clara de intereses.

---

### Características principales

- División en capas lógicas (ejemplo clásico: presentación, negocio, datos).
- Comunicación principalmente **vertical** (arriba–abajo).
- Puede ser **estricta** (una capa solo accede a la inmediata inferior) o **relajada** (una capa puede saltar a inferiores).
- Se busca **alta cohesión dentro de la capa** y **bajo acoplamiento entre capas**.

---

### Capas típicas

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image.png)

1. **Presentación (UI)**
    - Interacción con el usuario o sistemas externos.
    - Ejemplo: páginas web, apps móviles, API REST.
2. **Negocio (Dominio)**
    - Reglas de negocio y procesos centrales.
    - Ejemplo: validaciones, cálculos, lógica de facturación.
3. **Persistencia (Acceso a Datos)**
    - Conexión con bases de datos o repositorios.
    - Ejemplo: SQL, ORMs como Hibernate o Sequelize.
4. **Infraestructura (opcional)**
    - Servicios técnicos: seguridad, comunicación, logging.

---

### Ventajas

- **Mantenibilidad**: facilita localizar errores y aplicar cambios.
- **Reutilización**: capas como acceso a datos o UI pueden usarse en otros proyectos.
- **Trabajo en paralelo**: equipos distintos pueden desarrollar capas diferentes.
- **Flexibilidad tecnológica**: se puede reemplazar una capa sin alterar todo el sistema.

### Desventajas

- **Overhead en rendimiento**: las peticiones pasan por múltiples capas.
- **Rigidez en cambios**: una modificación puede requerir ajustes en varias capas.
- **Sobre-ingeniería**: puede ser innecesario para sistemas pequeños.

---

### Ejemplo

Un sistema de gestión hospitalaria:

- **Presentación**: pantalla web con historia clínica.
- **Negocio**: validación de turnos, control de recetas, facturación.
- **Persistencia**: tablas de pacientes, médicos y tratamientos en SQL Server.
- **Infraestructura**: auditoría de accesos y cifrado de datos.

---

### Comparación con Monolítica

- En una arquitectura monolítica todo está junto sin una división formal.
- En la arquitectura en capas existe una **organización explícita por niveles**, aunque al final se despliegue como un solo ejecutable.

---

### Extra

- Es una de las arquitecturas más usadas en sistemas empresariales.
- Muchas arquitecturas modernas (Hexagonal, Limpia, Onion) son **variantes** que buscan resolver limitaciones del enfoque en capas, sobre todo en el acoplamiento entre niveles.

---

# 📌 Estilo Arquitectónico: Partición de Dominio

### Definición

Consiste en **separar los componentes de nivel superior** según **flujos de trabajo y/o dominios de negocio**, en lugar de organizarlos solo de forma técnica (presentación, reglas, persistencia).

El sistema se modela siguiendo más de cerca la **estructura del negocio real**.

---

### Ejemplo de comparación

- **Particionamiento Técnico**: división por capas técnicas → Presentación, Reglas de Negocio, Servicios, Persistencia.
- **Particionamiento de Dominio**: división por áreas funcionales → Checkout de Catálogo, Actualización de Inventario, Envíos, Reportes, Contabilidad, etc.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%201.png)

---

### Ventajas

- El modelado refleja **cómo funciona el negocio**, no solo detalles técnicos.
- Más alineado con arquitecturas modernas:
    - **Monolito modular**.
    - **Microservicios** (cada dominio puede transformarse en un servicio independiente).
- El flujo de mensajes coincide con el **dominio del problema**.
- Facilita migrar datos y componentes hacia arquitecturas distribuidas.

### Desventaja

- **Código de personalización duplicado**: aparece en varios lugares cuando se repite lógica transversal entre dominios.

---

📖 **Resumen corto**:

La **partición de dominio** organiza la arquitectura por áreas funcionales del negocio (ej. catálogo, inventario, envíos) en lugar de hacerlo por capas técnicas. Esto la acerca al mundo real, facilita la migración hacia microservicios y mejora la alineación con el dominio, aunque puede duplicar lógica de personalización.

---

# 📌 Arquitectura Hexagonal (Puertos y Adaptadores)

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%202.png)

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%203.png)

## **Definición y sentido**

También llamada **Arquitectura de Puertos y Adaptadores**. Propone ubicar la **lógica de negocio** (núcleo de la aplicación) en el centro, **aislada** de detalles técnicos (base de datos, frameworks, UI, protocolos). La comunicación con el exterior se realiza a través de **puertos** definidos por el negocio y **adaptadores** que conectan esos puertos con tecnologías concretas. El **hexágono** es una metáfora visual para mostrar que puede haber **múltiples entradas y salidas** de forma simétrica.

### **Puertos (internos al negocio)**

- Son **interfaces/contratos** definidos desde el punto de vista del **negocio** (qué necesita la aplicación), no desde la tecnología (cómo se implementa).
- Expresan capacidades del núcleo: recibir **comandos/consultas** (entradas) y solicitar **servicios externos** (salidas).
- Al estar definidos en el centro, **el dominio no conoce** detalles técnicos (ORM, drivers, SDKs, HTTP, etc.).

### **Adaptadores (externos al negocio)**

- “Cierran la brecha” entre los **puertos** y el **mundo exterior** (DB, UI, servicios de terceros, mensajería, archivo, redes).
- Son **intercambiables**: puede haber **varios adaptadores para un mismo puerto**, reemplazables sin afectar el núcleo.
- Su misión es **traducir**: del protocolo/tecnología concreta al contrato del puerto, y viceversa.

### **Aplicación (núcleo)**

- Representada como el **hexágono**.
- **Recibe** comandos o consultas por sus **puertos de entrada** (desde clientes, UI, APIs, jobs).
- **Emite** solicitudes a otros actores (p. ej., persistencia, notificaciones, integraciones) por **puertos de salida**, que serán atendidos por adaptadores concretos.
- Mantiene **independencia** de frameworks y motores de datos; la **lógica de negocio** conduce el diseño.

### **Dirección de dependencias e inversión**

- Las **invocaciones** pueden ir del núcleo hacia afuera (p. ej., “guardar”, “publicar evento”), pero **las dependencias del código** apuntan **hacia adentro**.
- Los **puertos** viven en el núcleo; las **implementaciones** de esos puertos viven fuera (adaptadores).
- Esta **inversión de dependencia** evita que el negocio se acople a detalles técnicos.

---

## **Énfasis conceptual (frente a enfoques DB-first o framework-first)**

- La hexagonal **enfatiza la lógica de negocios** por sobre el diseño de la base de datos: se **planifica y desarrolla primero** el lado de negocio; la persistencia se **considera después** como un adaptador más.
- Los **cambios en la lógica** deben **impulsar** los cambios en la persistencia, **no al revés**.
- A medida que se incorporan **API REST** y **servicios de terceros**, ofrece **pautas claras** sobre **dónde ubicar** esos componentes: siempre como **adaptadores** alrededor del núcleo, evitando que contaminen el dominio.

---

### **Clasificación de adaptadores (visión de las diapositivas)**

- **De entrada / driving**: inician la interacción con el núcleo a través de **puertos de entrada** (p. ej., un controlador que recibe una petición).
- **De salida / driven**: son “utilizados” por el núcleo mediante **puertos de salida** (p. ej., persistencia, mensajería, e-mail, gateways).
- En ambos casos, el **contrato** está en el centro; afuera se materializa la tecnología que corresponda.

### **Consecuencias y propiedades**

- **Intercambiabilidad tecnológica**: cambiar DB, protocolo o proveedor implica **sustituir un adaptador** sin tocar el núcleo.
- **Testabilidad**: al trabajar con puertos, el núcleo se prueba **sin infraestructura**, usando dobles para los adaptadores.
- **Mantenibilidad/portabilidad**: el negocio queda protegido de “detalles” cambiantes (drivers, SDKs, frameworks).
- **Disciplina y costo inicial**: exige definir contratos claros y respetar límites; puede resultar **excesiva** en soluciones muy pequeñas o puramente CRUD.

---

## 🌐 Puertos y Adaptadores: distinción clara

- En la hexagonal todo se organiza en torno a la noción de **puertos** (interfaces) y **adaptadores** (implementaciones).
- Los **puertos** están definidos por el **núcleo** y representan lo que el sistema necesita ofrecer o consumir.
- Los **adaptadores** son externos y **traducen** la realidad tecnológica concreta al lenguaje de los puertos.

### ✦ Adaptadores de Entrada (Driving Adapters)

- Son los que **inician** interacciones con el núcleo.
- Se apoyan en **puertos de entrada**, que están **dentro** del hexágono.
- Ejemplo conceptual: un cliente que envía un pedido → el adaptador de entrada (controlador o interfaz) convierte la solicitud en un comando que el puerto de entrada entiende.
- Aquí, la **interfaz y su implementación** (el servicio de aplicación) están dentro del hexágono.

### ✦ Adaptadores de Salida (Driven Adapters)

- Son los que el núcleo necesita para **hacer algo hacia afuera**.
- Se apoyan en **puertos de salida**, definidos dentro del núcleo pero **implementados afuera**.
- Ejemplo conceptual: el sistema necesita guardar un pedido → el núcleo llama al puerto `RepositorioPedidos`, cuya implementación real está en un adaptador de base de datos.
- Aquí se aplica la **inversión de dependencias**: el núcleo define el contrato, y el adaptador lo implementa.

---

## 🔄 Dirección de Dependencias

- La clave de esta arquitectura es que las **dependencias de código siempre apuntan hacia el núcleo**.
- Esto garantiza que el dominio nunca dependa de tecnologías cambiantes.
- El flujo de mensajes puede ir del núcleo hacia afuera, pero el **acoplamiento de código** siempre va hacia adentro.

---

## 📊 Enfatizar el negocio por sobre la infraestructura

- Se propone **diseñar primero la lógica de negocio**.
- La persistencia (DB, repositorios) se piensa como **un adaptador más**, y se define después.
- Resultado: los cambios en reglas de negocio impulsan los cambios en la infraestructura, y no al revés.

---

## ⚙️ Adaptación al contexto moderno

- Con la adopción de **servicios web, APIs REST y terceros**, la arquitectura hexagonal da un **criterio claro** para ubicarlos:
    
    → Todos son **adaptadores de entrada o de salida** alrededor del núcleo.
    
- Esto evita que protocolos, SDKs o frameworks “contaminen” la lógica de negocio.

---

## Consecuencias y Beneficios

1. **Intercambiabilidad tecnológica**: cambiar DB, protocolo o servicio implica reemplazar un adaptador.
2. **Testabilidad alta**: se pueden usar puertos simulados sin levantar infraestructura.
3. **Mantenibilidad y portabilidad**: el negocio permanece intacto frente a cambios técnicos.
4. **Mayor claridad**: se sabe exactamente qué corresponde al núcleo y qué pertenece al entorno.

---

## Descripcion general de funcionamiento

- Nucleo de la aplicacion: contiene la lógica de negocio
y los algoritmos que resuelven el problema
- Puertos: define las interfaces que utiliza la aplicación
para comunicarse con los sistemas externos.
- Adaptadores: implementan los puertos y
proporcionan un puente entre la aplicación y los
sistemas externos
- Edge: maneja la comunicación entre el núcleo de la
aplicación y los sistemas externos
- Sistemas externos: interactúa con la aplicación a
través de los puertos y adaptadores

## Pasos para aplicar la arquitectura hexagonal

1. Identificar la lógica de su dominio principal y encapsularla en una o más clases o módulos. Este es el núcleo de su aplicación.
2. Identificar los actores externos y sus interacciones con su sistema. Estos son sus puertos primarios y secundarios.
3. Definir interfaces abstractas para cada puerto que especifiquen qué métodos u operaciones están disponibles para la comunicación.
4. Implementar adaptadores concretos para cada puerto que manejen la comunicación con los actores externos. Por ejemplo, puede utilizar un framework web para implementar un adaptador web o un controlador JDBC para implementar un adaptador de base de datos.
5. Conectar sus adaptadores a su núcleo mediante inyección de dependencia o inversión de control. De esta manera, puede cambiar o reemplazar fácilmente sus adaptadores sin afectar su núcleo.

---

## 🔎 Comparación con la Arquitectura en Capas

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%204.png)

La **arquitectura en capas** y la **hexagonal** tienen puntos en común (buscan organizar el sistema, separar responsabilidades, reducir acoplamiento), pero difieren en **cómo manejan las dependencias** y el rol del dominio.

### 📐 En Capas

- Organiza por niveles técnicos: **UI → Lógica → Persistencia**.
- El **flujo de dependencias** suele ir de arriba hacia abajo.
- El dominio termina **dependiendo** de detalles de la capa inferior (ORM, SQL, frameworks).
- Puede aparecer **duplicación de lógica**: reglas de negocio mezcladas en varias capas.

### 🛡️ En Hexagonal

- Organiza por **núcleo + puertos/adaptadores**.
- El **flujo de dependencias siempre apunta hacia el dominio**.
- La lógica de negocio es **independiente de DB y frameworks**.
- No se duplica lógica entre capas; el dominio manda y todo lo demás se adapta a él.

📌 **Conclusión**:

La hexagonal lleva un paso más allá la idea de separación, asegurando que la **infraestructura no contamine el negocio**.

---

## 💡 Beneficios destacados de la Hexagonal

1. **Alinea el diseño con el dominio** → la arquitectura refleja las reglas de negocio, no los detalles técnicos.
2. **Evolución tecnológica controlada** → se pueden cambiar DB, frameworks o protocolos sin tocar el núcleo.
3. **Mayor testabilidad** → el dominio se prueba con puertos simulados, sin levantar infraestructura real.
4. **Claridad estructural** → cada cosa tiene su lugar: el negocio en el centro, adaptadores alrededor.
5. **Escalabilidad hacia microservicios** → cada hexágono (núcleo + adaptadores) puede transformarse en un servicio independiente.

---

## ⚠️ Limitaciones

- **Mayor complejidad inicial**: hay que definir contratos (puertos) e implementaciones (adaptadores).
- **Disciplina estricta**: si se rompe la inversión de dependencias, se pierde el beneficio.
- **No siempre justificada**: para sistemas pequeños o puramente CRUD, puede ser una sobrecarga innecesaria.

---

## 📊 Resumen comparativo (Capas vs Hexagonal)

| Aspecto | En Capas | Hexagonal |
| --- | --- | --- |
| Organización | UI, Lógica, Persistencia | Núcleo (dominio) + puertos/adaptadores |
| Dependencias | Descendentes (dominio depende de infraestructura) | Siempre hacia adentro (infra depende del dominio) |
| DB | Central, suele condicionar el diseño | Un adaptador más, intercambiable |
| Frameworks | Integrados dentro de las capas | Periféricos, nunca en el núcleo |
| Testabilidad | Limitada (requiere más capas) | Alta (mocks de puertos) |
| Evolución | Costosa (afecta varias capas) | Localizada (reemplazo de adaptadores) |

---

- La **hexagonal** es presentada como una **evolución natural** de la arquitectura en capas.
- Propone un modelo más **robusto, flexible y alineado con el dominio**, especialmente útil en contextos modernos con **múltiples interfaces** (web, mobile, APIs, eventos, servicios externos).
- Refuerza los principios de **alta cohesión, bajo acoplamiento** y **inversión de dependencias**.

---

# 📌 Arquitectura Cliente/Servidor

## 🌐 Definición general

La arquitectura **Cliente/Servidor** es uno de los estilos fundamentales en software distribuido.

- **Cliente**: entidad que solicita servicios.
- **Servidor**: entidad que ofrece servicios.
- El **cliente inicia la comunicación**, mientras que el servidor atiende y responde.
- Los **clientes conocen a los servidores** (deben saber cómo conectarse), pero los servidores **no necesitan conocer a los clientes** de antemano.

👉 Cliente y Servidor son procesos lógicos que pueden ejecutarse en la misma máquina o en equipos distintos conectados por red.

---

## ✦ Variantes principales

### 🔹 Two-tier (2 capas)

- El cliente se conecta directamente al servidor.
- Ejemplo típico: aplicación de escritorio que se comunica con un servidor de base de datos.
- Limitación: el cliente debe manejar mucha lógica, y cualquier cambio en la DB afecta directamente al cliente.

### 🔹 Three-tier (3 capas)

- Interpone una **capa intermedia de aplicación/servicios** entre cliente y servidor de datos.
- Estructura: **Cliente → Servidor de Aplicaciones → Servidor de Base de Datos**.
- Ventaja: separa la lógica de negocio del cliente y de la base de datos, centralizando validaciones y procesos en el servidor de aplicaciones.

### 🔹 N-tier (multicapa)

- Extiende la idea de 3 capas, permitiendo varias capas intermedias según las necesidades (web server, application server, middleware, servicios distribuidos).
- Brinda **flexibilidad en la distribución**: cada capa puede estar en una máquina diferente o compartida.
- Se adapta bien al mundo web moderno, donde el cliente suele ser un **navegador**, conectado a un servidor web, y este a su vez a otros servicios internos.

---

## ⚙️ Propiedades clave (de las variantes cliente-servidor)

- **Separación de intereses**: presentación, lógica y datos se ubican en capas distintas.
- **Comunicación sincrónica**: suele ser del tipo “request/response” → el cliente espera la respuesta antes de continuar.
- **Distribución flexible**: el sistema puede desplegarse en una sola máquina o en varias según convenga.

---

## ✅ Ventajas de Cliente/Servidor

1. **Centralización**
    - Los datos y la lógica central están en el servidor.
    - Evita que los clientes mantengan información desactualizada.
2. **Seguridad**
    - El servidor suele estar protegido por **firewalls** o **subredes**, lo que restringe el acceso directo a la base de datos.
    - Facilita controlar accesos y permisos desde un único punto.
3. **Facilidad de instalación (en el cliente)**
    - El cliente suele ser una aplicación ligera (thin client) sin dependencias complejas.
    - Se instala y actualiza con facilidad.
4. **Separación de responsabilidades**
    - El cliente se ocupa de la presentación.
    - El servidor concentra la lógica y los datos.
5. **Portabilidad**
    - Se puede desplegar cliente y servidor en plataformas diferentes.
    - Ejemplo: servidor en Linux y cliente multiplataforma (Windows, Mac).

---

## ⚠️ Desventajas de Cliente/Servidor

1. **Actualizaciones de clientes pesados**
    - Si el cliente tiene mucha lógica, actualizarlo en múltiples terminales puede ser complejo.
2. **Concurrencia**
    - Muchos clientes simultáneos pueden saturar al servidor.
    - Se necesitan estrategias de escalamiento, pero la sobrecarga siempre es un problema.
3. **Todo o nada**
    - Si el servidor falla, todos los clientes quedan inoperables.
    - Punto único de fallo.
4. **Protocolos de bajo nivel**
    - Sockets, HTTP, RPC: implican complejidad para los desarrolladores.
5. **Dificultad de depuración**
    - Los errores se distribuyen entre clientes y servidor.
    - Es complicado rastrear trazas completas cuando no se tiene acceso a todos los nodos.

---

## 🖥️ Cliente/Servidor según la época y capacidades

- **Escritorio + Servidor de Base de Datos**
    - Arquitectura típica de los 90: la presentación está en el cliente (desktop app) y la carga intensiva en el servidor de base de datos.
- **Navegador + Servidor Web**
    - Con la llegada del desarrollo web: navegador (cliente) ↔ servidor web (backend) ↔ base de datos.
    - Modelo más común en la actualidad.
- **Tres niveles**
    - Frontend (HTML, JS), servidor de aplicaciones, servidor de base de datos.
    - Cada capa con responsabilidades específicas y posibilidad de desplegarse de manera independiente.

---

## 🔧 Patrones derivados de Cliente/Servidor

### 🔹 Patrón de acceso a datos remoto

- El cliente ejecuta lógica local pero consulta datos en un servidor remoto.

### 🔹 Patrón de presentación remota

- La presentación ocurre en el cliente, pero la lógica de negocio está completamente en el servidor.

### 🔹 Patrón de lógica dividida

- La funcionalidad de la aplicación se divide entre cliente y servidor.
- Requiere coordinación más compleja, ya que ambos lados deben tener programas de aplicación compatibles.

---

# 📌 Patrón Maestro–Esclavo (Master–Slave)

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%205.png)

## 🌐 Definición

El patrón **Maestro–Esclavo** organiza el sistema de manera jerárquica:

- Existe un **proceso maestro** encargado de la **computación, coordinación y comunicación**.
- Existen uno o varios **procesos esclavos**, subordinados, que ejecutan tareas específicas bajo el control del maestro.

El maestro distribuye trabajo entre los esclavos, recibe sus resultados y los combina para producir la salida final.

---

## ⚙️ Características

- Comúnmente usado en **sistemas de tiempo real**, donde el cumplimiento de plazos de procesamiento es crítico.
- El maestro se responsabiliza por la **asignación de tareas**, la sincronización y la integración de resultados.
- Los esclavos son dedicados a **acciones específicas**, como adquisición de datos, procesamiento parcial o monitoreo.
- Adecuado cuando es posible **predecir el procesamiento distribuido** y asignarlo claramente a cada esclavo.

---

## ✅ Ventajas

1. **Paralelismo** → los esclavos pueden trabajar en paralelo, mejorando el rendimiento.
2. **Escalabilidad** → se pueden agregar más esclavos para incrementar capacidad.
3. **Aislamiento de fallos** → si un esclavo falla, el maestro puede reasignar la tarea a otro.
4. **Organización clara** → el maestro centraliza la lógica, simplificando la coordinación.

---

## ⚠️ Desventajas

1. **Dependencia del maestro** → si el maestro falla, el sistema completo deja de funcionar.
2. **Sobrecarga de comunicación** → el maestro debe coordinar constantemente, lo que puede generar cuellos de botella.
3. **Menor flexibilidad** → los esclavos suelen estar especializados, no es trivial reasignarles otras funciones.

---

## 📊 Ejemplo del apunte (control de tráfico multiprocesador)

- **Maestro**: proceso central que coordina la lógica de control de semáforos.
- **Esclavos**:
    - Procesadores que manejan sensores de flujo de tráfico.
    - Procesadores que controlan los semáforos.
    - Consolas de operadores que muestran el estado del sistema.

Cada esclavo se dedica a una tarea muy acotada, mientras que el maestro integra toda la información y ejecuta la estrategia de control.

---

Síntesis:

El patrón **Maestro–Esclavo** es útil para sistemas donde se requiere procesamiento paralelo y coordinación centralizada. Se aplica en contextos de tiempo real (ej. tráfico, sensores), ofrece escalabilidad y paralelismo, pero depende críticamente de la disponibilidad del maestro.

---

# 📌 Arquitectura Peer-to-Peer (p2p)

## 🌐 Introducción

El **estilo p2p** surge como alternativa al modelo Cliente/Servidor jerárquico.

En este enfoque, los **nodos se comunican entre sí como pares (peers)** y las relaciones jerárquicas quedan prohibidas.

- No hay un nodo maestro ni central único.
- Cada nodo puede ser tanto **cliente** (consumidor de servicios) como **servidor** (proveedor).
- Esto da lugar a un sistema **distribuido, descentralizado y flexible**.

---

## ⚙️ Características fundamentales

1. **Igualdad entre nodos**: no existen roles fijos, los peers cambian de rol según necesidad.
2. **Descentralización total**: no depende de un servidor central, la lógica y los recursos se reparten.
3. **Flexibilidad**: un nodo puede proveer servicios en un momento y consumir en otro.
4. **Vista de ejecución (runtime)**: este estilo se describe por cómo funciona dinámicamente el sistema en tiempo real.

---

## 📊 Clasificación de arquitecturas p2p

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%206.png)

### 🔹 Peer-to-Peer Puro

- Todos los nodos tienen exactamente las mismas capacidades.
- Ninguno cumple un rol privilegiado.
- Ejemplo conceptual: redes de compartición de archivos sin servidor de índices.

### 🔹 Peer-to-Peer Híbrido

- Combina características p2p con ciertos elementos centralizados.
- Ejemplo clásico: redes donde existe un **servidor central de índices** (que sabe qué archivos tiene cada peer), pero la transferencia de datos ocurre entre pares.
- Mejora la localización de recursos, aunque introduce cierta dependencia de los nodos centrales.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%207.png)

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%208.png)

---

## ✅ Ventajas del enfoque p2p

1. **Escalabilidad** → al aumentar nodos, aumentan recursos compartidos.
2. **Tolerancia a fallas** → no hay un único punto de fallo, la red sigue funcionando aunque fallen nodos.
3. **Balanceo natural de carga** → los recursos (procesamiento, almacenamiento, ancho de banda) se distribuyen entre los pares.
4. **Autonomía dinámica** → los nodos pueden entrar y salir sin interrumpir la red global.
5. **Costos reducidos de infraestructura** → no requiere mantener servidores centrales de gran capacidad.

---

## ⚠️ Desventajas del enfoque p2p

1. **Complejidad de coordinación** → difícil mantener consistencia y sincronización de datos.
2. **Seguridad vulnerable** → mayor superficie de ataque porque cada nodo es potencialmente accesible.
3. **Imprevisibilidad en el rendimiento** → depende de la disponibilidad y estabilidad de los peers conectados.
4. **Sobrecarga de comunicación** → localizar y coordinar recursos en redes muy grandes puede generar retrasos.
5. **Mantenimiento difícil** → sin un nodo central, la administración global de la red se complica.

---

📖 **Síntesis:**

La arquitectura **Peer-to-Peer** elimina jerarquías, otorgando a todos los nodos la capacidad de ser cliente y servidor. Existen variantes puras (totalmente descentralizadas) e híbridas (con algún componente central de apoyo). Ofrece **alta escalabilidad, resiliencia y eficiencia en el uso de recursos**, pero con costos en **seguridad, coordinación y predictibilidad del rendimiento**.

---

Perfecto 🙌. Estas tres diapositivas de las págs. **68, 69 y 70** no son todavía un estilo arquitectónico en sí (como p2p o MVC), sino que funcionan como una **motivación previa** para explicar por qué ciertos **estilos de diseño** (como MVC, que arranca en la pág. 71) son tan relevantes en el desarrollo de aplicaciones **web y mobile**.

---

## Motivación de estilos para aplicaciones web/mobile

Los sistemas modernos deben poder adaptarse a cambios frecuentes y rápidos. Se mencionan cuatro escenarios típicos que motivan arquitecturas flexibles:

1. **Cambios en la interfaz de usuario solicitados por el cliente**
    - Ej.: mejorar la usabilidad, adaptabilidad o accesibilidad.
    - Los estilos arquitectónicos deben permitir modificar la UI sin impactar la lógica de negocio.
2. **Migración entre plataformas**
    - Ej.: pasar de escritorio a web, de web a mobile, etc.
    - El diseño debe facilitar portar la aplicación sin rehacer el núcleo.
3. **Actualizaciones del sistema a nuevas versiones**
    - El sistema debe evolucionar sin necesidad de reescribir todo.
4. **Cambios en el diseño de la base de datos**
    - La arquitectura debe tolerar modificaciones en la persistencia sin afectar la lógica ni la UI.

👉 En resumen, la motivación es **evitar que cambios en un área (UI, BD, plataforma) afecten al resto del sistema**.

---

## Principales aspectos a considerar en estilos para aplicaciones web

Se enumeran tres requisitos clave que guiarán la elección de estilos como **MVC**:

1. **Representación en múltiples formatos y ventanas**
    - La misma información debería poder mostrarse de diferentes maneras (ej.: tabla, gráfico, lista).
    - Esto implica separar la lógica de datos de la representación.
2. **Cambio dinámico de la interfaz de usuario**
    - La UI debería poder modificarse fácilmente, incluso en tiempo de ejecución, sin comprometer la lógica.
3. **Soporte para múltiples “look and feel”**
    - Cambiar la apariencia o el estilo visual de la aplicación no debería requerir cambios en el código de la lógica.
    - Esto exige una clara separación entre presentación y lógica.

👉 Estos puntos apuntan directamente a la necesidad de estilos que promuevan **separación de responsabilidades** (SoC, *Separation of Concerns*) y **modularidad** → lo que justifica la introducción del patrón **MVC (Model–View–Controller)** 

---

# 📌 Arquitectura **MVC (Model–View–Controller)**

---

## 🌐 Introducción

El patrón **MVC** surge como respuesta a los problemas planteados en las págs. 68–70: necesidad de separar responsabilidades, soportar múltiples interfaces de usuario, permitir cambios en la presentación sin afectar la lógica, y viceversa.

MVC divide el sistema en **tres componentes principales**:

- **Modelo (Model)**
- **Vista (View)**
- **Controlador (Controller)**

---

## ⚙️ Componentes principales

### 🔹 Modelo (Model)

- Representa los **datos** y la **lógica de negocio**.
- Gestiona el acceso, modificación y validación de la información.
- Es **independiente de la interfaz**: no “sabe” cómo se mostrarán sus datos.
- Puede notificar a las vistas cuando cambia el estado.

### 🔹 Vista (View)

- Encargada de la **representación visual de los datos** del modelo.
- Puede haber múltiples vistas mostrando la misma información de formas distintas (ej.: tabla, gráfico, formulario).
- No tiene lógica de negocio.

### 🔹 Controlador (Controller)

- Actúa como **intermediario** entre vista y modelo.
- Recibe las **entradas del usuario** (clics, comandos, formularios).
- Interpreta la acción y decide qué operaciones realizar en el modelo o qué vista actualizar.

---

## 🔄 Flujo de interacción

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%209.png)

1. El **usuario interactúa** con la **Vista**.
2. La Vista delega al **Controlador** la interpretación de la acción.
3. El **Controlador actualiza el Modelo** (agrega datos, valida, procesa).
4. El **Modelo cambia su estado** y **notifica a las Vistas** asociadas.
5. Las **Vistas se actualizan** automáticamente para reflejar los cambios.

👉 Resultado: se logra una **separación clara de responsabilidades** y una **sincronización automática** entre modelo y vista.

---

## ✅ Ventajas de MVC

1. **Separación de responsabilidades** → cada capa se centra en su función específica.
2. **Reutilización** → el mismo Modelo puede conectarse con múltiples Vistas.
3. **Flexibilidad** → se pueden cambiar interfaces gráficas sin modificar la lógica de negocio.
4. **Mantenibilidad** → los cambios se concentran en el componente correspondiente (UI, lógica, flujo).
5. **Soporte para múltiples interfaces** → web, mobile, desktop, todas sobre el mismo modelo.

---

## ⚠️ Desventajas de MVC

1. **Complejidad inicial** → más componentes a coordinar.
2. **Curva de aprendizaje** → no siempre resulta intuitivo para equipos novatos.
3. **Sobrecarga de comunicación** → puede haber mayor interacción entre capas para operaciones simples.
4. **Dificultad en proyectos pequeños** → en aplicaciones muy simples puede ser innecesariamente complejo.

---

## 📊 Contexto de uso

- MVC es ampliamente utilizado en **aplicaciones web y móviles**, donde la separación entre **código de presentación** y **lógica de negocio** es crucial.
- Es base de numerosos frameworks: **Spring MVC, Angular, React (con variantes), Django, Ruby on Rails**.
- Justifica su importancia en la capacidad de adaptarse a cambios frecuentes en UI y en la posibilidad de ofrecer múltiples representaciones de un mismo modelo.

---

## 📌 Ejemplos actuales

1. **Spring Boot (Java – Backend):**
    - `@RestController` = Controlador
    - `@Entity` + repositorios JPA = Modelo
    - `@RestController` devuelve JSON → que actúa como Vista en APIs.
2. **Django (Python):**
    - Models = base de datos
    - Views = lógica
    - Templates = interfaz HTML
3. **React/Angular:**
    - No se llama MVC, pero tienen la misma idea de separar capas (componente → controlador, estado → modelo, JSX/HTML → vista).

---

📖 **Síntesis:**

El patrón **MVC** divide la aplicación en **Modelo, Vista y Controlador**, logrando separación de responsabilidades, flexibilidad y reutilización. Responde a la necesidad de soportar cambios rápidos en UI, migraciones de plataforma y representación múltiple de datos. Sus beneficios principales son mantenibilidad y escalabilidad lógica, aunque introduce cierta complejidad inicial.

---

# 📌 Arquitectura SOA (Service-Oriented Architecture)

## 🌐 Introducción

La **Arquitectura Orientada a Servicios (SOA)** es un estilo arquitectónico que organiza un sistema en **servicios independientes**. Cada servicio implementa una funcionalidad del negocio y se comunica con otros mediante interfaces bien definidas y protocolos estándar.

La idea central es que los servicios sean **autónomos, reutilizables, componibles y descubiertos dinámicamente**, favoreciendo la **interoperabilidad** entre aplicaciones heterogéneas.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2010.png)

---

## ⚙️ Conceptos principales

### 🔹 Servicio

- Unidad básica de construcción en SOA.
- Encapsula lógica de negocio o una funcionalidad concreta.
- Expone su comportamiento a través de una **interfaz pública**.

### 🔹 Contrato

- Define cómo se accede al servicio.
- Incluye las operaciones que ofrece, el formato de los mensajes y los protocolos de comunicación.
- Ejemplos: **WSDL** en servicios SOAP, **OpenAPI** en servicios REST.

### 🔹 Comunicación basada en mensajes

- Los servicios se comunican mediante el **intercambio de mensajes estandarizados** (XML, JSON, etc.).
- Estos mensajes permiten que aplicaciones escritas en distintos lenguajes se integren sin problemas.

### 🔹 Bajo acoplamiento

- Los consumidores dependen solo de la **interfaz pública**, nunca de la implementación interna.
- Se puede cambiar la lógica interna sin afectar a los clientes.

---

## 🔄 Propiedades de SOA

1. **Autonomía** → cada servicio se ejecuta de manera independiente.
2. **Reutilización** → un mismo servicio puede ser consumido en múltiples aplicaciones.
3. **Composición** → los servicios se pueden combinar para formar procesos de negocio completos.
4. **Interoperabilidad** → facilita integrar sistemas desarrollados con distintas tecnologías.
5. **Descubrimiento dinámico** → mediante un registro de servicios, las aplicaciones pueden localizar y consumir servicios en tiempo de ejecución.

---

## 📊 Ejemplo conceptual (págs. 81–82)

Un sistema bancario podría diseñarse con distintos servicios:

- **Servicio de cuentas** (crear, consultar, cerrar cuentas).
- **Servicio de préstamos** (simulaciones, aprobación, amortización).
- **Servicio de pagos** (transferencias, débitos automáticos).
- **Servicio de notificaciones** (correo electrónico, SMS).

Cada servicio es autónomo, pero pueden **colaborar entre sí** para cubrir un proceso completo, como una operación de transferencia que requiere cuentas, pagos y notificaciones.

---

📖 **Síntesis:**

SOA organiza sistemas en torno a **servicios autónomos con contratos estandarizados**, que se comunican mediante mensajes. Estos servicios son **reutilizables, componibles y de bajo acoplamiento**, lo que permite construir aplicaciones flexibles y facilitar la integración de sistemas heterogéneos.

---

## Formas de aplicarlo y caracteristicas

### 1. **SOA con Web Services (SOAP)**

- Es la **implementación clásica** de SOA.
- Se basa en usar **Web Services SOAP** con **WSDL (contrato)**, **XML** como formato de mensajes y estándares como **WS-Security, WS-ReliableMessaging**.
- Aporta **interoperabilidad fuerte** entre plataformas heterogéneas.

📌 Es decir, es **un mecanismo tecnológico para llevar SOA a la práctica**.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2011.png)

---

### 2. **SOA: Composición de Servicios**

- Explica cómo, a partir de servicios **atómicos** (ej. CRUD), se pueden **componer servicios más complejos. Puede que en el primer nivel sean servicios basicos de CRUD y los servicios de mas nivel son operaciones completas:**
    - Servicios de negocio → combinan reglas.
    - Procesos compuestos → orquestaciones (p.ej., “reservar vuelo + hotel + auto”).
- Se diferencia entre:
    - **Orquestación** (flujo centralizado, BPM/BPEL).
    - **Coreografía** (cada servicio reacciona a eventos).

📌 Esto es **una característica de diseño en SOA** → permite flexibilidad y reutilización.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2012.png)

---

### 3. **SOA con Service Bus (ESB)**

- Introduce el uso de un **Enterprise Service Bus** como middleware para:
    - **Mediar mensajes** (transformaciones, enrutamiento, seguridad, monitoreo).
    - Evitar integraciones punto a punto (arquitectura “spaghetti”).
    - Centralizar aspectos transversales sin meter lógica de negocio.

📌 Es **una variante tecnológica de implementación**, donde SOA se apoya en un **bus de servicios**.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2013.png)

---

## SOA: Ventajas y Desventajas

## ✅ Ventajas

1. **Reduce el acoplamiento**
    - Los servicios se consumen a través de interfaces bien definidas.
    - Esto permite que la implementación interna cambie sin afectar a los clientes.
2. **Testabilidad**
    - Cada servicio es una unidad autónoma → se puede probar de manera individual.
3. **Reutilización**
    - Los servicios se diseñan para cumplir una sola responsabilidad (principio SRP).
    - Pueden reutilizarse en distintos procesos o aplicaciones.
4. **Agilidad**
    - Permite crear **nuevos servicios** combinando o componiendo servicios existentes.
5. **Escalabilidad**
    - Es posible desplegar varias instancias de un servicio y balancear la carga (ej.: con un ESB).
6. **Interoperabilidad**
    - Facilita la integración de aplicaciones heterogéneas mediante el uso de estándares abiertos (SOAP, XML, JSON, HTTP, etc.).

---

## ⚠️ Desventajas

1. **Performance**
    - Las llamadas entre servicios introducen latencia de red y overhead de protocolos (XML, SOAP).
    - Menor rendimiento que arquitecturas monolíticas.
2. **Volumetría**
    - No es recomendable cuando se necesita procesar **grandes volúmenes de datos en tiempo real**.
    - Puede saturar la red o aumentar los tiempos de respuesta.
3. **Gobernanza**
    - Es necesario establecer un **marco de gobierno** para controlar la creación, uso y versionado de servicios.
    - Sin gobernanza, puede generarse “caos de servicios” (duplicación, inconsistencias).
4. **Más puntos de falla**
    - Cada servicio expone un endpoint → aumenta la superficie de posibles fallas.
    - Requiere estrategias de **tolerancia a fallos**, **reintentos** y **monitorización**.

---

# 📌 Event-Driven Architecture (EDA)

---

## 🌱 Qué es EDA

- **Arquitectura asíncrona y distribuida**, pensada para aplicaciones **altamente escalables**.
- Las apps **emiten “eventos”** (hechos significativos) para que otros componentes **reaccionen**, los **procesen** y, si corresponde, **generen nuevos eventos** que continúan el flujo.
- **Evento** = cambio relevante en el estado del sistema (dato que cambió o acción ocurrida) que **merece ser observado** para tomar decisiones.

## 🌐 ¿Qué es EDA en sí?

La **Event-Driven Architecture (EDA)** es un **estilo de arquitectura** donde todo gira alrededor de **eventos**.

Un **evento** es simplemente *algo que pasó y es relevante para el sistema* (ejemplo: “se creó un pedido”, “se registró un pago”, “un sensor detectó temperatura alta”).

En lugar de que un módulo **llame directamente** al otro (como en Cliente/Servidor o en SOA con servicios invocándose), en EDA los módulos **publican eventos** en un canal común, y otros módulos que estén interesados los **escuchan y reaccionan**.

👉 O sea: **ya no hay un control central del flujo**, sino que el sistema se mueve solo en base a los hechos que ocurren.

---

## 🧩 Componentes y flujo

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2014.png)

- **Events**: los mensajes que describen hechos (inmutables).
- **Event Bus**: canal común donde se **publican** y **distribuyen** los eventos.
- **Event Channels**: canales/temas/colas específicos dentro del bus (segmentan por tipo de evento).
- **Event Processing** (consumidores): componentes que **se suscriben** a uno o más canales y **procesan** los eventos recibidos.
    
    Resultado: publicación → distribución por canales → procesamiento por los consumidores suscriptos.
    

---

## 🔀 Topologías de EDA

### A) **Con topología Broker (choreography)**

- **Sin control central del proceso**: no hay un “director” del flujo.
- Los **procesadores** consumen eventos y **pueden emitir otros** que activan nuevos procesadores, formando una **cadena de eventos**.
- **No** hace falta administrar conexiones punto a punto; el **broker** enruta por canales/temas.
- Pros: **desacoplamiento máximo**, alta escalabilidad.
    
    Contras: **coordinación implícita**, más difícil **trazar/depurar** el flujo end-to-end.
    
    ![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2015.png)
    

### B) **Con topología Mediator (orchestration)**

- **Hay un componente central** (**Event Mediator**) que **decide qué procesadores** se invocan y **en qué orden** cuando llega un evento.
- Pueden existir **múltiples mediadores** (en paralelo o jerárquicos).
- Pros: **control del flujo**, orden y políticas claras.
    
    Contras: riesgo de **cuello de botella** / **punto único de fallo** si no se diseña redundante.
    
    ![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2016.png)
    

> Regla mental: Broker = coreografía (cada uno baila reaccionando a la música de eventos).
> 
> 
> **Mediator = orquestación** (un director indica quién entra y cuándo).
> 

---

## ✅ Ventajas de EDA

- **Escalabilidad**: cada consumidor puede **escalar de forma independiente**; acoplamiento mínimo.
- **Despliegue**: bajo acoplamiento → **desplegás** sin preocuparte por dependencias estrictas; los componentes **solo se suscriben** y reaccionan.
- **Performance** (matiz): hay **pasos y colas** que agregan latencia, **pero** la **naturaleza asíncrona** permite **procesamiento en paralelo**, compensando en escenarios de alto volumen.
- **Flexibilidad**: cambios puntuales se **aislan** en un procesador; para un nuevo requerimiento **agregás** un procesador que escuche cierto evento sin tocar el resto.

---

## ⚠️ Desventajas de EDA

- **Testabilidad**: probar fin-a-fin es más complejo; hay que **simular eventos**, esperar **cadenas** de procesamiento y **verificar el resultado final**.
- **Desarrollo**: programar asíncrono es difícil; se necesitan **manejadores de errores** avanzados para que la falla de un procesador **no cause inconsistencias** en el resto (reintentos, idempotencia, compensaciones).

---

## 🧭 Cuándo elegir **Broker** vs **Mediator**

- **Broker**: dominios **altamente desacoplados**, muchas reacciones **independientes**, alta **escalabilidad** y tolerancia a fallas; aceptás menor control explícito del flujo.
- **Mediator**: procesos con **secuencia/negocio bien definidos**, **reglas de orden**, SLA por paso; preferís **gobernar el flujo** aunque cueste más mantener el “director”.

---

# 📌 Variantes de Event-Driven Architecture (EDA)

1. **Publish & Subscribe**
    - Modelo clásico donde un **productor publica un evento** en un canal/tópico, y **múltiples consumidores suscriptos** lo reciben.
    - Alta difusión: el productor no conoce a los consumidores → bajo acoplamiento.
    - Ideal para notificaciones y distribución masiva de información.
2. **Broker**
    - El flujo de eventos pasa por un **Event Broker** que los distribuye a los consumidores.
    - El broker decide el enrutamiento según **tópicos, reglas o contenido**.
    - Favorece la escalabilidad en sistemas grandes y con muchos consumidores.
3. **Messaging**
    - Los eventos se transmiten a través de **colas de mensajes**.
    - Los consumidores las procesan de manera asíncrona y en orden.
    - Garantiza confiabilidad (mensajes no se pierden), pero puede introducir más latencia.
4. **Process Coordinator** (también llamado *Mediator*)
    - Existe un **coordinador central** que recibe eventos y define qué pasos o procesos deben ejecutarse.
    - Útil cuando el flujo de eventos sigue una **secuencia definida** o requiere reglas de negocio complejas.
    - Permite orquestar procesos de principio a fin.

---

# 📌 Arquitectura Publish–Subscribe

## ¿Qué es?

- En lugar de que **A le hable directamente a B**, el publicante (**Publisher**) envía mensajes a un **tópico/canal**.
- Los interesados (**Subscribers**) se “anotan” a ese canal.
- Cuando hay un evento nuevo, **todos los suscriptores lo reciben automáticamente**.
- Los publicantes no saben quiénes son los suscriptores → **desacoplamiento total**.

## 1. Esquema básico

- **Publicante (Publisher)** → Componente que genera un evento y lo publica en un **tópico**.
- **Tópico** → Canal intermedio donde se “depositan” los eventos.
- **Suscriptores (Subscribers)** → Componentes que se registran en un tópico y reciben los eventos cuando ocurren.

👉 Flujo:

1. El publicante crea y publica un evento en un tópico.
2. Los suscriptores registrados reciben ese evento automáticamente.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2017.png)

---

## 2. Conceptos clave

- Los **componentes son independientes**: unos publican eventos y otros se suscriben.
- Los publicantes **no saben quién consume** sus eventos, y los suscriptores **no saben quién los generó** → solo importa el evento.
- Cada tópico puede tener **varios publicantes** y los publicantes pueden aparecer/desaparecer dinámicamente → gran **flexibilidad**.
- Los suscriptores pueden **suscribirse o des-suscribirse** dinámicamente a un tópico.
- Esto aplica a **tiempo de ejecución (runtime)** → es parte de la vista de ejecución de la arquitectura.

---

## 3. Diagrama conceptual UML

- Muestra varios **componentes** conectados a un canal común.
- Algunos componentes tienen la estereotipación «publish» (emiten eventos).
- Otros «subscribe» (reciben eventos).
- La idea visual es reforzar que todos interactúan a través de un **bus compartido**, sin vínculos directos entre publicante y suscriptor.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2018.png)

---

## 4. Propiedades clave

- **Mensajería muchos a muchos** → un evento publicado puede llegar a muchos suscriptores. También, varios publicantes pueden publicar en el mismo tópico.
- **Calidad de Servicio (QoS) configurable** →
    - Los mensajes pueden enviarse de forma segura o no.
    - Se puede configurar comunicación **punto-a-punto**, **multicast** o **broadcast**.
    - Ejemplo: un evento puede llegar a todos los suscriptores o solo a algunos seleccionados.
- **Bajo acoplamiento** →
    - Los publicantes no saben quién recibe su mensaje.
    - Los suscriptores no saben quién lo generó.
- **Consideración importante** → Publish/Subscribe es ideal para sistemas **muy flexibles**, con necesidades de mensajería asincrónica **uno-a-muchos, muchos-a-uno o muchos-a-muchos**.

---

## 5. Beneficios y propiedades adicionales

- **Principal beneficio**:
    - **Desacoplar productores y consumidores** → la arquitectura se vuelve **mantenible y evolucionable**.
- **Capa de indirección**:
    - El bus de eventos agrega una capa entre productor y consumidor.
    - Puede impactar la performance, pero a cambio se gana escalabilidad y flexibilidad.
    - Hay mecanismos reutilizables para compensar la performance (optimización de buses).

### 🔧 Propiedades de los canales (buses) de eventos

- **Durables** → garantizan que ningún mensaje se pierde ante una falla. Se logra persistiendo temporalmente los mensajes, lo que puede introducir cierta latencia.
- **Entrega en orden** → aseguran que los eventos se reciban en el mismo orden en que fueron publicados, o de forma priorizada.
- **Entrega en lotes** → los eventos se agrupan antes de entregarse, útil para procesar grandes volúmenes y evitar sobrecargas por eventos similares.

---

## 📖 Síntesis Publish & Subscribe

El patrón **Publish–Subscribe** desacopla productores y consumidores de eventos mediante tópicos. Permite comunicación **muchos-a-muchos**, flexibilidad en la suscripción dinámica y configuración de calidad de servicio. Aporta **mantenibilidad y escalabilidad**, aunque puede introducir **latencia adicional** debido al bus de eventos. Es ideal para aplicaciones donde múltiples componentes deben reaccionar a un mismo hecho sin estar directamente conectados.

---

Perfecto 🙌, vamos a desglosar bien la **Arquitectura Agente (Broker)** con las diapositivas que compartiste y luego te explico la relación con la **topología broker** que vimos antes en EDA.

---

# 📌 Arquitectura Agente (Broker)

## 1. Esquema básico

- **Remitentes (senders/producers)** → generan mensajes y los envían a un **puerto de entrada** del broker.
- **Broker (agente)** → componente central que recibe mensajes, los procesa, los enruta y los transforma según sea necesario.
- **Receptores (receivers/consumers)** → reciben los mensajes ya procesados desde los **puertos de salida** del broker.

👉 El broker funciona como **intermediario** entre productores y consumidores, agregando lógica de enrutamiento y transformación.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2019.png)

---

## 2. Propiedades clave del patrón

- **Arquitectura Hub-and-Spoke**
    - El broker es el “hub” (concentrador) y los remitentes/receptores son los “spokes” (rayos). (Metafora de una rueda)
    - Todos se conectan al broker, no entre sí directamente.
- **Ruteo de mensajes**
    - El broker decide **a qué puerto de salida** debe enviar un mensaje recibido.
    - Ejemplo: un mensaje de “Orden” se envía al servicio de facturación; un mensaje de “Stock” al servicio de inventario.
- **Transformación de mensajes**
    - El broker puede **cambiar el formato del mensaje** de entrada al que requiere el receptor.
    - Ejemplo: transformar JSON a XML, o adaptar un esquema de datos viejo a uno nuevo.

---

## 3. Consideraciones

- Es útil cuando los sistemas **intercambian mensajes con distintos formatos** (heterogeneidad).
- Permite **desacoplar remitente y receptor**:
    - Cada uno trabaja en su formato nativo.
    - El broker centraliza la lógica de transformación.
- Esto **simplifica el desarrollo y el mantenimiento**, porque la lógica de integración está en un único lugar.

---

### Relación con la “Topología Broker” que vimos en EDA

Cuando vimos **EDA con topología Broker**, nos referíamos al **patrón de interacción entre componentes**:

- Los eventos circulan a través de un broker de mensajería, que se encarga de distribuirlos.
- Allí el énfasis estaba en cómo se **orquesta la reacción a eventos** sin un control central explícito (flujo de coreografía).

En cambio, aquí en la **Arquitectura Agente (Broker)** hablamos de un **patrón de integración de mensajes**:

- El broker no solo distribuye, sino que también **procesa, enruta y transforma mensajes**.
- Se asemeja a un **Enterprise Service Bus (ESB) simplificado**.

👉 Podés pensar que:

- **Topología Broker (EDA)** = cómo se organizan los eventos en el runtime (flujo descentralizado).
- **Arquitectura Agente Broker** = un estilo de implementación para integrar remitentes y receptores a través de un intermediario inteligente.

---

📖 **Síntesis**

El **Agente Broker** es un patrón donde un componente central actúa como intermediario: recibe mensajes de múltiples remitentes, los procesa (ruteo + transformación) y los entrega a los receptores correctos. Se usa en escenarios de **heterogeneidad tecnológica** y necesidad de **desacoplar productores de consumidores**.

---

¡Genial! 🙌 Vamos con la **tercera variante de EDA: Arquitectura de Mensajería (Messaging)**. Te reescribo y organizo la info de las diapositivas que pasaste (págs. 101–103).

---

# 📌 Arquitectura Comunicando (Messaging)

## 1. Esquema básico (diapositiva 101)

- Los **clientes (emisores)** envían mensajes a una **cola intermedia**.
- La cola almacena los mensajes hasta que un **servidor (receptor)** los consume.
- Esto permite que clientes y servidores estén **desacoplados en el tiempo** (el servidor no necesita estar disponible cuando se envía el mensaje).

👉 Se basa en el uso de **colas de mensajería** (Message Queue).

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2020.png)

---

## 2. Conceptos principales

- **Desacopla emisores de receptores** → el cliente no necesita conectarse directamente al servidor.
- El emisor envía un mensaje con la certeza de que **eventualmente será entregado**, aunque la red esté caída o el receptor no esté disponible en ese momento.
- Este patrón promueve **bajo acoplamiento y alta modificabilidad**, ya que emisores y receptores evolucionan por separado.
- Problema posible → si los **formatos de mensajes** cambian en el receptor, puede impactar a los emisores.
- Solución → usar **mensajes auto-descriptivos** que reduzcan dependencias (ej.: incluir metadatos sobre la versión o el tipo de mensaje).

---

## 3. Propiedades clave

- **Comunicación asíncrona**
    - Los clientes publican en la cola y siguen con su trabajo sin esperar respuesta inmediata.
    - Los servidores procesan cuando estén disponibles.
- **Calidad de Servicio (QoS) configurable**
    - Se puede configurar para:
        - Alta velocidad, pero sin garantías (menos confiable).
        - Menor velocidad, pero confiable y persistente (ej. coordinado con transacciones de base de datos).
- **Bajo acoplamiento**
    - Ni el cliente sabe cuál servidor procesará su mensaje, ni el servidor sabe qué cliente lo envió.
    - La cola actúa como capa de indirección.

---

## 4. Consideraciones adicionales

- Aplicaciones que pueden dividir un pedido en **pasos discretos conectados por colas** utilizan una extensión llamada **patrón Pipe & Filter**.
- Es una **solución resiliente** → funciona bien en entornos con **conectividad inestable** o servidores poco confiables, ya que la cola retiene los mensajes.
- También puede implementarse en **modo síncrono**:
    - Se usan dos colas → una para pedidos y otra para respuestas.
    - Permite al cliente enviar un mensaje y esperar respuesta a través de la segunda cola.

---

📖 **Síntesis**

La arquitectura **Messaging** usa colas como intermediarias entre clientes y servidores. Garantiza entrega confiable, desacopla temporalmente productor y consumidor y permite comunicación asíncrona. Se puede configurar con distintos niveles de confiabilidad (QoS). Es ideal para sistemas donde la **disponibilidad es intermitente** o donde se necesita **resiliencia ante fallas**.

---

# 📌 Arquitectura Coordinador de Proceso (Process Coordinator)

## 1. Esquema básico (diapositiva 104)

- Existe un **Coordinador de Proceso** que recibe un **pedido de inicio de proceso**.
- El coordinador se encarga de invocar a los **servidores** en el **orden definido** (Paso 1, Paso 2, Paso 3, etc.).
- Al finalizar, devuelve el **resultado del proceso completo**.

👉 El coordinador centraliza la **lógica de orquestación**: sabe en qué orden y a qué servidores llamar para completar el flujo.

![image.png](Contenidos%20PDF%2010%2025db0f9d9c8980e29dc2f9754a48f416/image%2021.png)

---

## 2. Propiedades clave del patrón (diapositiva 105)

- **Encapsulamiento de Proceso**
    - Toda la secuencia de pasos del proceso de negocio está definida en el coordinador.
    - Los servidores solo exponen servicios específicos, pero no saben cuándo ni en qué orden serán invocados.
    - Esto simplifica la comprensión y modificación de procesos → la lógica está en un único lugar.
- **Bajo Acoplamiento**
    - Los servidores son “ignorantes” del flujo global → solo saben ejecutar sus servicios.
    - El coordinador es quien decide a quién llamar y cuándo.
    - Permite que los servidores evolucionen sin afectar la lógica general del proceso.
- **Comunicaciones Flexibles**
    - El coordinador puede comunicarse de forma **síncrona** (espera la respuesta de cada servidor antes de continuar) o **asíncrona** (usa colas/tópicos de respuesta, sigue y luego recoge resultados).
    - Esta flexibilidad permite combinar distintos estilos según la criticidad y tiempo de respuesta esperado.

---

## 3. Consideraciones (diapositiva 106)

- Es muy útil para implementar **procesos de negocio complejos** que requieren coordinar varios servidores distintos.
- Al encapsular la **lógica del proceso en un único componente**:
    - Es más fácil **cambiar** el flujo si el negocio lo requiere.
    - Se puede **administrar** y **monitorear la performance** del proceso de punta a punta.

---

📖 **Síntesis – Process Coordinator**

En esta variante de EDA, el **Coordinador de Proceso** centraliza la secuencia de ejecución de servicios para cumplir un flujo de negocio. Los servidores son simples ejecutores de tareas, mientras que el coordinador decide el orden, controla el flujo y entrega el resultado final.

- Ventaja: **mayor control y facilidad de administración**.
- Desventaja: puede volverse un **punto único de fallo** o un cuello de botella si no se diseña redundante.

---

# 📊 Super Cuadro Comparativo de Arquitecturas

| Arquitectura | Definición técnica | En criollo | Cuándo conviene / Diferencias | Relación con otras |
| --- | --- | --- | --- | --- |
| **Monolítica** | Todo el sistema está en un solo bloque desplegable. | “Todo junto en una olla” → código, lógica y datos están pegados. | Simple de empezar, difícil de mantener cuando crece (se vuelve rígida). | Base histórica de muchas arquitecturas modernas. |
| **Spaghetti** | Código desordenado, sin capas ni separación de responsabilidades. | “Un fideo enredado” → difícil de entender, modificar y probar. | Nunca conviene, es lo que se busca **evitar**. | Suele aparecer cuando un monolito no está bien diseñado. |
| **En Capas** | Divide el sistema en capas lógicas: presentación, negocio, datos, etc. | Como un pastel de capas → cada capa se comunica con la de abajo. | Bueno para **ordenar responsabilidades**. Separa UI de lógica y de persistencia. | Evolución del monolito; base de MVC. |
| **Partición de Dominio** | Separa componentes según el **flujo de negocio o dominio**. | “Agrupar por áreas del negocio” → ejemplo: módulo de ventas, módulo de envíos. | Mejora el alineamiento con el negocio y facilita migración a microservicios. | Complementa Capas y Hexagonal; más cercano al diseño por dominios (DDD). |
| **Hexagonal (Ports & Adapters)** | Separa la lógica central del negocio de las interfaces externas mediante puertos/adaptadores. | “El core en el centro, con enchufes alrededor” → podés cambiar UI, BD o APIs sin tocar el núcleo. | Ideal cuando querés independencia de frameworks, DBs o frontends. | Evolución de Capas y Partición de Dominio. |
| **Cliente/Servidor** | Un cliente pide y un servidor responde. | “Pedido en el bar” → el cliente pide, el mozo/servidor trae. | Modelo simple para aplicaciones de red; base de la web. | Base de variantes como 2-tier, 3-tier, n-tier, etc. |
| **Maestro-Esclavo** | Un nodo maestro controla y delega trabajo a esclavos. | “El jefe y sus ayudantes” → el maestro manda, los esclavos ejecutan. | Útil en procesamiento paralelo y bases de datos distribuidas. | Variante dentro del estilo Cliente/Servidor. |
| **Peer-to-Peer (p2p)** | Todos los nodos son iguales, pueden ser clientes y servidores a la vez. | “Red entre pares” → todos comparten recursos sin jerarquías. | Bueno para compartir recursos (ej. torrents, blockchain). | Contrasta con Cliente/Servidor (sin jerarquía). |
| **MVC (Model–View–Controller)** | Separa en Modelo (datos/lógica), Vista (UI) y Controlador (interacción). | “Tres roles” → el modelo guarda datos, la vista los muestra, el controlador hace de puente. | Útil en **apps web y móviles**; soporta múltiples interfaces con un mismo modelo. | Extiende el modelo en Capas, especializado en presentación. |
| **SOA (Service-Oriented Architecture)** | Sistema dividido en **servicios autónomos** que exponen interfaces estandarizadas. | “Caja de servicios” → cada servicio hace algo del negocio y puede ser reutilizado. | Ideal en empresas grandes con sistemas heterogéneos que deben integrarse. | Precursor de Microservicios; a menudo implementado con **Web Services (SOAP/ESB)**. |
| **EDA (Event-Driven Architecture)** | Arquitectura basada en la **generación, publicación y consumo de eventos**. | “Todos escuchan la radio de eventos” → cada componente reacciona a lo que le interesa. | Bueno para sistemas **reactivos, escalables y en tiempo real**. | Estilo general, con 4 variantes principales. |
| ➝ **Publish & Subscribe** | Productores publican eventos en un tópico; múltiples consumidores suscritos los reciben. | “Un megáfono” → uno grita, muchos escuchan. | Ideal para notificaciones o difusión masiva. | Variante dentro de EDA. |
| ➝ **Broker (Agente)** | Un broker central enruta y transforma mensajes entre remitentes y receptores. | “El cartero inteligente” → recibe cartas, decide a quién van y las adapta. | Útil para integración con distintos formatos (JSON, XML, etc.). | Variante dentro de EDA; parecido a ESB de SOA. |
| ➝ **Messaging** | Uso de colas para almacenar mensajes hasta que el receptor pueda procesarlos. | “El buzón” → dejo mi mensaje en la cola, alguien lo recogerá. | Excelente para **resiliencia y asincronía** (si el receptor no está, no se pierde). | Variante dentro de EDA. |
| ➝ **Process Coordinator (Mediator)** | Un coordinador central define la secuencia del proceso e invoca a los servicios en orden. | “El director de orquesta” → decide qué instrumento entra y cuándo. | Bueno para procesos de negocio complejos que requieren pasos secuenciales. | Variante dentro de EDA; similar a Mediator en patrones de diseño. |

---

# 🔑 Diferencias clave en criollo

- **Monolítico** → Todo en un bloque.
- **Spaghetti** → Desorden total (versión mala del monolito).
- **Capas** → Ordenar la torta en niveles (UI – negocio – datos).
- **Partición de dominio** → Agrupar por áreas del negocio.
- **Hexagonal** → Core aislado, con puertos para cambiar lo externo sin tocar el centro.
- **Cliente/Servidor** → Un pide, otro responde.
- **Maestro/Esclavo** → Uno manda, otros obedecen.
- **P2P** → Todos iguales, se ayudan entre sí.
- **MVC** → Modelo–Vista–Controlador → separar datos, UI y control.
- **SOA** → Servicios grandes, autónomos, conectados por estándares (generalmente SOAP).
- **EDA** → Todo se mueve por eventos. No hay “llamadas directas”, sino que cada uno escucha lo que le interesa.
    - **Publish & Subscribe** → megáfono con tópicos.
    - **Broker (Agente)** → cartero inteligente que enruta y transforma.
    - **Messaging** → buzón (cola).
    - **Process Coordinator** → director de orquesta que coordina pasos.

---

# 📌 Diferencia entre Arquitectura y Patrón

- **Arquitectura**:
    
    Es el **estilo global de organización del sistema**. Define cómo se estructura el software a gran escala: sus módulos principales, relaciones y principios de diseño.
    
    👉 Ejemplo: Monolítica, En Capas, Cliente/Servidor, SOA, EDA.
    
- **Patrón**:
    
    Es una **solución recurrente a un problema específico** dentro de un contexto de diseño. Suelen estar más focalizados, a menor escala, o como variantes dentro de un estilo arquitectónico.
    
    👉 Ejemplo: Publish & Subscribe, MVC, Broker como agente, etc.
    

---

# 📊 Clasificación de lo que vimos

| Nombre | Tipo | ¿Por qué? |
| --- | --- | --- |
| **Monolítica** | Arquitectura | Define todo el sistema en un bloque único. Es un estilo completo, no una solución parcial. |
| **Spaghetti** | No es arquitectura formal (anti-patrón) | Es un **anti-patrón** → un ejemplo de mala práctica. No se diseña así intencionalmente, sino que surge del desorden. |
| **En Capas** | Arquitectura | Organiza el sistema completo en niveles jerárquicos (presentación, negocio, datos). |
| **Partición de Dominio** | Arquitectura | Estructura el sistema según el dominio de negocio. Es un estilo general, no una solución puntual. |
| **Hexagonal (Ports & Adapters)** | Arquitectura | Define cómo aislar el núcleo del sistema de las interfaces externas. Afecta toda la organización del software. |
| **Cliente/Servidor** | Arquitectura | Modelo de comunicación general en red. Estructura todo el sistema en roles de cliente y servidor. |
| **Maestro–Esclavo** | Patrón | Es un caso específico de cliente/servidor, pensado para procesamiento distribuido. Resuelve un problema puntual de coordinación. |
| **Peer-to-Peer (p2p)** | Arquitectura | Es un estilo de red completo → todos los nodos iguales, sin jerarquía. |
| **MVC** | Patrón arquitectónico | No es un estilo global, sino una forma de separar responsabilidades en la presentación. Aplica dentro de un sistema mayor. |
| **SOA** | Arquitectura | Organiza todo el sistema en servicios autónomos, con contratos estándar. Estilo completo. |
| **EDA** | Arquitectura | Estilo de organización basado en eventos, define cómo se comunican los módulos. |
| ➝ **Publish & Subscribe** | Patrón | Es una forma puntual de implementar EDA → cómo se conectan productores y consumidores. |
| ➝ **Broker (Agente)** | Patrón | Soluciona el problema de enrutar y transformar mensajes. Se usa dentro de EDA o SOA. |
| ➝ **Messaging** | Patrón | Uso de colas para desacoplar productores/consumidores. Es una implementación puntual de EDA. |
| ➝ **Process Coordinator** | Patrón | Un coordinador central que orquesta el flujo de eventos. Variante de EDA. |

---

# 🔑 En criollo:

- **Arquitecturas** → el “mapa de la ciudad”. Cómo están organizados los barrios, calles y avenidas (estructura general).
- **Patrones** → las “soluciones concretas” para problemas recurrentes dentro de esa ciudad (semáforos, rotondas, pasos peatonales).

👉 Entonces:

- **Monolítica, Capas, Hexagonal, Cliente/Servidor, P2P, SOA, EDA, Partición de Dominio** = **Arquitecturas**.
- **MVC, Maestro-Esclavo, Publish & Subscribe, Broker, Messaging, Process Coordinator** = **Patrones** (algunos más grandes, llamados “patrones arquitectónicos”).
- **Spaghetti** = Anti-patrón (ejemplo de lo que NO hacer).

---