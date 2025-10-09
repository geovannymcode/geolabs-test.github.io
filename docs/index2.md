# 💼 SIMULACRO ENTREVISTA JAVA - EXTENDIDO

## 🔹 Sección 1: Fundamentos de POO

### ❓ Pregunta 1:

¿Qué es programación orientada a objetos?

### ✅ Respuesta:

La Programación Orientada a Objetos (POO) es un paradigma de programación que organiza el código en objetos que representan entidades del mundo real. Estos objetos contienen datos (atributos) y comportamientos (métodos). La POO permite crear software más modular, reutilizable y mantenible mediante conceptos como encapsulamiento, herencia, polimorfismo y abstracción.

---

### ❓ Pregunta 2:

¿Qué es una clase y qué es un objeto? ¿Cuál es la diferencia?

### ✅ Respuesta:

- **Clase**: Es un molde o plantilla que define la estructura y comportamiento de los objetos. Contiene atributos y métodos.
- **Objeto**: Es una instancia concreta de una clase. Es la materialización de la clase en memoria.

**Diferencia**: La clase es el diseño abstracto, el objeto es la realidad concreta. Por ejemplo, `Coche` es una clase, mientras que `miCoche = new Coche()` es un objeto específico.

```java
// Clase
class Coche {
    String marca;
    void acelerar() { }
}

// Objeto
Coche miCoche = new Coche();
```

---

### ❓ Pregunta 3:

¿Cuáles son los 4 pilares de la programación orientada a objetos?

### ✅ Respuesta:

**1. Herencia**: Permite que una clase herede atributos y métodos de otra clase padre.
```java
class Animal {
    void comer() { System.out.println("Comiendo..."); }
}

class Perro extends Animal {
    void ladrar() { System.out.println("Guau!"); }
}
```

**2. Polimorfismo**: Capacidad de un objeto de tomar múltiples formas. Un mismo método puede comportarse diferente según el objeto.
```java
class Animal {
    void hacerSonido() { }
}

class Perro extends Animal {
    void hacerSonido() { System.out.println("Guau"); }
}

class Gato extends Animal {
    void hacerSonido() { System.out.println("Miau"); }
}

Animal miAnimal = new Perro();
miAnimal.hacerSonido(); // Imprime "Guau"
```

**3. Abstracción**: Oculta los detalles complejos y muestra solo lo esencial.
```java
abstract class FiguraGeometrica {
    abstract double calcularArea();
}

class Circulo extends FiguraGeometrica {
    double radio;
    double calcularArea() {
        return Math.PI * radio * radio;
    }
}
```

**4. Encapsulamiento**: Oculta los datos internos y solo permite el acceso mediante métodos públicos (getters/setters).
```java
class CuentaBancaria {
    private double saldo;
    
    public double getSaldo() { return saldo; }
    public void depositar(double monto) { 
        if(monto > 0) saldo += monto; 
    }
}
```

---

## 🔹 Sección 2: Modificadores y Palabras Reservadas

### ❓ Pregunta 4:

¿Cuáles son los modificadores de acceso en Java?

### ✅ Respuesta:

- **`public`**: Accesible desde cualquier clase.
- **`private`**: Solo accesible dentro de la misma clase.
- **`protected`**: Accesible en el mismo paquete y en clases hijas.
- **`default` (sin modificador)**: Accesible solo en el mismo paquete.
- **`static`**: Pertenece a la clase, no a instancias específicas.

```java
public class Ejemplo {
    public int publico;        // Acceso desde cualquier lugar
    private int privado;       // Solo dentro de esta clase
    protected int protegido;   // Mismo paquete y subclases
    int porDefecto;           // Solo mismo paquete
    static int estatico;      // Pertenece a la clase
}
```

---

### ❓ Pregunta 5:

¿Cuáles son algunas palabras reservadas importantes de Java?

### ✅ Respuesta:

- **`null`**: Representa una referencia nula (sin objeto).
- **`char`**: Tipo de dato para caracteres.
- **`final`**: Impide modificación de variables, sobrescritura de métodos o herencia de clases.
- **`extends`**: Indica herencia de clases.
- **`implements`**: Implementa interfaces.
- **`try`**: Bloque para manejo de excepciones.
- **`void`**: Indica que un método no retorna valor.
- **`throw`**: Lanza una excepción manualmente.

```java
public final class Constantes {
    public static final int MAX = 100;
}

try {
    // código
} catch (Exception e) {
    throw new RuntimeException("Error");
}
```

---

## 🔹 Sección 3: Programación Funcional

### ❓ Pregunta 6:

¿Qué es programación funcional y qué son las funciones lambda?

### ✅ Respuesta:

**Programación Funcional**: Es un paradigma que trata la computación como evaluación de funciones matemáticas, evitando estados mutables y efectos secundarios.

**Funciones Lambda**: Son funciones anónimas (sin nombre) que permiten escribir código más conciso. Introducidas en Java 8.

```java
// Lambda simple
(parametros) -> expresion

// Ejemplo 1: Sin parámetros
() -> System.out.println("Hola")

// Ejemplo 2: Un parámetro
x -> x * x

// Ejemplo 3: Múltiples parámetros
(a, b) -> a + b

// Ejemplo 4: Con bloque
(x) -> {
    int resultado = x * 2;
    return resultado;
}
```

**Stream API**: Permite procesar colecciones de forma funcional.

```java
List<Integer> numeros = Arrays.asList(1, 2, 3, 4, 5);

// Filtrar números pares y multiplicar por 2
List<Integer> resultado = numeros.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 2)
    .collect(Collectors.toList());

// Suma de todos los elementos
int suma = numeros.stream()
    .reduce(0, (a, b) -> a + b);
```

---

## 🔹 Sección 4: Archivos, Hilos y Excepciones

### ❓ Pregunta 7:

¿Qué librerías conoces para consumo de archivos?

### ✅ Respuesta:

- **`java.nio.file` (Files, Paths)**: API moderna para manejo de archivos.
- **`java.io` (FileReader, BufferedReader, FileWriter)**: API clásica.
- **Apache Commons IO**: Librería externa con utilidades adicionales.
- **Jackson / Gson**: Para archivos JSON.
- **Apache POI**: Para archivos Excel.

```java
// Leer archivo con java.nio
List<String> lineas = Files.readAllLines(Paths.get("archivo.txt"));

// Escribir archivo
Files.write(Paths.get("salida.txt"), lineas);

// Leer archivo grande línea por línea
try (Stream<String> stream = Files.lines(Paths.get("archivo.txt"))) {
    stream.forEach(System.out::println);
}
```

---

### ❓ Pregunta 8:

¿Ha trabajado con hilos? Explique qué son hilos y su diferencia con procesos.

### ✅ Respuesta:

**Hilo (Thread)**: Es la unidad más pequeña de ejecución dentro de un proceso. Permite ejecutar múltiples tareas concurrentemente dentro de una aplicación.

**Proceso**: Es una instancia de un programa en ejecución con su propio espacio de memoria.

**Diferencias**:

- Los hilos comparten memoria dentro del mismo proceso, los procesos tienen memoria separada.
- Los hilos son más ligeros y rápidos de crear.
- La comunicación entre hilos es más rápida que entre procesos.

```java
// Crear hilo extendiendo Thread
class MiHilo extends Thread {
    public void run() {
        System.out.println("Ejecutando en hilo: " + getName());
    }
}

// Crear hilo implementando Runnable
class MiTarea implements Runnable {
    public void run() {
        System.out.println("Ejecutando tarea");
    }
}

// Uso
MiHilo hilo1 = new MiHilo();
hilo1.start();

Thread hilo2 = new Thread(new MiTarea());
hilo2.start();

// Con lambda
Thread hilo3 = new Thread(() -> {
    System.out.println("Hilo con lambda");
});
hilo3.start();
```

---

### ❓ Pregunta 9:

¿Qué excepciones conoces en Java?

### ✅ Respuesta:

**Excepciones comunes**:

- **`NullPointerException`**: Se accede a un objeto que es `null`.
- **`ArithmeticException`**: Error aritmético como división por cero.
- **`IllegalAccessException`**: Intento de acceder a un miembro privado.
- **`JPAException`**: Errores relacionados con persistencia JPA.
- **`IOException`**: Errores de entrada/salida.
- **`SQLException`**: Errores en operaciones de base de datos.
- **`NumberFormatException`**: Error al convertir String a número.

```java
try {
    String texto = null;
    texto.length(); // NullPointerException
    
    int division = 10 / 0; // ArithmeticException
    
    int numero = Integer.parseInt("abc"); // NumberFormatException
    
} catch (NullPointerException e) {
    System.out.println("Objeto nulo detectado");
} catch (ArithmeticException e) {
    System.out.println("Error en operación matemática");
} catch (Exception e) {
    System.out.println("Error general: " + e.getMessage());
} finally {
    System.out.println("Siempre se ejecuta");
}
```

---

## 🔹 Sección 5: Servicios REST y Spring

### ❓ Pregunta 10:

¿Qué son servicios REST?

### ✅ Respuesta:

**REST** (Representational State Transfer) es un estilo arquitectónico para servicios web que permite acceder a recursos mediante HTTP. Utiliza los métodos estándar:

- **GET**: Obtener recursos
- **POST**: Crear recursos
- **PUT**: Actualizar recursos completos
- **PATCH**: Actualizar recursos parcialmente
- **DELETE**: Eliminar recursos

```java
@RestController
@RequestMapping("/api/usuarios")
public class UsuarioController {
    
    @GetMapping
    public List<Usuario> obtenerTodos() { }
    
    @GetMapping("/{id}")
    public Usuario obtenerPorId(@PathVariable Long id) { }
    
    @PostMapping
    public Usuario crear(@RequestBody Usuario usuario) { }
    
    @PutMapping("/{id}")
    public Usuario actualizar(@PathVariable Long id, @RequestBody Usuario usuario) { }
    
    @DeleteMapping("/{id}")
    public void eliminar(@PathVariable Long id) { }
}
```

---

### ❓ Pregunta 11:

¿Qué códigos HTTP generan los servicios como respuesta?

### ✅ Respuesta:

**Códigos 2xx (Éxito)**:

- **200 OK**: Solicitud exitosa
- **201 Created**: Recurso creado exitosamente
- **204 No Content**: Éxito sin contenido de respuesta

**Códigos 3xx (Redirección)**:

- **301 Moved Permanently**: Recurso movido permanentemente
- **304 Not Modified**: Recurso no modificado desde última solicitud

**Códigos 4xx (Error del cliente)**:

- **400 Bad Request**: Solicitud mal formada
- **401 Unauthorized**: No autenticado
- **403 Forbidden**: No autorizado
- **404 Not Found**: Recurso no encontrado
- **409 Conflict**: Conflicto con el estado actual

**Códigos 5xx (Error del servidor)**:

- **500 Internal Server Error**: Error interno del servidor
- **502 Bad Gateway**: Error en gateway
- **503 Service Unavailable**: Servicio no disponible

---

### ❓ Pregunta 12:

¿Qué es Spring y qué es Spring Boot?

### ✅ Respuesta:

**Spring**: Es un framework de Java para desarrollo de aplicaciones empresariales. Proporciona inversión de control (IoC) e inyección de dependencias.

**Spring Boot**: Es una extensión de Spring que simplifica la configuración y el desarrollo. Permite crear aplicaciones stand-alone con configuración mínima, servidor embebido y dependencias auto-configuradas.

**Diferencias**:

- Spring requiere mucha configuración XML o Java.
- Spring Boot tiene configuración automática y opiniones por defecto.
- Spring Boot incluye servidor embebido (Tomcat, Jetty).
- Spring Boot facilita el arranque rápido de proyectos.

---

### ❓ Pregunta 13:

¿Para qué sirven estas anotaciones básicas de Spring?

### ✅ Respuesta:

- **`@Entity`**: Marca una clase como entidad JPA para mapeo con base de datos.

- **`@Repository`**: Indica que la clase es un repositorio (capa de acceso a datos). Maneja excepciones de persistencia.

- **`@Service`**: Marca la clase como servicio (lógica de negocio).

- **`@Controller`**: Define un controlador MVC que retorna vistas.

- **`@RestController`**: Combinación de `@Controller` + `@ResponseBody`. Retorna datos en JSON/XML directamente.

**Diferencia @Controller vs @RestController**:
```java
@Controller
public class VistaController {
    @GetMapping("/pagina")
    public String mostrarPagina() {
        return "vista"; // Retorna nombre de vista HTML
    }
}

@RestController
public class ApiController {
    @GetMapping("/datos")
    public Usuario obtenerDatos() {
        return new Usuario(); // Retorna JSON directamente
    }
}
```

- **`@RequestMapping`**: Define la ruta base del controlador o método específico.

```java
@RestController
@RequestMapping("/api/productos")
public class ProductoController {
    
    @GetMapping // GET /api/productos
    public List<Producto> listar() { }
    
    @PostMapping // POST /api/productos
    public Producto crear(@RequestBody Producto producto) { }
}
```

---

### ❓ Pregunta 14:

¿Para qué sirven las palabras reservadas `implements` y `extends`?

### ✅ Respuesta:

- **`extends`**: Se usa para herencia de clases. Una clase puede extender solo una clase padre.

- **`implements`**: Se usa para implementar interfaces. Una clase puede implementar múltiples interfaces.

```java
// extends - Herencia
class Animal {
    void comer() { }
}

class Perro extends Animal {
    void ladrar() { }
}

// implements - Implementación de interfaces
interface Volador {
    void volar();
}

interface Nadador {
    void nadar();
}

class Pato implements Volador, Nadador {
    public void volar() { }
    public void nadar() { }
}

// Combinación
class PatoDomestico extends Ave implements Volador, Nadador {
    // Hereda de Ave e implementa dos interfaces
}
```

---

### ❓ Pregunta 15:

¿Qué diferencia hay entre `@Component` y `@Bean`?

### ✅ Respuesta:

- **`@Component`**: Se coloca sobre una clase para que Spring la detecte automáticamente y la registre como bean. Es un estereotipo genérico.

- **`@Bean`**: Se coloca sobre un método en una clase de configuración (`@Configuration`) para registrar manualmente el objeto que retorna como bean.

**Uso**:
```java
// @Component - Auto-detección
@Component
public class MiServicio {
    public void hacer() { }
}

// @Bean - Configuración manual
@Configuration
public class AppConfig {
    
    @Bean
    public MiServicio miServicio() {
        return new MiServicio();
    }
    
    @Bean
    public ObjectMapper objectMapper() {
        return new ObjectMapper(); // Para librerías externas
    }
}
```

**Cuándo usar cada uno**:

- `@Component`: Para clases propias donde puedes modificar el código.
- `@Bean`: Para librerías externas o cuando necesitas configuración especial.

---

## 🔹 Sección 6: Seguridad

### ❓ Pregunta 16:

¿Qué es JWT (JSON Web Token) y cómo se implementa?

### ✅ Respuesta:

**JWT**: Es un estándar para transmitir información de forma segura entre partes como un objeto JSON. Se usa principalmente para autenticación y autorización.

**Estructura (xxx.yyy.zzz)**:

- **Header**: Tipo de token y algoritmo de encriptación.
- **Payload**: Datos del usuario (claims).
- **Signature**: Firma para verificar integridad.

```java
// Generar JWT
String jwt = Jwts.builder()
    .setSubject(usuario.getUsername())
    .setIssuedAt(new Date())
    .setExpiration(new Date(System.currentTimeMillis() + 86400000))
    .signWith(SignatureAlgorithm.HS512, "secretKey")
    .compact();

// Validar JWT
Claims claims = Jwts.parser()
    .setSigningKey("secretKey")
    .parseClaimsJws(jwt)
    .getBody();

String username = claims.getSubject();
```

**Flujo de implementación**:

1. Usuario se autentica con credenciales.
2. Servidor genera JWT y lo envía al cliente.
3. Cliente incluye JWT en cada petición (header Authorization).
4. Servidor valida JWT antes de procesar la petición.

---

### ❓ Pregunta 17:

¿Qué es OAuth y OAuth2? ¿Cuáles son sus diferencias?

### ✅ Respuesta:

**OAuth**: Protocolo de autorización que permite a aplicaciones obtener acceso limitado a cuentas de usuario sin exponer contraseñas.

**OAuth2**: Versión mejorada y más simple de OAuth. Es el estándar actual.

**Diferencias OAuth vs OAuth2**:

- OAuth2 es más simple de implementar.
- OAuth2 tiene mejor soporte para aplicaciones móviles.
- OAuth2 separa roles (Resource Owner, Client, Authorization Server, Resource Server).
- OAuth tiene firmas criptográficas complejas; OAuth2 usa HTTPS.

**Flujo OAuth2**:

1. Usuario solicita acceso a recurso.
2. Aplicación redirige a servidor de autorización.
3. Usuario se autentica y otorga permisos.
4. Servidor retorna código de autorización.
5. Aplicación intercambia código por access token.
6. Aplicación usa token para acceder a recursos protegidos.

---

### ❓ Pregunta 18:

¿Has consumido servicios REST/SOAP? ¿De qué formas lo haces?

### ✅ Respuesta:

**REST (JSON)**:
```java
// Con RestTemplate (Spring)
RestTemplate restTemplate = new RestTemplate();
Usuario usuario = restTemplate.getForObject("http://api.com/usuarios/1", Usuario.class);

// Con WebClient (Spring WebFlux) - Reactivo
WebClient client = WebClient.create("http://api.com");
Usuario usuario = client.get()
    .uri("/usuarios/1")
    .retrieve()
    .bodyToMono(Usuario.class)
    .block();

// Con HttpClient (Java 11+)
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("http://api.com/usuarios"))
    .GET()
    .build();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
```

**SOAP (WSDL/XML)**:
```java
// Usando clientes generados desde WSDL
URL wsdlUrl = new URL("http://servidor.com/servicio?wsdl");
QName serviceName = new QName("http://ejemplo.com/", "MiServicio");
Service service = Service.create(wsdlUrl, serviceName);
MiServicioPort port = service.getPort(MiServicioPort.class);
Respuesta resultado = port.miMetodo(parametros);
```

---

### ❓ Pregunta 19:

¿Qué es inyección de dependencias y cómo se implementa en Spring?

### ✅ Respuesta:

**Inyección de Dependencias (DI)**: Es un patrón de diseño donde los objetos reciben sus dependencias desde el exterior en lugar de crearlas ellos mismos. Promueve bajo acoplamiento y facilita testing.

**Formas de inyección en Spring**:

```java
@Service
public class UsuarioService {
    
    // 1. Inyección por constructor (recomendada)
    private final UsuarioRepository repository;
    
    @Autowired // Opcional desde Spring 4.3
    public UsuarioService(UsuarioRepository repository) {
        this.repository = repository;
    }
    
    // 2. Inyección por setter
    private EmailService emailService;
    
    @Autowired
    public void setEmailService(EmailService emailService) {
        this.emailService = emailService;
    }
    
    // 3. Inyección por campo (no recomendada)
    @Autowired
    private NotificacionService notificacionService;
}
```

**Ventajas**:

- Facilita pruebas unitarias (puedes inyectar mocks).
- Reduce acoplamiento entre clases.
- Mayor flexibilidad y mantenibilidad.

---

## 🔹 Sección 7: SOLID y Arquitecturas

### ❓ Pregunta 20:

**PREGUNTA CLAVE**: ¿Qué es SOLID y cómo se implementa?

### ✅ Respuesta:

**SOLID** son 5 principios de diseño orientado a objetos para crear software mantenible y escalable:

**S - Single Responsibility Principle (Responsabilidad Única)**
Cada clase debe tener una única razón para cambiar.
```java
// ❌ Mal
class Usuario {
    void guardar() { } // Persistencia
    void enviarEmail() { } // Notificación
}

// ✅ Bien
class Usuario { }
class UsuarioRepository {
    void guardar(Usuario u) { }
}
class EmailService {
    void enviar(String email) { }
}
```

**O - Open/Closed Principle (Abierto/Cerrado)**
Abierto para extensión, cerrado para modificación.
```java
// ✅ Extensible sin modificar
interface FormaPago {
    void pagar(double monto);
}

class PagoTarjeta implements FormaPago {
    public void pagar(double monto) { }
}

class PagoPayPal implements FormaPago {
    public void pagar(double monto) { }
}
```

**L - Liskov Substitution Principle (Sustitución de Liskov)**
Las subclases deben poder sustituir a sus clases base sin alterar el funcionamiento.
```java
class Ave {
    void mover() { }
}

class Aguila extends Ave {
    void mover() { /* volar */ }
}

class Pinguino extends Ave {
    void mover() { /* nadar/caminar */ }
}
```

**I - Interface Segregation Principle (Segregación de Interfaces)**
Es mejor tener varias interfaces específicas que una general.
```java
// ❌ Mal
interface Trabajador {
    void trabajar();
    void comer();
    void dormir();
}

// ✅ Bien
interface Trabajable {
    void trabajar();
}

interface Alimentable {
    void comer();
}
```

**D - Dependency Inversion Principle (Inversión de Dependencias)**
Depender de abstracciones, no de implementaciones concretas.
```java
// ✅ Depende de interfaz, no de clase concreta
interface NotificacionService {
    void enviar(String mensaje);
}

class EmailNotificacion implements NotificacionService {
    public void enviar(String mensaje) { }
}

class UsuarioController {
    private NotificacionService servicio;
    
    public UsuarioController(NotificacionService servicio) {
        this.servicio = servicio;
    }
}
```

---

### ❓ Pregunta 21:

¿Qué diferencias hay entre arquitectura monolítica y microservicios?

### ✅ Respuesta:

**Arquitectura Monolítica**:

- Aplicación única y completa.
- Todo está en un mismo código base.
- Se despliega como una unidad.

**Ventajas**:

- Simple de desarrollar y desplegar.
- Fácil de testear.
- Menos complejidad operacional.

**Desventajas**:

- Difícil de escalar.
- Acoplamiento fuerte.
- Despliegues arriesgados (todo o nada).
- Tecnología única.

**Microservicios**:

- Aplicación dividida en servicios pequeños e independientes.
- Cada servicio se despliega independientemente.
- Comunicación mediante APIs.

**Ventajas**:

- Escalabilidad independiente.
- Bajo acoplamiento.
- Flexibilidad tecnológica.
- Equipos independientes.
- Despliegues más seguros.

**Desventajas**:

- Mayor complejidad operacional.
- Requiere DevOps maduro.
- Debugging más complejo.
- Latencia de red.

---

### ❓ Pregunta 22:

¿Qué es arquitectura hexagonal?

### ✅ Respuesta:

**Arquitectura Hexagonal** (Ports and Adapters): Patrón que separa la lógica de negocio de los detalles de infraestructura (BD, APIs, UI).

**Capas**:
- **Dominio (Centro)**: Lógica de negocio pura, sin dependencias externas.
- **Puertos**: Interfaces que definen contratos.
- **Adaptadores**: Implementaciones concretas (BD, REST, etc.).

```
Estructura:
├── domain/
│   ├── model/
│   │   └── Usuario.java
│   ├── port/
│   │   ├── input/
│   │   │   └── CrearUsuarioUseCase.java
│   │   └── output/
│   │       └── UsuarioRepository.java
│   └── service/
│       └── UsuarioService.java
└── adapter/
    ├── input/
    │   └── rest/
    │       └── UsuarioController.java
    └── output/
        └── persistence/
            └── UsuarioJpaAdapter.java
```

**Ventajas**:

- Lógica de negocio independiente.
- Fácil de testear.
- Cambiar tecnologías sin afectar el core.

---

### ❓ Pregunta 23:
¿Qué es ACID?

### ✅ Respuesta:

**ACID** son propiedades que garantizan la confiabilidad de las transacciones en bases de datos:

- **A - Atomicity (Atomicidad)**: La transacción se completa totalmente o no se completa en absoluto (todo o nada).

- **C - Consistency (Consistencia)**: La transacción lleva la base de datos de un estado válido a otro estado válido.

- **I - Isolation (Aislamiento)**: Las transacciones concurrentes no interfieren entre sí.

- **D - Durability (Durabilidad)**: Una vez confirmada, la transacción persiste incluso ante fallos del sistema.

```java
@Transactional
public void transferir(Long origen, Long destino, double monto) {
    Cuenta cuentaOrigen = cuentaRepo.findById(origen);
    Cuenta cuentaDestino = cuentaRepo.findById(destino);
    
    cuentaOrigen.debitar(monto);    // Si falla aquí
    cuentaDestino.acreditar(monto); // y no ejecuta esto
    // Toda la operación se revierte (rollback)
}
```

---

## 🔹 Sección 8: Patrones de Diseño

### ❓ Pregunta 24:
¿Qué es alta cohesión y bajo acoplamiento?

### ✅ Respuesta:

**Alta Cohesión**: Los elementos dentro de un módulo están fuertemente relacionados y tienen un propósito bien definido.

**Bajo Acoplamiento**: Los módulos tienen poca dependencia entre sí. Los cambios en uno no afectan mucho a otros.

```java
// ❌ Baja cohesión (clase hace muchas cosas no relacionadas)
class UsuarioManager {
    void guardarUsuario() { }
    void enviarEmail() { }
    void generarReporte() { }
}

// ✅ Alta cohesión
class UsuarioRepository {
    void guardar() { }
    void buscar() { }
}

class EmailService {
    void enviar() { }
}

// ❌ Alto acoplamiento
class Pedido {
    MySQLDatabase db = new MySQLDatabase(); // Depende de implementación concreta
}

// ✅ Bajo acoplamiento
class Pedido {
    private Database db; // Depende de interfaz
    public Pedido(Database db) {
        this.db = db;
    }
}
```

---

### ❓ Pregunta 25:
¿Conoces algún patrón de diseño? Explica DTO, Adapter, Factory, Builder y Abstract Factory.

### ✅ Respuesta:

**Patrones Creacionales**: Cómo se crean objetos.

**1. Factory (Fábrica)**
Crea objetos sin exponer la lógica de creación.
```java
interface Vehiculo {
    void conducir();
}

class Coche implements Vehiculo {
    public void conducir() { }
}

class Moto implements Vehiculo {
    public void conducir() { }
}

class VehiculoFactory {
    public static Vehiculo crear(String tipo) {
        if (tipo.equals("coche")) return new Coche();
        if (tipo.equals("moto")) return new Moto();
        return null;
    }
}
```

**2. Abstract Factory (Fábrica Abstracta)**
Crea familias de objetos relacionados.
```java
interface FabricaMuebles {
    Silla crearSilla();
    Mesa crearMesa();
}

class FabricaModerna implements FabricaMuebles {
    public Silla crearSilla() { return new SillaModerna(); }
    public Mesa crearMesa() { return new MesaModerna(); }
}
```

**3. Builder (Constructor)**
Construye objetos complejos paso a paso.
```java
class Usuario {
    private String nombre;
    private String email;
    private int edad;
    
    private Usuario(Builder builder) {
        this.nombre = builder.nombre;
        this.email = builder.email;
        this.edad = builder.edad;
    }
    
    static class Builder {
        private String nombre;
        private String email;
        private int edad;
        
        public Builder nombre(String nombre) {
            this.nombre = nombre;
            return this;
        }
        
        public Builder email(String email) {
            this.email = email;
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
    .email("juan@mail.com")
    .edad(25)
    .build();
```

**Patrones Estructurales**: Cómo se organizan las clases.

**4. Adapter (Adaptador)**
Permite que interfaces incompatibles trabajen juntas.
```java
// Interfaz existente
interface MediaPlayer {
    void play(String tipo, String archivo);
}

// Interfaz avanzada incompatible
interface AdvancedPlayer {
    void playVlc(String archivo);
    void playMp4(String archivo);
}

// Adapter
class MediaAdapter implements MediaPlayer {
    private AdvancedPlayer advancedPlayer;
    
    public MediaAdapter(String tipo) {
        if (tipo.equals("vlc")) {
            advancedPlayer = new VlcPlayer();
        } else if (tipo.equals("mp4")) {
            advancedPlayer = new Mp4Player();
        }
    }
    
    public void play(String tipo, String archivo) {
        if (tipo.equals("vlc")) {
            advancedPlayer.playVlc(archivo);
        } else if (tipo.equals("mp4")) {
            advancedPlayer.playMp4(archivo);
        }
    }
}
```

**Patrones de Funcionalidad/Comportamiento**:

**5. DTO (Data Transfer Object)**
Objeto simple para transferir datos entre capas.
```java
public class UsuarioDTO {
    private String nombre;
    private String email;
    
    // Getters y setters
}

// Uso en controlador
@PostMapping("/usuarios")
public ResponseEntity<UsuarioDTO> crear(@RequestBody UsuarioDTO dto) {
    Usuario usuario = mapper.toEntity(dto);
    repository.save(usuario);
    return ResponseEntity.ok(mapper.toDTO(usuario));
}
```

---

### ❓ Pregunta 26:
¿Conoces patrones de arquitectura de microservicios?

### ✅ Respuesta:

**1. Retry (Reintentos)**
Reintenta operaciones fallidas automáticamente.
```java
@Retryable(
    value = {ServiceException.class},
    maxAttempts = 3,
    backoff = @Backoff(delay = 1000)
)
public String llamarServicio() {
    // Si falla, reintenta 3 veces con 1 segundo de espera
}
```

**2. Circuit Breaker (Cortocircuito)**
Previene llamadas a servicios que fallan constantemente.
```java
@CircuitBreaker(name = "servicioExterno", fallbackMethod = "fallback")
public String obtenerDatos() {
    return restTemplate.getForObject("http://servicio-externo/api", String.class);
}

public String fallback(Exception e) {
    return "Datos en caché o respuesta por defecto";
}

// Estados: Cerrado (normal) -> Abierto (falla) -> Semi-abierto (prueba)
```

**3. API Gateway**
Punto de entrada único para todos los microservicios.
- Enrutamiento de peticiones.
- Autenticación y autorización.
- Rate limiting.
- Transformación de respuestas.

**4. Patrón Saga**
Gestiona transacciones distribuidas entre microservicios.

**a) Saga Orquestado**: Un orquestador central coordina las transacciones.
```
Orquestador
    ├──> Servicio Pedido (crear)
    ├──> Servicio Inventario (reservar)
    ├──> Servicio Pago (cobrar)
    └──> Servicio Envío (programar)
    
Si falla alguno, ejecuta compensaciones en orden inverso
```

**b) Saga Coreografeado**: Cada servicio publica eventos y otros reaccionan.
```
Pedido creado (evento)
    ├──> Inventario escucha y reserva
    │    └──> Inventario reservado (evento)
    │         ├──> Pago escucha y procesa
    │         │    └──> Pago exitoso (evento)
    │         │         └──> Envío escucha y programa
    │         └──> Si falla, publica evento de compensación
```

---

### ❓ Pregunta 27:
¿Conoces Kafka o RabbitMQ?

### ✅ Respuesta:

Ambos son sistemas de **mensajería de colas** para comunicación asíncrona entre servicios.

**RabbitMQ**:
- Message broker tradicional.
- Protocolo AMQP.
- Garantiza orden y entrega de mensajes.
- Mejor para sistemas transaccionales.

**Apache Kafka**:
- Plataforma de streaming distribuido.
- Alta throughput (millones de mensajes/segundo).
- Almacena mensajes en disco (log distribuido).
- Mejor para event sourcing y big data.

```java
// Kafka Producer (Spring)
@Service
public class KafkaProducer {
    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;
    
    public void enviarMensaje(String topic, String mensaje) {
        kafkaTemplate.send(topic, mensaje);
    }
}

// Kafka Consumer
@KafkaListener(topics = "mi-topic", groupId = "mi-grupo")
public void consumir(String mensaje) {
    System.out.println("Recibido: " + mensaje);
}
```

**Casos de uso**:
- Desacoplamiento de servicios.
- Procesamiento asíncrono.
- Event-driven architecture.
- Logs y métricas centralizados.

---

### ❓ Pregunta 28:
¿Cómo controlan la concurrencia en Java?

### ✅ Respuesta:

**1. Synchronized**
```java
private int contador = 0;

public synchronized void incrementar() {
    contador++;
}

// O sincronizar un bloque
public void metodo() {
    synchronized(this) {
        // código crítico
    }
}
```

**2. Lock (ReentrantLock)**
```java
private final Lock lock = new ReentrantLock();

public void metodo() {
    lock.lock();
    try {
        // código crítico
    } finally {
        lock.unlock();
    }
}
```

**3. Atomic Variables**
```java
private AtomicInteger contador = new AtomicInteger(0);

public void incrementar() {
    contador.incrementAndGet();
}
```

**4. ThreadLocal**
```java
private ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);

public void usar() {
    threadLocal.set(10);
    int valor = threadLocal.get();
}
```

**5. CompletableFuture (Programación asíncrona)**
```java
CompletableFuture.supplyAsync(() -> {
    return "Resultado";
}).thenApply(resultado -> {
    return resultado + " procesado";
}).thenAccept(System.out::println);
```

---

## 🔹 Sección 9: DevOps e Infraestructura

### ❓ Pregunta 29:
¿Sabes qué es CI/CD? ¿Cómo se implementa?

### ✅ Respuesta:

**CI/CD**: Continuous Integration / Continuous Deployment

**CI (Integración Continua)**:
- Integrar código frecuentemente al repositorio.
- Ejecutar tests automáticamente.
- Detectar errores rápidamente.

**CD (Despliegue Continuo)**:
- Desplegar automáticamente a producción.
- Cada cambio que pasa tests se despliega.

**Implementación con pipelines**:
```yaml
# Ejemplo Azure DevOps Pipeline
trigger:
  branches:
    include:
      - develop
      - stage
      - master

stages:
  - stage: Build
    jobs:
      - job: Compile
        steps:
          - task: Maven@3
            inputs:
              goals: 'clean package'
          - task: Docker@2
            inputs:
              command: 'build'

  - stage: Test
    jobs:
      - job: UnitTests
        steps:
          - task: Maven@3
            inputs:
              goals: 'test'

  - stage: Deploy
    condition: eq(variables['Build.SourceBranch'], 'refs/heads/master')
    jobs:
      - job: Production
        steps:
          - task: Docker@2
            inputs:
              command: 'push'
          - task: Kubernetes@1
            inputs:
              command: 'apply'
```

**Ramas estándar**:
- `feature/*`: Desarrollo de nuevas funcionalidades.
- `develop`: Integración de features.
- `stage`: Ambiente de pre-producción.
- `master/main`: Producción.

---

### ❓ Pregunta 30:
¿Qué herramientas conoces? Docker, Azure DevOps.

### ✅ Respuesta:

**Docker**: Plataforma de contenedorización que empaqueta aplicaciones con sus dependencias.

```dockerfile
# Dockerfile
FROM openjdk:17-slim
WORKDIR /app
COPY target/mi-app.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=prod
    depends_on:
      - db
  db:
    image: postgres:14
    environment:
      POSTGRES_DB: midb
      POSTGRES_PASSWORD: secret
```

**Azure DevOps**: Plataforma completa de DevOps.
- **Azure Repos**: Control de versiones (Git).
- **Azure Pipelines**: CI/CD automatizado.
- **Azure Boards**: Gestión de trabajo (Kanban, Scrum).
- **Azure Test Plans**: Testing.
- **Azure Artifacts**: Gestión de paquetes.

---

### ❓ Pregunta 31:
¿Sabes qué es un clúster y con qué se administra?

### ✅ Respuesta:

**Clúster**: Conjunto de servidores (nodos) que trabajan juntos como un sistema único para alta disponibilidad y escalabilidad.

**Kubernetes (K8s)**: Sistema de orquestación de contenedores más usado para administrar clústeres.

**Componentes principales**:
- **Pod**: Unidad mínima desplegable (1+ contenedores).
- **Node**: Servidor físico o virtual del clúster.
- **Deployment**: Define el estado deseado de pods.
- **Service**: Expone pods a la red.
- **Ingress**: Gestiona acceso HTTP/HTTPS externo.

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mi-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mi-app
  template:
    metadata:
      labels:
        app: mi-app
    spec:
      containers:
      - name: app
        image: mi-app:1.0
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: mi-app-service
spec:
  selector:
    app: mi-app
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer
```

**Ventajas**:
- Auto-escalado.
- Auto-recuperación.
- Balanceo de carga.
- Despliegues sin downtime.

---

### ❓ Pregunta 32:
¿Qué servidores conoces? ¿Qué es nube híbrida, privada y pública?

### ✅ Respuesta:

**Servidores Linux**:
- **Ubuntu**: Distribución popular, fácil de usar, LTS (Long Term Support).
- **CentOS**: Basado en Red Hat, estable para servidores empresariales.
- **Debian**: Muy estable, base de Ubuntu.
- **Red Hat Enterprise Linux (RHEL)**: Versión comercial con soporte.

**Tipos de nube**:

**Nube Pública**:
- Infraestructura compartida.
- Proveedores: AWS, Azure, Google Cloud.
- Pago por uso.
- Ventajas: Escalable, sin mantenimiento de hardware.

**Nube Privada**:
- Infraestructura dedicada a una organización.
- Puede estar on-premise o gestionada por proveedor.
- Mayor control y seguridad.
- Mayor costo.

**Nube Híbrida**:
- Combinación de pública y privada.
- Datos sensibles en privada, cargas variables en pública.
- Flexibilidad y optimización de costos.

---

### ❓ Pregunta 33:
¿Qué servicios de AWS conoces?

### ✅ Respuesta:

**Computación**:
- **EC2** (Elastic Compute Cloud): Servidores virtuales.
- **Lambda**: Funciones serverless (sin servidor).
- **Elastic Beanstalk**: Despliegue automático de aplicaciones.

**Almacenamiento**:
- **S3** (Simple Storage Service): Almacenamiento de objetos (archivos).
- **EBS** (Elastic Block Store): Discos para EC2.

**Base de Datos**:
- **RDS**: Bases de datos relacionales (MySQL, PostgreSQL).
- **DynamoDB**: Base de datos NoSQL.

**Red**:
- **API Gateway**: Gestión de APIs.
- **CloudFront**: CDN (Content Delivery Network).
- **VPC**: Red privada virtual.

**Monitoreo**:
- **CloudWatch**: Logs y métricas.
- **CloudTrail**: Auditoría de acciones en AWS.

```java
// Ejemplo: Subir archivo a S3
AmazonS3 s3Client = AmazonS3ClientBuilder.standard()
    .withRegion(Regions.US_EAST_1)
    .build();

s3Client.putObject(
    "mi-bucket",
    "archivo.txt",
    new File("ruta/archivo.txt")
);
```

---

### ❓ Pregunta 34:
¿Cómo puedes desplegar continuamente infraestructura? ¿Conoces Terraform?

### ✅ Respuesta:

**Terraform**: Herramienta de Infrastructure as Code (IaC) que permite definir infraestructura como código y desplegarlo automáticamente.

**Ventajas**:
- Infraestructura versionada.
- Reproducible y consistente.
- Multi-cloud (AWS, Azure, GCP).
- Preview de cambios antes de aplicarlos.

```hcl
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "app_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "MiServidor"
  }
}

resource "aws_s3_bucket" "storage" {
  bucket = "mi-bucket-unico"
  
  tags = {
    Environment = "Production"
  }
}
```

**Comandos básicos**:
```bash
terraform init    # Inicializar proyecto
terraform plan    # Ver cambios que se aplicarán
terraform apply   # Aplicar cambios
terraform destroy # Eliminar infraestructura
```

---

## 🔹 Sección 10: Bases de Datos

### ❓ Pregunta 35:

Preguntas sobre SQL

### ✅ Respuesta:

**Operaciones básicas**:
```sql
-- SELECT
SELECT nombre, edad FROM usuarios WHERE edad > 18;

-- JOIN
SELECT u.nombre, p.titulo
FROM usuarios u
INNER JOIN pedidos p ON u.id = p.usuario_id;

-- GROUP BY
SELECT departamento, COUNT(*) as total
FROM empleados
GROUP BY departamento
HAVING COUNT(*) > 5;

-- Subconsultas
SELECT nombre FROM usuarios
WHERE id IN (SELECT usuario_id FROM pedidos WHERE total > 1000);

-- Window Functions
SELECT nombre, salario,
       RANK() OVER (PARTITION BY departamento ORDER BY salario DESC) as ranking
FROM empleados;
```

**Índices**:
```sql
CREATE INDEX idx_usuario_email ON usuarios(email);
```

**Transacciones**:
```sql
BEGIN;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;
COMMIT; -- o ROLLBACK si hay error
```

**JPA/Hibernate en Spring**:
```java
@Entity
@Table(name = "usuarios")
public class Usuario {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @OneToMany(mappedBy = "usuario", cascade = CascadeType.ALL)
    private List<Pedido> pedidos;
}

// Repository
public interface UsuarioRepository extends JpaRepository<Usuario, Long> {
    Optional<Usuario> findByEmail(String email);
    
    @Query("SELECT u FROM Usuario u WHERE u.edad > :edad")
    List<Usuario> buscarMayoresDe(@Param("edad") int edad);
}
```

---

## 🔹 Sección 11: Metodologías Ágiles

### ❓ Pregunta 36:

¿Conoces metodologías ágiles? ¿En qué consiste Scrum?

### ✅ Respuesta:

**Scrum**: Marco de trabajo ágil para desarrollo iterativo e incremental.

**Roles**:
- **Product Owner (PO)**: Define prioridades, gestiona backlog.
- **Scrum Master**: Facilita procesos, elimina impedimentos.
- **Development Team (Devs, QA)**: Equipo auto-organizado que desarrolla el producto.

**Artefactos**:
- **Product Backlog**: Lista priorizada de funcionalidades.
- **Sprint Backlog**: Tareas seleccionadas para el sprint actual.
- **Incremento**: Producto potencialmente entregable al final del sprint.

**Ceremonias**:

1. **Sprint Planning** (Planeación):
   - Definir objetivo del sprint.
   - Seleccionar historias de usuario.
   - Estimar con Planning Poker (Fibonacci: 1, 2, 3, 5, 8, 13, 21).
   - Criterios: Complejidad + Riesgo + Tamaño.

2. **Daily Stand-up** (Diaria):
   - 15 minutos máximo.
   - ¿Qué hice ayer?
   - ¿Qué haré hoy?
   - ¿Tengo impedimentos?

3. **Sprint Review** (Revisión):
   - Demostración del incremento a stakeholders.
   - Feedback del Product Owner.

4. **Sprint Retrospective** (Retrospectiva):
   - ¿Qué salió bien?
   - ¿Qué puede mejorar?
   - Acciones de mejora.

5. **Refinamiento (Grooming)**:
   - Revisar y estimar historias futuras.
   - Aclarar requisitos.

**Duración del Sprint**:

- Típicamente 2 semanas (puede ser 1-4 semanas).
- Tiempo fijo, no se extiende.

**Historia de Usuario**:
```
Como [rol]
Quiero [funcionalidad]
Para [beneficio]

Criterios de aceptación:
- Dado que [contexto]
- Cuando [acción]
- Entonces [resultado esperado]
```

**Estimación con Fibonacci**:

- 1: Muy simple (minutos).
- 2-3: Simple (horas).
- 5: Moderado (1 día).
- 8: Complejo (2-3 días).
- 13+: Muy complejo (debe dividirse).

---

## 📌 Resumen de Conceptos Clave

✅ **POO**: Herencia, Polimorfismo, Abstracción, Encapsulamiento  
✅ **SOLID**: S-R-P, O-C-P, L-S-P, I-S-P, D-I-P  
✅ **Spring**: IoC, DI, Anotaciones (@Service, @Repository, @RestController)  
✅ **Seguridad**: JWT, OAuth2  
✅ **Arquitectura**: Monolítica vs Microservicios, Hexagonal  
✅ **Patrones**: Factory, Builder, Adapter, DTO, Saga, Circuit Breaker  
✅ **DevOps**: CI/CD, Docker, Kubernetes, Terraform  
✅ **Cloud**: AWS (EC2, S3, Lambda, CloudWatch)  
✅ **Metodología**: Scrum (2 semanas, Planning Poker, Daily, Review, Retrospective)