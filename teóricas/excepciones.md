# Excepciones
Definición del mensaje principal que vamos a implementar para handlear excepciones:
- `try`: especifica el bloque que quiere intentar ejecutar   
- `catch`: define la condición de handleo de la excepción. Esta condición siempre está dada por el **tipo de una excepción que se levantó**   
- `handler` de la excepción.     
Acá se puede:     
1. Salir o retornar del bloque que levantó la excepción    
2. Pasar la excepción al próximo handler. 

En smalltalk:
```smalltalk
[...] when: Exception
      do: [:anException | handler exc]
``` 

Donde `[...]` es bloque que queremos intentar ejecutar. 

Implementación TDD que hizo Hernán:
- test 1: no se levanta una excepción, es decir, se ejecuta el bloque correctamente.    
- test 2: se levanta una excepción y se ejecuta el handler correctamente.    
- test 3: cuando se levanta la excepción se tiene que cortar la ejecución.    
- test 4: se levanta una excepción que no es handleada.    
- test 5: definimos una condición de handleo con un closure.       
- test 6: podemos tener un handler de un handler    
Bueno acá me cansé ya jajaj se vuelve re rebuscado


Importante:
- *Podemos definir como condición de handleo de una excepción cualquier cosa que sepa responder el mensaje `shouldHandle:`. Puede ser un bloque, un tipo de excepción, etc*

---
Entonces... (esto es de la bibliografía)
# ¿Qué es una excepción?
Las excepciones son un mecanismo para manejar situaciones inesperadas que interrumpen el flujo normal de ejecución. 

Un sistema de excepciones debe proveer tres propiedades fundamentales:
1. **Separación entre código normal y código de recuperación**. El manejo de errores se define aparte.     
2. **Propagación estructurada**. Si un bloque no maneja la excepción, esta sube hasta encontrar un handler adecuado.    
3. **Recuperación controlada**. Un handler puede decidir reparar, ignorar, reintentar, propagar, etc.

### Flujo de manejo de excepciones
1. Se ejecuta el bloque
2. Si ocurre una excepción:    
    1. Se crea un objeto excepción     
    2. El sistema busca un handler cuya condición satisfaga `condition shouldHandle: exception`     
    3. Se ejecuta el handler
3. El handler puede decidir:    
    - Retornar del bloque donde ocurrió la excepción    
    - Re-lanzar la excepción   
    - Consumirla y continuar    
    - Reintentar el bloque (Smalltalk no soporta esto)

- Las excepciones son objetos
- Los handlers son objetos
- Las condiciones de manejo son objetos
- El mecanismo es polimórfico
- Todo ocurre por envío de mensajes 
- Sistema flexible, modular y reusable

---
# MÁS SOBRE EXCEPCIONES 
## [A Fully Object-Oriented Exception Handling System: Rationale and Smalltalk Implementation](https://www.lirmm.fr/~dony/postscript/exc-book2001.pdf)

## Definiciones:
- Excepción: situación que lleva a la imposibilidad de la terminación de una operación.
- `EHS` (Exception Handling System) permite a los usuarios identificar excepciones y asociarlas a entidades (un programa, una clase, un objeto, etc). 
- El handler puede decidir:    
    1. *Reanudar*. Decide arreglar el problema y dejar que el programa siga ejecutándose justo después del punto donde ocurrió la excepción.
    2. *Terminación*. 
    3. *Propagación*. Handler delega la responsabilidad a un handler más general. 

## Modelo dual: reanudación y terminación
La primera gran decisión al crear un sistema de manejo de excepciones es:     
*¿Qué tipo de control puede tener un handler?*

La mayoría de los sistemas solo ofrecen terminación: Java, Python, C++...   
Cuando ocurre una excepción **no se puede volver al punto en donde se produjo**. 

Otros lenguajes ofrecen un **modelo dual**, como Smalltalk, Common Lisp, etc. Ofrecen la opción de **terminar o reanudar la ejecución después de arreglar la situación**. 

> *“forbid termination is a very specific choice because many exceptions are really fatal.”*     
> Ejemplo: división por cero.

> *"to forbid resumption (...) produces simpler systems (...) although reducing expressive power"*   
> Ventajas:   
> - sistema más simple    
> - compilador y análisis del programa se simplifican
> - implementación más fácil
> 
> Desventajas: 
> - lenguaje pierde expresividad

La reanudación es **costosa** ya que el sistema debe guardar el estado exacto donde ocurrió la excepción por si se desea continuar desde ahí.

Ejemplos donde es útil reanudar:
- App web pierde conexión -> usuario reintenta -> se continúa la operación.  
- Formulario está incompleto -> el usuario corrige -> se sigue sin reiniciar todo

## Scope de un handler
Cuando se lanza una excepción, el sistema debe decidir en qué orden buscar el handler que la maneje. 

### 1) Scope Léxico
Handlers que se buscan siguiendo la estructura textual del programa.     
Ejemplo
```C
module A
   module B
      ⟶ excepción
```
Si en `B` no hay handler, se busca en `A` porque está textualmente por afuera.

Handlers pueden manejar **todas las excepciones que ocurren dentro de su bloque textual**. 

**Ventaja**:
Se puede saber estáticamente (sin ejecutar el programa) qué handler atenderá cada excepción. 

**Desventaja**:
Como la búsqueda está restringida al texto, una excepción nunca sale del módulo donde ocurrió.   
Eso significa que un método que llama a otro nunca puede recuperar el control si el otro falla.

Ejemplo:
Smalltalk-80 sólo tenía **lexical scope handlers** basados en métodos de clase:

```smalltalk
self error: 'message'
```

### 2) Scope Dinámico
Handlers que se buscan en la **pila de ejecución**-

Cuando ocurre una excepción, se mira quién llamó a quién.

- Permiten distinguir:
    - Excepciones internas -> manejadas dentro del módulo
    - Fallas externas -> propagadas al cliente del módulo
- El caller puede recuperar el control. Esto permite que cada módulo tenga una interfaz de fallas.
- El contexto del caller importa. El handler puede tomar decisiones según dónde se llamó el método.

Smalltalk moderno:
```smalltalk
[ self doSomething ] 
    on: SomeError
    do: [ :ex | Transcript show: 'Error!' ].
```

Cuando ocurre un error dentro del `doSomething` lo que hace el sistema es:
- Busca este `on:do` en la pila de ejecución, hacia atrás.
- Si lo encuentra, lo usa.
### Para entender la diferencia
**Scope léxico**    
```
class A {
    class B {
        [código que falla]
    }
}
```
La búsqueda es:   
- ¿Hay handler en B?
- Si no, ¿hay uno definido en A?

**Scope dinámico**
```
A llama a B
B llama a C
C falla
```
La búsqueda es:
- ¿Hay handler en C?
- Si no, ¿hay handler en B?
- Si no, ¿hay handler en A?

## Tipos de excepciones
> Una excepción de primera clase es una excepción que es tratada como un objeto completo dentro del modelo de objetos del lenguaje: puede ser creado, pasado, inspeccionado, almacenado, y manipulado como cualquier otro objeto, sin depender de primitivas especiales del lenguaje.

### 1) Eventos excepcionales como objetos de primera clase (Primer nivel: solo las ocurrencias son de primera clase)
Cada **tipo de excepción es una clase**, y cada **ocurrencia concreta de esa excepción** es una **instancia de esa clase**.

Ejemplo:
- La excepción `FileNotFound` es una clase
- Si el sistema intenta abrir un archivo y no existe, ese evento específico es una instancia de la clase `FileNotFound`

**Ventajas**:
1. **Herencia entre excepciones**. Podemos organizar excepciones en jerarquías, compartir comportamiento, etc.
2. **Un mismo handler puede capturar varias excepciones**. Si varias excepciones heredan de un ancestro común, podemos manejar todas con un único bloque.
3. **El objeto de excepción puede contener información útil**. El “signalizador” (el que lanza la excepción) puede pasar datos al “handler” a través de los atributos del objeto excepción (slots).
4. **El usuario puede definir nuevas excepciones**. Y funcionan igual que las del sistema. Todo es uniforme.

### 2) Eventos excepcionales como entidades de primera clase (Segundo nivel: ahora clases + instancias son de primera clase, y el mecanismo también)
No solo las instancias (eventos) son objetos, sino que las propias clases de excepción también deben ser objetos totalmente integrados en el modelo OO. 

- **Excepciones son instancias de una metaclase**. Clases de excepción son entidades de primera clase, y esas clases de excepción son instancias de una metaclase.        
    Ejemplo:
    - metaclase llamada `ExceptionClass`
    - cada clase de excepción es una instancia de esta metaclase
    - esto permite definir comportamiento común para todas las clases de excepción
- **Dos clases núcleo en su modelo**   
    - **ExceptionalEvent**:    
        - **Clase base para las instancias**
        - Cada excepción concreta (tipo de excepción) es una subclase de esta
        - Proporciona el protocolo básico de manejo
    - **ExceptionClass**:
        - Metaclase
        - Todas las clases de excepción son instancias de esta
        - Proporciona el protocolo de señalización (signaling).

Las **clases de excepción heredan de ExceptionalEvent**, pero al mismo tiempo son **instancias de ExceptionClass**.

**Ventaja**:
El sistema de manejo de excepciones deja de ser un “mecanismo especial del lenguaje” y pasa a ser un conjunto de objetos completamente integrados en el modelo de objetos.

### Analogía:
**Modelo 1:**
- `FileNotFound` es una clase.
- `FileNotFound new` es el evento

**Modelo 2:**
- Idem modelo 1.
- `FileNotFound` además es un objeto cuya clase es `ExceptionClass`
- Esto permite definir métodos como:     
```smalltalk
FileNotFound signalWith: 'hola.txt'
```
Porque `signal` ya no es una primitiva ad-hoc, sino un **método definido en su metaclase**.

## Tipos de handlers
### Handlers asociados a expresiones
Son los handlers que **se aplican a un bloque de código específico**.
```smalltalk
[ expresión protegida ]
   when: SomeException
   do: [:ex | ... handler ...]
```
- El primer argumento es la clase de excepción a capturar.
- El bloque después de `do:` es el **handler**, recibe como parámetro el objeto excepción.

> Concepto clave:
El handler está asociado a un cacho de código, no al programa entero ni a un objeto.
### Handlers asociados a clases
> *“Mientras se ejecutan métodos de esta clase, si pasa tal excepción, manejarla de tal forma”*. Es decir, queremos que toda la clase tenga una política común de manejo de excepciones, sin tener que envolver cada método con handlers.

- se asocia a una clase (y se hereda hacia las subclases)
- tiene alcance dinámico
- intercepta cualquier ocurrencia de la excepción E durante la ejecución de cualquier método de esa clase o subclase.

Ejemplos:
- En una clase Stack, no quiero que un Overflow salga hacia afuera.
- En una clase DatabaseConnection, quiero que un Timeout genere un reconectar automático.
- En una clase Parser, quiero que SyntaxError se convierta en otra excepción más informativa.
### Handlers por defecto
Funcionan como último recurso (backup).   
Sirven para:
- darle un comportamiento global a cada tipo de excepción.
- que cada excepción pueda tener su propia política de manejo predeterminado.
- proveer una interfaz interactiva

