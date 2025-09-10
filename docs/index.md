# Guía Rápida: Preguntas de Entrevista Java

## 1) Características de Java

- Orientado a objetos
- Independiente de la plataforma (Write Once, Run Anywhere)
- Robusto (manejo de excepciones, recolector de basura)
- Seguro (sin punteros, sandbox de la JVM)
- Multihilo
- Arquitectura neutral (bytecode)
- Interpretado y de alto rendimiento (JIT)
- Distribuido (soporte de red integrado)

## 2) ¿Qué es un Constructor?

Método especial para inicializar objetos cuando se instancian. Tiene el mismo nombre que la clase, no tiene tipo de retorno y puede estar sobrecargado. Si no se define, Java proporciona un constructor por defecto sin parámetros.

## 3) Variable local vs. variable de instancia

- **Variable de instancia**: Declarada dentro de la clase, fuera de métodos. Cada objeto tiene su propia copia. Se inicializa automáticamente.
- **Variable local**: Declarada dentro de métodos/bloques. Solo accesible en su ámbito. No se inicializa automáticamente.

## 4) ¿Qué es una clase?

Plantilla para crear objetos que define atributos (estado) y métodos (comportamiento). Actúa como un tipo de dato que encapsula funcionalidad relacionada.

## 5) ¿Qué es la herencia?

Mecanismo donde una clase (subclase) adquiere propiedades y comportamientos de otra (superclase). Establece relación "es-un" y permite reutilización de código. En Java, se implementa con `extends` y solo permite herencia simple.

## 6) ¿Qué es la encapsulación?

Principio de ocultar los detalles internos y exponer solo lo necesario. Se implementa con modificadores de acceso (private, protected, public) y métodos getter/setter. Proporciona control de acceso y facilita el mantenimiento.

## 7) ¿Qué es el polimorfismo?

Capacidad de un objeto para tomar diferentes formas. En Java se implementa mediante:

- Sobrecarga: múltiples métodos con el mismo nombre pero diferentes parámetros
- Sobreescritura: redefinir un método heredado de la superclase
- Referencias de tipo interfaz/superclase a objetos de subclases

## 8) ¿Qué significa sobrescribir un método?

Redefinir en una subclase un método heredado de la superclase. Debe tener la misma firma (nombre, parámetros y tipo de retorno). Se usa la anotación `@Override` para verificar que realmente se está sobrescribiendo. El método en la subclase reemplaza al de la superclase cuando se llama desde un objeto de la subclase.

## 9) ¿Qué es la sobrecarga de métodos?

Definir múltiples métodos con el mismo nombre pero diferentes parámetros (tipo, número u orden). Java determina cuál utilizar en tiempo de compilación según los argumentos proporcionados. El tipo de retorno por sí solo no es suficiente para distinguir entre métodos sobrecargados.

## 10) ¿Qué es una Interfaz?

Contrato que especifica métodos que una clase debe implementar. Define qué se debe hacer, pero no cómo. Una clase puede implementar múltiples interfaces. Desde Java 8, pueden contener métodos default y static con implementación.

## 11) ¿Qué es una clase abstracta?

Clase que no puede ser instanciada directamente y está diseñada para ser extendida. Puede contener métodos abstractos (sin implementación) y métodos concretos. Una clase que extiende una clase abstracta debe implementar todos sus métodos abstractos o ser también abstracta.

## 12) String vs. StringBuilder vs. StringBuffer

- **String**: Inmutable. Cada modificación crea nuevo objeto.
- **StringBuilder**: Mutable, más eficiente para concatenaciones. No sincronizado (no thread-safe).
- **StringBuffer**: Similar a StringBuilder pero sincronizado (thread-safe), más lento.

## 13) HashMap vs. Hashtable

- **HashMap**: No sincronizado, permite una clave null y múltiples valores null, más eficiente.
- **Hashtable**: Sincronizado (thread-safe), no permite claves/valores null, más lento.

## 14) Constructores vs. Métodos

- Constructores: Mismo nombre que la clase, sin tipo de retorno, invocados con new, inicializan objetos.
- Métodos: Nombre diferente a la clase, tienen tipo de retorno (incluso void), invocados con referencias a objetos, realizan operaciones.

## 15) ¿Qué es un Array?

Estructura de datos que almacena elementos del mismo tipo en posiciones contiguas de memoria. Tamaño fijo definido en la creación. Se accede a elementos por índice (0-based). Ejemplo: `int[] numeros = new int[5];`

## 16) ¿Sobrecarga solo cambiando tipo de retorno?

No. Java no permite sobrecargar métodos cambiando solo el tipo de retorno. Debe haber diferencia en los parámetros (tipo, número u orden).

## 17) Clase abstracta vs. Interfaz

- **Clase abstracta**: Puede tener métodos con implementación y variables de instancia, constructores, soporta herencia simple.
- **Interfaz**: Antes de Java 8, solo métodos abstractos; después, permite métodos default y static. Variables solo public static final, no constructores, permite implementación múltiple.

## 18) Variable estática en Java

Variable que pertenece a la clase, no a instancias. Se comparte entre todos los objetos. Se declara con `static` y se accede con `NombreClase.variable`. Se inicializa cuando la clase se carga en memoria.

## 19) Bloque estático

Bloque de código precedido por `static` que se ejecuta una sola vez cuando la clase se carga en memoria. Se usa para inicializar variables estáticas o realizar acciones de inicialización que deben ocurrir una sola vez.

## 20) Uso de final en Java

- En variables: Constantes, no pueden cambiar de valor
- En métodos: No pueden ser sobrescritos
- En clases: No pueden ser heredadas

## 21) Tipos de excepciones

- **Checked**: Verificadas en compilación, deben ser declaradas/capturadas (IOException)
- **Unchecked (Runtime)**: No verificadas en compilación (NullPointerException)
- **Error**: Problemas graves del sistema (OutOfMemoryError)

## 22) ¿String es inmutable o mutable?

Inmutable. Una vez creado un objeto String, su contenido no puede cambiar. Cualquier operación que parezca modificarlo realmente crea un nuevo objeto String.

## 23) List, Set, Map

- **List**: Colección ordenada que permite duplicados (ArrayList, LinkedList)
- **Set**: Colección sin duplicados (HashSet, TreeSet)
- **Map**: Almacena pares clave-valor, sin claves duplicadas (HashMap, TreeMap)

## 24) ¿Por qué ArrayList es mejor que Arrays?

- Tamaño dinámico (crece automáticamente)
- API rica en métodos para manipular datos
- Admite solo objetos (no primitivos)
- Fácil conversión a/desde otros tipos de colecciones
- Implementa interfaces de colección

## 25) ArrayList vs. LinkedList

- **ArrayList**: Implementado con array redimensionable. Acceso aleatorio eficiente O(1). Inserción/eliminación ineficiente O(n) si no es al final.
- **LinkedList**: Implementado con lista doblemente enlazada. Acceso aleatorio ineficiente O(n). Inserción/eliminación eficiente O(1) si se tiene el iterador posicionado.

## 26) ArrayList vs. Vector

- **ArrayList**: No sincronizado (no thread-safe), más eficiente en entornos single-thread.
- **Vector**: Sincronizado (thread-safe), menos eficiente por sobrecarga de sincronización, crece 100% cuando se llena.

## 27) Iterator vs. ListIterator

- **Iterator**: Recorre colecciones en una dirección (adelante), solo permite eliminar.
- **ListIterator**: Solo para List, bidireccional (adelante/atrás), permite eliminar, agregar y modificar elementos.

## 28) TreeSet vs. SortedSet

SortedSet es una interfaz que define una colección ordenada sin duplicados. TreeSet es la implementación de SortedSet basada en un árbol rojo-negro, que proporciona tiempo logarítmico para operaciones básicas.

## 29) HashMap vs. Hashtable

- **HashMap**: No sincronizado, permite una clave null y valores null, implementación más reciente y eficiente.
- **Hashtable**: Sincronizado (thread-safe), no permite claves/valores null, clase legacy.

## 30) Iterator vs. Enumeration

- **Iterator**: Más moderno, permite eliminar elementos durante la iteración, usado en todas las colecciones modernas.
- **Enumeration**: Interfaz legacy, solo lectura (no permite modificar), usado en clases antiguas como Vector y Hashtable.