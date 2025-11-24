Resumen: "Active Variables in Smalltalk-80" de Messick y Beck
Información del Documento

    Título: Active Variables in Smalltalk-80

    Autores: Steven L. Messick, Kent L. Beck

    Institución: Computer Research Lab, Tektronix, Inc.

    Fecha: 7 de febrero de 1985

    Referencia: CR-85-09

1. Introducción a las Variables Activas

Las variables activas son una característica inspirada en LOOPS (Xerox PARC) que permite asociar procedimientos auxiliares (llamados demonios) a variables. Estos demonios se ejecutan automáticamente cuando la variable es accedida o modificada. Su utilidad incluye:

    Depuración de código.

    Implementación de interfaces visuales ("dials and gauges").

    Monitoreo de valores sin alterar el flujo principal del programa.

A diferencia de otras implementaciones, esta se integra directamente en el lenguaje Smalltalk-80, minimizando el impacto en el código existente.
2. Implementación de Variables Activas
2.1. Tipos de Variables Activas

    Variables de instancia activas: Declaradas en la definición de la clase.

    Variables temporales activas: Declaradas en métodos.

2.2. Declaración

    En clases:
    smalltalk

activeVariables: '(av1 getFn1 putFn1) (av2 getFn2 putFn2)'

En métodos:
smalltalk

| x y (av1 getFn1 putFn1) (av2 getFn2 putFn2) |

2.3. Clases Principales

    InactiveVariable: Representa una variable inactiva.

    ActiveVariable: Contiene demonios (selectores de mensajes) para get y set.

    SpecificActiveVariable: Similar a ActiveVariable, pero usa bloques de código en lugar de selectores.

2.4. Demonios (Daemons)

    Get Daemon: Se ejecuta al acceder a la variable.

    Set Daemon: Se ejecuta al modificar la variable.

Ejemplo de demonio de acceso:
smalltalk

genericGetDaemon: activeValue
  ^activeValue value

3. Ejemplo de Uso

Clase Test con variable activa randomValue:
smalltalk

Object subclass: #Test
  instanceVariableNames: 'randomValue'
  activeVariables: '(randomValue randomGetDaemon: nil)'
  ...

Test methodsFor: 'daemons'
randomGetDaemon: av
  ^av value next

Cada acceso a randomValue genera un nuevo número aleatorio.
4. Protocolo Adicional
4.1. Comportamiento de Clases

    Behavior y ClassDescription incluyen métodos para gestionar variables activas.

    Class extiende los mensajes de creación de subclases para soportar variables activas.

4.2. Métodos en Object

    get:, set:to:, shortStack:, transcriptShow:: Demonios predefinidos.

    Métodos para cambiar demonios en tiempo de ejecución.

5. Paquete "Dials and Gauges"

Inspirado en LOOPS, se implementaron visualizadores gráficos para variables activas. Ejemplo con la clase LCD:
smalltalk

t := Test new.
l := LCD onVar: 'randomValue' of: t.
l open.

Al cambiar randomValue, el display se actualiza automáticamente.
6. Métodos Aconsejados (Advised Methods)

Permiten insertar código antes o después de un método sin recompilar. Inspirado en Interlisp.

    advise: Abre un editor para agregar código de consejo.

    unadvise: Elimina el consejo.

Usos:

    Puntos de interrupción condicionales.

    Verificación de tipos.

    Trazado de ejecución.

7. Resumen y Conclusiones

    Las variables activas permiten monitorear y reaccionar a cambios en variables.

    Los métodos aconsejados ofrecen flexibilidad para extender comportamientos sin modificar código fuente.

    Ambas características enriquecen el entorno de programación Smalltalk-80, facilitando la depuración y visualización.

8. Referencias

    Bobrow & Stefik (1983): The LOOPS Manual.

    Delisle et al. (1983): Magpie – An Interactive Programming Environment for Pascal.

    London & Duisberg (1984): Animating Programs Using Smalltalk.

    Xerox (1983): Interlisp Reference Manual.

Nota: Este documento es un borrador confidencial de Tektronix, Inc., fechado en 1985.

