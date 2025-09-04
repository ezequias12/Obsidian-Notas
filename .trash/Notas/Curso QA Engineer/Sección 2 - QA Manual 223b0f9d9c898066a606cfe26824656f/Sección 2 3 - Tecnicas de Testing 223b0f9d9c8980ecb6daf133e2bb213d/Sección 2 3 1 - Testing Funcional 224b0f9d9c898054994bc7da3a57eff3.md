# Sección 2.3.1 - Testing Funcional

![image.png](Secci%C3%B3n%202%203%201%20-%20Testing%20Funcional%20224b0f9d9c898054994bc7da3a57eff3/image.png)

Las pruebas funcionales son un tipo de prueba de software que valida el sistema de software frente a los requisitos/especificaciones funcionales. El propósito de las pruebas funcionales es evaluar cada función de la aplicación de software proporcionando datos de entrada apropiados y verificando que la salida cumpla con los requisitos funcionales.

Valida que hace el sistema, no como lo hace, prueba entrada y salidas

## UAT - Prueba de Aceptación por el Usuario

Son las *pruebas de aceptación por el usuario*. Es la etapa final del testing donde los usuarios finales o clientes verifican si el sistema cumple con sus necesidades y requisitos antes de ser liberado en producción. Confirma que el producto es útil y funciona como esperaban.

Objetivos:

1. Validar los requisitos
2. Simular un entorno real
3. Detectar los problemas
4. Obtener aprobación

Características:

1. Centrado en el usuario final
2. Basado en escenarios reales
3. Entorno separado

**Ejemplo práctico:**
Escenario: Nueva funcionalidad de búsqueda avanzada que permite filtrar resultados por fecha, tipo de contenido y ubicación.

Fase 1: Preparación
Identificar usuarios: Estudiantes, investigadores, periodistas
Definir requisitos:

- Filtro de fecha
- Todo tipo de contenido
- Ubicación geográfica

Diseñar los casos de prueba:
Caso 1: Realizar una búsqueda sin filtros → Verificar los resultados más relevantes - PASSED
Caso 2: Aplicar filtro "última semana" → Verificar que los resultados estén dentro del rango - PASSED
Caso 3: Combinar tipo: imágenes, ubicación: España → Verificar que los resultados correspondan al tipo y ubicación indicados - PASSED
Caso 4: Intentar búsquedas inválidas (ej. caracteres especiales) → Verificar si maneja estos caracteres con un mensaje claro - FAILED

Fase 2: Ejecución de las pruebas

1. Preparar el entorno de pruebas
2. Ejecutar
3. Recopilar el feedback

Fase 3: Resultados de UAT
Resultado esperado: Todas las funciones cumplen con los requisitos
El sistema responde rápidamente

Errores encontrados:

- El filtro de ubicación no funciona correctamente
- Los resultados de la última semana incluyen artículos más antiguos
- Caso 4: Las búsquedas con caracteres especiales no muestran un mensaje de error claro - FAILED

Fase 4: Aprobación
Una vez corregidos los errores y confirmada la funcionalidad → Luz verde al despliegue a produccion

## Exploratory Testing

El Exploratory Testing es una técnica donde se prueba el software sin guiones o casos predefinidos, confiando en la creatividad y análisis crítico del tester. Se caracteriza por la adaptación durante el proceso y la documentación que se realiza después de ejecutar las pruebas.

No conoces bien el sistema, de las primeras pruebas que se realizan con un sistema nuevo

- Sin guion. No requiere casos de prueba, ya que exploras las funciones
- Creatividad y analisis critico
- Adaptacion (las pruebas se ajustan a meduda que se aprende mas del mismo)
- Documentacion posterior

Demo:

- Explorar el funcionamiento de la barra de busqueda de google
Pruebas iniciales.

Prueba 1: Consulta valida en el buscador
resultado actual: el buscador devuelve resultados correctos al realizar consultas validas
resultado esperado: el buscador debe devolver los resultados correctos en el cuadro de busqueda
bug:

prueba 2: prueba con entradas no validas
Resultado actual: el buscado devuelve resultados correctos al ingresar caracteres especiales

prueba 3: Interacciones con la interfaz
Resultado actual: las opciones de la interfaz estan funcionando correctamente

prueba 4: Problemas de conexion de internet
Resultado actual: responde bien cuando no tenemos conexion a internet

## Sanity Testing

El Sanity Testing es un tipo de prueba de software que se realiza después de una corrección de errores o cambios menores  en el codigo.

Características del Sanity Testing

- Enfocado en funcionalidades específicas:
- Rápido y no exhaustivo:
- Se realiza manualmente:
- Complemento del Regression Testing:

Demo pruebas: 

(serian pruebas relacionadas a cambios que se hicieron recientemente y especificos)

Pruebas básicas del autocompletado:
Paso 1: Escribe la palabra "clima" en la barra de búsqueda.
Resultado esperado: Se deben mostrar sugerencias relacionadas como "clima hoy", "clima en Madrid", etc.
Resultado obtenido: ✅ Las sugerencias aparecen correctamente.

Paso 2: Escribe un término incompleto como "tecnolog".
Resultado esperado: Se deben mostrar sugerencias como "tecnología", "tecnologías modernas", etc.
Resultado obtenido: ✅ Las sugerencias se generan adecuadamente.

## ¿Qué es Regression Testing?

verifica si los cambios recientes en el código (como correcciones de errores, actualizaciones o nuevas funcionalidades) no han afectado negativamente las partes ya existentes del sistema.

Características del Regression Testing

- Cobertura amplia:
- Reutilización de casos de prueba:
- Frecuente y automatizable:
- Foco en integridad:

Funcionalidad: Realizar una búsqueda básica
Paso 1: Abre Google y escribe "noticias".
Resultado esperado: Aparecen resultados relevantes relacionados con "noticias".
Resultado obtenido: ✅ Los resultados aparecen correctamente.

Funcionalidad: Verificar el autocompletado
Paso 1: Escribe "clima" en la barra de búsqueda.
Resultado esperado: Aparecen sugerencias relacionadas con el término "clima".
Resultado obtenido: ✅ El autocompletado funciona como se espera.

Funcionalidad: Cambiar configuración de idioma
Paso 1: Cambia el idioma de la página a francés.
Paso 2: Escribe "météo" (clima en francés).
Resultado esperado: Aparecen sugerencias y resultados relevantes en francés.
Resultado obtenido: ✅ Los resultados y sugerencias aparecen correctamente en el idioma seleccionado.

## Smoke Testing

una aplicación funcionan correctamente después de una nueva compilación o actualización.

Prueba 1: Verificar que la página se carga correctamente (el logo de Google y el cuadro de búsqueda están visibles).
Prueba 2: Verificar que se permite buscar por imagenes

## Unit Testing

Los unit testing (pruebas unitarias) son un tipo de prueba de software que verifica de forma aislada si una unidad funcional específica de tu código

Características principales:
Aisladas
Rápidas
Automatizadas

Ventajas:
Detectan errores
Facilitan el mantenimiento y la refactorización.
Documentan el comportamiento esperado de las funciones.

### Ejemplo de Unit Testing

```python
def google_search(query):
    """"simula una búsqueda en Google"""
    if not query:
        raise ValueError("La consulta no puede estar vacía")
    results = {
        "python" : ["python.org", "docs.python.org", "realpython.com"],
        "java" : ["oracle.com/java", "docs.oracle.com/javase", "javapoint.com"],
        "javascript" : ["developer.mozilla.org", "javascript.info", "w3schools.com"]
    }
    return results.get(query.lower(), [])

print(google_search("python"))
```

```python
import unittest
from main import google_search

class TestGoogleSearch(unittest.TestCase):
    # Test para verificar que la busqueda de google son correctas
    def test_valid_query(self):
        """"Prueba con una consulta válida"""
        self.assertEqual(google_search("python"), ["python.org", "docs.python.org", "realpython.com"])
    
    def test_invalid_query(self):
        """"Prueba con una consulta inválida"""
        self.assertEqual(google_search("hola"), ["hola"])
    
if __name__ == '__main__':
    unittest.main()
```

En la consola devuelve:

PS C:\Users\ezequ\Desktop\Nueva carpeta\Clase2-QA-Manual\pruebas_python> python -m unittest test_google_search.py
['[python.org](http://python.org/)', '[docs.python.org](http://docs.python.org/)', '[realpython.com](http://realpython.com/)']
F.

FAIL: test_invalid_query (test_google_search.TestGoogleSearch)
"Prueba con una consulta inválida

Traceback (most recent call last):
File "C:\Users\ezequ\Desktop\Nueva carpeta\Clase2-QA-Manual\pruebas_python\test_google_search.py", line 12, in test_invalid_query
self.assertEqual(google_search("hola"), ["hola"])
AssertionError: Lists differ: [] != ['hola']

Second list contains 1 additional elements.
First extra element 0:
'hola'

- []
- ['hola']

---

Ran 2 tests in 0.001s

FAILED (failures=1)

→ La idea era probar que cuando la query sea “hola” deberia devolver “hola”, pero devuelve algo vacio

## Integration Testing (Lo hice con IA)

Las pruebas de integración (Integration Testing) son un tipo de prueba de software que verifica si diferentes módulos o servicios utilizados por tu aplicación funcionan bien en conjunto. Es una fase que sigue a las pruebas unitarias y precede a las pruebas del sistema.

Características principales:

- Enfoque en las interfaces: Verifica la comunicación entre componentes
- Detecta problemas de integración: Identifica errores que surgen cuando los módulos interactúan
- Prueba el flujo de datos: Comprueba si la información fluye correctamente entre componentes
- Valida requisitos funcionales: Asegura que las funcionalidades integradas cumplen las especificaciones

Tipos de pruebas de integración:

- **Integración incremental**: Se añaden módulos uno a uno
- **Integración no incremental**: Se prueban todos los módulos juntos
- **Integración de abajo hacia arriba**: Se prueban primero los módulos de bajo nivel
- **Integración de arriba hacia abajo**: Se prueban primero los módulos de alto nivel

### Ejemplo de Integration Testing

```python
# Componente 1: Autenticación de usuario
def authenticate_user(username, password):
    # Simula verificación en base de datos
    valid_users = {"user1": "pass1", "user2": "pass2"}
    return username in valid_users and valid_users[username] == password

# Componente 2: Obtener resultados de búsqueda
def get_search_results(query, user_authenticated=False):
    if not user_authenticated:
        return {"error": "Usuario no autenticado"}
    
    # Simulación de búsqueda
    if query == "python":
        return {"results": ["python.org", "docs.python.org"]}
    return {"results": []}

# Integración: Sistema de búsqueda con autenticación
def search_system(username, password, query):
    # Integra autenticación con búsqueda
    auth_result = authenticate_user(username, password)
    return get_search_results(query, auth_result)

# Prueba de integración
def test_search_system():
    # Caso 1: Usuario válido, búsqueda válida
    result1 = search_system("user1", "pass1", "python")
    print(f"Usuario válido, búsqueda válida: {result1}")
    
    # Caso 2: Usuario inválido
    result2 = search_system("invalid", "wrong", "python")
    print(f"Usuario inválido: {result2}")
    
    # Caso 3: Usuario válido, búsqueda sin resultados
    result3 = search_system("user2", "pass2", "java")
    print(f"Usuario válido, búsqueda sin resultados: {result3}")

test_search_system()

```

Este ejemplo muestra cómo se integran dos componentes: un sistema de autenticación y un sistema de búsqueda. La prueba de integración verifica que ambos componentes funcionen correctamente juntos bajo diferentes escenarios.

Las pruebas de integración son cruciales porque incluso si los componentes individuales funcionan correctamente por separado, pueden surgir problemas cuando interactúan entre sí.