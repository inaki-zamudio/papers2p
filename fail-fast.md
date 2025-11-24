
# Resumen: "Fail Fast" de Jim Shore

## 📌 Concepto Principal
**"Fail Fast"** (Fallar Rápido) es una técnica de desarrollo de software que consiste en hacer que el software **falle de manera inmediata y visible** cuando se detecta un problema, en lugar de intentar manejarlo silenciosamente y permitir que el error se propague.

> 🔍 **Objetivo**: Hacer que los bugs sean más fáciles de encontrar y corregir, incluso si no se reduce su cantidad inicial.

---

## ⚖️ Fallar Rápido vs. Fallar Lentamente

| Fallar Rápido | Fallar Lentamente |
|---------------|-------------------|
| Falla al momento del error | Falla más tarde, de manera extraña |
| Excepción visible | Comportamiento incorrecto silencioso |
| Fácil depuración | Depuración difícil y tediosa |

### Ejemplo en código:
```java
// ❌ Enfoque que falla lentamente (retorna valor por defecto)
public int maxConnections() {
    string property = getProperty("maxConnections");
    if (property == null) {
        return 10; // Valor por defecto → error oculto
    }
    // ...
}

// ✅ Enfoque que falla rápido (lanza excepción)

```java
public int maxConnections() {
    string property = getProperty("maxConnections");
    if (property == null) {
        throw new NullReferenceException("maxConnections property not found in " + this.configFilePath);
    }
    // ...
}
```

---

## 🛠️ Cómo Implementar "Fail Fast"

### 1. Usar Aserciones
Las aserciones son fragmentos de código que **verifican una condición y fallan si no se cumple**.

Ejemplo de clase `Assert` en Java:
```java
public class Assert {
    public static void notNull(Object o) {
        if (o == null) throw new NullReferenceException();
    }
    // Más métodos: true(), cantReach(), impossibleException(), etc.
}
```

### 2. Cuándo Usar Aserciones
- **No** para verificar problemas dentro del método mismo (mejor usar TDD).
- **Sí** para verificar interacciones incorrectas con otros componentes del sistema.
- **Sí** para documentar suposiciones y contratos.

### 3. Regla Práctica para `Assert.notNull()`
- No uses aserciones en cada asignación.
- Úsalas cuando un **parámetro se asigna a una variable de instancia**, para evitar que el error se manifieste más tarde.

```java
// ✅ Buena práctica: validar parámetros en el constructor
public class Foo {
    private Object _instanceVariable;
    public Foo(Object instanceVariable) {
        Assert.notNull(instanceVariable);
        _instanceVariable = instanceVariable;
    }
}
```

---

## 📝 Escribir Mensajes de Aserción Útiles

Los mensajes deben dar **contexto**, no solo repetir la condición.

```java
// ❌ Mal: repite la condición
Assert.notNull(result, "result was null");

// ❌ Regular: poco contexto
Assert.notNull(result, "can't find property");

// ✅ Ideal: contexto claro
Assert.notNull(result, "can't find [" + key + "] property in config file [" + file + "]");
```

---

## 🧩 Manejo Robusto de Errores en Producción

### No Desactivar Aserciones en Producción
- Los errores en producción son los más difíciles de reproducir.
- Una aserción bien ubicada puede ahorrar días de depuración.

### Usar Manejadores Globales de Excepciones
Ejemplo para un sistema por lotes en C#:
```csharp
public static void Main() {
    try {
        foreach (var command in Batch()) {
            try {
                command.Process();
            } catch (Exception e) {
                ReportError("Exception in " + command, e);
                // Continuar con el siguiente comando
            }
        }
    } catch (Exception e) {
        ReportError("Exception in batch loader", e);
        // Error irrecuperable → salir
    }
}
```

### Recomendaciones:
- Evitar bloques `catch` genéricos que capturen todo.
- Usar `finally` o `using` para liberar recursos.

---

## ✅ Beneficios de "Fail Fast"

- ✅ Reduce el tiempo de depuración.
- ✅ Mejora la calidad del software a largo plazo.
- ✅ Los errores se detectan cerca de su origen.
- ✅ Facilita la mantenibilidad.

---

## 🚀 Conclusión

"Fail Fast" es una técnica práctica que puede adoptarse gradualmente:

1. Implementar un **manejador global de excepciones**.
2. Revisar y eliminar **manejadores genéricos de excepciones**.
3. Introducir **aserciones estratégicas** en el código.
4. Asegurar que los mensajes de error sean **informativos**.

> Resultado: Menos tiempo depurando, más calidad en el software.

---

*Resumen basado en el artículo "Fail Fast" de Jim Shore, publicado en IEEE Software, Sept/Oct 2004.*
```

Puedes guardar este contenido en un archivo `.md` y usarlo para referencia o estudio. ¿Necesitas también una versión en PDF o Word?
