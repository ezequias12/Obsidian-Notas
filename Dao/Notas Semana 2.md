# 📘 Semana 2 – Programación Funcional

---

## 1. Paradigmas de Programación

Un **paradigma** es un estilo o enfoque de programación. Los más usados:

|Paradigma|Idea principal|Ejemplo en Python|
|---|---|---|
|**Estructurado**|Se organiza en **secuencias de instrucciones**, usando variables simples (números, cadenas, booleanos).|`python\nx = 10\ny = 20\nresultado = x + y\nprint(resultado) # 30`|
|**Orientado a Objetos (OOP)**|Se centra en **objetos** que tienen **atributos (estado)** y **métodos (comportamiento)**.|`python\nclass Persona:\n def __init__(self, nombre):\n self.nombre = nombre\n\n def saludar(self):\n print(f\"Hola, soy {self.nombre}\")\n\np = Persona(\"Ana\")\np.saludar() # Hola, soy Ana`|
|**Funcional**|Las funciones son **ciudadanos de primera clase**: se pueden guardar en variables, pasar como parámetros y devolver como resultado.|`python\ndef saludar():\n print(\"Hola!\")\n\nf = saludar # asigno la función a una variable\nf() # se ejecuta -> Hola!`|

---

## 2. Variables de tipo función

- En funcional, las **variables pueden almacenar funciones**.
- Ejemplo:

```Python
def imprimir_mensaje():
    print("Mensaje desde una función.")

f = imprimir_mensaje   # guardo la función en una variable
f()  # ejecuta -> Mensaje desde una función.
```

---

## 3. Funciones de Orden Superior

Una función de orden superior es aquella que:

1. **Recibe otra función como parámetro**, o
2. **Devuelve otra función como resultado**.

Ejemplo 1 (función como parámetro):

```Python
def aplicar(funcion, valor):
    return funcion(valor)

resultado = aplicar(lambda x: x*2, 5)
print(resultado)  # 10
```

Ejemplo 2 (función como retorno):

```Python
def crear_multiplicador(n):
    return lambda x: x * n

doble = crear_multiplicador(2)
print(doble(5))  # 10
```

---

## 4. Colecciones de funciones

Podemos guardar varias funciones en una lista o diccionario:

```Python
def sumar(a, b): return a + b
def restar(a, b): return a - b
def multiplicar(a, b): return a * b
def dividir(a, b): return a / b

operaciones = [sumar, restar, multiplicar, dividir]

# usar índice como menú
print(operaciones[0](10, 5))  # 15 (suma)
print(operaciones[2](10, 5))  # 50 (multiplicación)
```

---

## 5. Funciones Lambda (anónimas)

- Son **funciones pequeñas, sin nombre**.
- Se escriben con la palabra clave `lambda`.

Ejemplo:

```Python
# normal
def cuadrado(x):
    return x**2

# con lambda
cuadrado = lambda x: x**2

print(cuadrado(4))  # 16
```

📌 Características:

- No llevan `def` ni `return`.
- Se usan para **operaciones simples** y rápidas.

---

## 6. Operaciones Funcionales con Secuencias

En Python se aplican mucho a **listas**:

### 🔹 `filter()` → seleccionar elementos

```Python
numeros = [1, 2, 3, 4, 5, 6]
pares = list(filter(lambda x: x % 2 == 0, numeros))
print(pares)  # [2, 4, 6]
```

### 🔹 `map()` → transformar elementos

```Python
numeros = [1, 2, 3, 4]
cuadrados = list(map(lambda x: x**2, numeros))
print(cuadrados)  # [1, 4, 9, 16]
```

### 🔹 `reduce()` → acumular en un único valor

```Python
from functools import reduce

numeros = [1, 2, 3, 4, 5]
suma_total = reduce(lambda a, b: a + b, numeros)
print(suma_total)  # 15
```

---

## 7. Ejemplo completo: Mini Calculadora Funcional

```Python
from functools import reduce

# definimos funciones
sumar = lambda a, b: a + b
restar = lambda a, b: a - b
multiplicar = lambda a, b: a * b
dividir = lambda a, b: a / b if b != 0 else "Error: div/0"

# las guardamos en un diccionario
calculadora = {
    "suma": sumar,
    "resta": restar,
    "multiplicación": multiplicar,
    "división": dividir
}

# uso
print(calculadora["suma"](10, 5))           # 15
print(calculadora["multiplicación"](3, 7))  # 21

# ejemplo con reduce: suma de lista
numeros = [1, 2, 3, 4, 5]
print(reduce(sumar, numeros))  # 15
```

---

# ✅ Conclusiones

- La **programación funcional** se basa en **tratar funciones como datos**.
- Permite **más expresividad y menos código repetido**.
- Se usan mucho **funciones lambda** y operaciones sobre secuencias (`filter`, `map`, `reduce`).
- Es un paradigma complementario a OOP y estructurado.

---