
# 📌 Contexto previo al inicio de la Unidad 3

## Unidad 2: Diseño en general

En la **U2** vimos el **Diseño de Sistemas de Información** en un sentido amplio:

- **El diseño** transforma el **modelo de análisis** (conceptual, lógico, genérico) en un **modelo físico** que servirá de plano para la implementación.
    
- En análisis se trabaja con **qué hace el sistema** (funcionalidad), mientras que en diseño se empieza a definir **cómo se va a construir**.
    
- El diseño incluye:
    
    - **Modelo de diseño** (clases, interfaces, subsistemas).
        
    - **Modelo de despliegue** (hardware, topología, comunicaciones).
        
    - Consideración de **requerimientos no funcionales** (ISO 25000: rendimiento, seguridad, usabilidad, mantenibilidad, etc.).
        
    - Aplicación de **principios de calidad**: bajo acoplamiento, alta cohesión, patrones de diseño.
        

👉 Con la U2 tenés la idea de que el diseño es un proceso iterativo, guiado por casos de uso, que da forma al sistema **desde lo lógico hacia lo físico**.

---

## Paso a la Unidad 3

La **U3** toma ese marco general del diseño y lo enfoca en un aspecto específico:  
👉 **el Diseño Arquitectónico de Software**, es decir, **la definición de la estructura global del sistema y las decisiones más importantes que guían toda la implementación**.

Por eso:

- Si en U2 viste “el diseño de todo”, en U3 se hace **zoom en la arquitectura**:
    
    - Qué estilos usar (capas, cliente-servidor, microservicios).
        
    - Qué vistas arquitectónicas documentar.
        
    - Qué requerimientos son más críticos para la arquitectura.
        
    - Cómo validar esas decisiones antes de implementar.
        

---

## Idea central de la transición

- **U2 = diseñar el sistema en general.**
    
- **U3 = diseñar la arquitectura, que es la parte más estratégica del diseño.**
    

En otras palabras:

> Primero definimos **cómo será cada parte** (U2), y ahora definimos **cómo se organiza el todo y qué decisiones estructurales no podemos fallar** (U3).

---

⚡ Ejemplo aplicado:

- En U2, para un sistema de cine, definís las **clases** (Pelicula, Entrada, Usuario), los **diagramas de secuencia**, los **subsistemas**.
    
- En U3, decidís:
    
    - ¿Será **monolito o microservicios**?
        
    - ¿Dónde se desplegará (nube, servidor local)?
        
    - ¿Cómo garantizo escalabilidad (RNF de performance) y seguridad (RNF de confidencialidad)?
        
    - ¿Qué vista de arquitectura usar para documentarlo?
        
