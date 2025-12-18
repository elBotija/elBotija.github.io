---
layout: post
title: "Python: 5 Trucos de Python que todo desarrollador debería conocer"
---

Python es famoso por ser legible y limpio, pero cuando venimos de otros lenguajes, a veces tendemos a escribir código sin aprovechar las formas cortas y expresivas que el lenguaje nos ofrece.

Escribir código más directo no es solo una cuestión de estética; suele ser más eficiente y mucho más fácil de mantener. Hoy te traigo 5 trucos que uso todos los días y que te van a ayudar a simplificar tu código.

## 1. List Comprehensions: Adiós a los bucles `for` vacíos

¿Cuántas veces escribiste algo así?

```python
numeros = [1, 2, 3, 4, 5]
cuadrados = []
for n in numeros:
    cuadrados.append(n ** 2)
```

Funciona, pero es verboso. Python nos permite hacer esto en una sola línea elegante:

```python
numeros = [1, 2, 3, 4, 5]
cuadrados = [n ** 2 for n in numeros]
```

La estructura es simple: `[lo_que_queremos_guardar for variable in lista_original]`. Básicamente le decís: "Poné en esta lista nueva el resultado de `n ** 2` para cada `n` que encuentres en `numeros`".

Incluso podés agregar condiciones. Si solo querés los cuadrados de los números pares:

```python
pares_cuadrados = [n ** 2 for n in numeros if n % 2 == 0]
```

Acá la lógica se lee: `[expresion for item in lista if condicion]`. Solo se ejecuta la expresión si la condición del final es verdadera.

## 2. f-strings: Formateo de strings moderno

Si seguís usando `.format()` o peor, concatenando con `+`, es hora de actualizarse. Desde Python 3.6, las f-strings son la forma más rápida y legible de interpolar variables.

**Antes:**
```python
nombre = "Pepe"
edad = 30
print("Hola, mi nombre es " + nombre + " y tengo " + str(edad) + " años.")
# O con format
print("Hola, mi nombre es {} y tengo {} años.".format(nombre, edad))
```

**Ahora:**
```python
print(f"Hola, mi nombre es {nombre} y tengo {edad} años.")
```

¡Incluso podés ejecutar expresiones dentro!

```python
print(f"El doble de tu edad es {edad * 2}")
```

## 3. Unpacking: Asignación múltiple limpia

Python permite asignar valores de una lista o tupla a variables individuales de forma muy intuitiva.

Supongamos que tenés una tupla con coordenadas:

```python
punto = (10, 20)
# Forma directa
x = punto[0]
y = punto[1]

# Forma simplificada
x, y = punto
```

Esto es súper útil cuando una función devuelve múltiples valores. También podés usar `*` para capturar el resto:

```python
numeros = [1, 2, 3, 4, 5]
primero, segundo, *resto = numeros

print(primero) # 1
print(resto)   # [3, 4, 5]
```

## 4. Enumerate: Olvidate de `range(len())`

Cuando necesitás el índice y el valor en un bucle, es común ver esto:

```python
frutas = ["manzana", "banana", "pera"]
for i in range(len(frutas)):
    print(f"{i}: {frutas[i]}")
```

Es difícil de leer y propenso a errores. `enumerate` te da ambos valores directamente:

```python
for i, fruta in enumerate(frutas):
    print(f"{i}: {fruta}")
```

## 5. Zip: Iterando listas en paralelo

A veces tenés dos listas relacionadas y querés recorrerlas juntas.

```python
nombres = ["Ana", "Juan", "Pedro"]
edades = [25, 30, 22]

# No hagas esto
for i in range(len(nombres)):
    print(f"{nombres[i]} tiene {edades[i]} años")
```

Usá `zip` para unirlas:

```python
for nombre, edad in zip(nombres, edades):
    print(f"{nombre} tiene {edad} años")
```

---

### Conclusión

Estos trucos son solo la punta del iceberg, pero incorporarlos a tu día a día va a hacer que tu código se sienta mucho más profesional.

En el próximo post vamos a hablar de IA Generativa y por qué debería importarte. ¡Nos vemos!
