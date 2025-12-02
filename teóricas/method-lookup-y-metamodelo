# Method lookup y Metamodelo

Metamodelo: modelo de objetos que me permite crear modelos.       
En el metamodelo de Smalltalk, tenemos las siguientes clases:
- Behaviour   
- Class Description    
- Class   
- Metaclass  

## Method lookup
Algoritmo que tiene la VM que busca un método a partir de un objeto receptor y un mensaje. Cada vez que se envía un mensaje a un objeto, la VM busca el método relacionado a ese mensaje para ese objeto.

En un ambiente de objetos de clasificación (cada objeto es instancia de una clase), cada vez que un objeto recibe un mensaje, la implementación de ese mensaje se va a buscar a la clase del objeto. 

Cuando no existe el método para un selector en la clase, debo ir a su superclase. Se empieza a buscar el método en la jerarquía de clases del objeto receptor del mensaje.

Cuando un objeto recibe un mensaje que no sabe responder, se devuelve el método relacionado al doesNotUnderstand. 

3 pasos importantes del method lookup: 
1. Cada vez que un objeto recibe un mensaje, busco la implementación del mensaje a la clase del objeto.
2. Si no encuentro el método para ese mensaje en esa clase, voy a la superclase. 
3. El doesNotUnderstand hace que un objeto pueda responder mensajes que no sabe responder.

## Metamodelo 
Tenemos un objeto, que es una fecha (`aDate`). Como es un objeto, y estamos en un nivel de clasificación, es instancia de una clase (`Date`).     
Supongamos que le mandamos el mensaje `dayNumber`. En el algoritmo de method lookup, primero busca el método en su clase. Si lo encuentra, **lo ejecuta en el contexto de `aDate`**.      
Si a el objeto le mando `printString`, el caso es diferente. `printString` está implementado en `Object`, y `Date` subclasifica `Object`. Por lo que lo encontrará ahí según el algoritmo.     

Como las clases son objetos, también le podemos mandar mensajes a las clases. Por ejemplo, mandarle `name` a `Date`. Como `Date` es un objeto, es instancia de una clase. Supongamos que es instancia de la clase `Class` (la que define el comportamiento de todas las clases). Entonces, buscará la implementación de `name` en `Class`. También le puedo mandar a `Object` el mensaje name, y `Object` es instancia de `Class`. 

Si hacemos que `Class` sea instancia de `Object`, esto significa que *toda clase se comporta como objeto*. 

Metaniveles:
- instancias: `aDate`
- clases: `Object`, `Date`
- metaclases/metamodelo: `Class`

Asimismo, `Class` debe ser instancia de alguna clase, debido a que es un objeto y así pasa con sus clases. Tenemos una regresión a infinito. Para evitar esto, usamos **circularidad**, haciendo que la clase `Class` sea clase de sí misma.  

Bootstrapping para evitar "el problema del huevo y la gallina". 

Este es el metamodelo de smalltalk 76.      
![metamodelo](/img/st76.png)     
- flechas punteadas: es instancia de
- flecha y triángulo: es subclase de

> **LIMITACIONES DE ESTE METAMODELO:**
El problema con este modelo empieza cuando se comienza a requerir **comportamiento especializado a las clases**. Es decir, que las clases puedan responder sus propios mensajes.      
En el ejemplo, esto sería, por ejemplo, esperar a que `Date` conteste `today`. Pero para eso, debería estar implementado `today` en la clase `Class` (debido a que `Date` es instancia de `Class`). Esto significaría que **todas las instancias de `Class` van a saber responder `today`**, por ejemplo, `Object`. Esto no tiene ningún sentido.     
> **Importante recordar que la implementación de un objeto siempre está en la clase del objeto, no en el objeto**.

La solución a esta limitación es la siguiente:

Para darle **comportamiento especializado a un objeto, voy a tener una clase para ese objeto**. Esto significaría, que la clase `Date` va a tener una clase: `Date class`. Es una metaclase. En `Date class` implementamos `today`.    
Asimismo, `Object` es instancia de `Object class`.    
Entonces, cada clase tiene su propia clase. 

En smalltalk, cada vez que creamos una clase, por detrás no sólo se crea la clase, si no que también su metaclase. Esta característica se denomina **metaclases implícitas**.

También, se **mantiene la relación de subclasificació entre las metaclases, es decir, siguen la misma relación de subclasificación que las clases**. 

Asimismo, tenemos una clase que representa a las clases, `Class`, que subclasifica a otra clase que se llama `Behaviour` que representa todo aquello que puede tener comportamiento.     
`Object class` subclasifica `Class` y  `Behaviour` subclasifica `Object`. **Toda clase se comporta como un objeto y, por otro lado, toda clase terminan comportándose como clase**. 

`Class` es instancia de `Class class`. *`Class` es una clase abstracta, no tiene instancias, solamente tiene subclases que son todas las metaclases*. También, `Behaviour` será instancia de `Behaviour class`. 

Las `... class` son instancias de la clase que representa las clases de las clases, la clase `Metaclass`.

`Metaclass` es instancia de `Metaclass class`. `Metaclass class` es instancia de `Metaclass`.

![metamodelo_2](/img/st80.png)

*CAPÍTULO 5 DEL BLUEBOOK aclara acerca de este tema.*

Relaciones interesantes:
- `Behaviour` subclasifica `Object`: todo aquello que representa comportamiento se comporta como un objeto.    
- `Object class` subclasifica `Class`: todas las clases se comportan como clases. 
- `Metaclass class` instancia de `Metaclass`: evita regresión a infinito.

