# Generación de números aleatorios con una computadora cuántica

## Introducción
El propósito del siguiente código es proporcionar en unas pocas líneas, una introducción a cómo utilizar una computadora cuántica desde un lenguaje familiar como Python.
*No es la intención aquí proporcionar las bases de la computación cuántica, se explicará lo básico para poder completar el objetivo. Asimismo, este objetivo, que será la generación de números aleatorios, no se puede tomar como un ejemplo productivo, y se debe tomar solo como un ejemplo con intenciones educativas y de demostraciones técnicas.* Es necesario tener conocimientos básicos de Python para poder comprender el código y la explicación.
 
## ¿Cómo se generan los números aleatorios en una *computadora clásica*?
Existen múltiples métodos para generar números aleatorios en una computadora. Si bien desde el punto de vista de un programador generalmente esto implica importar una librería y hacer una llamada a un método, hablando a bajo nivel la generación destos números no siempre es tan simple. Existen dos enfoques distintos para generar números al azar: PRNGs (Generador de números pseudo-aleatorios o Pseudo-Random Number Generator) y TRNGs (Generador de números aleatorios reales o True Random Number Generators).
El enfoque de PRNGs es el más común, ya que se basan en teoría matemática para generar los números que, como dicen las siglas son "pseudo-random". Esto significa que los números que nos devuelve un PRNG, no son 100% aleatorios, ya que para poder generarlos se implementa un algoritmo matemático. Esto hace que, aunque sea muy complejo y en algunos casos prácticamente imposible, los números pseudo-aleatorios puedan predecirse.
Por otro lado, los TRNG son hardware específicos que interpretan variables del aleatorias del entorno (ruido electromagnético, ruido térmico, etc.) y las traducen en señales electrónicas que pueden digitalizarse y representarse en forma de números. Este tipo de dispositivos son capaces de proporcionar números aleatorios reales, ya que no existe un algoritmo de por medio para generarlos. 
 
## Computadora cuántica vs. Computadora clásica
Existen numerosas diferencias entre ambos tipos de computadoras. Estas diferencias van desde lo relacionado al *hardware* hasta el *software*. Esto hace que tanto la forma de construirlas como la forma de programarlas sea distinta.
 
Comencemos con una diferencia fundamental:
- Las computadoras clásicas utilizan el sistema binario para representar toda la información, y la unidad mínima de información es el bit (proveniente de ***b***inary dig***it***). Cada bit puede contener solo dos valores posibles: *1* o *0*.
- Las computadoras cuánticas utilizan Qubits (Quantum bits) en vez de bits. Los Qubits tienen dos estados, que en la notación cuántica se conocen como |0> y |1>, pero que aquí podemos compararlos directamente con el 0 y 1 binario clásico ya que no indagaremos en profundidad qué representan esos estados. Entonces, si el qubit tiene dos estados, *¿Qué diferencia hay entre un Qubit y un bit?* La diferencia es enorme, y, si bien en nuestro mundo digital veremos al qubit como 0 o 1 al final del recorrido, dentro del mundo cuántico, el Qubit tiene una determinada *probabilidad* de ser 0 o 1. Es por esto que se suele decir que un Qubit tiene 3 estados, 0, 1 y 0/1. Este estado "ambigüo" se conoce como *superposición*. 
 
Esta diferencia fundamental es todo lo que necesitamos saber por ahora y para realizar el ejercicio que plantea el título (generar un número aleatorio con una computadora cuántica). De todas maneras, hay que mencionar algo no menor: Este fenómeno de superposición de estados hace que en una computadora cuántica sea posible representar múltiples estados al mismo tiempo, algo imposible de realizar con una computadora clásica dónde cada bit tiene dos estados perfectamente definidos. Es por esto que la computación cuántica promete un salto inmenso en la capacidad de procesamiento frente a la computación clásica. 
 
## Representación de números en una computadora
En los párrafos anteriores se mencionó que una computadora clásica utiliza el sistema binario para representar la información, y es necesario entender eso un poco más en detalle. 

El sistema binario es un sistema de numeración posicional (igual que el sistema decimal que utilizamos principalmente), pero que solo utiliza dos cifras (0 y 1), y como base aritmética las potencias de dos. Para comprenderlo de una manera más práctica, observemos el siguiente número:

*1011*

Si lo pensamos *en decimal*, el número es el mil once. Pero, ¿Qué número es si digo que está en binario? Al ser solo 1s y 0s los dígitos, podría serlo. Es difícil interpretar qué número es si está en binario, entonces hagamos un pasaje de ese número a decimal, que nos será más familiar y fácil de leer. Que el binario sea un sistema *posicional* que utiliza *base dos*, significa que cada dígito, dependiendo de su posición tiene un "peso", que corresponde a una potencia de dos. Por ejemplo, el dígito que está más a la derecha (menos significativo), representa la potencia 2^0. El segundo de izquierda a derecha "pesa" 2^1, y así podemos tener *n* dígitos, cada uno con su peso dependiendo de la posición. Para poder convertir nuestro número binario en decimal, multiplicaremos el valor de cada dígito, por el peso que corresponda a la posición:

Binario =    1 0 1 1

Decimal = $ (1*2^3) + (0*2^2) + (1*2^1) + (1*2^0) = 11 $

Como se puede observar, el número binario representaba el once. Los paréntesis en la fórmula matemática están solo de ayuda, para que sea más claro identificar la múltiplicación de cada dígito binario por el peso dado por una potencia de dos.

Habiendo visto cómo el binario se puede utilizar para representar números, estamos en condiciones de seguir adelante con nuestro propósito de generar los números aleatorios. De todas maneras recomiendo buscar recursos extra sobre los distintos sistemas de numeración, así como también los distintos *formatos* que utiliza una computadora para almacenar otro tipo de números (por ejemplo, el número 1011 utilizado antes, en formato de *complemento a 2*([Complemento a 2 en Wikipedia](https://es.wikipedia.org/wiki/Complemento_a_dos)) representa el número *-5*) 

## Herramientas para el desarrollo del algoritmo
Para poder desarrollar nuestro algoritmo de generación de números aleatorios, haremos uso de la plataforma de computación cuántica [IBM Quantum Experience](https://quantum-computing.ibm.com/). La misma nos permite acceder a hardware cuántico, además de proveer una librería completa de desarrollo [Qiskit](https://qiskit.org/) que podemos incluir en Python. En esta librería están incluidos distintos tipos de simuladores que nos serán de utilidad para probar nuestro código antes de ejecutarlo en un hardware real.

## Implementación del algoritmo
### Introducción y conceptos fundamentales
Un circuito cuántico se puede describir como un conjunto de *compuertas* que se aplican sobre uno o más *Qubits* para cambiar su estado. Una vez que el estado se encuentra como necesitamos, se realiza una *lectura* para observar el resultado. 

Previamente se mencionó que un *Qubit* no posee un estado definido, y que por el contrario, el estado está dado por una determinada *probabilidad* de ser 0 o 1. Para poder comprender esto mejor, propongo la siguiente analogía:

>Imaginemos que de frente tenemos una pelota que se mueve de izquierda a derecha constantemente. Cuando se encuentra en el punto más a la izquierda decimos que está en estado *0*, y cuando se encuentra en el punto más a la derecha decimos que está en estado *1*.<br>
Supongamos ahora que un objeto nos obstruye la visión y dejamos de ver la pelota. Sabemos que se mueve, pero no sabemos exactamente dónde está.
<br>Ahora bien, si quitamos el obstáculo volvemos a ver la pelota, y en función de su posición, diremos que está *a la izquierda* (*estado 0*) o *a la derecha* (*estado 1*).<br>
Si conociéramos las características del movimiento (por ejemplo, si acelera en algún momento cuando se mueve de izquierda a derecha), podríamos determinar la probabilidad de que, cuando saquemos del medio el obstáculo, la pelota esté en *0* o en *1*.<br>
Por último, supongamos que tenemos un control remoto que nos permite modificar las características del movimiento de la pelota (por ejemplo, le podemos cambiar la aceleración a medida que se acerca al lado derecho). Esto nos daría un poco más de control para que cuando veamos la pelota, tengamos más chances de verla donde nosotros queremos, sea a la izquierda o a la derecha.<br>
Ahora bien, ¿Dónde está la analogía? La pelota representa los **Qubits**, el control remoto serían las compuertas. ¿Por qué cuando la veamos va a estar en un lado o en el otro? Esto se da por un fenómeno cuántico llamado *decoherencia*. Cuando no vemos la pelota, decimos que está en un estado de *superposición cuántica* algo que no es *ni a la izquierda, ni a la derecha*, pero cuando volvemos a ver la pelota, ese estado de superposición **colapsa** para quedar a la izquierda (0) o a la derecha (1). Por último, el control remoto representaría las **compuertas**, que nos permiten modificar la probabilidad de que el **Qubit** colapse en 1 o en 0.<br>
**IMPORTANTE:** Una vez que el Qubit colapsó en alguno de los estados, permanecerá así. Cada vez que lo veamos, estará siempre igual, a menos que reiniciemos el estado general de nuestro sistema (o en el caso de un programa, volvamos a ejecutarlo).

Nuestro objetivo será crear un circuito que modifique el estado de un Qubit para que al leerlo podamos generar un número aleatorio. Para eso necesitaremos *codificar* el resultado cuántico a un resultado binario que entienda nuestra computadora clásica.

### Posible solución
Supongamos que tenemos disponible un Qubit. Utilizando compuertas modificamos el estado del mismo para que la probabilidad de de estar en 0/1 sea de 50% para cada lado. Luego *medimos* en qué estado está el Qubit.
Si ejecutamos esta secuencia *n* veces, obtendremos *n* 1s o 0s, que estarán dados por la probabilidad antes mencionada.<br>
¿Las computadoras clásicas no utilizaban 1s y 0s para representar los números? ¡Sí!<br>
¿Los *n* 1s y 0s que veamos no van a ser "aleatorios" por la probabilidad de 50% en cada lectura? ¡Sí!<br>

### Circuito correspondiente
Empecemos a escribir código. Primero será necesario importar las librerías:

    from qiskit import Aer, QuantumCircuit, execute

Definimos un circuito cuántico, en el que usaremos 1 Qubit y aplicaremos luego una compuerta que configure dicho Qubit en el estado que necesitamos.

    # Definimos un circuito cuántico con 1 qubit y 1 bit que utilizamos para obtener el resultado.
    # Los Qubits y bits en el circuito se encontrarán numerados comenzando en el índice 0
    CIRCUIT = QuantumCircuit(1,1)

    # Agregamos la compuerta Hadamard (h) a nuestro circuito. 
    # Esta compuerta configura el Qubit en el estado de superposición, 
    # que nos da un 50% de probabilidad de colapsar en alguno de los dos estados.
    CIRCUIT.h(0)

    # Medimos el Qubit 0 y almacenamos el resultado en el bit 0.
    CIRCUIT.measure(0,0)

Ya están importadas las librerías, creado el circuito, solo falta ejecutarlo, y para eso utilizaremos el simuladora.

    # Definimos el simulador
    BACKEND = Aer.get_backend('qasm_simulator')

    # Ejecutamos el circuito y guardamos el resultado. El parámetro shots
    # resulta fundamental ya que indica la cantidad de veces que ejecutaremos
    # nuestro circuito.
    JOB = execute(CIRCUIT,
                  backend=BACKEND,
                  shots=8,
                  optimization_level=3,
                  memory=True)

    # Obtenemos el resultado de las ejecuciones (shots). El mismo estará
    # representado como un array, en el que tendremos los 0s y 1s que medimos
    # con la operación measure del circuito.
    MEMORY = JOB.result().get_memory(CIRCUIT)

    # Imprimimos el resultado
    print("Binary generated  =", 
          "".join(MEMORY))
    print("Integer generated =",
          int("".join(MEMORY),2))

 
El código escrito arriba realiza nuestro objetivo de una manera simple. Debajo en este notebook hay una implementación con algunas modificaciones pero no impactan en la funcionalidad principal.

## Conclusión
Como se mencionó al principio, este código no debe ser tomado como algo productivo, de hecho no tendría sentido utilizar una computadora cuántica para este objetivo tan simple, pero quizás sirve como una introducción para entender cómo se comunican ambos mundos, o por lo menos tener una pequeña experiencia con una simulación cuántica.

Si se quiere profundizar en el tema, recomiendo ampliamente usar como recurso el libro que se encuentra en la documentación de Qiskit, [Learn Quantum Computation using Qiskit](https://qiskit.org/textbook/preface.html). 


