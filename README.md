# Cuarta tarea de APA 2023: Generación de números aleatorios

## Nom i cognoms Ada Salvador Avalos

## Generación de números aleatorios usando el algoritmo LGC

El algoritmo de generación lineal congruente
[LGC](https://en.wikipedia.org/wiki/Linear_congruential_generator) permite generar
secuencias pseudoaleatorias de características controladas. Se basa en aplicar
iterativamente la fórmula recursiva siguiente:

$$ x_{n + 1} = (a x_n + c) \mod m $$

Donde se denomina *módulo* a $m > 0$, *multiplicador* a $0 < a < m$, *incremento* a
$0 \le c < m$, y $0 \le x_0 < m$ es el valor inicial, o *semilla*, de la secuencia
aleatoria generada.

La secuencia es periódica, ya que, cada vez que repetimos un valor de $x_n$, volvemos a
generar la misma otra vez. Los valores generados cumplen $0 \le x_n < m$; por tanto, la
secuencia más larga posible es de longitud $m$, en cuyo caso, cada valor $0 \le x_n < m$
es producido una sola vez.

El módulo $m$ suele tomar como valor una potencia entera de dos para facilitar el cálculo
del resto de la división entera mediante el operador desplazamiento de bits. Una elección
adecuada del incremento $c$ y el multiplicador $a$ permite que la secuencia generada
tenga el periodo máximo, igual a $m$:

- $m$ y $c$ no deben tener factores primos en común.
- $a - 1$ debe ser divisible por todos los factores primos de $m$ (aunque no mucho).
- Si $m$ es divisible por 4, $a - 1$ también debe serlo, pero no por 8.

Por ejemplo, el generador aleatorio del estándar POSIX usa los valores siguientes:

$$\begin{eqnarray*}
        m & = & 2^{48} \\
        a & = & 25214903917 \\
        c & = & 11
\end{eqnarray*}$$

## Ejercicios

Escriba el fichero `aletaorios.py` que implemente la generación de números aleatorios
usando tanto una clase iterable, `Aleat`, como una función generadora `aleat()`.

### Generación de números aleatorios usando la clase `Aleat`

Escriba la clase `Aleat` que implemente un generador de números aleatorios en el rango
$0 \le x_n < m$ usando el método LGC con las características siguientes:

- Los objetos de la clase serán iteradores, para lo que habrá de definirse el método
  mágico `__next__()`, que será el que efectuará la generación en sí misma y deberá
  devolver el número aleatorio siguiente.

- Los valores de `m`, `a` y `c` y la semilla `x0` deben ser configurables al crear el
  objeto (argumentos opcionales del método mágico `__init__()`). Estos cuatro argumentos
  opcionales serán indicados, obligatoriamente, por clave (no pueden ser posicionales).

  Por defecto, los valores de `m`, `a` y `c` serán los usados por el estándar POSIX. El
  de la semilla será `x0=1212121`.

- El método mágico [`__call__()`](https://docs.python.org/3/reference/datamodel.html#object.__call__)
  que sobrecarga la llamada a función, es decir, el uso del objeto como si fuera una
  función con sus argumentos entre paréntesis, se usará para reiniciar la secuencia con
  la semilla indicada en su único argumento, que será forzosamente posicional.

#### Pruebas unitarias de `Aleat`

La cadena de documentación de la clase deberá incluir las siguientes pruebas unitarias
a ejecutar con la biblioteca `doctest`:

##### Comprobación del funcionamiento de `Aleat`

```python
>>> rand = Aleat(m=32, a=9, c=13, x0=11)
>>> for _ in range(4):
...     print(next(rand))
...
16
29
18
15
```

##### Comprobación del reinicio de `Aleat`

```python
>>> rand(29)
>>> for _ in range(4):
...     print(next(rand))
...
18
15
20
1
```

### Generación de números aleatorios usando la función generadora `aleat()`

Escriba la función generadora `aleat()` que implemente el mismo generador de números
aleatorios en el rango $0 \le x_n < m$ que en el ejercicio anterior.

- Los valores de `m`, `a` y `c` y la semilla `x0` deben ser configurables al crear la
  función, y tendrán los mismos valores por defecto que en el caso de la clase `Aleat`.

- En caso de enviársele un valor al generador, con su método `send()`, éste debe
  reiniciar la secuencia tomando el argumento como semilla de la nueva secuencia.

#### Pruebas unitarias de `aleat()`

La cadena de documentación de la clase deberá incluir las siguientes pruebas unitarias
a ejecutar con la biblioteca `doctest`:

##### Comprobación del funcionamiento de `aleat()`

```python
>>> rand = aleat(m=64, a=5, c=46, x0=36)
>>> for _ in range(4):
...     print(next(rand))
...
34
24
38
44
```

##### Comprobación del reinicio de `aleat()`

```python
>>> rand.send(24)
38
>>> for _ in range(4):
...     print(next(rand))
...
44
10
32
14
```

### Entrega

#### Fichero `aleatorios.py`

- El fichero debe incluir una cadena de documentación que incluirá el nombre del alumno
  y una descripción el contenido del fichero.

- La cadena de documentación de la clase `Aleat` debeá incluir:

  - Una descripción del cometido de la clase.
  - Una descripción de los atributos y métodos de la clase.
  - Las pruebas unitarias correspondientes.

- La cadena de documentación de la función generadora `aleat()` deberá incluir:

  - Una descripción del cometido de la función.
  - Los argumentos de la función y la salida proporcionada.
  - Las pruebas unitarias correspondientes.

- Se valorará lo pythónico de la solución; en concreto, su claridad y sencillez, y el
  uso de los estándares marcados por PEP-ocho.

#### Ejecución de los tests unitarios

Inserte a continuación una captura de pantalla que muestre el resultado de ejecutar el
fichero `aleatorios.py` con la opción *verbosa*, de manera que se muestre el
resultado de la ejecución de los tests unitarios.

<img src="testunitario.png" width="640" align="center">

#### Código desarrollado

Inserte a continuación el código de los métodos desarrollados en esta tarea, usando los
comandos necesarios para que se realice el realce sintáctico en Python del mismo (no
vale insertar una imagen o una captura de pantalla, debe hacerse en formato *markdown*).

```python

"""
Ada Salvador Avalos


"""


class Aleat :
    """
    En esta clase se aplica el algoritmo de generación lineal congruente LGC que permite generar secuencias pseudoaleatorias 
    de características controladas.
    Como atributos uso self.m , self.a, self.c, self.xn que son los proporcionados por el constructor, gracias a 
    el que hace una instáncia de la clase Ej : x = Aleat(m = 1, a =2 ,c = 3, x0 = 4) respectivamente.
    self.xn0 es la semilla o valor inicial. Que uso en el iterador (__iter__).


    >>> rand = Aleat(m = 32, a=9, c = 13, x0 = 11)
    Si m es divisible por 4, a - 1 también debe serlo, pero no por 8.
    >>> for _ in range(4):
    ...     print(next(rand))    
    16
    29
    18
    15

    >>> rand(29)
    >>> for _ in range(4):
    ...     print(next(rand))
    18
    15
    20
    1
    
    """
    def __init__(self,m = 2**48, a = 25214903917, c= 11, x0 = 1212121) :
        """
        Método mágico __init__(), es el constructor de una clase que devuelve un objeto de la misma al invocarla como si
        fuera una función. En este caso, los valores de m, a y c y la semilla x0 son configurables al crear el objeto.
        A la vez que comprueba si los valores son óptimos para la generación de números aleatorios 
        usando el algoritmo LGC.

        """
        self.m = m
        self.a = a
        self.c = c
        self.xn = x0
        self.xn0 = x0
        # compruebo si las condiciones son óptimas 
        mcd_mc = self.mcd(self.m , self.c)
        if mcd_mc != 1 :
            print('m y c no deben tener factores primos en común.')

        descomm = self.descompon(self.m)
        if not all((self.a -1)% divm == 0 for divm in descomm) :
            print('a−1 debe ser divisible por todos los factores primos de m (aunque no mucho).')

        if self.m%4 == 0:
            if not((self.a -1)%4==0 and (self.a -1)%8 != 0) :   
                print('Si m es divisible por 4, a - 1 también debe serlo, pero no por 8.')
    

    def __iter__(self) :
        """
         Método mágico __iter__(), se utiliza para permitir que los objetos de la clase sean iterables.
        """
        self.xn = self.xn0
        return self

    def __next__(self) :
        """
        Método mágico __next__(),  efectua la generación en sí misma y devuelve el número aleatorio siguiente.
        
        """
        self.xn = (self.a * self.xn + self.c) % self.m
        return self.xn



    def __call__(self, semilla) :
         """
         El método mágico __call__() que sobrecarga la llamada a función, es decir, el uso del objeto como si fuera una función con sus argumentos entre paréntesis,
         se usa para reiniciar la secuencia con la semilla.
         """
         self.xn = semilla



# funciones cogidas de primos.py para poder hacer las comprobaciones en la clase Aleat()
# ---------------------------------------------------------------------
    def descompon(self, numero):
        """
        Devuelve una **tupla** con la descomposición en factores primos de su argumento.
        La función tiene como argumento un número.

        """
        i= 2
        factor = []
        while range(2,numero):

            if numero%i==0:
                numero= numero//i
                factor.append(i)
            else : 
                i=i+1
        return tuple(factor)


    def dicFact(self,numero1, *numero2):
        """
        Devuelve el factor primo de un número con su correspondiente exponente.
        La función tiene como argumento uno o varios números.

        """
        factores1 = self.descompon(numero1)
        factores2 = []   #lista vacía
        for numero in numero2:
            factores2.extend(self.descompon(numero)) # descompone el numero2 en factores primos  
        #y si se suma la lista creada a la lista factores2
    
        factores = set(list(factores1) + list(factores2)) #se suman factores1 y factores2
    #transformándolos en listas.
        dicfact1 = {factor: 0 for factor in factores}
        dicfact2 = {factor: 0 for factor in factores}
        for factor in factores1: dicfact1[factor] += 1
        for factor in factores2: dicfact2[factor] += 1
        return dicfact1, dicfact2


    def mcd(self,numero1, numero2):
        """
        Devuelve el máximo común divisor de sus argumentos.
        La función tiene como argumento dos números.

        """
        mcd = 1
        dicFact1, dicFact2 = self.dicFact(numero1, numero2)
        for factor in  dicFact1 | dicFact2:
            mcd *= factor ** min(dicFact1[factor],dicFact2[factor])
        return mcd

#------------------------------------------------------------------------------

def  aleat(m = 2**48, a = 25214903917, c= 11, x0 = 1212121)  :
    """
    En esta función se aplica el algoritmo de generación lineal congruente LGC que permite generar secuencias pseudoaleatorias 
    de características controladas.
    Se comprueba si los números son óptimos y implementa el generador de números aleatorios LGC.
    Como argumentos tiene por clave m = 2**48, a = 25214903917, c= 11 y x0 = 1212121, respectivamente.
    Proporciona como salida un número/s aleatorio/s y permite el reinicio de send() usando yield.
    >>> rand = aleat(m=64, a=5, c=46, x0=36)
    >>> for _ in range(4):
    ...    print(next(rand))
    m y c no deben tener factores primos en común.
    34
    24
    38
    44

    >>> rand.send(24)
    38
    >>> for _ in range(4):
    ...     print(next(rand))
    ...
    44
    10
    32
    14
    """
        # compruebo si las condiciones son óptimas 
    mcd_mc = mcd(m , c)
    if mcd_mc != 1 :
        print('m y c no deben tener factores primos en común.')

    descomm = descompon(m)
    if not all((a -1)% divm == 0 for divm in descomm) :
        print('a−1 debe ser divisible por todos los factores primos de m (aunque no mucho).')

    if m%4 == 0:
        if not((a -1)%4==0 and (a -1)%8 != 0) :   
                print('Si m es divisible por 4, a - 1 también debe serlo, pero no por 8.')
    

    xn = x0
    while True :
        xn= (a * xn + c) % m
        reset = (yield xn)
        if reset : xn = reset



# funciones cogidas de primos.py para poder hacer las comprobaciones en la función generadora aleat()
# ---------------------------------------------------------------------
def descompon(numero):
    """
    Devuelve una **tupla** con la descomposición en factores primos de su argumento.
    La función tiene como argumento un número.

    """
    i= 2
    factor = []
    while range(2,numero):

        if numero%i==0:
            numero= numero//i
            factor.append(i)
        else : 
            i=i+1
    return tuple(factor)


def dicFact(numero1, *numero2):
    """
    Devuelve el factor primo de un número con su correspondiente exponente.
    La función tiene como argumento uno o varios números.
    """
    factores1 = descompon(numero1)
    factores2 = []   #lista vacía
    for numero in numero2:
        factores2.extend(descompon(numero)) # descompone el numero2 en factores primos  
    #y si se suma la lista creada a la lista factores2

    factores = set(list(factores1) + list(factores2)) #se suman factores1 y factores2
#transformándolos en listas.
    dicfact1 = {factor: 0 for factor in factores}
    dicfact2 = {factor: 0 for factor in factores}
    for factor in factores1: dicfact1[factor] += 1
    for factor in factores2: dicfact2[factor] += 1
    return dicfact1, dicfact2


def mcd(numero1, numero2):
    """
    Devuelve el máximo común divisor de sus argumentos.
    La función tiene como argumento dos números.
    """
    mcd = 1
    dicFact1, dicFact2 = dicFact(numero1, numero2)
    for factor in  dicFact1 | dicFact2:
        mcd *= factor ** min(dicFact1[factor],dicFact2[factor])
    return mcd

#-----------------------------------------------------------------------


import doctest
doctest.testmod()



```


#### Subida del resultado al repositorio GitHub y *pull-request*

La entrega se formalizará mediante *pull request* al repositorio de la tarea.

El fichero `README.md` deberá respetar las reglas de los ficheros Markdown y
visualizarse correctamente en el repositorio, incluyendo la imagen con la ejecución de
los tests unitarios y el realce sintáctico del código fuente insertado.
