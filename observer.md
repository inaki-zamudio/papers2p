# Resumen del Patrón Observer

## Intención
Definir una dependencia uno-a-muchos entre objetos, de modo que cuando un objeto cambie de estado, todos sus dependientes sean notificados y actualizados automáticamente.

## También Conocido Como
- Dependents
- Publish-Subscribe

## Motivación
Mantener la consistencia entre objetos relacionados sin acoplamiento fuerte. Ejemplo: una hoja de cálculo y un gráfico de barras que reflejan los mismos datos sin conocerse entre sí.

## Aplicabilidad
- Cuando una abstracción tiene dos aspectos, uno dependiente del otro.
- Cuando un cambio en un objeto requiere cambiar otros, sin saber cuántos.
- Cuando un objeto debe notificar a otros sin hacer suposiciones sobre ellos.

## Estructura
- **Subject**: Conoce a sus observadores; ofrece interfaz para adjuntar y desadjuntar.
- **Observer**: Interfaz de actualización para objetos que deben ser notificados.
- **ConcreteSubject**: Almacena el estado; notifica a los observadores cuando cambia.
- **ConcreteObserver**: Mantiene referencia al ConcreteSubject; actualiza su estado para mantener la consistencia.

## Colaboraciones
- El ConcreteSubject notifica a los observadores tras un cambio.
- Los observadores consultan al sujeto para actualizar su estado.

## Consecuencias
### Ventajas:
- Acoplamiento abstracto entre Subject y Observer.
- Comunicación broadcast.
- Reutilización independiente de sujetos y observadores.

### Desventajas:
- Actualizaciones inesperadas y en cascada.
- Puede ser difícil rastrear cambios sin protocolo adicional.

## Implementación
1. **Mapeo de sujetos a observadores**: Lista o tabla hash.
2. **Múltiples sujetos**: Extender `Update` para identificar el sujeto.
3. **¿Quién dispara la actualización?**: Opciones:
   - Subject después de cambiar estado.
   - Cliente responsable de llamar a `Notify`.
4. **Referencias colgantes**: Notificar antes de eliminar el sujeto.
5. **Consistencia del estado**: Asegurar que el estado sea consistente antes de notificar.
6. **Modelos push y pull**:
   - **Push**: Sujeto envía detalles del cambio.
   - **Pull**: Sujeto notifica mínimamente; observadores preguntan.
7. **Especificar eventos de interés**: Registrar observadores solo para ciertos eventos.
8. **ChangeManager**: Media entre sujetos y observadores para manejar actualizaciones complejas.
9. **Combinar Subject y Observer**: En lenguajes sin herencia múltiple.

## Usos Conocidos
- Smalltalk MVC (Model/View/Controller).
- InterViews, Andrew Toolkit, Unidraw.
- Bibliotecas de clases como ET++ y THINK.

## Patrones Relacionados
- **Mediator**: ChangeManager actúa como mediador.
- **Singleton**: ChangeManager puede ser único y global.

---

## Preguntas y Respuestas Clave

### 1. ¿Cuál es el propósito principal del patrón Observer?
Notificar automáticamente a múltiples objetos dependientes cuando cambia el estado de un sujeto, manteniendo un acoplamiento bajo.

### 2. ¿Qué ventaja ofrece el acoplamiento abstracto?
Permite que sujetos y observadores pertenezcan a diferentes capas de abstracción y sean reutilizables de forma independiente.

### 3. ¿Qué problema puede causar el modelo push?
Puede reducir la reutilización de los observadores si el sujeto hace suposiciones incorrectas sobre la información que necesitan.

### 4. ¿Cuándo es útil un ChangeManager?
Cuando las dependencias entre sujetos y observadores son complejas, especialmente si hay múltiples sujetos y se debe evitar notificaciones redundantes.

### 5. ¿Cómo se evitan las referencias colgantes?
El sujeto debe notificar a los observadores antes de ser eliminado, para que estos puedan liberar o resetear su referencia.

### 6. ¿Qué diferencia hay entre el modelo push y pull?
- **Push**: El sujeto envía toda la información del cambio.
- **Pull**: El sujeto solo notifica; los observadores deben consultar los detalles.

### 7. ¿Quién puede invocar el método `Notify`?
Puede ser el mismo sujeto tras modificar su estado, o un cliente externo después de realizar múltiples cambios.

### 8. ¿Cómo se manejan múltiples sujetos en un observador?
El método `Update` recibe como parámetro el sujeto que notificó, para que el observador pueda identificar cuál cambió.