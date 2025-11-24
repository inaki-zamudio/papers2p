# Resumen: Patrón Composite

## Intención
Componer objetos en estructuras de árbol para representar jerarquías parte-todo. Composite permite a los clientes tratar objetos individuales y composiciones de objetos de manera uniforme.

## Motivación
En aplicaciones gráficas como editores de dibujo, los usuarios construyen diagramas complejos a partir de componentes simples. El problema surge cuando el código debe tratar de manera diferente a los objetos primitivos (líneas, texto) y a los contenedores, incluso cuando el usuario los trata igual. Composite resuelve esto mediante composición recursiva.

## Aplicabilidad
Usar el patrón Composite cuando:

- Se quiere representar jerarquías parte-todo de objetos
- Se desea que los clientes ignoren la diferencia entre composiciones de objetos y objetos individuales, tratando toda la estructura de manera uniforme

## Estructura

```text
Client
  |
  ↓
Component
  △
  | \
  |  \
  |   \
Leaf  Composite
       |
       | contiene
       ↓
    children → List<Component>
```
- Cliente usa la interfaz de Component
- Leaf y Composite son subclases de Component.
- composite tiene una lista de hijos que pueden ser de cualquier tipo de Component.

## Participantes

- **Component** (Graphic): 
  - Declara la interfaz para objetos en la composición
  - Implementa comportamiento por defecto para operaciones comunes
  - Declara interfaz para acceso y gestión de componentes hijos
  - Opcionalmente define interfaz para acceder al padre

- **Leaf** (Line, Rectangle, Text):
  - Representa objetos hoja sin hijos
  - Define comportamiento para objetos primitivos

- **Composite** (Picture):
  - Define comportamiento para componentes con hijos
  - Almacena componentes hijos
  - Implementa operaciones relacionadas con hijos

- **Client**: Manipula objetos a través de la interfaz Component

## Colaboraciones
- Los clientes usan la interfaz Component para interactuar con la estructura
- Si el receptor es un Leaf, maneja la solicitud directamente
- Si es un Composite, normalmente reenvía solicitudes a sus hijos

## Consecuencias Clave

### Ventajas:
- **Estructuras Jerárquicas:** Define jerarquías de objetos primitivos y compuestos recursivamente
- **Simplicidad del Cliente:** Los clientes tratan estructuras compuestas y objetos individuales uniformemente
- **Extensibilidad:** Es fácil añadir nuevos tipos de componentes (Leaf o Composite)

### Desventajas:
- **Diseño Demasiado General:** Dificulta restringir los componentes de un composite, requiriendo verificaciones en tiempo de ejecución

## Implementación - Consideraciones Críticas

### 1. Referencias Explícitas a Padres
Mantener referencias de hijos a padres simplifica el recorrido y gestión. Se define en Component y se mantiene el invariante: todos los hijos de un composite tienen como padre a ese composite.

### 2. Maximizar la Interfaz Component
Component debe definir tantas operaciones comunes como sea posible, pero esto puede conflictuar con el principio de que una clase solo debe definir operaciones significativas para sus subclases.

### 3. Operaciones de Gestión de Hijos - Transparencia vs Seguridad
**Dilema fundamental:**
- **Interfaz en Component:** Máxima transparencia pero clientes pueden hacer operaciones inválidas en Leaves
- **Interfaz solo en Composite:** Seguridad pero se pierde transparencia

**Solución común:** Priorizar transparencia con operaciones que fallen por defecto en Leaves

### 4. Almacenamiento y Orden de Hijos
- **Estructuras de datos:** Listas, árboles, arrays según eficiencia
- **Ordenamiento:** Crítico en interfaces gráficas (orden z) y árboles de parse
- **Iteradores:** Patrón Iterator para recorrido

### 5. Optimización con Caché
Composites pueden cachear información de recorrido/búsqueda. Los cambios invalidan caches de padres, requiriendo referencias padre.

## Usos Conocidos

- **Smalltalk Model/View/Controller:** View original era un Composite
- **InterViews, ET++:** Sistemas de interfaz gráfica con composites
- **RTL Smalltalk:** Árboles de parse como estructuras composite
- **Dominio Financiero:** Portafolios que agregan activos individuales
- **Patrón Command:** MacroCommand como Composite de comandos

## Patrones Relacionados

- **Chain of Responsibility (223):** Usa enlaces componente-padre
- **Decorator (175):** A menudo usado con Composite, comparten interfaz Component
- **Flyweight (195):** Permite compartir componentes pero sin referencias padre
- **Iterator (257):** Para recorrer composites
- **Visitor (331):** Localiza operaciones distribuidas en Composite y Leaf

---

# Preguntas y Respuestas Clave: Patrón Composite

## 1. ¿Qué problema fundamental resuelve Composite?
**R:** La necesidad de tratar objetos individuales y composiciones de objetos de manera uniforme, eliminando la distinción en el código cliente entre elementos primitivos y contenedores.

## 2. ¿Cuál es el principio clave que permite la transparencia en Composite?
**R:** La interfaz Component unificada que permite a los clientes tratar tanto Leaves como Composites de la misma manera, sin conocer su tipo específico.

## 3. ¿Qué diferencia fundamental hay entre Leaf y Composite?
**R:** Leaf representa objetos terminales sin hijos, mientras que Composite almacena y gestiona componentes hijos, delegando operaciones en ellos.

## 4. ¿Qué trade-off existe en el diseño de la interfaz de gestión de hijos?
**R:** Transparencia vs Seguridad. Poner la interfaz en Component da transparencia pero permite operaciones inválidas; ponerla solo en Composite da seguridad pero pierde transparencia.

## 5. ¿Por qué Composite puede hacer el diseño "demasiado general"?
**R:** Porque dificulta restringir los tipos de componentes que un composite puede contener, forzando a verificaciones en tiempo de ejecución en lugar de usar el sistema de tipos.

## 6. ¿Qué ventaja ofrecen las referencias a padres en la implementación?
**R:** Simplifican el recorrido hacia arriba en la estructura, la eliminación de componentes y el soporte para patrones como Chain of Responsibility.

## 7. ¿Cómo se manejan las operaciones de gestión de hijos en los Leaves?
**R:** Típicamente con implementaciones por defecto que fallan (excepciones) o no hacen nada, dependiendo del balance transparencia/seguridad elegido.

## 8. ¿Qué papel juega el patrón Iterator con Composite?
**R:** Proporciona mecanismos estandarizados para recorrer las estructuras jerárquicas de composites, especialmente cuando el orden de los hijos es importante.

## 9. ¿En qué escenarios es especialmente útil el caching en composites?
**R:** Cuando se necesita frecuente traversía o búsqueda en estructuras grandes, cacheando información como bounding boxes en interfaces gráficas.

## 10. ¿Cómo se relacionan Decorator y Composite?
**R:** Comparten often una interfaz Component común, pero Decorator añade responsabilidades mientras Composite construye estructuras jerárquicas.