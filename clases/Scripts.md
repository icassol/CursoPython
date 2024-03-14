# Scripting

En esta sección profundizaremos en el proceso de crear scripts en Python. 

### ¿Qué es un script?

Un *script* es un programa que ejecuta una serie de comandos y termina. *Programa* en el sentido clásico de la palabra: una secuencia de eventos. Su traducción literal es **guión**, como el guión de una película, con introducción, nudo y desenlace.

```python
# programa.py

comando1
comando2
comando3
...
```

Hasta aquí mayormente hemos escrito scripts.

### Un problema


Cuando hayas escrito un script útil, éste va a comenzar a crecer en funciones y opciones. Vas a querer aplicarlo a otros problemas. Con el tiempo puede convertirse en un programa esencial, pero si no lo cuidás puede convertirse en un lío enorme, en un gran embrollo. Veamos como lo organizamos.


### Definir nombres

Los nombres deben estar definidos antes de usarse.

```python
def cuadrado(x):
    return x*x

a = 42
b = a + 2     # Requiere que 'a' haya sido definido antes.

z = cuadrado(b) # Requiere que 'cuadrado' y 'b' estén definidos.
```

**El orden importa.**
Casi siempre definimos las variables y las funciones al comienzo.

### Definir funciones

Es muy útil agrupar todo el código relevante a una misma *tarea* en el mismo lugar. Para eso sirven las funciones.

```python
def leer_precios(nombre_archivo):
    precios = {}
    with open(nombre_archivo) as f:
        f_csv = csv.reader(f)
        for linea in f_csv:
            precios[linea[0]] = float(linea[1])
    return precios
```
Una función simplifica las operaciones repetitivas.

```python
preciosviejos = leer_precios('preciosviejos.csv')
preciosnuevos = leer_precios('preciosnuevos.csv')
```

### ¿Qué es una función?

Una función es una secuencia de comandos, con un nombre.

```python
def nombrefunc(args):
  comando
  comando
  ...
  return resultado
```

*Cualquier* comando de Python puede usarse dentro de una función.

```python
def foo():
    import math
    print(math.sqrt(2))
    help(math)
```

No existen comandos *especiales* en Python (lo cual es muy fácil de recordar).

### Dónde definir funciones

En Python podemos *definir* funciones en cualquier orden.

```python
def foo(x):
    bar(x)

def bar(x):
    comandos

# OR
def bar(x):
    comandos

def foo(x):
    bar(x)
```

El único requisito es que la función esté definida al momento de ser *usada* (o llamada) durante la ejecución de un programa.

```python
foo(3)        # foo tiene que haber sido definida antes
```

El estilo que preferimos es definir funciones desde abajo hacia arriba ("*bottom-up*")  

### El estilo *Bottom-Up*

Este estilo trata a las funciones como ladrillos. Los ladrillos simples o más pequeños se definen primero, y luego se usan para armar funciones más complejas.

```python
# miprograma.py
def foo(x):
    ...

def bar(x):
    ...
    foo(x)          # Definida antes
    ...

def spam(x):
    ...
    bar(x)          # Definida antes
    ...

spam(42)            # El código que *usa* la función está al final
```

Las funciones complejas se basan en funciones más simples, definidas antes; aunque esto es sólo una cuestión de estilo. Lo único que realmente importa en ése programa es que la llamada a `spam(42)` esté después que la declaración de las funciones que éste invoca. El orden de las definiciones puede variar, siempre que sea anterior a su uso real.

### Diseño de funciones

Lo ideal es que una función sea una *caja negra*. Una función debería operar únicamente sobre los parámetros provistos, evitar variables globales y efectos secundarios no esperados. Hay dos conceptos clave: **Diseño Modular** y **Predecibilidad**.

[oski]:# (Esto hay que ampliarlo, explicar predecibilidad y modularidad)

### Doc-strings

Es buena costumbre incluir documentación en forma de doc-strings. Un doc-string ó "texto de documentación" es texto ubicado en la línea inmediata después del nombre de la función. El doc-string provee información a quien lee la función, pero también se integra con la función `help()`, IDEs y otras herramientas de programación y documentación.     

```python
def leer_precios(nombre_archivo):
    '''
    Lee precios de un archivo de datos CSV con dos columnas.
    La primera columna debe contener un nombre y la segunda un precio.
    '''
    precios = {}
    with open(nombre_archivo) as f:
        f_csv = csv.reader(f)
        for row in f_csv:
            precios[linea[0]] = float(linea[1])
    return precios
```

Un doc-string debe ser conciso e indicar qué hace la función. Si es necesario, podés incluir un ejemplo corto de uso y una descripción de los argumentos.

Veremos también la clase que viene que es posible incluir en el doc-string una descripción de lo que se espera que cumplan los parámetros y lo que garantizamos que cumpla la salida (como un contrato).

### Notas sobre el tipo de datos

También podés agregar, en la definición de funciones, notas sobre el tipo de datos de los parámetros y de la función.

```python
def leer_precios(nombre_archivo: str) -> dict:
    '''
    Lee precios de un archivo de datos CSV con dos columnas.
    La primera columna debe contener un nombre y la segunda un precio.
    Devuelve un diccionario {nombre:precio, ...}
    '''
    precios = {}
    with open(nombre_archivo) as f:
        f_csv = csv.reader(f)
        for linea in f_csv:
            precios[linea[0]] = float(linea[1])
    return precios
```

Estas notas no modifican al programa y son puramente informativas. Aún así pueden ser usadas por IDEs, comprobadores de código, y otras herramientas.

Aunque `-> dict` indica al programador que la función devuelve un diccionario, es útil anotar en el doc-string la estructura del diccionario devuelto.  

### Comentario

En Python es muy fácil escribir código en forma de un script relativamente poco estructurado, en el que tenés un archivo que contiene una secuencia de comandos. A la larga, casi siempre es mejor convertir estos scripts en funciones para organizar el código.

En algún momento, si ese script crece, vas a desear haber sido un poco más organizado desde el comienzo. Tratá de organizar tu código en funciones simples. Es un buen principio es que cada función haga una sola cosa sencilla y concreta, que tenga una sola responsabilidad.


