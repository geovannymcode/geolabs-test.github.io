# Guía de Programación Funcional en Java 8

## 1) ¿Qué nuevas características se añadieron en Java 8?

Java 8 incorporó varias características importantes:

- **Expresiones Lambda** - funciones anónimas `(a, b) -> a + b`
- **Referencias a métodos** - `String::valueOf`
- **Interfaces funcionales** - interfaces con un único método abstracto
- **Métodos default y static en interfaces**
- **API Stream** - procesamiento de colecciones de manera funcional
- **Optional** - contenedor para valores que pueden ser nulos
- **Nueva API de Fecha y Hora**
- **Nashorn** - motor JavaScript mejorado
- **Anotaciones repetibles**
- **Mejoras en concurrencia**

Ejemplo de expresión lambda:
```java
List<Integer> numeros = Arrays.asList(1, 2, 3, 4, 5);
// Antes de Java 8
for (Integer num : numeros) {
    System.out.println(num);
}
// Con Java 8
numeros.forEach(num -> System.out.println(num));
```

## 2) ¿Qué es una referencia a método?

Una referencia a método es una forma abreviada de expresión lambda que hace referencia a un método existente. Permite usar métodos como parámetros, facilitando la programación funcional.

Sintaxis: `TipoClase::nombreMetodo`

Tipos de referencias a método:

- A método estático: `Math::abs`
- A método de instancia de un objeto específico: `obj::metodo`
- A método de instancia de un objeto arbitrario: `String::length`
- A constructor: `ArrayList::new`

Ejemplo:
```java
// Lambda
Function<String, Integer> lambda = s -> s.length();
// Referencia a método equivalente
Function<String, Integer> referencia = String::length;

List<String> nombres = Arrays.asList("Ana", "Juan", "Carlos");
// Uso de referencia a método
nombres.forEach(System.out::println);
```

## 3) ¿Cuál es el significado de la expresión String::valueOf?

Es una referencia a método estático que apunta al método `valueOf` de la clase `String`. Permite convertir distintos tipos de datos a String de forma funcional.

Ejemplo:
```java
// Convertir un entero a String usando referencia a método
Function<Integer, String> conversor = String::valueOf;
String resultado = conversor.apply(123); // "123"

// Equivalente a la lambda:
Function<Integer, String> conversorLambda = x -> String.valueOf(x);
```

## 4) ¿Qué es Optional? ¿Cómo se puede usar?

`Optional<T>` es un contenedor que puede contener o no un valor no nulo. Ayuda a prevenir `NullPointerException` y a manejar valores ausentes de forma más elegante.

Operaciones principales:

- `of(valor)` - Crea Optional con valor no nulo
- `ofNullable(valor)` - Crea Optional que puede contener null
- `empty()` - Crea Optional vacío
- `isPresent()` - Verifica si hay valor
- `get()` - Obtiene valor (si existe)
- `orElse()` - Devuelve valor o alternativa
- `orElseGet()` - Alternativa mediante proveedor
- `orElseThrow()` - Lanza excepción si no hay valor
- `ifPresent()` - Ejecuta acción si hay valor
- `map()` - Transforma valor si existe

Ejemplo:
```java
// Antes de Java 8
Usuario buscarUsuario(String id) {
    // Puede devolver null
}

// Uso propenso a NullPointerException
String ciudad = buscarUsuario("123").getDireccion().getCiudad();

// Con Java 8 y Optional
Optional<Usuario> buscarUsuario(String id) {
    // Devuelve Optional
}

// Uso seguro
String ciudad = buscarUsuario("123")
                .map(Usuario::getDireccion)
                .map(Direccion::getCiudad)
                .orElse("Desconocida");
```

## 5) Describa algunas de las interfaces funcionales en la biblioteca estándar

Java 8 incluye varias interfaces funcionales en el paquete `java.util.function`:

- **`Function<T,R>`**: Toma T, devuelve R - `apply(T t)`
  ```java
  Function<String, Integer> longitud = s -> s.length();
  ```

- **`Predicate<T>`**: Toma T, devuelve boolean - `test(T t)`
  ```java
  Predicate<String> esVacio = s -> s.isEmpty();
  ```

- **`Consumer<T>`**: Toma T, no devuelve nada - `accept(T t)`
  ```java
  Consumer<String> imprimir = s -> System.out.println(s);
  ```

- **`Supplier<T>`**: No toma argumentos, devuelve T - `get()`
  ```java
  Supplier<Double> aleatorio = () -> Math.random();
  ```

- **`UnaryOperator<T>`**: Toma T, devuelve T - `apply(T t)`
  ```java
  UnaryOperator<String> mayusculas = s -> s.toUpperCase();
  ```

- **`BinaryOperator<T>`**: Toma dos T, devuelve T - `apply(T t1, T t2)`
  ```java
  BinaryOperator<Integer> suma = (a, b) -> a + b;
  ```

- **`BiFunction<T,U,R>`**: Toma T y U, devuelve R - `apply(T t, U u)`
  ```java
  BiFunction<String, Integer, String> repetir = (s, n) -> s.repeat(n);
  ```

## 6) ¿Qué es una interfaz funcional? ¿Cuáles son las reglas para definir una interfaz funcional?

Una interfaz funcional es aquella que contiene **exactamente un método abstracto**. Estas interfaces pueden usar la anotación `@FunctionalInterface` (opcional pero recomendada).

Reglas:

- Debe tener exactamente un método abstracto
- Puede tener múltiples métodos default o static
- Los métodos de Object sobreescritos no cuentan como método abstracto
- Si una interfaz hereda un método abstracto de otra y no agrega más métodos abstractos, sigue siendo una interfaz funcional

Ejemplo:
```java
@FunctionalInterface
interface Calculadora {
    int operar(int a, int b); // Único método abstracto
    
    default void mostrarInfo() {
        System.out.println("Calculadora básica");
    }
    
    static Calculadora suma() {
        return (a, b) -> a + b;
    }
}

// Uso
Calculadora sumador = (a, b) -> a + b;
int resultado = sumador.operar(5, 3); // 8
```

## 7) ¿Qué es un método default y cuándo lo usamos?

Un método default es un método con implementación dentro de una interfaz. Se introdujo en Java 8 para permitir agregar funcionalidades a interfaces existentes sin romper compatibilidad.

Usos principales:

- Evolucionar interfaces existentes sin romper código cliente
- Proporcionar implementaciones por defecto para métodos comunes
- Facilitar la herencia múltiple de comportamiento

Ejemplo:
```java
interface Vehículo {
    void acelerar();
    
    default void tocarBocina() {
        System.out.println("¡Beep! ¡Beep!");
    }
}

// La clase sólo necesita implementar acelerar()
class Coche implements Vehículo {
    @Override
    public void acelerar() {
        System.out.println("Coche acelerando");
    }
    // tocarBocina() ya tiene implementación por defecto
}
```

## 8) ¿Compilará el siguiente código?

La respuesta depende del código específico a analizar. Para evaluar si un código con interfaces funcionales, lambdas o métodos default compilará, debemos verificar:

- Si las interfaces funcionales tienen exactamente un método abstracto
- Si las expresiones lambda son compatibles con las interfaces funcionales
- Si no hay ambigüedad en la herencia múltiple de métodos default

## 9) ¿Qué es una expresión lambda y para qué se usa?

Una expresión lambda es una función anónima que puede ser pasada como argumento o almacenada en una variable. Se usa para implementar interfaces funcionales de forma concisa y expresiva.

Usos principales:

- Pasar comportamiento como parámetro
- Implementar callbacks
- Procesar colecciones funcionalmente (filter, map, reduce)
- Programación concurrente simplificada
- Implementar listeners/handlers de eventos

Ejemplo:
```java
// Antes de Java 8
Collections.sort(personas, new Comparator<Persona>() {
    @Override
    public int compare(Persona p1, Persona p2) {
        return p1.getNombre().compareTo(p2.getNombre());
    }
});

// Con lambda en Java 8
Collections.sort(personas, (p1, p2) -> p1.getNombre().compareTo(p2.getNombre()));
// O mejor aún
personas.sort(Comparator.comparing(Persona::getNombre));
```

## 10) Explique la sintaxis y características de una expresión lambda

Sintaxis:
```
(parámetros) -> expresión
```
o
```
(parámetros) -> { sentencias; }
```

Características:

- Los paréntesis pueden omitirse si hay un solo parámetro sin tipo explícito
- Las llaves pueden omitirse si el cuerpo es una sola expresión
- El tipo de los parámetros es opcional (inferencia de tipos)
- Si el cuerpo tiene llaves, necesita `return` explícito para devolver valor
- Accede a variables del ámbito circundante (debe ser final o efectivamente final)
- No puede acceder a `this` o `super` de la clase contenedora

Ejemplos:
```java
// Sin parámetros
Runnable r = () -> System.out.println("Hola");

// Un parámetro (paréntesis opcionales)
Consumer<String> c = s -> System.out.println(s);

// Múltiples parámetros
Comparator<String> comp = (s1, s2) -> s1.compareTo(s2);

// Con tipos explícitos
BiFunction<Integer, Integer, Integer> sum = (Integer a, Integer b) -> a + b;

// Cuerpo con múltiples sentencias
Comparator<Persona> comp = (p1, p2) -> {
    int resultado = p1.getApellido().compareTo(p2.getApellido());
    if (resultado == 0) {
        resultado = p1.getNombre().compareTo(p2.getNombre());
    }
    return resultado;
};
```

## 11) ¿Qué es Nashorn en Java 8?

Nashorn es un motor de JavaScript incluido en Java 8 que reemplaza al antiguo motor Rhino. Permite ejecutar código JavaScript dentro de aplicaciones Java.

Características:

- Mayor rendimiento (compilación JIT)
- Mejor integración con JVM
- Soporte para características de ECMAScript 5.1
- Acceso bidireccional entre Java y JavaScript

Ejemplo:
```java
// Evaluando JavaScript desde Java
ScriptEngineManager manager = new ScriptEngineManager();
ScriptEngine engine = manager.getEngineByName("nashorn");

// Ejecutar código JavaScript
engine.eval("print('Hola desde JavaScript');");

// Pasar variables de Java a JavaScript
int numero = 42;
engine.put("javaVar", numero);
engine.eval("print('El número es: ' + javaVar);");

// Llamar funciones JavaScript desde Java
engine.eval("function sumar(a, b) { return a + b; }");
Invocable invocable = (Invocable) engine;
Object resultado = invocable.invokeFunction("sumar", 5, 3);
System.out.println("Resultado: " + resultado); // 8
```

## 12) ¿Qué es JJS?

JJS es el nuevo ejecutable o herramienta de línea de comandos incluida en Java 8 que se utiliza para ejecutar código JavaScript en la consola mediante el motor Nashorn.

Características:

- Ejecución de scripts JavaScript desde la terminal
- Modo interactivo (REPL)
- Acceso a las APIs de Java desde JavaScript
- Opciones para compilación, depuración y optimización

Ejemplo de uso:
```bash
# Ejecutar un archivo JavaScript
jjs script.js

# Modo interactivo
jjs
>> var saludo = "Hola Mundo";
>> print(saludo);
Hola Mundo
>> var lista = Java.type("java.util.ArrayList");
>> var miLista = new lista();
>> miLista.add("Item 1");
>> print(miLista.size());
1
```

## 13) ¿Qué es un Stream? ¿En qué se diferencia de una Colección?

Un Stream es una secuencia de elementos que soporta operaciones agregadas secuenciales y paralelas. Permite procesamiento de datos declarativo (qué hacer, no cómo hacerlo).

Diferencias con colecciones:

- **Procesamiento vs. almacenamiento**: Streams procesan datos, colecciones almacenan datos
- **Evaluación perezosa**: Streams evalúan elementos solo cuando es necesario
- **Consumo único**: Streams no pueden ser reutilizados después de una operación terminal
- **Operaciones funcionales**: Streams favorecen operaciones que no modifican la fuente
- **Paralelismo**: Streams pueden operar en paralelo sin código adicional

Ejemplo:
```java
List<String> nombres = Arrays.asList("Ana", "Juan", "Carlos", "Pedro", "María");

// Colección: Enfoque imperativo
List<String> nombresLargosImpera = new ArrayList<>();
for (String nombre : nombres) {
    if (nombre.length() > 4) {
        nombresLargosImpera.add(nombre.toUpperCase());
    }
}

// Stream: Enfoque declarativo
List<String> nombresLargos = nombres.stream()
    .filter(nombre -> nombre.length() > 4)
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

## 14) ¿Cuál es la diferencia entre operaciones intermedias y terminales?

Las operaciones de Stream se combinan en tuberías (pipelines). Todas las operaciones son intermedias o terminales:

**Operaciones intermedias**:

- Devuelven un nuevo Stream (transformado)
- Son perezosas (no se ejecutan hasta que haya una operación terminal)
- Pueden encadenarse
- Ejemplos: `filter()`, `map()`, `sorted()`, `distinct()`, `limit()`

**Operaciones terminales**:

- Producen un resultado o efecto secundario
- Activan la ejecución del pipeline
- Consumen el Stream (no se puede reutilizar después)
- Ejemplos: `forEach()`, `collect()`, `reduce()`, `count()`, `anyMatch()`

Ejemplo:
```java
List<Integer> numeros = Arrays.asList(1, 2, 3, 4, 5, 6);

numeros.stream()
    .filter(n -> {             // Intermedia
        System.out.println("Filtrando: " + n);
        return n % 2 == 0;
    })
    .map(n -> {                // Intermedia
        System.out.println("Mapeando: " + n);
        return n * n;
    })
    .limit(2)                  // Intermedia
    .forEach(System.out::println); // Terminal

// Salida:
// Filtrando: 1
// Filtrando: 2
// Mapeando: 2
// 4
// Filtrando: 3
// Filtrando: 4
// Mapeando: 4
// 16
```

## 15) ¿Cuál es la diferencia entre las operaciones map y flatMap?

Ambas transforman elementos en un Stream, pero:

**map**: Transforma cada elemento en exactamente otro elemento.

- Aplicación uno-a-uno
- Devuelve Stream con misma cantidad de elementos
- Función aplicada: `T -> R`

**flatMap**: Transforma cada elemento en 0 o más elementos, "aplanando" el resultado.

- Aplicación uno-a-muchos
- Útil para "aplanar" Streams anidados
- Función aplicada: `T -> Stream<R>`

Ejemplo:
```java
// map: transformación uno-a-uno
List<String> palabras = Arrays.asList("Hola", "Mundo");
List<Integer> longitudes = palabras.stream()
    .map(String::length)  // String -> Integer
    .collect(Collectors.toList());
// Resultado: [4, 5]

// flatMap: aplanamiento de streams
List<List<Integer>> listaDeNumeros = Arrays.asList(
    Arrays.asList(1, 2),
    Arrays.asList(3, 4, 5)
);

List<Integer> todosLosNumeros = listaDeNumeros.stream()
    .flatMap(Collection::stream)  // List<Integer> -> Stream<Integer>
    .collect(Collectors.toList());
// Resultado: [1, 2, 3, 4, 5]

// Otro ejemplo: Obtener palabras únicas de frases
List<String> frases = Arrays.asList("Hola mundo", "Adiós mundo");
List<String> palabrasUnicas = frases.stream()
    .flatMap(frase -> Arrays.stream(frase.split(" ")))
    .distinct()
    .collect(Collectors.toList());
// Resultado: [Hola, mundo, Adiós]
```

## 16) ¿Qué es el Stream pipelining en Java 8?

El Stream pipelining es una técnica de procesamiento de datos donde múltiples operaciones se encadenan formando una tubería (pipeline). Consiste en una fuente, cero o más operaciones intermedias y una operación terminal.

Características:

- Evaluación perezosa: Las operaciones intermedias no se evalúan hasta que se ejecuta una operación terminal
- Fusión (fusion): Múltiples operaciones se combinan para mejorar rendimiento
- Procesamiento paso a paso: Cada elemento recorre el pipeline completo antes de procesar el siguiente

Ejemplo de pipeline:
```java
double promedioPrecio = productos.stream()    // Fuente
    .filter(p -> p.getCategoria().equals("Electrónica"))  // Intermedia
    .mapToDouble(Producto::getPrecio)                     // Intermedia
    .average()                                            // Terminal
    .orElse(0.0);

// El pipeline no se ejecuta hasta llegar a average()
// Cada producto pasa por filter y luego por mapToDouble antes de pasar al siguiente
```

## 17) Hable sobre la nueva API de Fecha y Hora en Java 8

Java 8 introdujo una nueva API de fecha y hora en el paquete `java.time` para resolver problemas de las clases antiguas (Date, Calendar).

Características principales:

- Inmutable y thread-safe
- Más intuitiva y consistente
- Mejor soporte para zonas horarias
- Separación clara de conceptos (fecha, hora, fecha-hora)
- Mayor precisión (nanosegundos)

Clases principales:

- `LocalDate`: Solo fecha (sin hora ni zona horaria)
- `LocalTime`: Solo hora (sin fecha ni zona horaria)
- `LocalDateTime`: Fecha y hora (sin zona horaria)
- `ZonedDateTime`: Fecha y hora con zona horaria
- `Instant`: Punto específico en la línea temporal
- `Duration`: Cantidad de tiempo (segundos, nanosegundos)
- `Period`: Cantidad de tiempo (años, meses, días)
- `DateTimeFormatter`: Formateo y análisis de fechas

Ejemplo:
```java
// Crear fechas
LocalDate hoy = LocalDate.now();
LocalDate cumpleaños = LocalDate.of(2023, Month.JANUARY, 15);

// Crear horas
LocalTime ahora = LocalTime.now();
LocalTime mediodía = LocalTime.of(12, 0);

// Fecha y hora
LocalDateTime fechaHoraActual = LocalDateTime.now();
LocalDateTime evento = LocalDateTime.of(2023, Month.DECEMBER, 24, 20, 0);

// Con zona horaria
ZonedDateTime aquíAhora = ZonedDateTime.now();
ZonedDateTime tokio = ZonedDateTime.now(ZoneId.of("Asia/Tokyo"));

// Manipulación (inmutable - devuelve nuevas instancias)
LocalDate mañana = hoy.plusDays(1);
LocalDate semanaPasada = hoy.minusWeeks(1);
LocalDate primerDíaMes = hoy.withDayOfMonth(1);

// Formateo
DateTimeFormatter formato = DateTimeFormatter.ofPattern("dd/MM/yyyy");
String fechaFormateada = hoy.format(formato);

// Parsing
LocalDate fechaParseada = LocalDate.parse("15/01/2023", formato);

// Cálculos
Period periodo = Period.between(cumpleaños, hoy);
int años = periodo.getYears();
```