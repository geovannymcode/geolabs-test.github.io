# Guía de Entrevista: Programación Reactiva y Spring WebFlux

## Fundamentos de Programación Reactiva

### 1. ¿Qué es la programación reactiva y cuáles son sus principios?

La programación reactiva es un paradigma declarativo centrado en el trabajo con flujos de datos asíncronos y la propagación de cambios. Sus principios fundamentales son:

- **Responsive**: El sistema responde de manera oportuna.
- **Resilient**: El sistema permanece responsivo ante fallos.
- **Elastic**: El sistema se mantiene responsivo bajo carga variable.
- **Message-Driven**: Comunicación basada en mensajes asíncronos.

La implementación se basa en el patrón Observer, donde los consumidores reaccionan a los datos a medida que son emitidos por los productores, en lugar de solicitarlos.

### 2. ¿Qué es el manifiesto reactivo?

El Manifiesto Reactivo es un documento que establece los principios de los sistemas reactivos. Define que los sistemas reactivos deben ser responsivos, resilientes, elásticos y orientados a mensajes. Fue creado por Jonas Bonér, Dave Farley, Roland Kuhn y Martin Thompson en 2013, y ha sido firmado por numerosas organizaciones para promover este enfoque de desarrollo de software.

### 3. ¿Qué es Reactive Streams y cómo se implementa en Java?

Reactive Streams es una iniciativa para proporcionar un estándar para el procesamiento asíncrono de flujos de datos con contrapresión (backpressure). En Java, se implementa a través de las interfaces:

- `Publisher<T>`: Fuente de datos que emite elementos.
- `Subscriber<T>`: Consumidor que recibe elementos.
- `Subscription`: Enlace entre Publisher y Subscriber, permite solicitar elementos (backpressure).
- `Processor<T,R>`: Combina Publisher y Subscriber, para transformaciones.

Las implementaciones más populares en Java son:

- Project Reactor (usado por Spring WebFlux)
- RxJava
- Akka Streams
- Java 9 Flow API

### 4. ¿Qué es la contrapresión (backpressure) y por qué es importante?

La contrapresión es un mecanismo que permite a un consumidor controlar cuántos elementos puede procesar, evitando ser sobrecargado por un productor que emite datos demasiado rápido. Es esencial para sistemas reactivos porque:

1. Previene desbordamientos de memoria (OutOfMemoryError)
2. Evita la pérdida de datos
3. Optimiza el uso de recursos
4. Mejora la estabilidad del sistema

En Reactive Streams, el Subscriber puede solicitar N elementos mediante `subscription.request(N)`, controlando así el flujo de datos.

## Spring WebFlux

### 5. ¿Qué es Spring WebFlux y en qué se diferencia de Spring MVC?

Spring WebFlux es un framework web reactivo no bloqueante que forma parte de Spring 5+. Sus principales diferencias con Spring MVC son:

| Spring MVC | Spring WebFlux |
|------------|---------------|
| Bloqueante (1 hilo por solicitud) | No bloqueante (pocos hilos para muchas solicitudes) |
| Basado en Servlet API | Basado en Reactive Streams |
| Modelo imperativo | Modelo reactivo (declarativo) |
| Contenedor tradicional (Tomcat, Jetty) | Contenedores tradicionales o Netty (predeterminado) |
| Procesamiento secuencial | Procesamiento asíncrono |
| Mayor consumo de recursos bajo carga | Mayor eficiencia con recursos bajo carga |

WebFlux es ideal para aplicaciones con alto throughput y operaciones intensivas de E/S, mientras que MVC es más simple para aplicaciones CRUD estándar.

### 6. ¿Cuándo deberías usar WebFlux en lugar de Spring MVC?

Deberías considerar usar WebFlux cuando:

- Tu aplicación requiere manejar muchas conexiones concurrentes (miles)
- Trabajas con operaciones de E/S de larga duración o servicios remotos
- Necesitas streaming de datos (SSE, WebSockets)
- Tienes alta latencia en operaciones de E/S
- Quieres maximizar el uso de recursos del servidor
- Trabajas con microservicios que se comunican de forma asíncrona

No es recomendable cuando:

- Tu equipo no está familiarizado con programación reactiva
- Usas bibliotecas o APIs bloqueantes
- Necesitas integrar con sistemas que no tienen soporte reactivo
- Las operaciones son principalmente de CPU y no de E/S
- No hay necesidad de alto throughput o gran concurrencia

### 7. ¿Cómo se configuran las rutas en WebFlux usando el enfoque funcional?

WebFlux ofrece un enfoque funcional para definir rutas usando `RouterFunction` y `HandlerFunction`:

```java
@Configuration
public class UsuarioRouter {
    
    @Bean
    public RouterFunction<ServerResponse> rutasUsuario(UsuarioHandler handler) {
        return route()
            .GET("/usuarios", handler::listarUsuarios)
            .GET("/usuarios/{id}", handler::obtenerUsuario)
            .POST("/usuarios", handler::crearUsuario)
            .PUT("/usuarios/{id}", handler::actualizarUsuario)
            .DELETE("/usuarios/{id}", handler::eliminarUsuario)
            .build();
    }
}

@Component
public class UsuarioHandler {
    
    public Mono<ServerResponse> listarUsuarios(ServerRequest request) {
        return ServerResponse.ok()
            .contentType(MediaType.APPLICATION_JSON)
            .body(servicioUsuario.findAll(), Usuario.class);
    }
    
    // Otros métodos handler...
}
```

Este enfoque separa claramente el enrutamiento de la lógica de manipulación, permitiendo un código más declarativo y componible.

### 8. ¿Qué son los controladores reactivos en WebFlux y cómo se implementan?

Los controladores reactivos en WebFlux son similares a los controladores de Spring MVC, pero devuelven tipos reactivos (`Mono<T>` o `Flux<T>`). Se implementan usando la anotación `@RestController`:

```java
@RestController
@RequestMapping("/usuarios")
public class UsuarioController {
    
    private final UsuarioService usuarioService;
    
    @Autowired
    public UsuarioController(UsuarioService usuarioService) {
        this.usuarioService = usuarioService;
    }
    
    @GetMapping
    public Flux<Usuario> listarUsuarios() {
        return usuarioService.findAll();
    }
    
    @GetMapping("/{id}")
    public Mono<Usuario> obtenerUsuarioPorId(@PathVariable String id) {
        return usuarioService.findById(id);
    }
    
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Usuario> crearUsuario(@RequestBody Usuario usuario) {
        return usuarioService.save(usuario);
    }
}
```

Los controladores reactivos procesan las solicitudes de forma no bloqueante, permitiendo manejar más solicitudes con menos recursos.

## Mono y Flux

### 9. ¿Qué son Mono y Flux en Project Reactor?

`Mono` y `Flux` son las implementaciones principales de `Publisher` en Project Reactor:

- **Mono<T>**: Representa una secuencia asíncrona de 0 o 1 elemento. Equivalente reactivo a `CompletableFuture<T>` o `Optional<T>`.
- **Flux<T>**: Representa una secuencia asíncrona de 0 a N elementos. Equivalente reactivo a `Stream<T>` o `List<T>`.

Ambos tipos ofrecen operadores para transformar, combinar y manipular flujos de datos de manera declarativa y no bloqueante.

```java
// Ejemplo de Mono
Mono<Usuario> usuarioMono = repositorio.findById("123");

// Ejemplo de Flux
Flux<Usuario> usuariosFlux = repositorio.findAll();
```

### 10. ¿Cuáles son los principales operadores de Mono y Flux y para qué se utilizan?

Project Reactor ofrece numerosos operadores para trabajar con flujos reactivos:

**Operadores de Transformación**:

- **map**: Transforma cada elemento aplicando una función que devuelve un nuevo valor.
  ```java
  Flux<String> nombres = usuariosFlux.map(Usuario::getNombre);
  ```

- **flatMap**: Transforma cada elemento en un Publisher y "aplana" los resultados en un único flujo. Los elementos pueden entremezclarse (orden no garantizado).
  ```java
  Flux<Comentario> comentarios = usuarios
      .flatMap(u -> comentarioRepository.findByUsuarioId(u.getId()));
  ```

- **concatMap**: Similar a flatMap, pero preserva el orden de los elementos originales, procesando cada fuente completamente antes de pasar a la siguiente.
  ```java
  Flux<Comentario> comentariosOrdenados = usuarios
      .concatMap(u -> comentarioRepository.findByUsuarioId(u.getId()));
  ```

- **flatMapMany**: Específico para Mono, convierte un Mono en un Flux aplicando una función que devuelve múltiples elementos.
  ```java
  Mono<Usuario> usuarioMono = repository.findById(id);
  Flux<Comentario> comentarios = usuarioMono
      .flatMapMany(u -> comentarioRepository.findByUsuarioId(u.getId()));
  ```

**Operadores de Filtrado**:

- **filter**: Incluye solo los elementos que cumplen con el predicado dado.
  ```java
  Flux<Usuario> adultos = usuarios.filter(u -> u.getEdad() >= 18);
  ```

- **distinct**: Elimina elementos duplicados, manteniendo solo la primera ocurrencia.
  ```java
  Flux<String> distintas = palabras.distinct();
  ```

- **take**: Limita el flujo a un número específico de elementos, tomando solo los primeros N elementos.
  ```java
  Flux<Usuario> primerosTres = usuarios.take(3);
  ```

- **skip**: Omite los primeros N elementos del flujo y emite los restantes.
  ```java
  Flux<Usuario> despuesDeDos = usuarios.skip(2);
  ```

**Operadores de Combinación**:

- **zip**: Combina elementos de múltiples Publishers emparejándolos por posición.
  ```java
  Flux<UsuarioConEstadistica> combinado = Flux.zip(
      usuarios, estadisticas, (u, e) -> new UsuarioConEstadistica(u, e)
  );
  ```

- **merge**: Combina múltiples Publishers en uno solo, entremezclando elementos a medida que llegan.
  ```java
  Flux<Notificacion> todas = Flux.merge(notificacionesEmail, notificacionesSMS);
  ```

- **concat**: Combina múltiples Publishers secuencialmente, esperando a que cada uno complete.
  ```java
  Flux<Dato> todosDatos = Flux.concat(datosCache, datosDB);
  ```

**Operadores de Reducción**:

- **reduce**: Combina todos los elementos en un único valor aplicando una función acumuladora.
  ```java
  Mono<Integer> suma = numeros.reduce(0, (acumulado, actual) -> acumulado + actual);
  ```

- **count**: Cuenta el número total de elementos en el flujo.
  ```java
  Mono<Long> total = usuarios.count();
  ```

- **collectList**: Recolecta todos los elementos en una lista.
  ```java
  Mono<List<Usuario>> listaUsuarios = usuarios.collectList();
  ```

- **collectMap**: Recolecta elementos en un Map usando una función para determinar las claves.
  ```java
  Mono<Map<String, Usuario>> mapaPorId = usuarios.collectMap(Usuario::getId);
  ```

**Manejo de errores**:

- **onErrorReturn**: Sustituye el error con un valor predeterminado y completa el flujo.
  ```java
  Mono<Usuario> usuario = repository.findById(id)
      .onErrorReturn(new Usuario("default", "Usuario por defecto"));
  ```

- **onErrorResume**: Cambia a un Publisher alternativo cuando ocurre un error.
  ```java
  Mono<Usuario> usuario = primaryRepo.findById(id)
      .onErrorResume(e -> backupRepo.findById(id));
  ```

- **retry**: Reintenta toda la secuencia un número específico de veces en caso de error.
  ```java
  Mono<Datos> datos = servicio.getDatosInestables().retry(3);
  ```

**Contexto y Threading**:

- **publishOn**: Cambia el Scheduler donde los operadores subsiguientes se ejecutarán.
  ```java
  Flux<Data> datos = repository.findAll()
      .publishOn(Schedulers.parallel())
      .map(this::procesoPesado);
  ```

- **subscribeOn**: Especifica el Scheduler donde la suscripción inicial se ejecutará.
  ```java
  Flux<Data> datos = operacionBloqueanteWrapper()
      .subscribeOn(Schedulers.boundedElastic());
  ```

### 11. ¿Cómo se maneja la suscripción en Reactor y qué ocurre si no te suscribes?

En Reactor, la suscripción es el desencadenante que inicia el flujo de datos desde el `Publisher` hacia el `Subscriber`. Sin suscripción, nada ocurre (lazy evaluation).

Una suscripción completa incluye tres callbacks:
```java
flux.subscribe(
    data -> System.out.println("Dato: " + data),     // onNext
    error -> System.err.println("Error: " + error),  // onError
    () -> System.out.println("Completado")           // onComplete
);
```

Si no te suscribes:

1. No se ejecuta ninguna operación
2. No se emiten datos
3. No se generan efectos secundarios

Esto se conoce como "ensamblaje vs ejecución" - puedes construir un flujo complejo sin ejecutarlo hasta la suscripción.

### 12. ¿Cómo se manejan los errores en programación reactiva con Reactor?

Reactor ofrece varios operadores para manejar errores de forma declarativa:

- **onErrorReturn**: Proporciona un valor por defecto en caso de error
  ```java
  Mono<Usuario> usuarioODefault = usuarioMono
      .onErrorReturn(new Usuario("default"));
  ```

- **onErrorResume**: Cambia a un Publisher alternativo en caso de error
  ```java
  Mono<Usuario> conRecuperacion = usuarioMono
      .onErrorResume(e -> fallbackService.getUsuario());
  ```

- **onErrorMap**: Transforma un error en otro tipo de error
  ```java
  Mono<Usuario> conErrorMapeado = usuarioMono
      .onErrorMap(RuntimeException.class, 
          e -> new UsuarioException("Error al obtener usuario", e));
  ```

- **doOnError**: Ejecuta un efecto secundario cuando ocurre un error
  ```java
  Mono<Usuario> conLog = usuarioMono
      .doOnError(e -> log.error("Error en flujo: {}", e.getMessage()));
  ```

- **retry/retryWhen**: Reintenta la secuencia en caso de error
  ```java
  Mono<Usuario> conReintento = usuarioMono.retry(3);
  ```

Los errores en programación reactiva siguen el principio de "fail-fast" y terminan la secuencia inmediatamente a menos que sean manejados.

## WebClient y WebTestClient

### 13. ¿Qué es WebClient y cómo se utiliza para consumir APIs?

WebClient es el cliente HTTP reactivo de Spring WebFlux, diseñado para realizar peticiones de forma no bloqueante. Reemplaza al antiguo RestTemplate con un enfoque reactivo.

Características principales:

- No bloqueante y reactivo
- Fluent API para configuración y peticiones
- Soporte para codificación/decodificación de objetos
- Manejo de errores reactivo
- Integración con ReactiveAdapterRegistry

Ejemplo básico:
```java
WebClient webClient = WebClient.create("https://api.ejemplo.com");

Mono<Usuario> usuario = webClient.get()
    .uri("/usuarios/{id}", 123)
    .header("Authorization", "Bearer token")
    .retrieve()
    .bodyToMono(Usuario.class);

// O con más control sobre respuestas HTTP
Mono<Usuario> usuarioDetallado = webClient.get()
    .uri("/usuarios/{id}", 123)
    .accept(MediaType.APPLICATION_JSON)
    .exchangeToMono(response -> {
        if (response.statusCode().equals(HttpStatus.OK)) {
            return response.bodyToMono(Usuario.class);
        } else if (response.statusCode().is4xxClientError()) {
            return Mono.error(new ClienteException("Error de cliente"));
        } else {
            return Mono.error(new ServidorException("Error de servidor"));
        }
    });
```

### 14. ¿Cuáles son las ventajas de usar WebClient sobre RestTemplate?

WebClient ofrece varias ventajas sobre RestTemplate:

1. **Modelo no bloqueante**: Utiliza menos hilos para manejar más peticiones.
2. **API funcional y fluida**: Más expresiva y componible.
3. **Streaming reactivo**: Soporte para flujos de datos continuos (SSE, WebSockets).
4. **Backpressure**: Control de flujo para evitar sobrecarga.
5. **Cancelación**: Permite cancelar peticiones en curso.
6. **Composición**: Facilita la combinación de múltiples llamadas API.
7. **Mejor testabilidad**: Diseñado para ser fácilmente mockeado.
8. **Compatibilidad con Reactor y RxJava**: Interoperabilidad con otras librerías reactivas.

```java
// RestTemplate (bloqueante)
ResponseEntity<Usuario> response = restTemplate.getForEntity("/usuarios/{id}", Usuario.class, id);
Usuario usuario = response.getBody();

// WebClient (no bloqueante)
Mono<Usuario> usuarioMono = webClient.get()
    .uri("/usuarios/{id}", id)
    .retrieve()
    .bodyToMono(Usuario.class);
```

### 15. ¿Qué es WebTestClient y cómo se utiliza para probar endpoints reactivos?

WebTestClient es una herramienta especializada para probar aplicaciones WebFlux, proporcionando una API fluida para realizar y verificar solicitudes HTTP. Puede trabajar contra servidores reales, controladores Spring WebFlux o funciones de enrutamiento.

Características:

- API fluida similar a WebClient
- Aserciones específicas para pruebas
- Soporte para verificar respuestas reactivas
- Compatible con JUnit y otros frameworks de pruebas

```java
@SpringBootTest
@AutoConfigureWebTestClient
class UsuarioControllerTest {
    
    @Autowired
    private WebTestClient webTestClient;
    
    @Test
    void testObtenerUsuario() {
        webTestClient.get()
            .uri("/usuarios/{id}", 1)
            .accept(MediaType.APPLICATION_JSON)
            .exchange()
            .expectStatus().isOk()
            .expectHeader().contentType(MediaType.APPLICATION_JSON)
            .expectBody(Usuario.class)
            .value(usuario -> {
                assertThat(usuario.getId()).isEqualTo("1");
                assertThat(usuario.getNombre()).isEqualTo("Juan");
            });
    }
    
    @Test
    void testListarUsuarios() {
        webTestClient.get()
            .uri("/usuarios")
            .exchange()
            .expectStatus().isOk()
            .expectBodyList(Usuario.class)
            .hasSize(3)
            .contains(new Usuario("1", "Juan"));
    }
}
```

Para probar controladores específicos sin iniciar toda la aplicación:
```java
@WebFluxTest(UsuarioController.class)
class UsuarioControllerTest {
    @Autowired
    private WebTestClient webTestClient;
    
    @MockBean
    private UsuarioService usuarioService;
    
    @Test
    void testObtenerUsuario() {
        when(usuarioService.findById("1"))
            .thenReturn(Mono.just(new Usuario("1", "Juan")));
            
        // Prueba como en el ejemplo anterior
    }
}
```

### 16. ¿Cómo se manejan las pruebas de integración en aplicaciones WebFlux?

Las pruebas de integración en WebFlux utilizan `WebTestClient` con un enfoque más completo:

1. **Configuración de prueba**:

```java
@SpringBootTest
@AutoConfigureWebTestClient
class ApiIntegrationTest {
    
    @Autowired
    private WebTestClient webTestClient;
    
    // Tests...
}
```

2. **Pruebas con base de datos reactiva** (ej. MongoDB reactivo):

```java
@DataMongoTest
@Import(UsuarioRepositoryImpl.class)
class UsuarioRepositoryTest {
    
    @Autowired
    private UsuarioRepository repository;
    
    @Test
    void testGuardarYBuscar() {
        Usuario usuario = new Usuario("1", "Juan");
        
        StepVerifier.create(repository.save(usuario))
            .expectNextMatches(u -> u.getId().equals("1"))
            .verifyComplete();
            
        StepVerifier.create(repository.findById("1"))
            .expectNext(usuario)
            .verifyComplete();
    }
}
```

3. **Pruebas de flujos de datos reactivos con StepVerifier**:

```java
@Test
void testStreamingEndpoint() {
    Flux<String> responseFlux = webTestClient.get()
        .uri("/stream/datos")
        .exchange()
        .expectStatus().isOk()
        .returnResult(String.class)
        .getResponseBody();
        
    StepVerifier.create(responseFlux)
        .expectNext("dato1")
        .expectNext("dato2")
        .expectNext("dato3")
        .expectComplete()
        .verify(Duration.ofSeconds(5));
}
```

4. **Pruebas de WebSockets**:

```java
@Test
void testWebSocketEndpoint() {
    WebSocketClient client = new ReactorNettyWebSocketClient();
    URI uri = URI.create("ws://localhost:" + port + "/ws");
    
    Mono<Void> output = client.execute(uri, session -> {
        Mono<Void> input = session.send(Mono.just(session.textMessage("Hola")));
        Mono<Void> receive = session.receive()
            .map(WebSocketMessage::getPayloadAsText)
            .doOnNext(text -> {
                assertThat(text).isEqualTo("HOLA");
            })
            .then();
            
        return input.then(receive);
    });
    
    StepVerifier.create(output)
        .expectComplete()
        .verify(Duration.ofSeconds(5));
}
```

## Desventajas y Consideraciones

### 17. ¿Cuáles son las desventajas de usar Reactive Streams?

A pesar de sus beneficios, la programación reactiva tiene varias desventajas:

1. **Curva de aprendizaje pronunciada**: El paradigma reactivo requiere cambiar la forma de pensar.
2. **Debugging más complejo**: Las stack traces son menos intuitivas y más difíciles de seguir.
3. **Mayor complejidad de código**: El código reactivo puede ser más difícil de leer y mantener.
4. **Posible sobreutilización**: No todas las aplicaciones necesitan reactividad.
5. **Ecosistema limitado**: No todas las bibliotecas tienen versiones reactivas.
6. **Problemas de desarrollo**: Las herramientas de desarrollo (IDEs, profilers) están menos optimizadas.
7. **Mayor uso de memoria**: En algunos casos, puede requerir más memoria que el enfoque imperativo.
8. **Riesgo de bloqueo accidental**: Mezclar código bloqueante con no bloqueante puede degradar el rendimiento.

Ejemplo de código más complejo:
```java
// Imperativo (simple)
List<Usuario> usuarios = usuarioRepository.findAll();
List<Usuario> activos = usuarios.stream()
    .filter(Usuario::isActivo)
    .collect(Collectors.toList());

// Reactivo (más complejo)
Flux<Usuario> usuariosFlux = usuarioRepository.findAll();
Mono<List<Usuario>> activosMono = usuariosFlux
    .filter(Usuario::isActivo)
    .collectList()
    .doOnError(e -> log.error("Error: {}", e.getMessage()))
    .onErrorResume(e -> Mono.just(Collections.emptyList()));
```

### 18. ¿Cuándo NO deberías usar programación reactiva?

No deberías usar programación reactiva en estos escenarios:

1. **Aplicaciones CRUD simples**: Las aplicaciones con operaciones simples de base de datos sin necesidad de alta concurrencia.
2. **Equipos sin experiencia**: Cuando el equipo no está familiarizado con el paradigma reactivo y no hay tiempo para capacitación.
3. **Dependencias bloqueantes**: Cuando dependes de bibliotecas o APIs que son inherentemente bloqueantes.
4. **Operaciones intensivas de CPU**: La programación reactiva brilla en operaciones de E/S, no tanto en cálculos intensivos.
5. **Necesidades de rendimiento modestas**: Si tu aplicación no requiere manejar miles de conexiones concurrentes.
6. **Proyectos con plazos ajustados**: La curva de aprendizaje puede afectar los plazos de entrega.
7. **Cuando la simplicidad es prioritaria**: La legibilidad y mantenibilidad pueden verse comprometidas.
8. **Aplicaciones batch o de procesamiento por lotes**: Donde el procesamiento secuencial es más apropiado.

### 19. ¿Cómo afecta la programación reactiva al rendimiento y consumo de recursos?

La programación reactiva puede tener diferentes impactos en el rendimiento:

**Beneficios**:

- **Mejor uso de CPU**: Menos hilos pueden manejar más solicitudes.
- **Mayor throughput**: Puede procesar más solicitudes por segundo.
- **Mejor manejo de concurrencia**: Sin bloqueos de hilos para operaciones de E/S.
- **Escalabilidad vertical**: Aprovecha mejor los recursos de una sola máquina.
- **Menor sobrecarga de contexto**: Menos cambios de contexto entre hilos.

**Consideraciones**:

- **Overhead inicial**: Mayor uso de memoria para estructuras reactivas.
- **Presión en GC**: Más objetos temporales pueden aumentar la actividad del recolector de basura.
- **Complejidad de optimización**: Más difícil de perfilar y optimizar.
- **Rendimiento vs. complejidad**: El beneficio debe justificar la complejidad adicional.

Comparativa de rendimiento:

- Con **pocas conexiones simultáneas** (< 100): Spring MVC puede ser más eficiente.
- Con **muchas conexiones simultáneas** (> 1000): WebFlux suele ofrecer mejor rendimiento.
- Con **operaciones bloqueantes**: WebFlux puede degradarse significativamente.

### 20. ¿Cómo se integra la seguridad en aplicaciones WebFlux?

Spring Security se integra con WebFlux a través de un modelo reactivo:

```java
@Configuration
@EnableWebFluxSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
        return http
            .csrf().disable()
            .authorizeExchange()
                .pathMatchers("/public/**").permitAll()
                .pathMatchers("/api/admin/**").hasRole("ADMIN")
                .anyExchange().authenticated()
            .and()
            .httpBasic()
            .and()
            .formLogin()
            .and()
            .build();
    }
    
    @Bean
    public ReactiveUserDetailsService userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
            .username("user")
            .password("password")
            .roles("USER")
            .build();
            
        UserDetails admin = User.withDefaultPasswordEncoder()
            .username("admin")
            .password("password")
            .roles("ADMIN")
            .build();
            
        return new MapReactiveUserDetailsService(user, admin);
    }
}
```

Para trabajar con JWT:
```java
@Bean
public SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
    return http
        .csrf().disable()
        .authorizeExchange()
            .pathMatchers("/auth/**").permitAll()
            .anyExchange().authenticated()
        .and()
        .addFilterAt(jwtAuthenticationFilter, SecurityWebFiltersOrder.HTTP_BASIC)
        .exceptionHandling()
            .authenticationEntryPoint((swe, e) -> 
                Mono.fromRunnable(() -> swe.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED)))
        .and()
        .build();
}
```

La autenticación y autorización en WebFlux se basan en operadores reactivos:
```java
@GetMapping("/usuario/actual")
public Mono<Usuario> getUsuarioActual(Authentication authentication) {
    return Mono.justOrEmpty(authentication)
        .map(Authentication::getName)
        .flatMap(usuarioService::findByUsername);
}

// O usando SecurityContextHolder reactivo
public Mono<Usuario> getUsuarioActual() {
    return ReactiveSecurityContextHolder.getContext()
        .map(SecurityContext::getAuthentication)
        .map(Authentication::getName)
        .flatMap(usuarioService::findByUsername);
}
```