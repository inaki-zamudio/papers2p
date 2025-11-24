# Resumen del Patrón Strategy

## Intención
Definir una familia de algoritmos, encapsular cada uno de ellos y hacerlos intercambiables. Strategy permite que el algoritmo varíe independientemente de los clientes que lo utilizan.

## También Conocido Como
Policy (Política)

## Motivación
Evitar hard-codear algoritmos en clases que los requieren porque:
- Los clientes se vuelven más complejos y difíciles de mantener.
- Diferentes algoritmos son apropiados en diferentes momentos.
- Es difícil añadir nuevos algoritmos o variar los existentes si están integrados en el cliente.

**Solución**: Definir clases que encapsulen los diferentes algoritmos (estrategias). Ejemplo: diferentes estrategias para dividir texto en líneas (SimpleCompositor, TeXCompositor, ArrayCompositor).

## Aplicabilidad
Usar el patrón Strategy cuando:
- Muchas clases relacionadas difieren sólo en su comportamiento.
- Se necesitan diferentes variantes de un algoritmo.
- Un algoritmo usa datos que los clientes no deben conocer.
- Una clase define muchos comportamientos que aparecen como múltiples sentencias condicionales.

## Estructura
```
Context → Strategy
↓
ConcreteStrategyA, ConcreteStrategyB, etc.
```


- **Context**: Configurado con un objeto ConcreteStrategy.
- **Strategy**: Interfaz común para todos los algoritmos.
- **ConcreteStrategy**: Implementa el algoritmo usando la interfaz Strategy.

## Participantes
- **Strategy** (Compositor): Declara una interfaz común para todos los algoritmos.
- **ConcreteStrategy** (SimpleCompositor, TeXCompositor, ArrayCompositor): Implementa el algoritmo.
- **Context** (Composition): Mantiene referencia a Strategy, puede permitir acceso a datos.

## Colaboraciones
- Strategy y Context colaboran para implementar el algoritmo.
- Context pasa datos a Strategy o se pasa a sí mismo.
- Los clientes crean y configuran la ConcreteStrategy.

## Consecuencias

### Beneficios:
1. **Familias de algoritmos relacionados**: Jerarquías reutilizables.
2. **Alternativa a la herencia**: Evita hardcodear comportamientos en Context.
3. **Elimina sentencias condicionales**: Evita switch/large if-else.
4. **Elección de implementaciones**: Diferentes trade-offs (tiempo/espacio).

### Desventajas:
5. **Clientes deben conocer las estrategias**: Deben entender diferencias.
6. **Comunicación Strategy-Context**: Puede haber parámetros no usados.
7. **Aumento de objetos**: Más objetos en la aplicación (compartibles si son stateless).

## Implementación
1. **Interfaces Strategy y Context**: 
   - Pasar datos por parámetros (menos acoplamiento).
   - Context pasarse a sí mismo (Strategy solicita lo que necesita).

2. **Strategies como parámetros de plantilla** (C++):
   - Útil si la estrategia se selecciona en tiempo de compilación.
   - No necesita interfaz abstracta.

3. **Strategies opcionales**: Context puede tener comportamiento por defecto.

## Usos Conocidos
- **ET++ e InterViews**: Algoritmos de line-breaking.
- **RTL System**: Optimización de compiladores (asignación de registros).
- **ET++ SwapsManager**: Cálculo de precios de instrumentos financieros.
- **Booch components**: Estrategias de asignación de memoria (template).
- **RApp**: Layout de circuitos integrados (ruteo).
- **ObjectWindows (Borland)**: Validación de datos en diálogos.

## Patrones Relacionados
- **Flyweight**: Los objetos Strategy a menudo son buenos flyweights.

---

# Preguntas y Respuestas Clave

### ¿Cuál es el propósito principal del patrón Strategy?
Permitir que los algoritmos varíen independientemente de los clientes que los usan, encapsulándolos en clases separadas e intercambiables.

### ¿Qué problema resuelve Strategy?
Evita el acoplamiento fuerte entre un cliente y un algoritmo específico, facilitando la modificación, extensión e intercambio de algoritmos sin cambiar el código del cliente.

### ¿Cuándo debería usarse el patrón Strategy?
Cuando se tienen múltiples variantes de un algoritmo, cuando una clase tiene muchos comportamientos condicionales, o cuando se necesita evitar exponer estructuras de datos complejas del algoritmo al cliente.

### ¿Qué ventaja tiene Strategy sobre usar herencia directa?
Strategy permite cambiar el algoritmo dinámicamente en tiempo de ejecución, mientras que la herencia lo fija en la jerarquía de clases. Además, evita clases infladas con múltiples comportamientos.

### ¿Qué desventaja puede tener Strategy?
Puede aumentar el número de objetos y requiere que el cliente conozca las diferencias entre las estrategias para seleccionar la apropiada.

### ¿Cómo se relacionan Context y Strategy?
Context delega las solicitudes a su objeto Strategy. Context puede pasar los datos necesarios al Strategy o pasar una referencia a sí mismo.

### ¿Se puede usar Strategy si el algoritmo no cambia nunca?
Si el algoritmo es fijo y no hay necesidad de variarlo, probablemente no sea necesario usar Strategy, ya que añadiría complejidad innecesaria.

### ¿Puede un Strategy no usar toda la información que recibe del Context?
Sí, es común que algunas ConcreteStrategies ignoren parte de la información proporcionada, lo que puede generar cierta sobrecarga de comunicación.

### ¿Cómo se pueden reducir los objetos creados por Strategy?
Implementando las estrategias como objetos sin estado (stateless) que pueden ser compartidos entre múltiples Contextos (patrón Flyweight).

### ¿Es posible tener un comportamiento por defecto en Context?
Sí, Context puede verificar si tiene una Strategy configurada. Si no la tiene, ejecuta un comportamiento por defecto.
