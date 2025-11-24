# Resumen del Patrón Proxy

## Intención
Proporcionar un sustituto o marcador de posición para otro objeto para controlar el acceso al mismo.

## También Conocido Como
Surrogate (Sustituto)

## Motivación
Controlar el acceso a un objeto puede ser necesario para diferir el coste completo de su creación e inicialización hasta que realmente se necesite usarlo. Por ejemplo, en un editor de documentos que puede incrustar objetos gráficos, algunos como imágenes raster grandes pueden ser costosos de crear. 

La solución es usar un **proxy de imagen** que actúe como representante de la imagen real. El proxy se comporta como la imagen y se encarga de instanciarla cuando es requerida.

## Aplicabilidad
El patrón Proxy es aplicable cuando se necesita una referencia más versátil o sofisticada a un objeto que un simple puntero:

1. **Remote Proxy**: Proporciona un representante local para un objeto en un espacio de direcciones diferente.
2. **Virtual Proxy**: Crea objetos costosos bajo demanda.
3. **Protection Proxy**: Controla el acceso al objeto original, útil cuando los objetos deben tener diferentes derechos de acceso.
4. **Smart Reference**: Reemplaza un puntero simple realizando acciones adicionales cuando se accede al objeto.

## Estructura

```
Client → Subject → Proxy → RealSubject
```

## Participantes
- **Proxy** (ImageProxy):
  - Mantiene referencia que permite acceder al sujeto real
  - Proporciona interfaz idéntica a Subject
  - Controla el acceso al sujeto real

- **Subject** (Graphic):
  - Define la interfaz común para RealSubject y Proxy

- **RealSubject** (Image):
  - Define el objeto real que el proxy representa

## Colaboraciones
- Proxy reenvía solicitudes a RealSubject cuando es apropiado, dependiendo del tipo de proxy.

## Consecuencias
1. **Nivel de indirección** al acceder a un objeto
2. **Remote Proxy** oculta que un objeto reside en diferente espacio de direcciones
3. **Virtual Proxy** permite optimizaciones como creación bajo demanda
4. **Protection proxies y smart references** permiten tareas adicionales

## Implementación
- **C++**: Sobrecarga del operador de acceso a miembros (`operator->`)
- **Smalltalk**: Uso del método `doesNotUnderstand:` para reenvío automático
- El proxy no siempre necesita conocer el tipo exacto del sujeto real

## Usos Conocidos
- **ET++**: Clases de bloques de construcción de texto
- **NEXTSTEP**: Clase NXProxy para objetos distribuidos
- **Smalltalk**: Acceso a objetos remotos

## Patrones Relacionados
- **Adapter**: Proporciona una interfaz diferente vs Proxy (misma interfaz)
- **Decorator**: Añade responsabilidades vs Proxy controla acceso

---

## Preguntas y Respuestas Clave

### ¿Cuál es el propósito principal del patrón Proxy?
**R:** Controlar el acceso a un objeto proporcionando un sustituto o marcador de posición que puede añadir funcionalidad adicional como creación bajo demanda, control de acceso, o comunicación remota.

### ¿Qué tipos de proxy existen y para qué sirven?
**R:**
- **Remote Proxy**: Para objetos en diferentes espacios de direcciones
- **Virtual Proxy**: Para creación de objetos costosos bajo demanda  
- **Protection Proxy**: Para control de acceso al objeto original
- **Smart Reference**: Para acciones adicionales al acceder al objeto

### ¿En qué se diferencia un Proxy de un Adapter?
**R:** Un adapter proporciona una interfaz diferente al objeto que adapta, mientras que un proxy proporciona la misma interfaz que su sujeto.

### ¿En qué se diferencia un Proxy de un Decorator?
**R:** Un decorador añade responsabilidades a un objeto, mientras que un proxy controla el acceso a un objeto.

### ¿Qué ventaja proporciona el "copy-on-write"?
**R:** Permite posponer la copia de objetos pesados hasta que realmente se modifiquen, evitando el coste de copia innecesario cuando el objeto solo se lee.

### ¿Qué problema resuelve el Virtual Proxy en el ejemplo del editor de documentos?
**R:** Evita la creación inmediata de imágenes costosas al abrir el documento, creándolas solo cuando se vuelven visibles, mejorando así el tiempo de apertura del documento.