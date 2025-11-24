# Resumen: Patrón Facade

## Intención
Proporcionar una interfaz unificada a un conjunto de interfaces en un subsistema. Facade define una interfaz de nivel superior que hace que el subsistema sea más fácil de usar.

## Motivación
Estructurar un sistema en subsistemas ayuda a reducir la complejidad. Un objetivo común es minimizar la comunicación y dependencias entre subsistemas. Facade logra esto introduciendo un objeto que proporciona una interfaz única y simplificada a las facilidades más generales de un subsistema.

**Ejemplo:** Un compilador con clases como Scanner, Parser, ProgramNode. La mayoría de clientes solo quieren compilar código sin conocer los detalles. La clase Compiler actúa como facade, ofreciendo una interfaz simple unificada.

## Aplicabilidad
Usar el patrón Facade cuando:

- Se quiere proporcionar una interfaz simple a un subsistema complejo
- Existen muchas dependencias entre clientes y las clases de implementación
- Se quiere estratificar subsistemas, definiendo puntos de entrada claros

## Estructura

```text
    Client
      ↓ (usa)
    Facade
      ↓ (coordina)
Subsystem
  ├── ClassA
  ├── ClassB
  └── ClassC
```
- Client → Facade: Dependencia/uso directo
- Facade → SubsystemClass: Composición/delegación
- SubsystemClass → (ninguna relación con Facade): Las clases del subsistema no conocen al facade

Flujo de comunicación:
```text
Client → Facade.Operation()
           ↓
Facade → SubsystemClassA.OperationA1()
Facade → SubsystemClassB.OperationB1()
Facade → SubsystemClassC.OperationC1()
           ↓
Client ← Resultado unificado
```

## Participantes

- **Facade (Compiler):**
  - Conoce qué clases del subsistema son responsables de cada solicitud
  - Delega solicitudes de clientes a objetos apropiados del subsistema

- **Clases del Subsistema (Scanner, Parser, ProgramNode):**
  - Implementan la funcionalidad del subsistema
  - Manejan el trabajo asignado por el Facade
  - No tienen conocimiento del facade (no mantienen referencias a él)

## Colaboraciones
- Los clientes se comunican con el subsistema enviando solicitudes al Facade
- El Facade reenvía las solicitudes a los objetos apropiados del subsistema
- Los clientes que usan el facade no necesitan acceder directamente a los objetos del subsistema

## Consecuencias Clave

### Beneficios:
1. **Protege a los clientes de los componentes del subsistema:** Reduce el número de objetos que los clientes manejan directamente
2. **Promueve acoplamiento débil:** Permite variar componentes del subsistema sin afectar clientes
3. **No previene el uso directo:** Las aplicaciones aún pueden usar clases del subsistema si lo necesitan

### Consideraciones:
- **Dependencias de compilación:** Vital en sistemas grandes, reduce recompilaciones
- **Portabilidad:** Facilita portar sistemas a otras plataformas

## Implementación - Consideraciones Importantes

### 1. Reducir Acoplamiento Cliente-Subsistema
- **Facade abstracto:** Clase abstracta con subclases concretas para diferentes implementaciones
- **Configuración:** Facade configurado con diferentes objetos del subsistema

### 2. Interfaces Públicas vs Privadas
- **Interfaz pública:** Clases accesibles por todos los clientes (incluyendo Facade)
- **Interfaz privada:** Solo para extensores del subsistema
- **Namespaces** (C++) permiten exponer solo clases públicas del subsistema

## Usos Conocidos

- **ObjectWorks/Smalltalk:** Sistema compilador inspirado en el ejemplo
- **ET++:** Clase ProgrammingEnvironment como facade para herramientas de browsing
- **Choices OS:** FileSystemInterface (almacenamiento) y Domain (espacios de direcciones) como facades

## Patrones Relacionados

- **Abstract Factory (87):** Puede usarse con Facade para crear objetos del subsistema de manera independiente
- **Mediator (273):** Similar pero abstrae comunicación entre objetos colegas, mientras Facade solo simplifica la interfaz
- **Singleton (127):** Usualmente solo se requiere un objeto Facade

---

# Preguntas y Respuestas Clave: Patrón Facade

## 1. ¿Qué problema fundamental resuelve Facade?
**R:** La complejidad de usar subsistemas con múltiples interfaces, proporcionando una interfaz unificada y simplificada.

## 2. ¿Cómo promueve Facade el bajo acoplamiento?
**R:** Aislando a los clientes de las clases internas del subsistema, permitiendo cambios en el subsistema sin afectar a los clientes que solo usan el facade.

## 3. ¿Facade previene el acceso directo a las clases del subsistema?
**R:** No, permite tanto el uso simplificado a través del facade como el acceso directo para clientes que necesitan más control.

## 4. ¿Cuál es la diferencia clave entre Facade y Mediator?
**R:** Facade simplifica el acceso a un subsistema, mientras que Mediator centraliza la comunicación compleja entre objetos relacionados.

## 5. ¿Por qué las clases del subsistema no conocen al Facade?
**R:** Para mantener la separación de responsabilidades - el subsistema debe poder funcionar independientemente del facade.

## 6. ¿Qué ventaja ofrece en sistemas grandes?
**R:** Reduce dependencias de compilación, limitando la recompilación necesaria cuando cambian clases del subsistema.

## 7. ¿Cómo se puede hacer un Facade más flexible?
**R:** Usando un Facade abstracto con múltiples implementaciones, o configurándolo con diferentes objetos del subsistema.

## 8. ¿Facade añade nueva funcionalidad?
**R:** Normalmente no, solo coordina la funcionalidad existente del subsistema detrás de una interfaz simplificada.

## 9. ¿Por qué se suele implementar como Singleton?
**R:** Porque normalmente solo se necesita una instancia para proporcionar el punto de acceso unificado al subsistema.

## 10. ¿Cómo se relaciona con la estratificación de sistemas?
**R:** Define puntos de entrada claros para cada nivel de subsistema, simplificando las dependencias entre ellos.