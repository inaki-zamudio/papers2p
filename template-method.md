# Resumen: Patrón Template Method

## Clasificación
**Tipo:** Patrón de Comportamiento  
**Categoría:** De Clase

## Intención
Definir el esqueleto de un algoritmo en una operación, postergando algunos pasos a las subclases. Template Method permite que las subclases redefinan ciertos pasos de un algoritmo sin cambiar su estructura.

## Motivación
Se utiliza cuando un framework define la estructura general de un algoritmo, pero permite que las subclases personalizen pasos específicos. Por ejemplo, en un framework de aplicaciones, la clase `Application` define cómo abrir un documento (`OpenDocument`), pero delega la creación del documento (`DoCreateDocument`) y la lectura (`DoRead`) a subclases específicas.

## Aplicabilidad
- Para implementar las partes invariantes de un algoritmo una vez y dejar que las subclases implementen el comportamiento variable.
- Para factorizar comportamientos comunes en una clase base y evitar duplicación de código.
- Para controlar las extensiones de las subclases mediante operaciones "hook".

## Estructura

### Participantes
- **AbstractClass** (ej: `Application`):
  - Define operaciones primitivas abstractas que las subclases concretas implementan.
  - Implementa el **método plantilla** que define el esqueleto del algoritmo.

- **ConcreteClass** (ej: `MyApplication`):
  - Implementa las operaciones primitivas para llevar a cabo pasos específicos del algoritmo.

### Colaboraciones
- `ConcreteClass` depende de `AbstractClass` para implementar los pasos invariantes del algoritmo.

## Consecuencias
- **Reutilización de código:** Es una técnica fundamental para evitar duplicación.
- **Principio de Hollywood:** "No nos llames, nosotros te llamamos". La clase base llama a las operaciones de las subclases.
- **Operaciones llamadas por el método plantilla:**
  - Operaciones concretas.
  - Operaciones de la clase abstracta.
  - Operaciones primitivas (abstractas).
  - Métodos de fábrica.
  - Operaciones hook (comportamiento por defecto, a menudo vacío).

## Implementación
1. **Control de acceso en C++:** Las operaciones primitivas pueden ser `protected` y el método plantilla puede ser no virtual.
2. **Minimizar operaciones primitivas:** Reducir la cantidad de métodos que las subclases deben overridear.
3. **Convenciones de nombres:** Usar prefijos como "Do-" para identificar operaciones a overridear.

## Usos Conocidos
Muy común en clases abstractas. Frameworks como MacApp y AppKit de NeXT lo utilizan.

## Patrones Relacionados
- **Factory Method:** Suele ser llamado por el Template Method.
- **Strategy:** Template Method usa herencia para variar partes de un algoritmo; Strategy usa delegación para variar el algoritmo completo.

---

## Preguntas y Respuestas Clave

### ❓ ¿Qué problema resuelve el Template Method?
✅ Permite definir la estructura de un algoritmo en una clase base, permitiendo que las subclases redefinan ciertos pasos sin modificar la estructura general.

### ❓ ¿Qué es una "operación hook"?
✅ Es una operación que tiene una implementación por defecto (a menudo vacía) en la clase base, que las subclases pueden extender si lo necesitan. No es obligatorio overridearla.

### ❓ ¿Qué es el "Principio de Hollywood"?
✅ Es un principio de diseño que se resume en: "No nos llames, nosotros te llamamos". Significa que la clase base (padre) controla el flujo y llama a las operaciones de las subclases, y no al revés.

### ❓ ¿Cuándo debería usarse este patrón?
✅ Cuando se tiene un algoritmo con pasos invariantes y pasos variables, y se quiere evitar la duplicación de código centralizando la estructura del algoritmo en una clase base.

### ❓ ¿Cómo se diferencia de Strategy?
✅ **Template Method** usa **herencia** para variar partes de un algoritmo.  
✅ **Strategy** usa **delegación** (composición) para variar el algoritmo completo.

### ❓ ¿Qué ventaja tiene declarar el método plantilla como no virtual?
✅ Evita que las subclases puedan overridear y modificar la estructura del algoritmo, garantizando que la secuencia de pasos definida en la clase base se mantenga.

### ❓ ¿Qué convención de nomenclatura se sugiere para operaciones a overridear?
✅ Se sugiere usar prefijos como "Do-" (ej: `DoCreateDocument`, `DoRead`).