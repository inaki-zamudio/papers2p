# Técnicas de implementación de llamadas polimórficas

3 categorías: 
- Técnicas generales
- Técnicas dinámicas: pueden computar alguna información en tiempo de compilación, pero modifican dinámicamente en tiempo de ejecución sus estructuras de búsqueda. Debido a esta característica, las técnicas dinámicas pueden explotar información que se obtiene en tiempo de ejecución para mejorar los algoritmos y la performance.
- Técnicas estáticas: calculan todas las estructuras de búsqueda en tiempo de compilación o linkedición y no permiten realizar cambios en tiempo de ejecución. Utilizan solo la información obtenida de la compilación de código fuente de los programas. 

### Técnicas generales: 
- Dispatch Table Search
- Selector Table Indexing

### Técnicas estáticas:
- Virtual Function Tables
- Virtual Function Tables- Multiple Inheritance
- Selector Coloring
- Row Displacement
- Compact Tables

### Técnicas dinámicas:
- Global Lookup Table
- Inline Cache
- Polymorphic Inline Cache

### Factores que influyen en los algoritmos de despacho de mensajes

1. Costo de ejecución. 
2. Espacio de memoria de las estructuras de búsqueda. 
3. Espacio del código producido para la búsqueda. Hay algoritmos de búsqueda que se basan en una única "función" como punto de entrada del proceso de enviar un mensaje. ej: la implementación de Squeak, el cual siempre llama al método findNewMethodInClass: de la máquina virtual.
Existen otras técnicas que se basan en producir código de búsqueda dependiendo de cada selector. Este código es agregado como parte del envío del mensaje. Por ejemplo, si el compilador determina que el mensaje m1 posee una única implementación, podría llamar a un método de búsqueda especializado para dicha característica.
4. Código del prólogo del método.
Hay soluciones que utilizan un prólogo dentro de cada método que se encarga de verificar si la clase en la cual se está ejecutando es igual a la que se estaba esperando. En caso negativo, se realiza una nueva búsqueda, en caso positivo se continúa con la ejecución del método.
5. Tiempo de generación. 

# Técnicas generales de llamadas polimórficas

# Técnicas generales
### Algoritmo básico de despacho de mensajes (DTS)
En lenguajes orientados a objetos la resolución de una llamada polimórfica es denominada message dispatch. 
Message dispatch, dado un nombre de mensaje (selector) y la clase del objeto al que se le envía mensajes (receiver), busca la correcta implementación, denominada método.

Utiliza:
- una **tabla por clase**, donde se almacena una estructura de búsqueda (generalmente un diccionario) los pares `{selector, method}`. Cuando un *objeto recibe un mensaje*, el mismo le *pide a su clase que le devuelva el método para dicho mensaje*. *Si dicho mensaje no es implementado en esa clase, se transmite la búsqueda a la superclase, y así sucesivamente hasta llegar a la raíz del árbol de jerarquía*. En el caso que no se haya encontrado ninguna implementación en toda la jerarquía, se emite un error.

---
- Cada clase sólo tiene las **referencias a los métodos que implementa**, por lo que el total de **espacio ocupado por las estructuras de búsqueda será proporcional a la cantidad de métodos del sistema**
- **DTS** es utilizado generalmente **COMO ESTRATEGIA DE BACKUP**, es decir, es invocado cuando un algoritmo de búsqueda más rápido falla.

### Tabla indexada de selectores (STI)
Utilizamos un **arreglo** para guardar **referencias a objetos** (en este caso instancias de la clase Selector). 
- usando **índice**: índices *no se pueden repetir*, por lo que encontramos la entrada correspondiente a un objeto. El problema es que *se debe saber de antemano cual es la dimensión máxima que debe tener el arreglo*. 
- usando **número de hashing**: me indica donde *podría* estar un objeto dentro del arreglo, pero no lo asegura puesto a que el número de hashing es compartido por varios objetos. Debido a esto, es necesario *realizar una comparación por igualdad para asegurar que se encontró el objeto requerido* y en caso negativo recorrer el arreglo hasta encontrarlo o determinar de alguna manera que no se encuentra en él. Esta búsqueda es más lenta.

- Tiempo consumido por el algoritmo es **constante**.
- En un sistema de M clases y N métodos, STI tiene un array bidimensional de M*N entradas, donde cada intersección contiene la referencia al método para la clase y el selector correspondiente. En caso de que el método no exista para esa clase, contiene una referencia a la implementación del selector #doesNotUnderstand:.
- **Rápida** pero **consume mucho espacio en memoria**. Además, un **alto porcentaje de la misma se encuentra completamente vacía**.

# Técnicas dinámicas
Se basan en *"cachear"* los resultados de la búsqueda realizada por algún algoritmo para luego no tener que utilizar nuevamente tiempo de ejecución en la búsqueda de métodos ya ejecutados. 

Tienen dos características principales:
1. Se basan en estadísticas sacadas sobre la ejecución de programas típicos, por lo que *su efectividad depende del comportamiento de los programas*. 
2. Los algoritmos difieren en cómo utilizan la caché, por ejemplo si poseen una cache global para todo el sistema, o una por cadad lugar donde se envíe un mensaje.

## Cache Global de Búsqueda (Global Look-up Cache, GLC)
La idea es tener una **cache global** en la cual se almacenan las tuplas **{clase, selector, método}**.
En el momento de enviar un mensaje, el algoritmo busca el método correspondiente en la cache global. En caso de no ser encontrado, se utiliza un algoritmo de backup para buscarlo y agregarlo a la cache.

- Se produce un 90% y 95% de cache hit sobre el total de mensajes enviados,  
- la disminución de los tiempos que se obtienen debido al alto grado de cache hit, se ve contrarrestado por el gran tiempo que insume el algoritmo de búsqueda de backup.
- grandes falencias cuando el código es altamente polimórfico y el grado de cache hit es bajo

## Cache en-sitio (Inline Cache)
Se basa en el hecho de que *la clase del objeto receptor de un mensaje varía raramente en el punto en el que se envía dicho mensaje*. 
Esto significa que si se envía el mensaje "m1" al objeto "x", la clase del objeto "x" será generalmente la misma. 

Inline cache varía la ubicación de la cache con respecto a GLC, de global a local a cada envío de mensaje. 

- Inicialmente la llamada dentro del método que se está ejecutando está apuntando a una función de búsqueda de backup, la cual devuelve el método a ejecutar. 
- Una vez realizada la primer búsqueda el código es modificado para que apunte a la dirección del método encontrado. La próxima vez que se ejecute este envío de mensaje, el programa irá directamente a ejecutar el código del método encontrado anteriormente.
- El problema se produce cuando la clase del objeto receptor del mensaje cambia, puesto que esto implica que el valor de la cache dejó de ser válido. Para impedir que se ejecute un código indebido, todos los métodos tienen un prólogo que comparan que la clase del método que se está ejecutando coincida con la clase del objeto receptor del mensaje, que en caso positivo implica ejecutar el método. En caso negativo, se llama a la rutina de búsqueda de backup y se actualiza el puntero que tenía el método llamador hacia el método llamado.
- Mientras la clase del objeto receptor no cambie, el costo de la llamada polimórfica se mantiene constante en todo momento y el único costo adicional en el que se incurre es en el del chequeo de la clase en el prólogo del método.


## Cache polimórfica en Sitio (Polymorphic Inline Caching, PIC)
Problema con Inline Cache -> en algunos casos el sistema utiliza un 25% de su tiempo manejando cache misses, más para aquellos sistemas altamente polimórficos.

- Al principio, PIC se comporta como IC. Inicialmente la llamada correspondiente al envío del mensaje apunta a una rutina de resguardo que serça la encargada de buscar la dirección del método a ejecutar. Una vez encontrado, modifica dicha direcciçon por la del prólogo del método encontrado. 
- Si la clase cambia, la nueva dirección no se sobreescribe como en IC. Por el cntrario, crea un "stub" y hace que la llamada apunte ahora a dicho stub. El stub se encarga de buscar el método a ejecutar a partir de la clase del objeto receptor del mensaje y llamar al método correspondiente en caso de encontrarlo. 

De las técnicas dinámicas, la que mejor performance tiene es PIC, sin embargo es la más complicada de implementar. IC tiene una performance aceptable pero no se comporta bien en lugares donde se envían mensajes a objetos que pueden ser instancias de distintas clases. 

# Técnicas estáticas
Tienden a dar prioridad a la performance sobre la flexibilidad del sistema. 
Las técnicas estáticas pre-calculan las estructuras de búsqueda en tiempo de compilación y/o linkedición para disminuir el tiempo que se utilizaría en tiempo de ejecución para realizar la búsqueda de los métodos.

## Tabla de Funciones Virtuales (VTBL)
Funciona únicamente con lenguajes tipados estáticamente.
A diferencia de STI, en VTBL las tablas de métodos son por clase y no globales. 

Por cada clase C:
- se determina la cantidad de métodos que posee desde la raíz del árbol de jerarquía
- se crea un array de dicha dimensión
- se numeran los métodos empezando por la clase raíz
- a cada objeto se le agrega un puntero a dicha tabla virtual
- el código del envío del mensaje hace uso de la numeración de los métodos como índices dentro de la tabla

Desventajas:
- solo puede ser utilizada en lenguajes con un sistema de tipos estático, con la consiguiente pérdida de flexibilidad
- genera más información de la estrictamente necesaria. Esto se debe a que cada clase no solo contiene la dirección de los métodos que ella define sino también la de sus superclases. 
- tener que re-numerar los métodos de las subclases cuando se modifica una superclase. 

Conclusiones sobre técnicas estáticas:
- tienden a producir un despacho de mensajes mucho más rápido que las dinámicas.
- el tiempo utilizado para la búsqueda tiene cota superior puesto que no depende del comportamiento del programa como en GLC o de la localidad del mismo como IC y PIC.
- tiempo que utilizan para generar las estructuras de datos que necesitan para funcionar es mucho mayor que las de las técnicas dinámicas. 
- mayor inconveniente: tiempo de compilación necesario y falta de flexibilidad y adaptabilidad al comportamiento del sistema.