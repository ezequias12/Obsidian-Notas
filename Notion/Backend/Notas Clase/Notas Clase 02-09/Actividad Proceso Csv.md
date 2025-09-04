
# Mapa rápido de cómo se conectan

1. **Proyecto “Strategy + OpenCSV” (App + Step01/02/03)**
    
    - Es **un andamiaje** para _leer un CSV de distintas formas_.
        
    - **Step01**: Scanner + `split`. **Step02**: OpenCSV en `List<String[]>`. **Step03**: OpenCSV con `Map<String,String>` (con encabezados).
        
    - La idea: **un único `App`** que elige _la estrategia_ por parámetro (`paso01|paso02|paso03`).
        
    - En criollo: te da el **esqueleto para la entrada de datos**; cambiás la “estrategia” sin tocar el resto.
        
2. **Actividad RRHH (Empleados Permanentes/Contratados + Categoría + Informes)**
    
    - Esto es el **modelo de dominio y la lógica** (POO + polimorfismo): `Empleado`, `EmpleadoPermanente`, `EmpleadoContratado`, `Categoria`.
        
    - Usa un **CSV de empleados** como fuente de datos (justo lo que el proyecto Strategy sabe leer).
        
    - Conecta así: elegís un _Step_ (ideal **Step03** por legibilidad), lo usás para **parsear el CSV → List**, y luego corrés **los informes** (máx/min por tipo/categoría, totales, porcentajes, antigüedad promedio).
        
    - En criollo: **Strategy = cómo leer**. **RRHH = qué hacer con lo leído** (modelo + cálculos).

---

# Pensalo como 4 capas (para ambos ejercicios)

- **Entrada (CSV)**: manual u OpenCSV (Strategy).
    
- **Dominio**: clases que representan la realidad (Empleados o Ventas).
    
- **Procesamiento**: reglas de negocio e **informes** (polimorfismo y/o streams).
    
- **Ejecución/Presentación**: `App`, `Menu`, args de Maven, etc.
    

El **proyecto Strategy** te resuelve la **capa Entrada** de forma limpia y conmutando implementaciones.  
La **actividad RRHH** te hace practicar **Dominio + Procesamiento (polimorfismo)** apoyándote en esa Entrada.  
El **enunciado Cafetería** te hace practicar **Dominio + Procesamiento (streams/colecciones)** y además **Presentación (menú genérico + acciones)**; la Entrada vuelve a ser CSV (podés usar o no Strategy).

---
