# Resumen del Patrón State

## 📘 Intención
Permitir que un objeto altere su comportamiento cuando su estado interno cambia. El objeto parecerá cambiar de clase.

## 🔁 También conocido como
Objects for States (Objetos para Estados)

## 🎯 Motivación
- Un objeto (ej. `TCPConnection`) puede estar en distintos estados (Established, Listening, Closed) y responder de manera diferente a las mismas solicitudes según su estado.
- En lugar de usar condicionales largos, se delega el comportamiento específico de cada estado a objetos especializados.

## ✅ Aplicabilidad
Usar el patrón State cuando:
- El comportamiento de un objeto depende de su estado y debe cambiar en tiempo de ejecución.
- Las operaciones contienen condicionales largos y multiparte que dependen del estado del objeto.

## 🏗️ Estructura

```
Context --(contiene)--> State (interfaz)
                         ^
                         |
         +---------------+---------------+
         |               |               |
ConcreteStateA   ConcreteStateB   ConcreteStateC
```

## 👥 Participantes

- **Context** (ej. `TCPConnection`):
  - Define la interfaz de interés para los clientes.
  - Mantiene una instancia de una subclase `ConcreteState` que define el estado actual.

- **State** (ej. `TCPState`):
  - Define una interfaz para encapsular el comportamiento asociado a un estado particular del Context.

- **ConcreteState** (ej. `TCPEstablished`, `TCPListen`, `TCPClosed`):
  - Cada subclase implementa el comportamiento asociado a un estado del Context.

## 🤝 Colaboraciones
- **Context** delega las solicitudes específicas del estado al objeto `ConcreteState` actual.
- El contexto puede pasarse a sí mismo como argumento al objeto State.
- Los clientes interactúan principalmente con el Context, no con los State.
- La transición entre estados puede ser decidida por el Context o por las subclases ConcreteState.

## 📌 Consecuencias

1. **Localiza el comportamiento específico del estado**: Todo el código relacionado con un estado está en un solo objeto.
2. **Hace explícitas las transiciones de estado**: Cambiar de estado implica cambiar el objeto State, lo que es más claro que asignar variables.
3. **Los objetos State pueden compartirse**: Si no tienen estado interno, pueden ser *flyweights*.

## ⚙️ Implementación

1. **¿Quién define las transiciones de estado?**
   - Puede ser el Context (si son fijas) o las subclases State (más flexible).

2. **Alternativa basada en tablas**:
   - Usa tablas para mapear entradas a transiciones de estado.
   - Ventaja: Regularidad.
   - Desventajas: Menos eficiente, menos explícito, difícil añadir acciones.

3. **Creación y destrucción de State objects**:
   - Crear bajo demanda: Si los estados cambian poco.
   - Crear previamente: Si los cambios son muy frecuentes.

4. **Herencia dinámica**:
   - Algunos lenguajes permiten cambiar la clase en tiempo de ejecución (ej. Self).

## 🛠️ Usos conocidos
- Editores de dibujo (HotDraw, Unidraw): La herramienta seleccionada (lápiz, selección, texto) cambia el comportamiento del editor.
- Protocolos de conexión TCP.

## 🔗 Patrones relacionados
- **Flyweight**: Los objetos State sin estado interno pueden compartirse.
- **Singleton**: Cada ConcreteState suele ser un Singleton.

---

## ❓ Preguntas y Respuestas Clave

### **1. ¿Cuál es el problema principal que resuelve el patrón State?**
Elimina condicionales largos que dependen del estado de un objeto, reemplazándolos por objetos que encapsulan el comportamiento para cada estado.

### **2. ¿Cómo se logra que el objeto Context cambie su comportamiento?**
Manteniendo una referencia a un objeto State que representa el estado actual. Todas las solicitudes se delegan a este objeto.

### **3. ¿Quién controla las transiciones entre estados?**
Puede ser el Context o los mismos objetos State. La segunda opción es más flexible y facilita añadir nuevos estados.

### **4. ¿Bajo qué condiciones pueden compartirse los objetos State?**
Cuando no tienen variables de instancia (es decir, no mantienen estado interno).

### **5. ¿Cuál es la diferencia entre el patrón State y una máquina de estados basada en tablas?**
- **State**: Modela el comportamiento específico de cada estado usando polimorfismo.
- **Tablas**: Se enfoca en definir transiciones entre estados de manera uniforme, pero es menos expresiva para acciones complejas.

### **6. ¿Qué ventaja tiene State sobre el uso de variables de estado y condicionales?**
- Mejor organización del código.
- Facilita añadir nuevos estados sin modificar código existente.
- Las transiciones de estado son más explícitas y seguras.

### **7. Menciona un uso concreto del patrón State en una aplicación real.**
En editores gráficos, donde la herramienta activa (dibujo, selección, texto) determina el comportamiento del editor ante eventos del mouse o teclado.