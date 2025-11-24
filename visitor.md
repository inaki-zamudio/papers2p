# Resumen del Patrón Visitor

## Intención
Representar una operación a realizar sobre los elementos de una estructura de objetos. Visitor permite definir nuevas operaciones sin cambiar las clases de los elementos sobre los que opera.

## Motivación
En compiladores que representan programas como árboles de sintaxis abstracta, se necesitan múltiples operaciones (verificación de tipos, generación de código, optimización, etc.) que tratan diferentes tipos de nodos de manera distinta. Distribuir estas operaciones en las clases de nodos lleva a un sistema difícil de entender, mantener y cambiar.

## Solución
- Se encapsulan operaciones relacionadas en objetos **Visitor** separados
- Los elementos de la estructura implementan una operación **Accept** que toma un visitor como argumento
- El visitor contiene métodos **Visit** para cada tipo concreto de elemento

## Estructura

```
Cliente → Visitor → Element
                    ↓
           ConcreteElementA, ConcreteElementB
```

**Participantes:**
- **Visitor**: Declara operación Visit para cada ConcreteElement
- **ConcreteVisitor**: Implementa las operaciones para cada elemento
- **Element**: Define operación Accept que toma un visitor
- **ConcreteElement**: Implementa Accept llamando al método correspondiente del visitor
- **ObjectStructure**: Puede enumerar sus elementos

## Aplicabilidad
Use el patrón Visitor cuando:
- Una estructura contiene muchos objetos con interfaces diferentes
- Existen muchas operaciones distintas no relacionadas que deben realizarse
- Las clases de la estructura raramente cambian, pero se agregan nuevas operaciones frecuentemente

## Consecuencias

### Ventajas:
1. **Agregar operaciones es fácil**: Nuevas operaciones = nuevos visitors
2. **Agrupa operaciones relacionadas**: Comportamiento relacionado se localiza en un visitor
3. **Puede acumular estado**: El visitor puede mantener estado durante el recorrido
4. **Funciona con jerarquías no relacionadas**: No requiere herencia común entre elementos

### Desventajas:
1. **Agregar elementos es difícil**: Nuevos elementos requieren cambios en todos los visitors
2. **Rompe encapsulación**: Requiere exponer la interfaz interna de los elementos
3. **Puede violar el principio de encapsulación**

## Implementación Clave
- **Doble despacho (Double Dispatch)**: La operación ejecutada depende del tipo del visitor y del elemento
- **Responsabilidad del recorrido**: Puede estar en la estructura, el visitor o un iterator

## Colaboraciones
1. Cliente crea ConcreteVisitor
2. Cliente recorre la estructura llamando Accept en cada elemento
3. Cada elemento llama al método Visit correspondiente del visitor

---

## Preguntas y Respuestas Clave

### ¿Qué problema resuelve el patrón Visitor?
**R:** Resuelve el problema de tener múltiples operaciones distribuidas en muchas clases diferentes, lo que hace el sistema difícil de mantener y extender.

### ¿Cuándo NO debería usarse Visitor?
**R:** Cuando las clases de elementos cambian frecuentemente, ya que cada nuevo elemento requiere cambios en todos los visitors existentes.

### ¿Qué es el "doble despacho"?
**R:** Es el mecanismo donde la operación ejecutada depende de dos factores: el tipo del visitor y el tipo del elemento que se está visitando.

### ¿Quién debe ser responsable del recorrido de la estructura?
**R:** Puede ser:
- La estructura de objetos (más común)
- El visitor mismo
- Un iterator separado

### ¿Cómo afecta Visitor a la encapsulación?
**R:** Generalmente la rompe, ya que requiere que los visitors accedan a la interfaz interna de los elementos, exponiendo detalles de implementación.

### ¿Qué ventaja tiene sobre simplemente agregar métodos a las clases?
**R:** Permite agregar funcionalidad sin modificar las clases existentes y agrupa operaciones relacionadas en un solo lugar.

### ¿Por qué es difícil agregar nuevos elementos con Visitor?
**R:** Porque cada nuevo ConcreteElement requiere agregar un nuevo método abstracto en la clase Visitor y implementaciones en todos los ConcreteVisitors existentes.

### ¿En qué se diferencia de otros patrones de comportamiento?
**R:** Visitor se enfoca en operaciones que dependen de tipos específicos, mientras que otros como Iterator se enfocan en recorrer estructuras sin depender de tipos concretos.