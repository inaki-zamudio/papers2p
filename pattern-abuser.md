# Resumen: "Las reglas son para tontos, los patrones para tontos geniales"

## Información del Artículo
- **Título:** Rules Are for Fools, Patterns Are for Cool Fools
- **Autor:** Mahesh Dodani
- **Publicado en:** SIGS Publications, 1999

---

## Resumen Ejecutivo

El artículo narra la historia de "Pat", un programador que, tras descubrir los patrones de diseño del libro "Gang of Four" (GoF), se transforma en un "evangelista" de su uso, aplicándolos de manera indiscriminada y dogmática. Esta obsesión le reporta fama, reconocimiento y una exitosa carrera como consultor, pero finalmente lo lleva a una crisis de conciencia al darse cuenta de que su aplicación excesiva y mecánica de patrones ha complicado los sistemas en lugar de mejorarlos.

---

## Historia de Pat: El Ascenso y la Caída

### 1. El Programador Común
Pat era un programador práctico, sin formación formal en diseño, que trabajaba de manera aislada utilizando un enfoque de "modificar el código hasta que funcione".

### 2. La Llegada del "Salvador": JERR
Un diseñador llamado Jerry (JERR) introduce a Pat y su equipo en el mundo de los patrones de diseño, presentándolos como la solución definitiva para crear código reusable, mantenible y de alta calidad.

### 3. La Conversión y el Fanatismo
Pat memoriza todos los patrones del libro GoF y comienza a refactorizar todo su código para incorporarlos. Se convierte en un "evangelista", imponiendo el uso de patrones en revisiones de código y midiendo la calidad del software por la cantidad de patrones utilizados.

### 4. El Éxito y la Fama
Esta métrica superficial le reporta éxito profesional. Es promocionado a diseñador, se vuelve un consultor muy bien pagado y una celebridad en conferencias.

### 5. El "Patrón para Aplicar Patrones"
Pat desarrolla una metodología sistemática y forzada para aplicar la mayor cantidad posible de patrones del GoF a una sola jerarquía de clases, usando como ejemplo una clase `Account` (Cuenta) de un banco. Su método incluye:

- **State:** Para variables de instancia (ej. `balance`).
- **Strategy:** Para cada método (ej. `calcInterest`).
- **Chain of Responsibility:** Para agrupaciones de instancias.
- **Composite, Iterator, Visitor:** Para estructuras de árbol de objetos.
- **Observer:** Para notificar a clientes de cambios.
- **Proxy:** Para diferentes interfaces de servicio.
- **Façade:** Para simplificar subsistemas complejos.
- **Bridge:** Para separar interfaz e implementación en métodos complejos.
- **Adapter:** Para integrar código legacy.
- **Abstract Factory, Builder, Factory Method, Prototype:** Para la creación de objetos.

### 6. El Lado Oscuro: La Toma de Conciencia
A pesar del éxito aparente, Pat comienza a deprimirse. Se da cuenta de que:
- Los desarrolladores de sus clientes no pueden mantener o modificar los sistemas "patternizados" debido a su extrema complejidad.
- No tiene respuestas reales para cuestiones fundamentales: ¿Cuándo un patrón es realmente aplicable? ¿Cómo se mide la "mejoría" del software? ¿Realmente se logra la reusabilidad?
- Su "solución" ante los problemas siempre es la misma: aplicar más patrones, perpetuando un ciclo vicioso.

---

## Conclusión y Mensaje Principal

El artículo es una **sátira crítica** y una advertencia contra el **abuso de los patrones de diseño**. El mensaje central es:

> **Los patrones son herramientas, no un fin en sí mismos.** Su aplicación dogmática, sin una comprensión profunda del problema que se pretende resolver y sin considerar la simplicidad y la mantenibilidad, conduce a diseños sobre-ingenierizados, complejos e inmantenibles. La calidad del software no se mide por la cantidad de patrones utilizados, sino por su eficacia, claridad y adaptabilidad al cambio.

---

## Cita Clave

> "Me di cuenta de que tenía varios problemas importantes que no podía resolver con patrones. ¿Cómo sé cuándo un patrón es realmente aplicable y tendrá el impacto necesario en la aplicación?"

---

**Referencia:** Gamma, E., Helm, R., Johnson, R., & Vlissides, J. (1994). *Design Patterns: Elements of Reusable Object-Oriented Software*. Addison-Wesley.
