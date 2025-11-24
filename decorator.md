# Resumen: Patrón Decorator

## Intención
Adjuntar responsabilidades adicionales a un objeto de forma dinámica. Los decoradores proporcionan una alternativa flexible a la herencia para extender funcionalidad.

## También Conocido Como
Wrapper (Envoltorio)

## Motivación
A veces necesitamos añadir responsabilidades a objetos individuales, no a una clase completa. Por ejemplo, en un toolkit de interfaz gráfica, queremos añadir propiedades como bordes o comportamientos como scroll a cualquier componente.

**Problema con herencia:** Es inflexible - la elección del border se hace estáticamente y afecta a todas las subclases.

**Solución con Decorator:** Envolver el componente en otro objeto que añade la funcionalidad, manteniendo la misma interfaz para que sea transparente al cliente.

## Aplicabilidad
Usar el patrón Decorator cuando:

- Se necesite añadir responsabilidades a objetos individuales de forma dinámica y transparente
- Las responsabilidades puedan ser retiradas
- La extensión por subclasificación no sea práctica (explosión de subclases, clase oculta)

## Estructura
```text
    Component
      △
     / \
    /   \
   /     \
ConcreteComponent   Decorator → component : Component
                      △
                      |
           ConcreteDecoratorA
```
- Component → ConcreteComponent: Herencia (is-a)
- Component → Decorator: Herencia (is-a)
- Decorator → Component: Composición (has-a) - el decorador envuelve un componente
- Decorator → ConcreteDecorator: Herencia (is-a)

## Participantes

- **Component (VisualComponent):** Define la interfaz para objetos que pueden tener responsabilidades añadidas dinámicamente
- **ConcreteComponent (TextView):** Define un objeto al que se le pueden adjuntar responsabilidades adicionales
- **Decorator:** Mantiene referencia a un objeto Component y define interfaz que conforma con Component
- **ConcreteDecorator (BorderDecorator, ScrollDecorator):** Añade responsabilidades al componente

## Colaboraciones
- Decorator reenvía requests a su objeto Component
- Puede realizar operaciones adicionales antes y después del reenvío

## Consecuencias Clave

### Beneficios:
1. **Mayor flexibilidad que herencia estática:** Responsabilidades pueden añadirse/removerse en runtime
2. **Evita clases sobrecargadas:** Enfoque "paga por lo que usas" - no hay que soportar todas las features en una clase compleja

### Desventajas:
3. **No son idénticos:** Un decorador y su componente no son el mismo objeto - no confiar en identidad de objeto
4. **Muchos objetos pequeños:** Resulta en sistemas con muchos objetos que se ven igual, difíciles de depurar

## Implementación - Consideraciones Importantes

### 1. Conformidad de Interfaz
El decorador debe conformar exactamente la interfaz del componente que decora.

### 2. Clase Component Liviana
La clase Component común debe ser liviana - definir interfaz, no almacenar datos, para que los decoradores no sean pesados.

### 3. Decorator vs Strategy
- **Decorator:** Cambia la "piel" del objeto (comportamiento externo)
- **Strategy:** Cambia las "entrañas" (comportamiento interno)
- **Strategy es mejor** cuando Component es intrínsecamente pesado

### 4. Decoradores Transparentes
Los decoradores son transparentes al componente - el componente no sabe que está decorado.

## Usos Conocidos

- **Toolkits de UI:** InterViews, ET++, ObjectWorks/Smalltalk - para añadir bordes, scroll, etc.
- **Streams de E/S:** ET++ streaming classes - para compresión, conversión ASCII
- **Comportamiento de eventos:** MacApp y Bedrock usan "behavior objects" para manejo de eventos

## Patrones Relacionados

- **Adapter (139):** Decorator cambia responsabilidades, Adapter cambia la interfaz completa
- **Composite (163):** Decorator puede verse como un composite degenerado con un componente
- **Strategy (315):** Decorator cambia la piel, Strategy cambia las entrañas

---

# Preguntas y Respuestas Clave: Patrón Decorator

## 1. ¿Qué problema fundamental resuelve Decorator?
**R:** La necesidad de añadir funcionalidad a objetos individuales dinámicamente sin usar herencia, que es estática y puede causar explosión de subclases.

## 2. ¿Cómo logra Decorator la transparencia para el cliente?
**R:** Los decoradores implementan exactamente la misma interfaz que el componente original, por lo que el cliente no puede distinguir entre un componente decorado y uno sin decorar.

## 3. ¿Qué ventaja tiene sobre la herencia múltiple?
**R:** Permite añadir responsabilidades en tiempo de ejecución, mezclar y combinar decoradores, y añadir la misma propiedad múltiples veces (ej: doble borde).

## 4. ¿Cuál es la diferencia clave entre Decorator y Adapter?
**R:** Decorator no cambia la interfaz del objeto, solo añade responsabilidades. Adapter da al objeto una interfaz completamente nueva.

## 5. ¿Por qué se dice que "un decorador y su componente no son idénticos"?
**R:** Desde la perspectiva de identidad de objeto, el decorador envuelve al componente pero no es el mismo objeto, por lo que no se debe confiar en comparaciones de identidad.

## 6. ¿Qué significa "evita clases sobrecargadas en la jerarquía"?
**R:** En lugar de crear una clase compleja que soporte todas las features posibles, se crea una clase simple y se añade funcionalidad incrementalmente con decoradores.

## 7. ¿Cuándo es preferible Strategy sobre Decorator?
**R:** Cuando el componente es pesado y Decorator sería muy costoso, o cuando se necesita cambiar el comportamiento interno (entrañas) en lugar del externo (piel).

## 8. ¿Por qué la clase Component debe ser liviana?
**R:** Para que los decoradores no sean demasiado pesados cuando se usan en cantidad, ya que cada decorador hereda de Component.

## 9. ¿Cómo se maneja la adición de múltiples responsabilidades?
**R:** Anidando decoradores recursivamente - cada decorador envuelve a otro decorador o al componente base.

## 10. ¿Qué patrón está más relacionado con la agregación de objetos?
**R:** Composite, mientras que Decorator se enfoca en añadir responsabilidades a objetos individuales.