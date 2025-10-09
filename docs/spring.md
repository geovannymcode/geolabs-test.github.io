# Guía de Entrevista Java y Spring Boot

## Fundamentos de Java

### 1. ¿Qué diferencias hay entre una **interfaz** y una clase **abstracta** en Java?
Una **interfaz** define un contrato que las clases deben implementar, mientras que una clase **abstracta** puede contener implementación parcial. Desde Java 8, las interfaces también pueden tener métodos con implementación (`default`). Sin embargo, una clase solo puede extender una clase abstracta, pero puede implementar múltiples interfaces.

### 2. ¿Qué significa que Java tenga **"pass-by-value"**?
En Java, todo se pasa por valor. Para tipos primitivos se pasa una copia del valor. Para objetos, se pasa una copia de la referencia, lo que significa que puedes modificar el objeto referenciado, pero no cambiar la referencia original.

### 3. ¿Qué diferencia hay entre **==** y **.equals()** en Java?
`==` compara referencias (si apuntan al mismo objeto). `.equals()` compara el contenido del objeto. Por ejemplo, dos String diferentes pueden tener el mismo contenido, pero estar en posiciones distintas de memoria.

### 4. ¿Qué es el **heap** y la **stack** en la memoria de Java?
- **Stack**: Almacena variables locales y llamadas a métodos.
- **Heap**: Almacena objetos creados con `new`.
- El recolector de basura (GC) limpia el heap cuando ya no hay referencias.

### 5. ¿Para qué sirve **final** en **clases**, **métodos** o **variables**?
- **En variables**: Impide su modificación.
- **En métodos**: No se pueden sobrescribir.
- **En clases**: No se pueden heredar.

## Programación Orientada a Objetos (POO)

### 6. ¿Qué es el principio de **inversión de dependencias** (DIP)?
Establece que las clases de alto nivel no deben depender de clases de bajo nivel, sino de abstracciones. También, las abstracciones no deben depender de detalles, sino los detalles de las abstracciones. Se logra mediante interfaces e inyección de dependencias.

### 7. ¿Cómo aplicas la **herencia** y la **composición**? ¿Cuándo usarías cada una?
- **Herencia**: Relación ***"es-un"*** (`Perro extends Animal`).
- **Composición**: Relación ***"tiene-un"*** (`Coche` tiene un `Motor`).
- La composición es más flexible y favorece el bajo acoplamiento.

### 8. ¿Qué es el **polimorfismo** en Java?
Es la capacidad de usar objetos de diferentes clases a través de una referencia común. Por ejemplo, un método que acepta un parámetro de tipo `Animal` puede trabajar con `Perro`, `Gato`, etc.

### 9. ¿Se puede sobrescribir **(override)** un método estático?
No. Los métodos estáticos pertenecen a la clase, no a la instancia. No pueden sobrescribirse, solo ocultarse (hiding).

### 10. ¿Qué es **super** y **this**?

- **this**: Se refiere a la instancia actual.
- **super**: Se usa para acceder a la clase padre (por ejemplo, su constructor o métodos).

## Colecciones y Streams

### 11. ¿Cuál es la diferencia entre **List**, **Set** y **Map**?

- **List**: Mantiene orden, permite duplicados.
- **Set**: No permite duplicados, puede no mantener orden.
- **Map**: Almacena pares clave-valor, no permite claves duplicadas.

### 12. ¿Para qué sirve **Optional** en Java?
Es una forma segura de representar valores que pueden estar presentes o no. Ayuda a evitar `NullPointerException`.

### 13. ¿Qué hace este fragmento de código?
```java
List<String> nombres = List.of("Ana", "Luis", "Carlos");
nombres.stream()
 .filter(n -> n.length() > 3)
 .map(String::toUpperCase)
 .forEach(System.out::println);
```
Filtra los nombres con más de 3 letras, los convierte a mayúsculas y los imprime:
```
LUIS
CARLOS
```

## Manejo de Errores

### 14. ¿Cuál es la diferencia entre **throw** y **throws**?

- **throw**: Lanza una excepción.
- **throws**: Declara que un método puede lanzar una excepción (checked).

### 15. ¿Qué son las excepciones **checked** y **unchecked**?

- **Checked**: Deben capturarse o declararse (ej. `IOException`).
- **Unchecked**: No es obligatorio capturarlas (`NullPointerException`, `IllegalArgumentException`, etc.).

## Archivos, HTTP, JSON y Concurrencia

### 16. ¿Cómo leer y escribir archivos en Java moderno?

- Leer: `Files.readAllLines(path)` o `Files.lines(path)`
- Escribir: `Files.write(path, List<String>)`

### 17. ¿Cómo haces una petición **HTTP** en Java sin usar Spring?
Con HttpClient de Java 11:
```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
 .uri(URI.create("https://api.example.com"))
 .build();
HttpResponse<String> response = client.send(request, BodyHandlers.ofString());
```

### 18. ¿Cómo convertir un objeto a **JSON** y viceversa?

Usando **Jackson** y **ObjectMapper**:
```java
ObjectMapper mapper = new ObjectMapper();
String json = mapper.writeValueAsString(obj);
MiClase nuevo = mapper.readValue(json, MiClase.class);
```

### 19. ¿Cuál es la diferencia entre **synchronized**, **ExecutorService** y **parallelStream()**?

- **synchronized**: Control de acceso exclusivo a secciones críticas.
- **ExecutorService**: Gestión avanzada de hilos y tareas.
- **parallelStream()**: Procesamiento paralelo declarativo.

## Testing y Buenas Prácticas

### 20. ¿Qué tipo de test harías para asegurar que un controlador devuelve correctamente un código 200?

Test de integración con ***MockMvc*** o ***WebTestClient***, validando el ***status***, ***contenido*** y ***cabeceras***.

### 21. ¿Qué patrones de diseño conoces y cuáles has usado en tus proyectos?

#### Patrones Creacionales:

- **Singleton**: Garantiza una única instancia de una clase.
  ```java
  public class ConexionBD {
      private static ConexionBD instancia;
      private ConexionBD() { /* Constructor privado */ }
      
      public static synchronized ConexionBD getInstancia() {
          if (instancia == null) {
              instancia = new ConexionBD();
          }
          return instancia;
      }
  }
  ```
  *Ejemplo real*: Gestor de configuración de aplicación que debe ser único.

- **Factory Method**: Define una interfaz para crear objetos, pero permite a las subclases decidir qué clase instanciar.
 
  ```java
  interface Notificacion { void enviar(String mensaje); }
  class Email implements Notificacion { /*...*/ }
  class SMS implements Notificacion { /*...*/ }
  
  class NotificacionFactory {
      public Notificacion crearNotificacion(String canal) {
          if (canal.equals("email")) return new Email();
          else if (canal.equals("sms")) return new SMS();
          return null;
      }
  }
  ```
  *Ejemplo real*: Sistema de pagos con diferentes procesadores (PayPal, Stripe, etc.)

- **Builder**: Separa la construcción de un objeto complejo de su representación.
  
  ```java
  class Usuario {
      private final String nombre;
      private final int edad;
      // Constructor privado
      private Usuario(Builder builder) {
          this.nombre = builder.nombre;
          this.edad = builder.edad;
      }
      
      public static class Builder {
          private String nombre;
          private int edad;
          
          public Builder nombre(String nombre) {
              this.nombre = nombre;
              return this;
          }
          
          public Builder edad(int edad) {
              this.edad = edad;
              return this;
          }
          
          public Usuario build() {
              return new Usuario(this);
          }
      }
  }
  
  // Uso
  Usuario usuario = new Usuario.Builder()
      .nombre("Juan")
      .edad(30)
      .build();
  ```
  *Ejemplo real*: Construcción de objetos de configuración con muchos parámetros opcionales.

#### Patrones Estructurales:

- **Adapter**: Permite que interfaces incompatibles trabajen juntas.
  ```java
  interface ClienteAPI {
      void procesarDatos(String[] datos);
  }
  
  class SistemaLegado {
      public void procesarInformacion(List<String> info) {
          // Lógica existente
      }
  }
  
  class SistemaLegadoAdapter implements ClienteAPI {
      private SistemaLegado legado;
      
      public SistemaLegadoAdapter(SistemaLegado legado) {
          this.legado = legado;
      }
      
      @Override
      public void procesarDatos(String[] datos) {
          legado.procesarInformacion(Arrays.asList(datos));
      }
  }
  ```
  *Ejemplo real*: Integración con bibliotecas o APIs de terceros con interfaces diferentes.

- **Repository**: Abstrae el acceso a la capa de datos.
  ```java
  interface UsuarioRepository {
      Usuario findById(Long id);
      void save(Usuario usuario);
  }
  
  class UsuarioRepositoryImpl implements UsuarioRepository {
      @Override
      public Usuario findById(Long id) {
          // Lógica de acceso a base de datos
          return usuario;
      }
      
      @Override
      public void save(Usuario usuario) {
          // Persistir en base de datos
      }
  }
  ```
  *Ejemplo real*: Capa de acceso a datos en aplicaciones empresariales.

#### Patrones de Comportamiento:

- **Strategy**: Define una familia de algoritmos, encapsula cada uno y los hace intercambiables.
  ```java
  interface EstrategiaPago {
      void pagar(double monto);
  }
  
  class PagoTarjeta implements EstrategiaPago {
      public void pagar(double monto) { /*...*/ }
  }
  
  class PagoPayPal implements EstrategiaPago {
      public void pagar(double monto) { /*...*/ }
  }
  
  class Compra {
      private EstrategiaPago estrategia;
      
      public void setEstrategiaPago(EstrategiaPago estrategia) {
          this.estrategia = estrategia;
      }
      
      public void realizarPago(double monto) {
          estrategia.pagar(monto);
      }
  }
  ```
  *Ejemplo real*: Diferentes algoritmos de cálculo de impuestos según el país.

- **Observer**: Define una dependencia uno a muchos entre objetos, de modo que cuando un objeto cambia su estado, todos sus dependientes son notificados.
  ```java
  interface Observer {
      void actualizar(String mensaje);
  }
  
  class Suscriptor implements Observer {
      private String nombre;
      
      public Suscriptor(String nombre) {
          this.nombre = nombre;
      }
      
      @Override
      public void actualizar(String mensaje) {
          System.out.println(nombre + " recibió: " + mensaje);
      }
  }
  
  class NotificadorEventos {
      private List<Observer> observadores = new ArrayList<>();
      
      public void agregarObservador(Observer o) {
          observadores.add(o);
      }
      
      public void notificarTodos(String mensaje) {
          for (Observer o : observadores) {
              o.actualizar(mensaje);
          }
      }
  }
  ```
  *Ejemplo real*: Sistema de notificaciones en tiempo real en aplicaciones web.

### 21b. Principios SOLID con ejemplos prácticos

#### S - Principio de Responsabilidad Única (SRP)

Una clase debe tener una sola razón para cambiar.

*Ejemplo*: Un servicio de Facturación no debe ocuparse de la lógica de cálculo de impuestos, envío de correos y persistencia de datos.

```java
// MAL: Clase con múltiples responsabilidades
class Factura {
    public void calcularTotal() { /*...*/ }
    public void guardarEnBD() { /*...*/ }
    public void enviarPorEmail() { /*...*/ }
}

// BIEN: Separación de responsabilidades
class Factura {
    public void calcularTotal() { /*...*/ }
}

class RepositorioFacturas {
    public void guardar(Factura factura) { /*...*/ }
}

class NotificadorFacturas {
    public void enviarPorEmail(Factura factura) { /*...*/ }
}
```

*Ejemplo real*: En una aplicación de e-commerce, separar la lógica de procesamiento de pagos de la lógica de inventario.

#### O - Principio de Abierto/Cerrado (OCP)

Las entidades deben estar abiertas para extensión pero cerradas para modificación.

*Ejemplo*: Añadir nuevos tipos de descuentos sin modificar el cálculo existente.

```java
// Diseño cerrado para extensión
class CalculadorPrecio {
    public double calcularPrecioFinal(Producto p) {
        if (p.esAlimentacion()) return p.getPrecio() * 0.9;
        if (p.esElectronica()) return p.getPrecio() * 0.8;
        return p.getPrecio();
    }
}

// Diseño abierto para extensión
interface EstrategiaDescuento {
    double aplicarDescuento(double precio);
}

class DescuentoAlimentacion implements EstrategiaDescuento {
    public double aplicarDescuento(double precio) { return precio * 0.9; }
}

class DescuentoElectronica implements EstrategiaDescuento {
    public double aplicarDescuento(double precio) { return precio * 0.8; }
}

class CalculadorPrecio {
    public double calcularPrecioFinal(Producto p, EstrategiaDescuento descuento) {
        return descuento.aplicarDescuento(p.getPrecio());
    }
}
```

*Ejemplo real*: Sistema de procesamiento de pagos que permite añadir nuevas pasarelas de pago sin modificar el código existente.

#### L - Principio de Sustitución de Liskov (LSP)

Las subclases deben poder ser sustituidas por sus clases base sin alterar el comportamiento.

*Ejemplo*: Todas las aves pueden volar... ¿o no?

```java
// MAL: Viola LSP
class Ave {
    public void volar() { /*...*/ }
}

class Pinguino extends Ave {
    @Override
    public void volar() {
        throw new UnsupportedOperationException("Los pingüinos no vuelan");
    }
}

// BIEN: Respeta LSP
interface AvesQueVuelan {
    void volar();
}

class Ave {
    // Comportamiento común a todas las aves
}

class Golondrina extends Ave implements AvesQueVuelan {
    public void volar() { /*...*/ }
}

class Pinguino extends Ave {
    // No implementa AvesQueVuelan
}
```

*Ejemplo real*: En un sistema bancario, diferentes tipos de cuentas (Ahorro, Corriente) deben poder usarse de manera intercambiable en operaciones básicas.

#### I - Principio de Segregación de Interfaces (ISP)

Ningún cliente debe verse forzado a depender de métodos que no usa.

*Ejemplo*: Una impresora multifunción tiene diferentes capacidades.

```java
// MAL: Interfaz sobrecargada
interface DispositivoOficina {
    void imprimir();
    void escanear();
    void enviarFax();
    void grapar();
}

// BIEN: Interfaces segregadas
interface Impresora {
    void imprimir();
}

interface Escaner {
    void escanear();
}

interface Fax {
    void enviarFax();
}

class ImpresoraSimple implements Impresora {
    public void imprimir() { /*...*/ }
}

class DispositivoMultifuncion implements Impresora, Escaner, Fax {
    public void imprimir() { /*...*/ }
    public void escanear() { /*...*/ }
    public void enviarFax() { /*...*/ }
}
```

*Ejemplo real*: API de pagos que ofrece interfaces específicas para diferentes funcionalidades: procesamiento de pagos, reembolsos, suscripciones.

#### D - Principio de Inversión de Dependencias (DIP)

Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones.

*Ejemplo*: Un servicio no debe depender directamente de la implementación de la base de datos.

```java
// MAL: Dependencia directa
class ServicioUsuario {
    private RepositorioUsuarioMySQL repo = new RepositorioUsuarioMySQL();
    
    public Usuario buscar(Long id) {
        return repo.findById(id);
    }
}

// BIEN: Dependencia en abstracción
interface RepositorioUsuario {
    Usuario findById(Long id);
}

class RepositorioUsuarioMySQL implements RepositorioUsuario {
    public Usuario findById(Long id) { /*...*/ }
}

class ServicioUsuario {
    private RepositorioUsuario repo;
    
    // Inyección de dependencia
    public ServicioUsuario(RepositorioUsuario repo) {
        this.repo = repo;
    }
    
    public Usuario buscar(Long id) {
        return repo.findById(id);
    }
}
```

*Ejemplo real*: Una aplicación empresarial que abstrae el acceso a datos permitiendo cambiar entre diferentes bases de datos (MySQL, MongoDB, etc.) sin afectar la lógica de negocio.

## Java Moderno

### 22. ¿Qué diferencias hay entre **Array** y **ArrayList**?

- **Array**: Tamaño fijo, puede contener tipos primitivos.
- **ArrayList**: Tamaño dinámico, solo objetos, ofrece métodos como `add()`, `remove()`.

### 23. ¿Qué es **var** en Java?

Desde Java 10, permite inferencia de tipo local. El tipo es estático y se infiere en tiempo de compilación:
```java
var lista = new ArrayList<String>();
```

### 24. ¿Qué es un **enum** y cuándo se usa?

Un enum representa un conjunto fijo de constantes. Útil para modelar categorías o estados como `DIA`, `NOCHE`, `ESTADO.ACTIVO`.

### 25. ¿Qué es un record en Java?

Introducido en Java 14+, define una clase inmutable con mínimo código:
```java
record Usuario(String nombre, int edad) {}
```

### 26. ¿Qué es la inferencia de tipos en lambdas?

Java deduce el tipo de parámetros en funciones lambda automáticamente:
```java
lista.forEach(item -> System.out.println(item));
```

### 27. ¿Para qué se usa el patrón **Builder**?

Para construir objetos complejos paso a paso, evitando constructores con muchos parámetros y mejorando la legibilidad.

## Fundamentos de Spring Boot

### 28. ¿Qué diferencia hay entre **@Component**, **@Service**, **@Repository** y **@Controller**?

Todos son estereotipos de Spring que marcan una clase como un componente a gestionar. `@Component` es genérico, `@Service` se usa para lógica de negocio, `@Repository` para acceso a datos (además traduce excepciones automáticamente), y `@Controller` para controlar las peticiones HTTP.

### 29. ¿Qué función cumple el archivo **application.properties** o **application.yml**?

Permite definir la configuración de la aplicación: puertos, datos de conexión, variables personalizadas, perfiles activos, propiedades de Spring, entre otros.

## Controladores y manejo HTTP

### 30. ¿Qué diferencia hay entre **@GetMapping** y **@RequestMapping(method = RequestMethod.GET)**?

`@GetMapping` es una anotación específica para simplificar `@RequestMapping(method = RequestMethod.GET)`, haciéndolo más legible. Ambas hacen lo mismo, pero `@GetMapping` es más concisa.

### 31. ¿Cómo puedes recibir datos de una petición **POST** en Spring Boot?

Utilizando un DTO o entidad como parámetro con `@RequestBody`. Spring deserializa automáticamente el cuerpo JSON en ese objeto.

## Manejo de errores y excepciones

### 32. ¿Qué es un **@ControllerAdvice**?

Es una clase que maneja excepciones de manera global en todos los controladores. Permite capturar errores comunes y devolver respuestas personalizadas con `@ExceptionHandler`.

### 33. ¿Cómo personalizar la respuesta de **error** para una **excepción** específica?

Se puede crear un método con `@ExceptionHandler(NombreDeLaExcepcion.class)` dentro de una clase anotada con `@ControllerAdvice`, y devolver un objeto con el mensaje, código de estado, etc.

## Asincronía, concurrencia y tareas programadas

### 34. ¿Cómo se ejecuta un método de forma asíncrona en Spring Boot?

Se anota el método con `@Async` y se habilita el soporte con `@EnableAsync` en una clase de configuración. El método debe retornar `CompletableFuture`, `Future` o `void`.

### 35. ¿Qué es **@Scheduled** y cómo se usa?

Permite ejecutar tareas periódicas. Se configura con expresiones cron o parámetros como `fixedRate`. Requiere `@EnableScheduling`.

## Auto configuración y estructura base de Spring Boot

### 36. ¿Qué es la auto configuración en Spring Boot?

Es una funcionalidad que configura automáticamente beans comunes en función de las dependencias del classpath. Esto elimina la necesidad de configurar todo manualmente y se activa con `@SpringBootApplication`.

### 37. ¿Qué hace la anotación @SpringBootApplication?

Es una combinación de `@Configuration`, `@EnableAutoConfiguration` y `@ComponentScan`. Marca el punto de entrada principal de la app y permite que Spring configure todo automáticamente.

### 38. ¿Qué convenciones sigue Spring Boot para buscar **beans**?

Spring escanea automáticamente todos los paquetes hijos del paquete donde se encuentra la clase con `@SpringBootApplication`. Si los beans están fuera de ese árbol de paquetes, no se detectarán salvo que se especifique el `@ComponentScan`.

### 39. ¿Qué diferencia hay entre **@Configuration** y **@Component**?

`@Configuration` define una clase que puede contener métodos con `@Bean`, y está pensada para la configuración de la aplicación. `@Component` es una anotación más genérica para cualquier clase que deba ser gestionada por el contenedor de Spring.

## Spring MVC y desarrollo Web

### 40. ¿Qué hace el **@RestController**?

Es una combinación de `@Controller` y `@ResponseBody`, lo que permite que los métodos devuelvan directamente datos (como JSON) sin necesidad de escribir `@ResponseBody` en cada uno.

### 41. ¿Cómo se mapean rutas en Spring Boot Web?

Usando anotaciones como `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, etc., encima de métodos dentro de un `@RestController`. También se pueden usar `@RequestMapping` con el método definido.

### 42. ¿Cómo se accede a parámetros en la **URL**?

Usando `@PathVariable` para capturar partes de la ruta (`/users/{id}`), o `@RequestParam` para capturar parámetros tipo query (`/search?term=x`).

### 43. ¿Qué diferencia hay entre **@RequestBody** y **@ModelAttribute**?

`@RequestBody` toma el cuerpo crudo de la petición (generalmente JSON) y lo deserializa en un objeto. `@ModelAttribute` toma datos del formulario o query params, útil para formularios en apps web tradicionales.

### 44. ¿Cómo se devuelven respuestas personalizadas en Spring Boot?

Usando objetos `ResponseEntity<T>` que permiten controlar el cuerpo de la respuesta, cabeceras y código de estado HTTP.

## Validaciones en Spring Boot

### 45. ¿Cómo se activan las **validaciones** en un controlador de Spring Boot?

Usando `@Valid` o `@Validated` en el parámetro del método del controlador. Spring valida automáticamente los atributos anotados con `@NotNull`, `@Size`, `@Email`, etc., y lanza una excepción si no se cumplen.

### 46. ¿Cómo se personaliza el **mensaje de error de una validación**?

Se puede usar el atributo message en la anotación, por ejemplo: `@NotNull(message = "El nombre es obligatorio")`. También se pueden usar ficheros `.properties` para externalizar los mensajes.

### 47. ¿Qué diferencia hay entre **@Valid** y **@Validated**?

`@Valid` es de Javax y se usa principalmente en controladores. `@Validated` es de Spring y permite validaciones por grupos o validaciones a nivel de servicio, no solo en controladores.

### 48. ¿Cómo se gestionan los **errores** de validación?

Spring lanza una excepción (`MethodArgumentNotValidException`) que se puede capturar con `@ControllerAdvice` y un método con `@ExceptionHandler`.

## Testing en Spring Boot Web

### 49. ¿Qué hace **@WebMvcTest**?

Carga solo el contexto relacionado con **Spring MVC (controladores, filtros, validaciones)**, ignorando servicios, repositorios y demás beans. Ideal para pruebas unitarias de controladores.

### 50. ¿Qué herramienta se usa para simular peticiones HTTP?

`MockMvc` permite hacer peticiones simuladas a endpoints (`GET`, `POST`, etc.), verificar códigos de estado, cabeceras y contenido del JSON de respuesta.

### 51. ¿Qué diferencia hay entre **@SpringBootTest** y **@WebMvcTest**?

`@SpringBootTest` carga todo el contexto de Spring (ideal para tests de integración). `@WebMvcTest` solo carga el contexto web para tests de controladores.

## Mapeo entre capas (DTOs, entidades y modelos)

### 52. ¿Qué es un **DTO** y por qué se usa?

Un **DTO** ***(Data Transfer Object)*** es un objeto diseñado para intercambiar datos entre capas, evitando exponer entidades directamente. Mejora la seguridad y desacopla las capas.

### 53. ¿Cómo se realiza el mapeo entre **entidad** y **DTO**?

Se puede hacer manualmente en un mapper (por ejemplo, un método `fromEntity()` y `toEntity()`), o automáticamente usando librerías como MapStruct o ModelMapper.

### 54. ¿Qué ventajas ofrece **MapStruct**?

**MapStruct** genera código en tiempo de compilación, es rápido, seguro y fácil de mantener. Reduce el boilerplate y errores comunes en el mapeo manual.

### 55. ¿Por qué no se deben exponer directamente las entidades JPA en los controladores?

Porque puede generar problemas de seguridad, acoplamiento, rendimiento (lazy loading), y dificulta la evolución del modelo de dominio.

## Arquitectura y diseño en Spring Boot Web

### 56. ¿Cuál es la estructura más común en una app Spring Boot?

Una estructura por capas: `controller`, `service`, `repository`, `model` y `dto`. A veces se usa una estructura más vertical por funcionalidades.

### 57. ¿Qué beneficios aporta la arquitectura en capas?

Separación de responsabilidades, mejor mantenibilidad, testabilidad y facilidad para aplicar principios SOLID.

### 58. ¿Qué es el **@Service** y qué hace?

Es una anotación de Spring que marca una clase como componente de negocio. Permite separar la lógica de negocio del controlador.