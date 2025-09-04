# Esquema Arquitectura Microservicios

![image.png](Esquema%20Arquitectura%20Microservicios%2022db0f9d9c898019bb17c56bbeaff9fa/image.png)

Representa cómo está **dividida la aplicación en microservicios**, cómo se comunican y qué tecnologías usa.

### 🔍 **Componentes principales explicados en simple**

| **Componente** | **Función práctica en el sistema** |
| --- | --- |
| **Spring Cloud Config Server** | Guarda archivos de configuración centralizados (YAML) para todos los microservicios. Ej: la DB o puertos no se configuran en cada microservicio, sino aquí. |
| **Git Repo** | Donde se guardan esos archivos YAML versionados (para control de cambios). |
| **Eureka Service Discovery** | Permite registrar y descubrir los microservicios automáticamente (sin saber IPs o puertos exactos). Es como un "buscador de microservicios". |
| **UI + API Gateway** | 🔥 **Este es clave**. Es la **puerta de entrada única** de toda la aplicación. ✅ Sirve el frontend (HTML, JS, CSS). ✅ Recibe requests y los redirige al microservicio correspondiente. ✅ Ej: si el usuario consulta veterinarios, lo deriva a `vets-service`. |
| **customers-service** | Microservicio REST API que gestiona **clientes** de la veterinaria (dueños de mascotas). |
| **vets-service** | Microservicio REST API que gestiona **veterinarios**. |
| **visits-service** | Microservicio REST API que gestiona **visitas** o consultas de las mascotas. |
| **Spring Boot Admin Server** | Herramienta para **monitorear** los microservicios (estado, memoria, CPU, etc.). |
| **Zipkin Distributed Logging Server** | Herramienta para **ver los logs distribuidos** de todos los microservicios, muy útil para debugging y seguimiento de requests entre servicios. |

---

### **¿Cómo fluye la comunicación?**

1. **El usuario accede al API Gateway (UI + API Gateway)** desde su navegador.
2. El **API Gateway**:
    - Sirve la **interfaz web** al usuario (HTML, CSS, JS).
    - Recibe las peticiones (ej. ver veterinarios) y las envía al microservicio adecuado (`vets-service`) a través de REST.
3. Cada **microservicio**:
    - Se registra en **Eureka** cuando arranca (le dice “acá estoy” con su IP y puerto).
    - Consulta a **Spring Cloud Config Server** para obtener su configuración.
    - Se conecta a su **base de datos propia** (en la parte inferior de cada microservicio se ve su DB).
4. Los logs de todos los microservicios se envían a **Zipkin** para trazabilidad y monitoreo centralizado.
5. **Spring Boot Admin Server** monitorea el estado de cada microservicio.

---

### **¿El API Gateway conecta con el frontend y el backend?**

✅ **Sí.**

- Desde el **punto de vista del usuario**, el API Gateway es el **único backend que ve**, porque ahí accede al sistema.
- Desde el **punto de vista interno**, el API Gateway **redirecciona requests** al microservicio correspondiente.

💡 **Ejemplo práctico real:**

- Frontend ➔ **API Gateway** ➔ `vets-service` ➔ DB de veterinarios

El **API Gateway no implementa lógica de negocio**, solo distribuye las peticiones y puede aplicar seguridad, logging y balanceo de carga.

---

### **Resumen en una frase**

👉 **El API Gateway es el punto de entrada único al sistema (frontend y backend), y cada microservicio implementa una parte específica de la lógica de negocio.**

---

### 📝 **6. Ejemplo visual resumido**

```
less
CopiarEditar
[Usuario Frontend]
        ↓
   [API Gateway]
   /     |      \
[customers][vets][visits]
   ↓       ↓      ↓
  DB      DB     DB

```

✅ Frontend pide info a Gateway

✅ Gateway reenvía la petición al microservicio

✅ Microservicio procesa y consulta su DB

✅ Respuesta sube hasta el frontend

---

## Conceptos

## Yaml

Sirven para la conexion a la bd

**Contienen la configuración de conexión a la base de datos**, además de muchas otras configuraciones necesarias para cada microservicio.

 Son archivos de **configuración externa** que contienen variables y propiedades que tu aplicación necesita para funcionar, como:

- **Datos de conexión a la base de datos**:
    - URL de la base de datos
    - Usuario
    - Contraseña
    - Driver (por ejemplo, `org.postgresql.Driver` o `com.mysql.cj.jdbc.Driver`)
- Configuraciones de puertos
- Nombres de servicios
- Timeouts
- Credenciales de APIs externas

### **¿Por qué se usan en microservicios?**

En una arquitectura de microservicios como la de **PetClinic**, cada microservicio necesita **su propia configuración.**

✔️ Para **no hardcodear** (escribir) esas configuraciones dentro del código, se guardan en archivos YAML centralizados en el **Spring Cloud Config Server**.

### **Ejemplo práctico de un archivo `application.yml`**

```yaml
yaml
CopiarEditar
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/veterinaria
    username: root
    password: 1234
    driver-class-name: com.mysql.cj.jdbc.Driver

server:
  port: 8081

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/

```

 **¿Qué hace este archivo?**

1. Configura la **base de datos** (url, user, password, driver)
2. Define el **puerto** donde correrá este microservicio
3. Configura la conexión al **Eureka Service Discovery**