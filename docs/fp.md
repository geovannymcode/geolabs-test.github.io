# Guía de Entrevista: Java 8 y Programación Funcional

## 1) ¿Qué características importantes se introdujeron en Java 8?

- **Expresiones lambda**: Funciones anónimas sin clase `(a, b) -> a + b`
- **Referencias a métodos**: Forma abreviada de lambda `String::valueOf`
- **Interfaces funcionales**: Interfaces con un único método abstracto
- **Métodos default y static en interfaces**: Para extender interfaces sin romper compatibilidad
- **API Stream**: Para procesamiento funcional de colecciones
- **Optional**: Contenedor para valores potencialmente nulos
- **Nueva API de Fecha y Hora**: Reemplazo inmutable y thread-safe para Date/Calendar
- **Nashorn**: Motor JavaScript mejorado

```java
// Ejemplo de uso de nuevas características
List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro");
nombres.stream()                            // Stream API
      .filter(s -> s.length() > 3)          // Lambda
      .map(String::toUpperCase)             // Referencia a método
      .forEach(System.out::println);        // Referencia a método
```

## 2) ¿Qué es una expresión lambda y cuál es su caso de uso ideal?

Una función anónima que puede referenciarse y pasarse como objeto. Caso de uso ideal: implementar interfaces funcionales de forma concisa.

```java
// Antes de Java 8
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Ejecutando");
    }
};

// Con lambda en Java 8
Runnable r = () -> System.out.println("Ejecutando");
```

## 3) Explique la sintaxis de una expresión lambda

Componentes:

- **Parámetros**: Entre paréntesis (opcionales si hay un solo parámetro sin tipo)
- **Flecha** `->`: Separa parámetros del cuerpo
- **Cuerpo**: Expresión única o bloque de código entre llaves

Formatos:
```java
// Sin parámetros
() -> System.out.println("Hola")

// Un parámetro (paréntesis opcionales)
n -> n * n
(String s) -> s.length()

// Múltiples parámetros
(a, b) -> a + b
(int x, int y) -> { return x + y; }

// Bloque de código (requiere return explícito)
(String s) -> {
    String result = s.toUpperCase();
    return result;
}
```

## 4) Ejecute un hilo usando una expresión lambda

```java
// Antes de Java 8
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Ejecutando hilo");
    }
}).start();

// Con lambda en Java 8
new Thread(() -> System.out.println("Ejecutando hilo")).start();
```

## 5) ¿Qué es una referencia a método?

Una forma abreviada de expresión lambda que refiere a un método existente. Se usa cuando la lambda simplemente llama a un método.

Tipos:

- A método estático: `Math::abs`
- A método de instancia de objeto específico: `objeto::método`
- A método de instancia de objeto arbitrario: `String::length`
- A constructor: `ArrayList::new`

```java
// Lambda
Function<String, Integer> lambda = s -> s.length();
// Referencia a método equivalente
Function<String, Integer> referencia = String::length;

// Uso con forEach
List<String> nombres = Arrays.asList("Ana", "Juan", "Carlos");
nombres.forEach(System.out::println);
```

## 6) ¿Qué es una interfaz funcional?

Una interfaz con exactamente un método abstracto. Se usa como tipo objetivo para expresiones lambda.

```java
@FunctionalInterface  // Anotación opcional
interface Calculadora {
    int operar(int a, int b);  // Único método abstracto
    
    // Puede tener métodos default o static
    default void mostrarInfo() {
        System.out.println("Calculadora");
    }
}

// Implementación con lambda
Calculadora suma = (a, b) -> a + b;
int resultado = suma.operar(5, 3);  // 8
```

## 7) ¿Cuáles son los tipos de interfaces funcionales más comunes?

Java 8 incluye varias interfaces funcionales en `java.util.function`:

- **`Consumer<T>`**: Acepta un valor, no devuelve nada - `accept(T t)`
  ```java
  Consumer<String> imprimir = s -> System.out.println(s);
  ```

- **`Supplier<T>`**: No acepta parámetros, devuelve un valor - `get()`
  ```java
  Supplier<Double> aleatorio = () -> Math.random();
  ```

- **`Function<T,R>`**: Acepta T, devuelve R - `apply(T t)`
  ```java
  Function<String, Integer> longitud = s -> s.length();
  ```

- **`Predicate<T>`**: Acepta T, devuelve boolean - `test(T t)`
  ```java
  Predicate<String> esVacio = s -> s.isEmpty();
  ```

- **`UnaryOperator<T>`**: Acepta T, devuelve T - `apply(T t)`
  ```java
  UnaryOperator<String> mayusculas = s -> s.toUpperCase();
  ```

- **`BinaryOperator<T>`**: Acepta dos T, devuelve T - `apply(T t1, T t2)`
  ```java
  BinaryOperator<Integer> suma = (a, b) -> a + b;
  ```

## 8) ¿Qué es un método default y un método static en interfaces?

**Método default**: Método con implementación dentro de una interfaz. Permite añadir funcionalidades a interfaces existentes sin romper compatibilidad.

**Método static**: Método estático en interfaz, pertenece solo a la interfaz y no a implementaciones.

```java
interface Vehículo {
    void acelerar();  // Método abstracto tradicional
    
    // Método default (todas las implementaciones lo heredan)
    default void tocarBocina() {
        System.out.println("¡Beep!");
    }
    
    // Método static (pertenece solo a la interfaz)
    static Vehículo crearVehículoEléctrico() {
        return new VehículoEléctrico();
    }
}
```

## 9) ¿Qué condiciones deben cumplirse para que una expresión lambda coincida con una interfaz funcional?

1. La interfaz debe tener exactamente un método abstracto
2. Los parámetros de la lambda deben coincidir con los del método
3. El tipo de retorno de la lambda debe ser compatible con el del método

## 10) ¿Qué es la clase Optional?

Contenedor que puede contener un valor no nulo o estar vacío. Evita `NullPointerException` y proporciona operaciones para tratar valores ausentes.

```java
// Crear Optional
Optional<String> empty = Optional.empty();
Optional<String> nombre = Optional.of("Juan");  // No puede ser null
Optional<String> posibleNull = Optional.ofNullable(obtenerNombre());  // Puede ser null

// Usar Optional
if (nombre.isPresent()) {
    System.out.println(nombre.get());
}

// Mejor forma (funcional)
nombre.ifPresent(n -> System.out.println(n));

// Valor alternativo si no existe
String resultado = nombre.orElse("Desconocido");

// Lanzar excepción si no existe
String valor = nombre.orElseThrow(() -> new NoSuchElementException());

// Transformar si existe (sin NullPointerException)
Optional<Integer> longitud = nombre.map(String::length);
```

## 11) ¿Qué es la API Stream y cómo se usa?

Secuencia de elementos que soporta operaciones agregadas. Permite procesamiento de datos declarativo (qué hacer, no cómo).

Componentes:

1. Fuente de datos (colección, array)
2. Operaciones intermedias (transforman el stream)
3. Operación terminal (produce resultado)

```java
List<String> nombres = Arrays.asList("Ana", "Juan", "Carlos", "Pedro");

// Filtrar nombres que empiezan con 'J' y convertir a mayúsculas
List<String> resultado = nombres.stream()        // Fuente
    .filter(n -> n.startsWith("J"))              // Intermedia
    .map(String::toUpperCase)                    // Intermedia
    .collect(Collectors.toList());               // Terminal
```

## 12) ¿Qué son las operaciones intermedias y terminales?

**Operaciones intermedias**:

- Devuelven un nuevo stream
- Son perezosas (no se ejecutan hasta que haya una operación terminal)
- Ejemplos: `filter()`, `map()`, `sorted()`, `distinct()`, `limit()`

**Operaciones terminales**:

- Producen un resultado o efecto secundario
- Consumen el stream (no se puede reutilizar)
- Ejemplos: `forEach()`, `collect()`, `reduce()`, `count()`, `anyMatch()`

```java
long contador = Stream.of(1, 2, 3, 4, 5)
    .filter(n -> {                     // Intermedia
        System.out.println("Filtrando: " + n);
        return n % 2 == 0;
    })
    .count();                          // Terminal
// Solo se procesan los elementos cuando se llega a count()
```

## 13) ¿Qué es un Collector y cómo se usa?

Mecanismo para combinar elementos de un stream en un resultado final (lista, mapa, cadena, etc.).

```java
// Recolectar en una lista
List<String> lista = stream.collect(Collectors.toList());

// Recolectar en un conjunto
Set<String> conjunto = stream.collect(Collectors.toSet());

// Recolectar en una cadena
String cadena = stream.collect(Collectors.joining(", "));

// Agrupar por alguna propiedad
Map<Integer, List<Persona>> porEdad = personas.stream()
    .collect(Collectors.groupingBy(Persona::getEdad));

// Particionar según un predicado
Map<Boolean, List<Persona>> adultos = personas.stream()
    .collect(Collectors.partitioningBy(p -> p.getEdad() >= 18));
```

## 14) ¿Cuál es la diferencia entre las operaciones map y flatMap?

**map**: Transforma cada elemento en exactamente otro elemento (uno a uno).
```java
// String -> Integer (longitud)
Stream<Integer> longitudes = Stream.of("a", "bc", "def")
    .map(String::length);  // [1, 2, 3]
```

**flatMap**: Transforma cada elemento en 0 o más elementos y "aplana" el resultado (uno a muchos).
```java
// Lista de listas -> Lista plana
List<List<Integer>> listas = Arrays.asList(
    Arrays.asList(1, 2),
    Arrays.asList(3, 4, 5)
);
List<Integer> plana = listas.stream()
    .flatMap(Collection::stream)  // [1, 2, 3, 4, 5]
    .collect(Collectors.toList());
```

## 15) ¿Qué ventajas ofrece la nueva API de Fecha y Hora en Java 8?

- Inmutable y thread-safe
- Más intuitiva y consistente
- Mejor soporte para zonas horarias
- Separación clara de conceptos
- Mayor precisión (nanosegundos)

```java
// Fecha actual
LocalDate hoy = LocalDate.now();  // 2023-09-10

// Hora actual
LocalTime ahora = LocalTime.now();  // 15:30:45.123

// Fecha y hora
LocalDateTime fechaHora = LocalDateTime.now();

// Con zona horaria
ZonedDateTime zonaHora = ZonedDateTime.now(ZoneId.of("Europe/Madrid"));

// Manipulación (inmutable)
LocalDate mañana = hoy.plusDays(1);
LocalDate primerDíaMes = hoy.withDayOfMonth(1);

// Formateo
DateTimeFormatter formato = DateTimeFormatter.ofPattern("dd/MM/yyyy");
String fechaFormateada = hoy.format(formato);  // "10/09/2023"
```

## 16) ¿Qué es la programación funcional?

Paradigma de programación donde las funciones son ciudadanas de primera clase y los programas se construyen componiendo funciones puras sin efectos secundarios.

Características en Java 8:
- Funciones como objetos (lambdas)
- Inmutabilidad (evitar cambios de estado)
- Operaciones de orden superior (funciones que operan sobre otras funciones)
- Evaluación perezosa (streams)
- Procesamiento de colecciones mediante operaciones funcionales

Beneficios:
- Código más conciso y expresivo
- Menos errores por estado mutable
- Mejor paralelización
- Mayor capacidad de composición

## 17) ¿Qué son las operaciones terminales y qué hacen?

Las operaciones terminales son las que cierran un Stream y devuelven un resultado. Marcan el final del procesamiento y desencadenan la ejecución de todas las operaciones intermedias pendientes.

Operaciones terminales comunes:
- `collect()`: Acumula elementos en una colección
- `forEach()`: Ejecuta una acción para cada elemento
- `reduce()`: Combina elementos en un único resultado
- `count()`: Cuenta elementos
- `min()/max()`: Encuentra el valor mínimo/máximo
- `anyMatch()/allMatch()/noneMatch()`: Comprueba condiciones

```java
// Ejemplos de operaciones terminales
long cantidad = stream.count();
Optional<Persona> masJoven = personas.stream().min(Comparator.comparing(Persona::getEdad));
boolean todosMayores = personas.stream().allMatch(p -> p.getEdad() >= 18);
int suma = numeros.stream().reduce(0, Integer::sum);
```

## 18) Recolecte elementos de un Stream en una Lista usando el método toList()

```java
// Java 8
List<Integer> numeros = Stream.of(1, 2, 3)
    .collect(Collectors.toList());

// Java 16+
List<Integer> numeros = Stream.of(1, 2, 3)
    .toList();  // Método más corto en versiones recientes
```

## 19) Determine el número de elementos en un Stream

```java
// Crear un Stream
Stream<String> stream = Stream.of("a", "b", "c", "d", "e");

// Contar elementos
long cantidad = stream.count();
System.out.println("Número de elementos: " + cantidad);  // 5
```

## 20) ¿Para qué se usa la palabra clave random?

En Java 8, la palabra `random` no es una palabra clave como tal, pero hay varios métodos y clases para generar valores aleatorios:

```java
// Usando Random
Random random = new Random();
int numAleatorio = random.nextInt(100);  // 0-99

// Usando ThreadLocalRandom (Java 7+)
int aleatorio = ThreadLocalRandom.current().nextInt(1, 101);  // 1-100

// Usando streams de números aleatorios (Java 8)
IntStream numerosAleatorios = random.ints(5, 1, 100);  // 5 números entre 1-99
numerosAleatorios.forEach(System.out::println);
```

## 21) ¿Qué es un Supplier?

Un `Supplier<T>` es una interfaz funcional que no recibe argumentos y produce un resultado de tipo T. Se usa cuando queremos retrasar la creación de un objeto o la evaluación de una expresión.

```java
// Definición
@FunctionalInterface
public interface Supplier<T> {
    T get();
}

// Ejemplos
Supplier<Double> aleatorio = () -> Math.random();
Supplier<LocalDate> hoy = LocalDate::now;
Supplier<List<String>> listaVacía = ArrayList::new;

// Uso
Double valor = aleatorio.get();  // Obtiene un número aleatorio
List<String> lista = listaVacía.get();  // Crea una nueva lista
```

## 22) ¿Qué es un Consumer?

Un `Consumer<T>` es una interfaz funcional que acepta un argumento de tipo T y no devuelve nada. Utilizado para realizar operaciones o acciones en un objeto.

```java
// Definición
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}

// Ejemplos
Consumer<String> imprimir = System.out::println;
Consumer<List<String>> agregarItem = lista -> lista.add("Nuevo");
Consumer<Integer> procesarYMostrar = n -> {
    int resultado = n * 2;
    System.out.println("El doble es: " + resultado);
};

// Uso
imprimir.accept("Hola mundo");  // Imprime "Hola mundo"
List<String> items = new ArrayList<>();
agregarItem.accept(items);  // Agrega "Nuevo" a la lista
```

## 23) ¿Cuál es la sintaxis para una interfaz Predicate?

Un `Predicate<T>` es una interfaz funcional que acepta un argumento de tipo T y devuelve un boolean. Se usa para filtrar o comprobar condiciones.

```java
// Definición
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}

// Ejemplos
Predicate<String> esVacio = String::isEmpty;
Predicate<Integer> esPar = n -> n % 2 == 0;
Predicate<Persona> esMayor = p -> p.getEdad() >= 18;

// Uso
boolean resultado = esPar.test(4);  // true
List<Integer> pares = numeros.stream()
    .filter(esPar)
    .collect(Collectors.toList());
```

## 24) ¿Qué es la programación funcional?

La programación funcional es un paradigma donde los programas se construyen aplicando y componiendo funciones. Características principales:

- Funciones como "ciudadanos de primera clase"
- Inmutabilidad (evitar cambios de estado)
- Ausencia de efectos secundarios
- Transparencia referencial (mismo resultado para misma entrada)
- Funciones puras (no dependen de estado externo)

Ventajas en Java:
- Código más conciso y declarativo
- Mejor manejo de concurrencia
- Reducción de errores por estado mutable
- Mayor facilidad para pruebas
- Mejor paralelización

```java
// Ejemplo de estilo imperativo
List<Integer> duplicados = new ArrayList<>();
for(Integer n : numeros) {
    if(n % 2 == 0) {
        duplicados.add(n * 2);
    }
}

// Ejemplo de estilo funcional
List<Integer> duplicados = numeros.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 2)
    .collect(Collectors.toList());
```