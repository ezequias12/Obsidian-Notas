
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

# EDA - Arquitectura Coordinador de Proceso

## 📌 Diferencia fundamental

### 🔹 Antes (EDA con broker, pub/sub, cola de mensajes)

- **El productor publica un evento completo** (ej.: _PedidoCreado_).
    
- **Los receptores** lo leen y cada uno decide qué parte usar.
    
    - Ej.: Inventario descuenta stock, Facturación emite factura, Notificaciones avisa al cliente.
        
- 👉 Cada receptor recibe el **evento completo** y hace lo suyo.
    
- 👉 No hay un “director de orquesta”; los flujos emergen solos.
    

---

### 🔹 Ahora (EDA con Process Coordinator / Mediator)

- **El productor publica el evento** (_PedidoCreado_).
    
- El **coordinador de procesos interpreta** ese evento y decide:
    
    1. Primero llamar a Inventario.
        
    2. Después, si Inventario confirma, llamar a Facturación.
        
    3. Finalmente, llamar a Notificaciones.
        
- **Cada receptor ya no recibe el evento completo**, sino **la parte o tarea que le corresponde**.
    
    - Ej.: Inventario recibe solo “descontar stock”, no toda la info de pedido completa.
        
- 👉 El coordinador **desglosa el flujo en pasos** y los va delegando.
    

---

## 📌 Cambio en el rol del receptor

- Antes: receptores eran más “autónomos”, sacaban lo que les servía de un evento grande.
    
- Ahora: receptores son más “especializados” y **esperan órdenes concretas** del coordinador.
    
    - Funcionan casi como **servicios atómicos** (ej.: “validar pago”, “enviar mail”).
        
- 👉 La lógica del proceso se concentra en el **coordinador**, no en cada receptor.
    

---
