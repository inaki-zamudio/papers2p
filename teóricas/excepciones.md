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

