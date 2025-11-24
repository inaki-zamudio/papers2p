# Resumen: Patrón Builder

## Intención
Separar la construcción de un objeto complejo de su representación, para que el mismo proceso de construcción pueda crear diferentes representaciones.

## Motivación
El problema surge cuando necesitamos crear objetos complejos que pueden tener múltiples representaciones. Por ejemplo, un lector de RTF que debe convertir documentos a diferentes formatos (ASCII, TeX, widgets editables). La solución es configurar el proceso de construcción con un objeto "builder" que se especialice en cada representación particular.

## Aplicabilidad
Usar el patrón Builder cuando:

- El algoritmo para crear un objeto complejo debe ser independiente de las partes que lo componen y cómo se ensamblan
- El proceso de construcción debe permitir diferentes representaciones del objeto construido

## Estructura
```text
Director → Builder → ConcreteBuilder → Product
```


## Participantes

- **Builder** (TextConverter): Interfaz abstracta para crear partes del Producto
- **ConcreteBuilder** (ASCIIConverter, TeXConverter): 
  - Construye y ensambla las partes del producto
  - Define y mantiene la representación que crea
  - Proporciona interfaz para recuperar el producto
- **Director** (RTFReader): Construye un objeto usando la interfaz Builder
- **Product** (ASCIIText, TeXText): Representa el objeto complejo en construcción

## Colaboraciones
1. El cliente crea el objeto Director y lo configura con el Builder deseado
2. El Director notifica al Builder cuando debe construir una parte del producto
3. El Builder maneja las solicitudes del Director y añade partes al producto
4. El cliente recupera el producto del Builder

## Consecuencias Clave

### 1. Variación de Representación Interna
El Builder oculta la representación y estructura interna del producto. Para cambiar la representación, solo hay que definir un nuevo Builder.

### 2. Aislamiento de Código
Encapsula cómo se construye y representa un objeto complejo. Los clientes no necesitan conocer las clases que definen la estructura interna del producto.

### 3. Control Granular del Proceso
Construye el producto paso a paso bajo el control del Director, a diferencia de otros patrones creacionales que construyen en un solo paso.

## Implementación

### Consideraciones Importantes:

**Interfaz de Construcción:** Debe ser lo suficientemente general para todos los builders. Normalmente se usa un modelo donde los resultados se van añadiendo al producto.

**Productos sin Clase Abstracta:** Los productos de diferentes builders pueden ser tan distintos que no justifican un padre común.

**Métodos Vacíos por Defecto:** En C++, los métodos de construcción se definen vacíos, no como virtual puros, permitiendo que las subclases sobrescriban solo lo necesario.

## Usos Conocidos

- **ET++:** Conversor RTF usando builders para diferentes formatos
- **Smalltalk-80:** 
  - Parser con ProgramNodeBuilder para árboles sintácticos
  - ClassBuilder para crear subclases
  - ByteCodeStream para métodos compilados
- **Adaptive Communications Environment:** Construcción de componentes de servicio de red

## Patrones Relacionados

**Abstract Factory (87):** Similar pero se enfoca en familias de productos. Builder construye paso a paso, Abstract Factory devuelve el producto inmediatamente.

**Composite (163):** Es lo que el Builder frecuentemente construye.

---

# Preguntas y Respuestas Clave: Patrón Builder

## 1. ¿Cuál es el problema fundamental que resuelve Builder?
**R:** La necesidad de crear objetos complejos que pueden tener múltiples representaciones, separando el proceso de construcción de la representación final.

## 2. ¿Qué ventaja principal ofrece sobre otros patrones creacionales?
**R:** Proporciona control granular sobre el proceso de construcción paso a paso, permitiendo diferentes representaciones del mismo proceso de construcción.

## 3. ¿Cuál es la diferencia clave entre Director y ConcreteBuilder?
**R:** El Director orquesta el proceso de construcción, mientras que el ConcreteBuilder implementa la construcción específica y mantiene la representación.

## 4. ¿Por qué los productos de diferentes builders no suelen tener una clase padre común?
**R:** Porque las representaciones pueden ser demasiado diferentes (ej: texto ASCII vs widget interactivo) y no comparten interfaz común.

## 5. ¿Qué significa que Builder "aísla código de construcción y representación"?
**R:** Que el cliente no necesita conocer los detalles internos del producto, solo interactúa con la interfaz del Builder, mejorando la modularidad.

## 6. ¿Cómo se implementa típicamente la clase Builder abstracta en C++?
**R:** Con métodos virtuales vacíos (no puros), permitiendo que las subclases implementen solo los métodos que necesitan.

## 7. ¿En qué se diferencia Builder de Abstract Factory?
**R:** Builder se enfoca en construcción paso a paso de un objeto complejo, mientras que Abstract Factory en familias de productos relacionados que se obtienen inmediatamente.

## 8. ¿Qué ejemplo clásico ilustra el patrón Builder?
**R:** El conversor de RTF que puede generar ASCII, TeX o widgets editables usando el mismo proceso de parsing con diferentes builders.

## 9. ¿Qué ventaja ofrece el control paso a paso en la construcción?
**R:** Permite mayor flexibilidad en la estructura interna del producto resultante y mejor control sobre el proceso de construcción.

## 10. ¿Qué patrón está frecuentemente relacionado con lo que construye Builder?
**R:** Composite, ya que Builder suele construir estructuras de objetos complejas jerárquicas.