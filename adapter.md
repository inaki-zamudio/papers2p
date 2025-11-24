# Resumen: Patrón Adapter

## Intención
Convertir la interfaz de una clase en otra interfaz que los clientes esperan. El Adapter permite que clases con interfaces incompatibles trabajen juntas.

## También Conocido Como
Wrapper (Envoltorio)

## Motivación
Se necesita reutilizar una clase existente, pero su interfaz no coincide con la requerida por el dominio de la aplicación.

**Ejemplo:** Un editor de dibujos que define una abstracción `Shape` (Forma). Se desea reutilizar una clase `TextView` de un toolkit de interfaz de usuario para implementar un `TextShape`, pero sus interfaces son incompatibles.

**Solución:** Crear un adaptador (`TextShape`) que adapte la interfaz de `TextView` a la interfaz `Shape`, permitiendo su uso en el editor.

## Aplicabilidad
Usar el patrón Adapter cuando:

- Se desea usar una clase existente, y su interfaz no coincide con la necesaria.
- Se quiere crear una clase reusable que coopere con clases no relacionadas o imprevistas.
- (Solo adaptador de objetos) Se necesita adaptar varias subclases existentes, y es impracticable adaptar cada una mediante subclasificación.

## Estructura

### Adaptador de Clase (Herencia Múltiple)
- El `Adapter` hereda tanto de `Target` (interfaz pública) como de `Adaptee` (implementación privada).
- **C++:** Herencia pública de `Target`, privada de `Adaptee`.

**Estructura:**
```text
Client -> Target::Request()
              ↑
        Adapter::Request() -> Adaptee::SpecificRequest()
```


### Adaptador de Objetos (Composición)
- El `Adapter` contiene una instancia de `Adaptee` y delega las solicitudes.

**Estructura:**
```text
Client -> Target::Request()
              ↑
        Adapter::Request() -> adaptee->SpecificRequest()
```

## Participantes

- **Target (`Shape`):** Define la interfaz específica del dominio que usa el Client.
- **Client (`DrawingEditor`):** Colabora con objetos que se ajustan a la interfaz Target.
- **Adaptee (`TextView`):** Define una interfaz existente que necesita ser adaptada.
- **Adapter (`TextShape`):** Adapta la interfaz de Adaptee a la interfaz Target.

## Colaboraciones
- El Client llama a operaciones en una instancia de Adapter.
- El Adapter traduce esas llamadas a una o más operaciones del Adaptee.

## Consecuencias

### Adaptador de Clase
- **Ventajas:**
  - Se compromete a una clase Adaptee concreta.
  - Permite sobrescribir comportamiento de Adaptee (es una subclase).
  - Introduce un solo objeto, sin indirección adicional.
- **Desventajas:**
  - No funciona para adaptar una clase y todas sus subclases.

### Adaptador de Objetos
- **Ventajas:**
  - Un solo adaptador puede trabajar con múltiples Adaptees (y sus subclases).
  - Puede añadir funcionalidad a todos los Adaptees a la vez.
- **Desventajas:**
  - Es más difícil sobrescribir comportamiento de Adaptee (requiere subclasificar Adaptee).

### Consideraciones Adicionales

1. **Grado de Adaptación:** Puede variar desde un simple cambio de nombres hasta soportar un conjunto de operaciones completamente diferente.
2. **Adaptadores Enchufables (Pluggable Adapters):** Permiten que una clase sea más reusable incorporando la adaptación de interfaz directamente.
   - **Ejemplo:** Un widget `TreeDisplay` que debe poder mostrar diferentes estructuras jerárquicas (ej: directorios, herencia de clases) con interfaces distintas.
3. **Adaptadores Bidireccionales (Two-way Adapters):** Proporcionan transparencia permitiendo que un objeto sea visto con diferentes interfaces por distintos clientes.

## Implementación

### Enfoques para Adaptadores Enchufables:

1. **Usando Operaciones Abstractas:**
   - La clase (ej: `TreeDisplay`) define operaciones abstractas para la interfaz reducida.
   - Las subclases implementan estas operaciones para adaptar el objeto estructurado.

2. **Usando Objetos Delegados:**
   - La clase (ej: `TreeDisplay`) delega las solicitudes de acceso a un objeto delegado.
   - Permite cambiar la estrategia de adaptación sustituyendo el delegado.
   - En lenguajes estáticos (C++), se define una interfaz abstracta para el delegado.

3. **Adaptadores Parametrizados (Smalltalk):**
   - Se parametriza el adaptador con bloques (ej: para obtener hijos, crear nodos gráficos).
   - Soporta adaptación sin necesidad de subclasificación.

## Usos Conocidos

- **ET++Draw:** Usa `TextShape` para adaptar clases de edición de texto de ET++.
- **InterViews 2.6:** Usa `GraphicBlock` (adaptador de objetos) para adaptar la interfaz `Graphic` a `Interactor`.
- **ObjectWorks/Smalltalk:** Incluye `PluggableAdaptor` para adaptar objetos a la interfaz `ValueModel`, y `TableAdaptor` para adaptar secuencias de objetos a una presentación tabular.
- **NeXT's AppKit:** Usa objetos delegados para adaptación de interfaz (ej: `NXBrowser`).
- **"Marriage of Convenience" (Meyer):** Es una forma de adaptador de clase donde `FixedStack` adapta `Array` a la interfaz `Stack`.

## Patrones Relacionados

- **Bridge (151):** Estructura similar al adaptador de objetos, pero su intención es separar interfaz de implementación para variarlas independientemente. El Adapter cambia la interfaz de un objeto existente.
- **Decorator (175):** Mejora un objeto sin cambiar su interfaz, es más transparente y permite composición recursiva.
- **Proxy (207):** Actúa como representante de otro objeto sin cambiar su interfaz.

# Preguntas y Respuestas Clave: Patrón Adapter

## 1. ¿Cuál es la intención principal del patrón Adapter?
**R:** Convertir la interfaz de una clase en otra interfaz que los clientes esperan, permitiendo que clases con interfaces incompatibles trabajen juntas.

## 2. ¿Qué problema fundamental resuelve el Adapter?
**R:** El problema de reutilizar clases existentes cuya interfaz no coincide con la requerida por el dominio de aplicación, sin modificar el código original de dichas clases.

## 3. ¿Cuáles son los dos tipos principales de adaptadores y en qué se diferencian?
**R:**
- **Adaptador de Clase:** Usa herencia múltiple. Hereda tanto de Target como de Adaptee.
- **Adaptador de Objetos:** Usa composición. Contiene una instancia de Adaptee y delega solicitudes.

**Diferencia clave:** El adaptador de clase se compromete con una clase concreta, mientras que el de objetos puede trabajar con múltiples Adaptees y sus subclases.

## 4. ¿En qué escenario sería preferible usar un adaptador de objetos sobre uno de clase?
**R:** Cuando se necesita adaptar una clase y todas sus subclases, o cuando se quiere añadir funcionalidad a múltiples Adaptees a la vez.

## 5. ¿Qué ventaja tiene el adaptador de clase respecto al de objetos?
**R:** Permite sobrescribir más fácilmente el comportamiento del Adaptee, ya que es una subclase directa. Además, no introduce indirección adicional de punteros.

## 6. ¿Qué son los "adaptadores enchufables" (Pluggable Adapters)?
**R:** Son adaptadores con adaptación de interfaz incorporada, que permiten reutilizar una clase en sistemas que esperan diferentes interfaces, minimizando las suposiciones que otras clases deben hacer.

## 7. Nombra tres formas de implementar adaptadores enchufables
**R:**
1. **Usando operaciones abstractas:** Subclases implementan la interfaz reducida.
2. **Usando objetos delegados:** Se delega la adaptación a otro objeto.
3. **Usando adaptadores parametrizados:** Se parametriza con bloques (común en Smalltalk).

## 8. ¿Qué problema resuelven los adaptadores bidireccionales (Two-way Adapters)?
**R:** La falta de transparencia en adaptadores normales, permitiendo que un objeto sea visto con diferentes interfaces por distintos clientes.

## 9. ¿Cómo se implementa un adaptador de clase en C++?
**R:** Heredando públicamente de Target (para la interfaz) y privadamente de Adaptee (para la implementación).

## 10. ¿Cuál es la diferencia clave entre Adapter y Bridge?
**R:** Adapter cambia la interfaz de un objeto existente, mientras que Bridge separa la interfaz de la implementación para permitir que ambas varíen independientemente.

## 11. ¿Por qué Decorator es más transparente que Adapter?
**R:** Porque Decorator no cambia la interfaz del objeto, solo la extiende, permitiendo composición recursiva.

## 12. ¿Qué representa cada participante en el patrón Adapter?
**R:**
- **Target:** Interfaz que el cliente espera
- **Adaptee:** Interfaz existente que necesita adaptación
- **Adapter:** Clase que adapta Adaptee a Target
- **Client:** Usa objetos a través de la interfaz Target

## 13. ¿Qué consideración importante debe tenerse sobre el grado de adaptación?
**R:** La cantidad de trabajo que hace el Adapter puede variar desde simple cambio de nombres de operaciones hasta soportar conjuntos de operaciones completamente diferentes, dependiendo de cuán similares sean las interfaces Target y Adaptee.

## 14. ¿En qué caso de uso real se menciona el uso de delegados para adaptación?
**R:** En NeXT's AppKit, la clase NXBrowser usa objetos delegados para acceder y adaptar datos jerárquicos.

## 15. ¿Qué ventaja ofrecen los adaptadores parametrizados en Smalltalk?
**R:** Permiten adaptación sin necesidad de subclasificación, usando bloques para cada solicitud individual, ofreciendo una alternativa conveniente a la herencia.