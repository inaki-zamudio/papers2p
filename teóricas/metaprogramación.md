# Metaprogramación y metamodelo
- [Video Hernán metaprogramación](https://www.youtube.com/live/gNv_MHG1DFg?si=uB3p6kU7TcXzCmMC)

### Índice:
- [Metaprogramación](#metaprogramación)
    - [Tipos de metaprogramación](#ejemplos-de-tipos-de-metaprogramación-que-podemos-hacer)
    - [Resumen](#resumen-metaprogramación-y-conceptos-clave)

# Metaprogramación
"Meta *algo*": Habla sobre o define sobre *algo*. Por ejemplo, las clases se podrían llamar "meta instancia", porque es el *objeto que define cómo son sus instancias*.

Metaprogramación: programar sobre el dominio del problema (el programa).

Existe una conexión causal entre el sistema y el dominio porque cada vez que cambia algo en el dominio cambia algo en el sistema y viceversa.  

Un metasistema sería un sistema cuyo dominio de problema es otro sistema. 

Sistema reflexivo: un sistema que está causalmente conectado consigo mismo (puede reflexionar y actuar sobre sí mismo). Los seres humanos, simplificando, somos sistemas reflexivos ya que podemos reflexionar sobre lo que hacemos. 

- **Reflexión**: habilidad integral de una entidad para representar, operar sobre y tratar consigo mismo en la misma manera que representa, opera sobre y trata con su sujeto primario. Otro ejemplo: los lenguajes naturales (cuando uno habla del castellano, puede utilizar el castellano para hablar de él, y si tengo que definir una nueva palabra utilizo el castellano para hacerlo), son lenguajes **metacirculares**. 

- **Instrospección**: habilidad de un programa de razonar acerca de si mismo y/o la implementación del lenguaje de programación (read). *Posibilidad de leer aquello sobre lo que está reflexionando*

- **Intercession**: la habilidad de un programa de "actuar" sobre la reificaciones de si mismo y la implementación del lenguaje de programación (write). *Posibilidad de escribir/actual sobre aquello sobre lo que está reflexionando*.      
*REIFICAR: hacer objeto algo que no era. Por ejemplo, cuando a un método lo convertimos en una clase.*

En los lenguajes de programación, además de tener metaprogramación sobre la estructura del lenguaje, también tenemos reflexión sobre el comportamiento del lenguaje, sobre la ejecución de un programa. 

## Ejemplos de tipos de metaprogramación que podemos hacer:

|                 | **Read**                                   | **Write**                       |
|-----------------|---------------------------------------------|----------------------------------|
| **Structure**   | - All classes<br>- Does implement?<br>- Design rules? | - addInstVarNamed:<br>- compile: |
| **Behaviour**   | - Assertion Name<br>- Debugger              | - create method<br>- Pluggable Proxy<br>- Debugger |

### Metaprogramación estructural-lectura:
*Ejemplo: analizar una jerarquía de clases.*

```smalltalk
Object subclasses
```
Estamos haciendo lectura sobre la jerarquía de clases cuya superclase es Object. Nos devuelve las inmediatas.

Otro mensaje que podemos usar es `allSubclasses` que devuelve todas las subclases de Object, que no son solo las inmediatas. 

También le podemos pedir la superclase a cualquier clase. El tope de las superclases es ProtoObject. 

También dada una clase, podemos hacer lectura de las variables de instancia definidas:

```smalltalk
OrderedCollection instVarNames
```

**Este tipo de metaprogramación existe en la mayoría de los lenguajes de la actualidad.** Lo podemos hacer en java, C#, python, etc.

A una clase le podemos pedir todos los mensajes que implementa (mensajes que van a saber responder sus instancias):
```smalltalk
OrderedCollection selectors
```

También le podemos pedir cómo está implementado un mensaje:
```smalltalk
OrderedCollection compiledMethodAt: #removeAll
```
Nos devuelve un objeto que representa un método.

A un método también le podemos hacer metaprogramación estructural (porque es un objeto). Por ejemplo, le podríamos preguntar cuál es el código fuente relacionado a él. 

Entonces podemos leer estructuras de clases, de objetos, etc. 

**Cuando hacemos metaprogramación rompemos el encapsulamiento, porque tiene la posibilidad de poder meterse en todos los objetos**.

En smalltalk, también podemos buscar todas las instancias de una clase:
```smalltalk
Set allInstances
```
Nos devuelve un array con todos los objetos que tiene el sistema que son instancias de la clase `Set`. Es metaprogramación estructural-lectura **sobre el sistema completo** (en este caso sobre la memoria).

*Metaprogramación significa que tenemos metaobjetos que observan a otros objetos. Un metaobjeto de alguna manera representa a un programador.*

> *¿Para qué se puede utilizar la programación estructural a nivel lectura? Algunos usos:*     
> 1. Poder definir reglas de diseño que quiero que todo el grupo de desarrollo cumpla cuando está programando. Ya que podemos analizar los programas que escribimos, podemos formalizar la verificación de que esas reglas de diseño se cumplan. Ejemplo: *regla de diseño que se asegure de que nuestros métodos no tengan más de cierta cantidad de colaboraciones. Para implementarlo, podríamos pedir el AST del método (Abstract Syntax Tree) con `methodNode` y recorrerlo para sumar los que son de tipo `messageSend`, para saber la cantidad de colaboraciones del método.*      
> 2. El patrón Visitor debe seguir ciertas reglas, por ejemplo todas las clases de la jerarquía que se visita tienen que saber responder el mensaje `accept` con el visitor. Y la implementación de ese mensaje para cada clase de la jerarquía lo que tiene que hacer es enviar el mensaje `visit` con el nombre de la clase al visitor. Ejemplo de código:       
```smalltalk
Deposit >> accept: aVisitor
    aVisitor visitDeposit: self
```

### Metaprogramación estructural-escritura:
Es cuando podemos **modificar la estructura** de la jerarquía de clases, de una clase o de un método, etc. 

Por ejemplo, crear una clase. Es metaprogramación estructural-escritura ya que modificó la jerarquía de clases de Object.

A las clases también les podemos mandar el mensaje `addInstVarName` para agregar una variable de instancia. También podemos borrar una haciendo `removeInstVarName`. También podemos agregar (`compile:`) o borrar métodos (`removeSelector:`).

En definitiva es, poder modificar los colaboradores de cualquier objeto, sean clases, objetos comunes y corrientes, etc. 

**Hacer metaprogramación estructural-escritura no está permitido en la mayoría de los lenguajes de objetos.** Por ejemplo, en un lenguaje estáticamente tipado nunca voy a poder hacer metaprogramación estructural-escritura a nivel clase, porque si mientras el sistema está ejecutando podemos cambiarlo, tendríamos que asegurarnos de que no se rompa el sistema de tipos. 

> *¿Para qué se puede utilizar la programación estructural a nivel escritura? Algunos usos:*     
> - Para implementar refactorings. *Ejemplo: renombrar una variable de instancia.*    
> - Generar herramientas para poder modificar el sistema que estamos desarrollando

### Metaprogramación comportamiento-lectura:
Analizar el stack de ejecución, y ver quién envió qué mensaje, etc. 

Ejemplo:
```smalltalk
test 01
    self m1

m1 
    ^thisContext sender selector
```

# Resumen metaprogramación y conceptos clave
- `Metaprogramación`: es la **capacidad de un sistema de tratar programas como datos**, y por lo tanto analizarlos, modificarlos o generarlos.      
1. *Reificación*: el sistema expone sus partes internas (clases, métodos, contexto de ejecución, AST, etc) como objetos manipulables.     
2. *Reflexión*: la capacidad de un sistema de razonarse y modificarse a sí mismo.

- `Metasistema`: un sistema cuyo **dominio de problema es otro sistema**.

- `Sistema reflexivo`: un sistema **causalmente conectado consigo mismo**. Es decir, puede observar su estructura/comportamiento y cambiar sus propias reglas de ejecución. 

Escribo solo las de comportamiento porque son las que quedaron más flojas:   
- `Metaprogramación comportamiento-lectura`: Es introspección del estado dinámico del programa. Por ejemplo, `thisContext` es el objeto en Smalltalk que representa el contexto de ejecución actual. Podemos inspeccionar el stack, debuggear el flujo, ver el contexto de envío de mensajes, etc. 

- `Metaprogramación comportamiento-escritura`: Es la capacidad de modificar el comportamiento en tiempo de ejecución. Ejemplo: crear métodos, pluggable proxies, modificar el debugger. 