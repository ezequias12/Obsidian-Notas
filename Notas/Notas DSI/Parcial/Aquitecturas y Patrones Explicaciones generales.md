
# Mvc
## Como lo aplicamos en el caso de bicirent

### Se puede aplicar Mvc tanto en el frontend como en el backend 

### 🔹 En el **backend** (Node/Express con Sequelize)

- **Modelo (M):**
    
    - Eran tus clases/tablas de Sequelize (`Tarifa`, `Alquiler`, etc.).
        
    - Se encargaban de la persistencia (mapear objetos ↔ base de datos).
        
- **Controlador (C):**
    
    - Los endpoints de Express (`tarifasController`, `alquilerController`).
        
    - Recibían las solicitudes HTTP del frontend, llamaban a los modelos y decidían qué devolver.
        
- **Vista (V):**
    
    - En un backend tipo API, la “vista” no es HTML, sino el **JSON** que se arma y se devuelve al frontend.
        
    - O sea, la representación de la respuesta.
        

---

### 🔹 En el **frontend** (React)

- **Modelo (M):**
    
    - Podría ser tu capa de _services_ o _hooks_ que llamaban a la API y guardaban datos (ej. en `useState` o `Redux`).
        
    - Maneja datos, no estética.
        
- **Controlador (C):**
    
    - La lógica intermedia: eventos de usuario (clicks, submits) que disparaban funciones para actualizar estado o llamar a la API.
        
    - Ejemplo: un `handleSubmit` que manda un POST a `/tarifas`.
        
- **Vista (V):**
    
    - Los componentes React que renderizan la UI.
        
    - Botones, inputs, tablas, etc.
        

---

## 🎯 Idea clave

- Tu proyecto tenía **Cliente–Servidor**: React (cliente) ↔ Node/Express (servidor).
    
- Dentro de cada lado aplicaste **MVC**:
    
    - Backend → Sequelize (M), Controladores Express (C), JSON response (V).
        
    - Frontend → State/hooks (M), funciones/eventos (C), componentes React (V).
        

---
