[Contenidos](../Contenidos.md) \| [Anterior (3 Temas de diseño)](03_306Design_discussion.md) \| [Próximo (5 Documentación y estilo**)](05_Documentar_y_Estilo.md)

# 6.4 Especificación, Documentación y contratos+

En esta unidad se le una mayor formalización a algunos temas presentados
anteriormente, como por ejemplo la documentación de las funciones.

Se formalizarán las condiciones que debe cumplir un algoritmo al comenzar, en
su transcurso, y al terminar, y algunas técnicas para tener en cuenta estas
condiciones.

También se verá una forma de modelar el espacio donde *viven* las
variables.

## Documentación

Comenzamos formalizando un poco más acerca de la documentación, cuál es su
objetivo y las distintas formas de documentar.

###  Comentarios vs documentación

En Python tenemos dos convenciones diferentes para documentar nuestro código:
la *documentación* propiamente dicha (lo que ponemos entre `'` o
`'''` al principio de cada función o módulo), y los *comentarios*
(`#`).  En la mayoría de los lenguajes de programación hay convenciones
similares. ¿Por qué tenemos dos formas diferentes de documentar?

La *documentación* tiene como objetivo explicar *qué* hace el código.
La documentación está dirigida a cualquier persona que desee utilizar la
función o módulo, para que pueda entender cómo usarla sin necesidad de leer el
código fuente.  Esto es útil incluso cuando quien implementó la función es la
misma persona que la va a utilizar, ya que permite separar responsabilidades.

Los *comentarios* tienen como objetivo explicar *cómo* funciona el
código, y *por qué* se decidió implementarlo de esa manera. Los comentarios
están dirigidos a quien esté leyendo el código fuente.

Podemos ver la diferencia entre la documentación y los comentarios en la
función `elegir_codigo` de nuestra implementación del juego Mastermind:

```python
def elegir_codigo():
    '''Devuelve un codigo de 4 digitos elegido al azar'''
    digitos = ('0','1','2','3','4','5','6','7','8','9')
    codigo = ""
    for i in range(4):
        candidato = random.choice(digitos)
        # Debemos asegurarnos de no repetir digitos
        while candidato in codigo:
            candidato = random.choice(digitos)
        codigo = codigo + candidato
    return codigo
```

### ¿Por qué documentamos?

Muchas veces se plantea el siguiente interrogante: ¿Para qué repetir con
palabras lo que ya está estipulado en el código? La documentación es algo que
muy a menudo se deja *para después* por resultar tedioso y quizás aburrido en
hoy. Pero en ese *después*, está el yo del futuro, u otre del futuro que
quiere volver a usar el código y que agradecerá esas líneas que le evitaran
varios dolores de cabeza.

Es muy frecuente que durante el desarrollo de un proyecto el código evolucione
con el tiempo. Si nos olvidemos de actualizar la documentación para reflejar
los cambios, entonces tendremos documentación de mala calidad, ya que posible
que este incompleta e incluso incorrecta.

Pese a todo esto, la realidad sigue siendo que una buena documentación es
componente esencial de cualquier proyecto exitoso (NumPy, matplotlib, etc. tienen
buena documentación). Esto en parte se debe a que
el código fuente transmite en detalle las operaciones individuales que componen
un algoritmo o programa, pero no suele transmitir en forma transparente cosas
como la *intención* del programa, el *diseño* de alto nivel, las
*razones* por las que se decidió utilizar un algoritmo u otro, etc. También se
pueden incluir ejemplos [clarificar su uso](https://numpy.org/doc/stable/reference/generated/numpy.resize.html)

### Código autodocumentado

En teoría, si nuestro código pudiera transmitir en forma eficiente todos esos
conceptos, la documentación sería menos necesaria. De hecho, existe una técnica de
programación llamada *código autodocumentado*, en la que la idea principal
es elegir los nombres de funciones y variables de forma tal que la
documentación sea menos indispensable.

Tomemos como ejemplo el siguiente código:

```python
a = 9.81
b = 5
c = 0.5 * a * b**2
```

Leyendo esas tres líneas de código podemos razonar cuál será el valor final de
las variables `a`, `b` y `c`, pero no hay nada que nos indique qué representan
esas variables, o cuál es la intención del código. Una opción para mejorarlo sería
utilizar comentarios para aclarar la intención:

```python
a = 9.81   # Valor de la constante G (aceleración gravitacional), en m/s²
b = 5      # Tiempo en segundos
c = 0.5 * a * b**2  # Desplazamiento (en metros)
```

Otra opción, según la técnica de código autodocumentado, es simplemente asignar
nombres descriptivos a las variables:

```python
aceleracion_gravitacional = 9.81
tiempo_segundos = 5
desplazamiento_metros = 0.5 * aceleracion_gravitacional * tiempo_segundos ** 2
```

De esta manera logramos que la intención del código esté más clara, y que
se reduzca la necesidad de comentarios y documentación para comprenderlo.

La técnica de código autodocumentado presenta varias limitaciones. Entre ellas:


- Elegir buenos nombres es una tarea difícil, que requiere tener en cuenta cosas como: qué tan descriptivo es el nombre (cuanto más, mejor), la longitud del identificador (no debe ser excesivamente largo), el alcance del identificador (cuánto más grande, más descriptivo debe ser el nombre), y convenciones (`i` para índices, `c` para caracteres, etc).
- La documentación de todas formas termina siendo necesaria, ya que por muy bien que elijamos los nombres, muchas veces la única forma de explicar la intención del código y todos sus detalles es en lenguaje coloquial.
- En ciertos contextos sigue siendo deseable, o imprescindible, que quien quiera utilizar nuestra función o módulo pueda entender su funcionamiento sin necesidad de leer el código fuente.



### Un error común: la sobredocumentación

Si bien la ausencia de documentación suele ser perjudicial, el otro extremo
también lo es: la *sobredocumentación*. Después de todo, en la vida
diaria no necesitamos carteles que nos recuerden cosas como "esta es la
puerta", "este es el picaporte" y "empujar hacia abajo para abrir". De
la misma manera, podríamos decir que el siguiente código peca de ser sobredocumentado:

```python
def buscar_elemento(lista_de_numeros, numero):
    '''Esta función devuelve el índice (contando desde 0) en el que se
       encuentra el número `numero` en la lista `lista_de_numeros`.
       Si el elemento no está en la lista devuelve -1.
    '''
    # Recorremos todos los índices de la lista, desde 0 (inclusive) hasta N
    # (no inclusive)
    for indice in range(len(lista_de_numeros)):
        # Si el elemento en la posicion `indice` es el buscado
        if lista_de_numeros[indice] == numero:
            # Devolvemos el índice en el que está el elemento
            return indice
    # No lo encontramos, devolvemos -1
    return -1
```

Algunas cosas que podemos mejorar:

- En la firma de la función los nombres `buscar_elemento`, `lista_de_numeros` y `numero` se pueden simplificar a `indice`, `secuencia` y `elemento`. Cambiamos `lista_de_numeros` por `lista`, ya que la función puede recibir secuencias de cualquier tipo, con elementos de cualquier tipo, y no hay ninguna razón para limitar a que sea una lista de números.
- Las variable interna `indice` también se puede simplificar: por convención podemos usar `i`.
- "Esta función" es redundante: cuando alguien lea la documentación ya va a saber que se trata de una función.
- "contando desde 0" es redundante: en Python siempre contamos desde 0.
- Los comentarios son excesivos: la función es suficientemente simple y cualquier persona que sepa programación básica podrá entender el algoritmo.


Corrigiendo todos estos detalles resulta:

```python
def indice(lista, elemento):
    '''Devuelve el índice en el que se encuentra el `elemento` en la `lista`,
       o -1 si no está.
    '''
    for i in range(len(lista)):
        if lista[i] == elemento:
            return i
    return -1
```

## Contratos

Cuando hablamos de **contratos** o *programación por
contratos*, nos referimos a la necesidad de estipular tanto lo que necesita
como lo que devuelve nuestro código. En el se establece el compromiso de una
función, en la cual si se cumplen los requisitos estipulados, se asegura cierto
resultado. Es bueno que el contrato de una función esté incluido en su documentación.

Algunos ejemplos de cosas que deben ser estipuladas como parte del contrato
son: cómo deben ser los parámetros recibidos, qué va a ser lo que se devuelve,
y si la función provoca algún efecto secundario (como por ejemplo modificar
alguno de los parámetros recibidos).

Las condiciones que se deben cumplir al momento de ejecutar el código o
función se llaman *precondiciones*. Si se cumplen la precondición,
el código ejecutará transformando las variables de manera que al finalizar
su ejecución, el estado final de las variables y de valor de retorno, estarán
caracterizados en una *poscondición*.

### Precondiciones

La precondición de una función debe cumplirse antes de ejecutarla para que se
comporte correctamente: cómo deben ser los parámetros que recibe, cómo debe
ser el estado global, etc. Si no se cumplen, no hay garantías del funcionamiento
del código (podría colgarse, o dar error, o peor aun dar resultados erroneos).

Por ejemplo, en una función que realiza la división entre dos números,
las precondición debe decir que son que ambos parámetros deben ser números,
y que el divisor debe ser distinto de 0.

Si incluimos la precondición como parte de la documentación, en el cuerpo
de la función podremos asumir que son ciertas, y no será necesario escribir
código para lidiar con los casos en los que no se cumplen.

### Poscondiciones

La poscondición caracterizará cómo será el valor de retorno y cómo se
modificarán las variables de entrada (en caso de que corresponda) al finalizar
la ejecución siempre asumiendo que se cumplió la precondición al inicio.

En el ejemplo anterior, la función división, dada la precondición, nos permite
asegurar que la función devolverá un número y este será el cociente solicitado.

### El qué, no el cómo

Notar que al especificar un problema con pre y poscondición estamos
definiendo qué es lo que debe suceder. En ningún momento decimos cómo es que esto
sucede. Para una misma especificación podemos definir varias funciones que cumplan
el contrato, y cada una puede resolverlo a su manera.

### Aseveraciones

Retomamos aquí el concepto de aseveración que introdujimos en la [Sección 4.1](../04_Random_Plt_Dbg/01_Debugger.md#aseveraciones-assert). Tanto las precondiciones como las poscondiciones pueden pensarse como *aseveraciones* (en inglés *assertions*). Es decir, afirmaciones realizadas en un momento particular de la ejecución sobre el estado computacional. Si una aseveración llegara a ser falsa, se levanta un excepción interrumpiendo la normal ejecución del programa.

En algunos casos puede ser útil incorporar estas afirmaciones desde el código, y para ello podemos utilizar la instrucción `assert`. Esta instrucción
recibe una condición a verificar (o sea, una expresión booleana).
Si la condición es `True`, la instrucción no hace nada; en caso contrario se
produce un error.

```python
>>> assert True
>>> assert False
(^Traceback (most recent call last):
  File '<stdin>', line 1, in <module>
AssertionError^)
```

Opcionalmente, la instrucción `assert` puede recibir
un mensaje de error que mostrará en caso que la condición no se cumpla.

```python
>>> n = 0
>>> assert n `= 0, 'El divisor no puede ser 0'
(^Traceback (most recent call last):
  File '<stdin>', line 1, in <module>
AssertionError: El divisor no puede ser 0^)
```

**Atención:**
Es importante tener en cuenta que `assert` está pensado para ser
usado en la etapa de desarrollo. Un programa terminado nunca debería dejar
de funcionar por este tipo de errores.

### Ejemplos

Usando los ejemplos anteriores, la función `division` nos
quedaría de la siguiente forma:

```python
def division(dividendo, divisor):
    '''Cálculo de la división

    Pre: Recibe dos números, divisor debe ser distinto de 0.
    Pos: Devuelve un número real, con el cociente de ambos.
    '''
    assert divisor != 0, 'El divisor no puede ser 0'
    return dividendo / divisor
```

Otro ejemplo, tal vez más interesante: una función que implemente
una sumatoria *sum_i=desde^hasta i*.
![\sum_{i=desde}^{hasta} i
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Csum_%7Bi%3Ddesde%7D%5E%7Bhasta%7D+i%0A)

 En este caso,
analizando los parámetros que recibirá la función, podemos definir la
precondición para indicar lo que estos deberán cumplir.

La función `sumar_enteros` tomará un valor `desde`, un valor `hasta`.
Es decir que recibe dos parámetros.

```python
def sumar_enteros(desde, hasta):
```

Tanto `desde` como `hasta` deben ser números enteros,
y dependiendo de la implementación a realizar o de los requisitos
con los que nos enfrentamos al escribir la precondición puede ser
necesario exigir que `hasta` sea mayor o igual a
`desde`.

La declaración de la función, incluyendo documentación, precondición
y poscondición queda de la siguiente manera.

```python
def sumar_enteros(desde, hasta):
    '''Calcula la sumatoria de los números entre desde y hasta.
       Si hasta < desde, entonces devuelve cero

    Pre: desde y hasta son números enteros
    Pos: Se devuelve el valor de sumar todos los números del intervalo
        [desde, hasta]. Si el intervalo es vacío se devuelve 0
    '''
```

Prestá atención a que tanto la pre como la pos no dicen cómo hace la función
para resolver el problema, sino que caracterizan el resultado. La implementación
(o código) serán el cómo. En este caso puede ser con un ciclo que emule los pasos
de dichas sumas, podría utilizarse una fórmula cerrada que calcule el valor sin
utilizar un ciclo, entre otras opciones. Lo importante es ver que a fines de la
especificación, eso no importa.

En definitiva, la estipulación de pre y poscondiciones dentro de la
documentación de las funciones es una forma de definir claramente el
comportamiento del código.  Son en efecto un
*contrato* entre el código invocante (o usuarie) y el invocado (o función).


### Ejercicio 6.8: 
Realizar la implementación correspondiente a la función `sumar_enteros`.


##  Invariantes de ciclo

Los invariantes se refieren a estados o situaciones que no cambian dentro
de un contexto o porción de código.  Hay invariantes de ciclo, que son los
que veremos a continuación, e invariantes de estado, que se verán más
adelante.

El invariante de ciclo es, una aseveración que debe ser verdadera al
comienzo de cada iteración del ciclo y al salir del mismo.

Por ejemplo, si el problema es ir desde el punto A al punto B, las
precondición dice que tenemos que estar parados en A y la poscondición que
al terminar estaremos parados en B. Son invariantes:  "estamos en algún punto entre
A y B", "estamos en el punto más cercano a B que estuvimos hasta ahora", etc.
Son todas aseveraciones que podría tener nuestro código (y dependen
exclusivamente de como lo programamos).

Pensar en términos de invariantes de ciclo nos ayuda a reflexionar y comprender mejor
que es lo que debe realizar nuestro código y nos ayuda a desarrollarlo.

Por ejemplo, para la función `maximo`, que busca el valor más grande de una
lista desordenada, podemos enunciar:
- precondición: la lista contiene elementos que tienen una relación de orden (son comparables con <)
- poscondición: se devolverá el elemento máximo de la lista si tiene elementos, sino se retornará None.

```python
def maximo(lista):
    'Devuelve el elemento máximo de la lista o None si está vacía.'
    if not lista:
        return None
    max_elem = lista[0]
    for elemento in lista:
        if elemento > max_elem:
            max_elem = elemento
    return max_elem
```

En este caso, el invariante del ciclo es que `max_elem` contiene el
valor máximo de la porción de lista que ya fue analizada.

Los invariantes son de gran importancia al momento de demostrar formalmente que un algoritmo funciona, pero aún cuando no hagamos una demostración formal resulta útil tener los invariantes a la vista, ya que de esta forma es más fácil entender cómo funciona un algoritmo y encontrar posibles errores.

Los invariantes, además, son útiles a la hora de determinar las condiciones
iniciales de un algoritmo, ya que también deben cumplirse para ese caso.  Por
ejemplo, consideremos el algoritmo para obtener la potencia `n` de
un número.

```python
def potencia(base, exp):
    'Calcula la potencia exp del número base, con exp entero mayor que 0.'
    resultado = 1
    for i in range(exp):
        resultado *= b
    return resultado
```

En este caso, el invariante del ciclo es que la variable `resultado`
contiene el valor de la potencia correspondiente a esa iteración. Teniendo en
cuenta esta condición, es fácil ver que `resultado` debe comenzar el ciclo
con un valor de 1, ya que ese es el valor correspondiente a $p^0$.

De la misma manera, si la operación que se quiere realizar es sumar todos los
elementos de una lista, el invariante será que una variable `suma`
contenga la suma de todos los elementos ya recorridos. Antes de empezar a recorrer
siguiendo lo expresado en este invariante, esta `suma` debe ser 0 ya que no
recorrió ningún elemento.

```python
def suma(lista):
    'Devuelve la suma de todos los elementos de la lista.'
    suma = 0
    for elemento in lista:
        suma += elemento
    return suma
```

En resumen, el concepto de invariante de ciclo es una herramienta que nos permiten comprender (explicitar) mejor cómo funciona un algoritmo. Resulta fundamental en la teoría de algoritmos, donde es necesario para *demostrar* que:
- un algoritmo es correcto, es decir que realiza la tarea descripta por la pre y poscondición
- un algoritmo termina (y no se cuelga)

### Parámetros mutables e inmutables

Las funciones reciben parámetros que pueden ser mutables o inmutables.

Si dentro del cuerpo de la función se modifica uno de estos parámetros para
que *referencie* a otro valor, este cambio no se verá reflejado fuera de la
función.  Si, en cambio, se modifica el *contenido* de alguno de los
parámetros mutables, este cambio *sí* se verá reflejado fuera de la
función.

A continuación un ejemplo en el cual se asigna la variable recibida, a un
nuevo valor.  Esa asignación sólo tiene efecto dentro de la función.

```python
>>> def no_cambia_lista(lista):
...     lista = [0, 1, 2, 3]
...     print('Dentro de la funcion lista =', lista)
...
>>> lista = [10, 20, 30, 40]
>>> no_cambia_lista(lista)
Dentro de la funcion lista = [0, 1, 2, 3]
>>> lista
[10, 20, 30, 40]
```

A continuación un ejemplo en el cual se modifica la variable recibida. En este
caso, los cambios realizados tienen efecto tanto dentro como fuera de la
función.

```python
>>> def cambia_lista(lista):
...     for i in range(len(lista)):
...         lista[i] = lista[i] ** 3
...
>>> lista = [1, 2, 3, 4]
>>> cambia_lista(lista)
>>> lista
[1, 8, 27, 64]
```

**Atención:** Salvo que sea explícitamente aclarado, una función no debe modificar los valores de sus parámetros. En el caso en que por una decisión de diseño o especificación se modifiquen los parámetros mutables recibidos, esto debe estar claramente documentado como parte de las poscondiciones.


## Resumen

- La **documentación** tiene como objetivo explicar *qué* hace el código,
    y está dirigida a quien desee utilizar la función o módulo.
- Los **comentarios** tienen como objetivo explicar *cómo* funciona el
    código y *por qué* se decidió implementarlo de esa manera, y están dirigidos a
    quien esté leyendo el código fuente.
- El **contrato** de una función especifica qué condiciones se deben
    cumplir para que la función pueda ser invocada (**precondición**),
    y qué condiciones se garantiza que serán válidas cuando la función termine
    su ejecución (**poscondición**).
- Los **invariantes de ciclo** son las condiciones que deben
cumplirse al comienzo de cada iteración de un ciclo.
- En el caso en que estas **aseveraciones** no sean verdaderas, se
deberá a un error en el diseño o utilización del código.
- Si una función modifica un valor mutable que recibe por parámetro, eso
    debe estar explícitamente aclarado en su documentación.


## Ejercicios

### Ejercicio 6.9: Analizar cada una de las siguientes funciones.
¿Cuál es el contrato de la función? ¿Cómo sería su documentación?
¿Es necesario agregar comentarios?
¿Se puede simplificar el código y/o mejorar su legibilidad?
¿Hay algún invariante de ciclo?

```python
def valor_absoluto(n):
    if n >= 0:
        return n
    else:
        return -n
```

```python
def suma_pares(l):
    res = 0
    for e in l:
        if e % 2 ==0:
            res += e
        else:
            res += 0

    return res
```

```python
def collatz(n):
    res = 1

    while n!=1:
        if n % 2 == 0:
            n = n/2
        else:
            n = 3 * n + 1
        res += 1

    return res
```



[Contenidos](../Contenidos.md) \| [Anterior (3 Temas de diseño)](03_306Design_discussion.md) \| [Próximo (5 Documentación y estilo**)](05_Documentar_y_Estilo.md)

