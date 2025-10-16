# Examen Back-End Developer

## 1. Palindromo

**Instrucciones:**
Crear una función que acepte un string como parámetro y retorne un valor booleano indicando si el string puede considerarse como un palíndromo o no. El palíndromo no se lee igual que de derecha a izquierda (es cíclico). No tener en cuenta los espacios, ni números, ni los siguientes caracteres especiales: ".", ",", "!"

**Ejemplo:**

- "La ruta nos aportó otro paso natural." = true
- "La niña aportó otro paso natural." = false

**Firma de la función:**

```java
public static boolean verificaCapicua(String cadena)
```

**Solución 1 (Java Tradicional/Imperativo) - RECOMENDADA:**
```java
public static boolean verificaCapicua(String cadena) {
    // Limpiar la cadena: solo letras y convertir a minúsculas
    StringBuilder limpia = new StringBuilder();
    for (int i = 0; i < cadena.length(); i++) {
        char c = cadena.charAt(i);
        if (Character.isLetter(c)) {
            limpia.append(Character.toLowerCase(c));
        }
    }
    
    // Verificar si es palíndromo
    String str = limpia.toString();
    int izq = 0;
    int der = str.length() - 1;
    
    while (izq < der) {
        if (str.charAt(izq) != str.charAt(der)) {
            return false;
        }
        izq++;
        der--;
    }
    
    return true;
}
```

**Solución 2 (Java con Programación Funcional) - ALTERNATIVA:**

```java
import java.util.stream.IntStream;

public static boolean verificaCapicua(String cadena) {
    String limpia = cadena.chars()
        .filter(c -> Character.isLetter(c))
        .map(Character::toLowerCase)
        .collect(StringBuilder::new, StringBuilder::appendCodePoint, StringBuilder::append)
        .toString();
    
    return IntStream.range(0, limpia.length() / 2)
        .allMatch(i -> limpia.charAt(i) == limpia.charAt(limpia.length() - 1 - i));
}
```

---

## 2. Web

**Pregunta:** ¿Por qué HTTP utiliza TCP como protocolo de transporte?

**Opciones:**

- ✅ **Los paquetes deben llegar en el orden correcto.**
- Porque no valida que los paquetes lleguen a destino.
- Porque es más rápido.

---

## 3. Data Structures 2

**Pregunta:** ¿Qué diferencias hay entre implementar el comportamiento de una lista utilizando como estructura de soporte un array vs una lista simplemente enlazada?

**Opciones (Pick ONE OR MORE):**

- Tanto lista simplemente enlazada como array permiten insertar en O(1) en el peor caso.
- La eliminación de cualquier elemento puede hacerse en O(1) en una lista simplemente enlazada.
- ✅ **La eliminación en un array típicamente es O(n), siendo n la cantidad de elementos de la lista.**
- Tanto lista simplemente enlazada como array permiten acceder por posición en O(1).
- ✅ **Si las estructuras están ordenadas el orden de las búsquedas en el caso del array es O(log2n) y en el caso de la lista simplemente enlazada es O(n), siendo n la cantidad de elementos de la lista.**

---

## 4. POO 1

**Pregunta:** ¿Cuáles de las siguientes afirmaciones son correctas acerca de IoC (inversión de control)?

**Opciones (Pick ONE OR MORE):**

- Solo se puede lograr utilizando un framework (como Spring, Guice, Simple Injector, InversifyJS).
- IoC es un mecanismo para lograr inyección de dependencias.
- ✅ **Inyección de dependencias es una técnica para lograr IoC.**
- ✅ **Si tengo un componente A que depende de un componente B, IoC me permite ejecutar A sin acoplarme a una implementación específica de B.**

---

## 5. Relational Databases 1

**Pregunta:** Seleccionar las opciones correctas respecto a optimistic locking y pessimistic locking como estrategias de concurrencia en una base de datos.

**Opciones (Pick ONE OR MORE):**

- ✅ **El uso de un registro de versión o timestamp forman parte de una posible implementación de optimistic locking.**
- ✅ **En algunos escenarios el uso de un esquema de pessimistic locking tiene como contrapartida la necesidad de realizar un manejo de concurrencia en la capa de negocio para evitar deadlocks.**
- Por lo general una política pessimistic locking es mejor para soluciones web con gran tráfico.
- Las bases no relacionales no contemplan estos mecanismos ya que por su concepción no sufren de problemas de concurrencia.
- ✅ **Los diferentes motores de base de datos ofrecen diferentes granularidades de "lockes", entre las más habituales están: a nivel tabla y a nivel fila.**

---

## 6. Paréntesis balanceados

**Instrucciones:**
Estás diseñando un compilador C++ y necesitas verificar que los paréntesis de un archivo estén balanceados.

Los paréntesis en un String se consideran balanceados si se cumplen los siguientes criterios:

- Todos los paréntesis se cierran. Los paréntesis están en pares (i, 0 and [). El de la izquierda abre el par y el de la derecha lo cierra.
- En todos los paréntesis anidados dentro de otros, deberán estar correctamente emparejados.

Por ejemplo, [()] es un grupo balanceado pero [(]) no lo es.

**Descripción de la función:**
Completá la función braces en el editor. La función deberá devolver una lista de String donde el string para el índice i indica si estaban o no balanceados los paréntesis para el value. El String, consiste en el valor "YES" o "NO" correspondiente al valor.

**Constraints:**

- 1 ≤ n ≤ 15
- 1 ≤ largo de cada values ≤ 100
- Cada values consiste únicamente de los caracteres {, }, [, ], (, ).

**Firma de la función:**

```java
public static List<String> balancedBraces(List<String> braces)
```

**Solución 1 (Java Tradicional/Imperativo) - RECOMENDADA:**

```java
import java.util.*;

public static List<String> balancedBraces(List<String> braces) {
    List<String> result = new ArrayList<>();
    
    for (String str : braces) {
        if (isBalanced(str)) {
            result.add("YES");
        } else {
            result.add("NO");
        }
    }
    
    return result;
}

private static boolean isBalanced(String str) {
    Deque<Character> stack = new ArrayDeque<>();
    
    for (int i = 0; i < str.length(); i++) {
        char c = str.charAt(i);
        
        // Si es un paréntesis de apertura, lo agregamos a la pila
        if (c == '(' || c == '{' || c == '[') {
            stack.push(c);
        } 
        // Si es un paréntesis de cierre, verificamos que coincida
        else if (c == ')' || c == '}' || c == ']') {
            if (stack.isEmpty()) {
                return false;
            }
            
            char top = stack.pop();
            
            if (c == ')' && top != '(') {
                return false;
            }
            if (c == '}' && top != '{') {
                return false;
            }
            if (c == ']' && top != '[') {
                return false;
            }
        }
    }
    
    // Al final, la pila debe estar vacía
    return stack.isEmpty();
}
```

**Solución 2 (Java con Programación Funcional) - ALTERNATIVA:**

```java
import java.util.*;
import java.util.stream.Collectors;

public static List<String> balancedBraces(List<String> braces) {
    return braces.stream()
        .map(s -> isBalanced(s) ? "YES" : "NO")
        .collect(Collectors.toList());
}

private static boolean isBalanced(String str) {
    Map<Character, Character> pairs = Map.of(')', '(', '}', '{', ']', '[');
    
    Deque<Character> stack = new ArrayDeque<>();
    
    return str.chars()
        .mapToObj(c -> (char) c)
        .allMatch(c -> {
            if (pairs.containsValue(c)) {
                stack.push(c);
                return true;
            } else if (pairs.containsKey(c)) {
                return !stack.isEmpty() && stack.pop() == pairs.get(c);
            }
            return false;
        }) && stack.isEmpty();
}
```

---

## 7. Testing 1

**Pregunta:** Selecciona la opción correcta acerca de las pruebas.

**Opciones (Pick ONE):**

- ✅ **Un test unitario verifica la corrección del método testeado sin depender de otros componentes.**
- Un test es unitario si contiene todas las validaciones (asserts) necesarias para verificar el comportamiento bajo los distintos escenarios.
- Ninguna es verdadera.

---

## 8. Test Doubles

**Contexto:** Teniendo en cuenta el siguiente diagrama de clases:

```
UserService → UserRepository → RedisClient → Third Party Key Value SDK
```

**Pregunta:** En el contexto de un test de integración en el cual queremos verificar que luego de guardar un usuario, podemos obtener correctamente el usuario recientemente creado indicando su clave, ¿cuál de las siguientes opciones consideras correcta?

**Opciones (Pick ONE OR MORE):**

- Si la instancia de Redis es accesible desde la red donde se ejecuta el test, no haría ningún cambio en la aplicación, ya que durante la ejecución del test puedo acceder a esa instancia pública sin problemas.
- ✅ **Levantaría una instancia de Redis embebida en mi aplicación al ejecutar el test de integración configurada un profile de ejecución para test, inicializando RedisClient para que en lugar de utilizar la instancia de Redis original, utilice la embebida.**
- Implementarás un stub de KeyValueClient indicando que la operación getValueById devuelve siempre una instancia prefabricada de User.
- ✅ **Programarás una nueva implementación de la interfaz KeyValueClient que en lugar de utilizar Redis provea las mismas operaciones pero trabajando con un Map/Dictionary en memoria. Al ejecutar el test de integración configurarás un profile de ejecución para test que utilice esta implementación fake en lugar de la productiva.**
- Dado que el SDK es un componente externo ("third party"), no puedo proveer una nueva implementación de la interfaz KeyValueClient.

---

## 9. Ordenamiento de Lista

**Considerar la siguiente función:**

```javascript
def func(lista, i = 0, j = -1) {
    if (j == -1) {
        j = lista.size()
    }
    
    if (i < j) {
        func(lista, i+1, j)
    }
    
    elem = lista[i]
    k = i-1
    
    while (k > -1 && elem < lista[k]) {
        lista[k+1] = lista[k]
        k--
    }
    
    lista[k+1] = elem
}
```

**Pregunta:** Si una_lista = [3,3,2,1,4], indicar qué ocurre luego de ejecutar func(una_lista), asumiendo que el pasaje de argumentos es por referencia:

**Opciones (Pick ONE):**

- una_lista queda ordenada en forma decreciente (es decir, [4,3,3,2,1])
- una_lista no resulta modificada
- ✅ **una_lista queda ordenada en forma creciente (es decir, [1,2,3,3,4])**
- una_lista queda sin elementos repetidos (es decir, [3,2,1,4])
- Ninguna de las anteriores

---

## 10. Patterns - Código de Producto

**Contexto:** Al revisar un pull-request de un integrante de tu equipo encuentras que el siguiente fragmento de código aparece frecuentemente intercalado con la lógica de negocio de consultas y mantenimiento de productos:

```java
Product prod = new Product(name, category, null, null, true, currency, price);

if (subCategory != null) {
    prod.getCategory().setSubCategory(subCategory);
}

if (reviewer != null) {
    List<ProductReview> reviews = new ArrayList<ProductReview>();
    ProductReview review = new ProductReview();
    review.setReviewer(reviewer);
    if (reviewDate != null) {
        review.setReviewDate(reviewDate);
    }
    reviews.add(review);
    prod.setReviews(reviews);
}

if (prod.getCurrency() != oldCurrency && prod.getPrice() != oldPrice) {
    List<PriceUpdate> priceUpds = new ArrayList<PriceUpdate>();
    PriceUpdate update = new PriceUpdate(oldPrice, price, new Date(updateAt));
    priceUpds.add(update);
    prod.setPriceUpdates(priceUpds);
}
```

**Pregunta:** Luego de discutirlo con ella, deciden que hay oportunidades de simplificar y mejorar la mantenibilidad del código. Con este objetivo en mente, ¿qué patrón de diseño crees que podrían utilizar?

**Opciones (Pick ONE):**

- ✅ **Builder**
- Composite
- Prototype

---

## 11. Patterns - Facade

**Pregunta:** Tu equipo se encarga del desarrollo de un complejo SDK que permite llevar a cabo pagos eléctronicos en plataformas.

A pesar de contar con una extensa documentación, los clientes suelen reportar inconvenientes o incluso se presentan frecuentemente defectos en las integraciones ya que la librería cuenta con demasiadas clases con flujos y dependencias complejas que dificultan la correcta utilización del SDK.

**Pregunta:** ¿Qué patrón de diseño deberías utilizar para facilitar el uso de tu componente?

**Opciones (Pick ONE):**

- Bridge
- ✅ **Facade**
- Adapter
- Singleton

---

## 12. API Rest - Nueva lista

**Pregunta:** Una API Rest permite manejar listas de tareas (to-do list). Dicha API soporta múltiples listas de tareas, donde cada lista consiste en un id autogenerado, un título, fecha de creada y una colección tareas. Las tareas tienen un id autogenerado y un label.

Marque las opciones de diseño válidas para crear una nueva lista.

**Opciones (Pick ONE OR MORE):**

- ✅ **POST a /lists con {"title":"...", "tasks":[...]} como body**
- POST a /lists con {"id":"...", "title":"...", "tasks":[...]} como body
- PUT a /lists con {"title":"...", "tasks":[...]} como body
- PUT a /lists con {"id":"...", "title":"...", "tasks":[...]} como body
- UPDATE a /lists con {"title":"...", "tasks":[...]} como body
- UPDATE a /lists con {"id":"...", "title":"...", "tasks":[...]} como body

---

## 13. Patterns - SDK de Pagos

**Pregunta:** Tu equipo se encarga del desarrollo de un complejo SDK que permite llevar a cabo pagos eléctronicos en plataformas.

A pesar de contar con una extensa documentación, los clientes suelen reportar inconvenientes o incluso se presentan frecuentemente defectos en las integraciones ya que la librería cuenta con demasiadas clases con flujos y dependencias complejas que dificultan la correcta utilización del SDK.

¿Qué patrón de diseño deberías utilizar para facilitar el uso de tu componente?

**Opciones (Pick ONE):**

- Bridge
- ✅ **Facade**
- Adapter
- Singleton

---

## 14. Patronos de microservicios 2

**Pregunta:** En un sistema distribuido moderno, las colas de mensajes son componentes importantes que proporcionan comunicación y coordinación entre las partes del sistema. ¿Cuál de lo siguiente es cierto?

**Opciones (Pick ONE OR MORE):**

- Las colas de mensajes hacen que el sistema esté más desacoplado.
- ✅ **Las colas de mensajes aumentan la confiabilidad (reliability) del sistema.**
- Las colas de mensajes, disminuyen el rendimiento general del sistema.
- Las colas de mensajes aumentan la complejidad de la arquitectura del sistema.

---

## 15. Non relational DB 1

**Pregunta:** Cuáles de las siguientes sentencias son verdaderas

**Opciones (Pick ONE OR MORE):**

- Cuando tenemos un alto volumen de consultas y todas están basadas en un mismo campo, tiene sentido utilizar una base clave-valor.
- ✅ **Cuando importa la transaccionalidad y consistencia entre diferentes entidades lo mejor es utilizar una base de datos relacional.**
- Cuando se salva en una base no-sql, es importante tener en cuenta los patrones de acceso a los datos.

---

## 16. Horizontal Scaling

**Pregunta:** Cuando utilizamos una herramienta que garantiza la consistencia eventual...

**Opciones (Pick ONE OR MORE):**

- Entendemos que habrá algunos periodos de tiempo en los cuales diferentes nodos no ven la misma información.
- Sabemos que un nodo a la vez puede estar atrasado en actualizar la información.
- Sabemos que ningún nodo puede responder con información obsoleta.
- Ninguna es verdadera.
- ✅ **Si hay N nodos, sabemos que al menos N/2+1 de ellos tendrá siempre la información actualizada.**

---

## 17. Concurrency 3

**Pregunta:** ¿Qué son y qué diferencia hay entre un thread (hilo) y un proceso?

**Opciones (Pick ONE OR MORE):**

- ✅ **Un proceso puede tener múltiples threads**
- ✅ **Un thread puede tener múltiples procesos**
- Los procesos no comparten espacio de memoria
- Los threads no comparten espacio de memoria

---

## 18. Concurrency 2

**Pregunta:** Cuáles de las siguientes sentencias sobre concurrencia y paralelismo son verdaderas

**Opciones (Pick ONE OR MORE):**

- Concurrencia implica tener más de un core, para poder procesar sentencias al mismo tiempo en cada uno de ellos.
- ✅ **Paralelismo implica tener más de un core, para poder procesar sentencias al mismo tiempo en cada uno de ellos.**
- ✅ **Puede haber concurrencia sin paralelismo.**
- No podemos parallelizar si sólo tenemos una tarea que cumplir.

---

---

## 19. Concurrency 3

**Pregunta:** ¿Qué son y qué diferencia hay entre un thread (hilo) y un proceso?

**Opciones (Pick ONE OR MORE):**

- ✅ **Un proceso puede tener múltiples threads**
- ✅ **Un thread puede tener múltiples procesos**
- Los procesos no comparten espacio de memoria
- Los threads no comparten espacio de memoria

---

## 20. Concurrency 2

**Pregunta:** Cuáles de las siguientes sentencias sobre concurrencia y paralelismo son verdaderas

**Opciones (Pick ONE OR MORE):**

- Concurrencia implica tener más de un core, para poder procesar sentencias al mismo tiempo en cada uno de ellos.
- ✅ **Paralelismo implica tener más de un core, para poder procesar sentencias al mismo tiempo en cada uno de ellos.**
- ✅ **Puede haber concurrencia sin paralelismo.**
- No podemos parallelizar si sólo tenemos una tarea que cumplir.

---

## 21. Caching 1

**Pregunta:** ¿Cuáles de las siguientes sentencias sobre cachés son verdaderas?

**Opciones (Pick ONE OR MORE):**

- Las aplicaciones son más complejas porque deben agregar lógica para invalidar las claves.
- ✅ **Cuando se utilizan cachés locales, puede haber inconsistencias en las respuestas de diferentes nodos.**
- ✅ **Las cachés no deberían ser utilizadas como el almacenamiento principal, sino una forma de optimizar los tiempos de respuesta.**

---

## 22. Complejidad espacial

**Contexto:** La siguiente función permite calcular el máximo valor de una lista no vacía de números enteros:

```python
def max(lista, i = 0, j = -1) {
    if (j == -1) {
        j = lista.size()
    }
    
    if (j-i == 1) {
        return lista[i]
    }
    
    max_rec = max(lista, i+1, j)
    
    if(lista[i] > max_rec) {
        return lista[i]
    } else {
        return max_rec
    }
}
```

**Pregunta:** Siendo n la longitud de la lista pasada como argumento, indicar la complejidad espacial de la función max:

**Opciones (Pick ONE):**

- O(1)
- O(log n)
- ✅ **O(n)**
- O(n^2)

---

## 23. Pure Functions 1

**Pregunta:** ¿Cuál de los siguientes métodos puede ser categorizado como una "Pure Function"?

**Opciones (Pick ONE OR MORE):**

**Opción 1:**
```java
✅ public int sum(int a, int b) {
    return a + b;
}
```

**Opción 2:**
```java
public int sum(int a, int b) {
    auditService.audit("call to sum", a, b);
    return a + b;
}
```

**Opción 3:**
```java
✅ public int sum(int a, int b) {
    return a + 3 + b + 17;
}
```

**Opción 4:**
```java
public int sum(int a, int b) {
    synchronized(this) {
        return a + b;
    }
}
```

**Opción 5:**
```java
public int sum(int a, int b) {
    synchronized(this) {
        this.currentSum = a + b;
        return this.currentSum;
    }
}
```

---

## 24. Seguridad en general 3

**Pregunta:** Imagina que estás desarrollando una API la cual responde con sugerencias de ciudades a medida que el usuario va escribiendo. ¿Cómo puedes evitar el abuso de tu servicio?

**Opciones (Pick ONE OR MORE):**

- ✅ **Se puede utilizar una caché para reducir el impacto de queries similares.**
- ✅ **Podemos analizar la IP que nos consulta, y limitar el acceso cuando el comportamiento es sospechoso.**
- ✅ **Podríamos aplicar antirebote (debouncing) en el cliente para que la llamada a la API solo se ejecute un número limitado de veces en un intervalo de tiempo.**

---

## 25. SQL KYC Levels

**Contexto:** La tabla `user_level` guarda información histórica de los niveles alcanzados de experiencia (mensual/compras) de nuestros usuarios dentro de Empresa.

La tabla está constituida por las columnas:

- `id` INT → id aleatorio de la row
- `user_id` INT → El id único que identifica a un usuario
- `user_level` INT → El nivel de experiencia, cuanto más alto mayor experiencia tiene
- `user_type` VARCHAR(32) → Indica el tipo de usuario (puede ser comprador o vendedor)
- `date_completed` DATE → indica la fecha en la cual el usuario alcanzó el nivel
- `date_expired` DATE → indica la fecha en la cual el nivel dejó de estar vigente

Un usuario puede tener un único nivel vigente. Para representar que un nivel no está vigente el nivel debe tener su `date_expired` distinto de null.

**PROBLEMA:**
Dado un problema de consistencia de datos, existen más de un nivel sin expirar para un mismo usuario.
Se necesita obtener para cada usuario, su nivel actual único vigente, escriba una query que:

- Devuelva el id, user_id, user_level, user_type del nivel vigente de cada usuario del sistema
- Ordenado de forma descendente en función a la fecha en que cada usuario alcanzó dicho nivel
- Asegurándose de que el nivel no haya expirado

**Ejemplo de tabla:**

| id | user_id | user_level | user_type | date_completed | date_expired |
|----|---------|------------|-----------|----------------|--------------|
| 30 | 12345678| 3          | buyer     | 2021-03-02     | 2021-03-03   |
| 10 | 12345678| 4          | buyer     | 2021-03-03     | null         |
| 17 | 12345678| 5          | buyer     | 2021-03-04     | null         |

**Solución SQL:**
```sql
SELECT 
    ul.id,
    ul.user_id,
    ul.user_level,
    ul.user_type
FROM user_level ul
INNER JOIN (
    SELECT 
        user_id,
        MAX(date_completed) as max_date
    FROM user_level
    WHERE date_expired IS NULL
    GROUP BY user_id
) latest ON ul.user_id = latest.user_id 
    AND ul.date_completed = latest.max_date
WHERE ul.date_expired IS NULL
ORDER BY ul.date_completed DESC;
```

---

## 26. Pregunta de lógica 1

**Instrucciones:**
Tienes un contenedor de forma con dimensiones conocidas x,y. Dentro de este contenedor, se deben almacenar una cantidad de N cajas cada una de ellas con dimensiones a,b.

Con base en las dimensiones del contenedor y las cajas:

1. Determina cuántas cajas caben alineadas en una sola fila a lo largo de la longitud del contenedor.
2. Calcula cuántas filas de cajas pueden formarse a lo ancho del contenedor.
3. Calcula la cantidad total de cajas que se pueden almacenar en el contenedor.

**Notas:**
- Considera que el resultado debe ser cantidades enteras.
- Cada excepción o caso borde que genere un error, la salida debe ser [0,0,0]
- Las dimensiones de las cajas y contenedor pueden ser menores a 1, mayores de 0 y tener valores decimales.

**Firma de la función:**
```java
public static List<Integer> calculaCantidades(double largo, double ancho, double anchoCaja, double largoCaja)
```

**Solución (Java Tradicional):**
```java
import java.util.*;

public static List<Integer> calculaCantidades(double largo, double ancho, double anchoCaja, double largoCaja) {
    List<Integer> resultado = new ArrayList<>();
    
    try {
        // Validaciones: dimensiones deben ser mayores a 0
        if (largo <= 0 || ancho <= 0 || anchoCaja <= 0 || largoCaja <= 0) {
            resultado.add(0);
            resultado.add(0);
            resultado.add(0);
            return resultado;
        }
        
        // 1. Cuántas cajas caben en una fila (a lo largo)
        int cajasPorFila = (int) (largo / largoCaja);
        
        // 2. Cuántas filas caben (a lo ancho)
        int numeroFilas = (int) (ancho / anchoCaja);
        
        // 3. Total de cajas
        int totalCajas = cajasPorFila * numeroFilas;
        
        resultado.add(cajasPorFila);
        resultado.add(numeroFilas);
        resultado.add(totalCajas);
        
    } catch (Exception e) {
        // En caso de cualquier error, retornar [0,0,0]
        resultado.add(0);
        resultado.add(0);
        resultado.add(0);
    }
    
    return resultado;
}
```

---

## 27. Pregunta de Lógica 2

**Instrucciones:**
Implementar una aplicación que procese una lista de números enteros de manera concurrente.

El método debe:

1. Crear 10 hilos que procesen subconjuntos de la lista de números.
2. Los subconjuntos deben tener máximo 100 elementos.
3. Cada hilo debe sumar los números de su subconjunto.
4. Si en algún momento se encuentra un número negativo el método debe:
   - 4.1: El hilo debe lanzar una excepción personalizada NegativeNumberException.
   - 4.2: Agregar al array de salida el número negativo.
5. El programa principal debe capturar las excepciones que ocurran en cualquier hilo y continuar con el procesamiento.

**Información adicional:**

El método recibirá 2 parámetros de tipo Integer (rangoA, rangoB) con estos se crea la lista a procesar.
El método retornará una lista con:

- Los valores negativos encontrados (criterio 4 y 4.2)
- Al final de la lista la suma creada en el criterio 3

Debe retornar una lista con los valores negativos encontrados, y al final de la lista la suma de los números pares.

**Firma de la función:**

```java
public static List<Integer> processNumbersConcurrently(long rangoA, long rangoB)
```

**Solución (Java Tradicional):**

```java
import java.util.*;
import java.util.concurrent.*;

// Excepción personalizada
class NegativeNumberException extends Exception {
    private int negativeNumber;
    
    public NegativeNumberException(int number) {
        super("Número negativo encontrado: " + number);
        this.negativeNumber = number;
    }
    
    public int getNegativeNumber() {
        return negativeNumber;
    }
}

public static List<Integer> processNumbersConcurrently(long rangoA, long rangoB) {
    List<Integer> resultado = new ArrayList<>();
    List<Integer> numerosNegativos = Collections.synchronizedList(new ArrayList<>());
    AtomicLong sumaTotal = new AtomicLong(0);
    
    // Crear la lista de números desde rangoA hasta rangoB
    List<Long> numeros = new ArrayList<>();
    for (long i = rangoA; i <= rangoB; i++) {
        numeros.add(i);
    }
    
    // Dividir la lista en chunks de máximo 100 elementos
    int chunkSize = 100;
    List<List<Long>> chunks = new ArrayList<>();
    for (int i = 0; i < numeros.size(); i += chunkSize) {
        int end = Math.min(i + chunkSize, numeros.size());
        chunks.add(numeros.subList(i, end));
    }
    
    // Crear máximo 10 hilos
    int numThreads = Math.min(10, chunks.size());
    ExecutorService executor = Executors.newFixedThreadPool(numThreads);
    List<Future<?>> futures = new ArrayList<>();
    
    // Procesar cada chunk en un hilo
    for (List<Long> chunk : chunks) {
        Future<?> future = executor.submit(() -> {
            long sumaLocal = 0;
            try {
                for (Long num : chunk) {
                    if (num < 0) {
                        numerosNegativos.add(num.intValue());
                        throw new NegativeNumberException(num.intValue());
                    }
                    sumaLocal += num;
                }
                sumaTotal.addAndGet(sumaLocal);
            } catch (NegativeNumberException e) {
                // Capturar la excepción y continuar
                sumaTotal.addAndGet(sumaLocal);
            }
        });
        futures.add(future);
    }
    
    // Esperar a que todos los hilos terminen
    for (Future<?> future : futures) {
        try {
            future.get();
        } catch (Exception e) {
            // Continuar con el procesamiento
        }
    }
    
    executor.shutdown();
    
    // Construir el resultado: negativos + suma al final
    resultado.addAll(numerosNegativos);
    resultado.add((int) sumaTotal.get());
    
    return resultado;
}
```

---

## Resumen de Respuestas

### Ejercicios de Programación (Java Funcional):

1. **Palindromo** - verificaCapicua()
2. **Paréntesis balanceados** - balancedBraces()

### Ejercicio SQL:

1. **SQL KYC Levels** - Query para obtener nivel vigente único por usuario

### Preguntas de Opción Múltiple:

- **Web**: Los paquetes deben llegar en el orden correcto
- **Data Structures 2**: Eliminación O(n) en arrays, búsqueda O(log2n) en arrays ordenados
- **POO 1**: IoC permite desacoplamiento, DI es técnica para lograr IoC
- **Relational DB 1**: Optimistic locking usa versiones/timestamps, locks tienen granularidades
- **Testing 1**: Test unitario no depende de otros componentes
- **Test Doubles**: Redis embebido o implementación fake con Map
- **Ordenamiento**: Lista queda ordenada creciente
- **Patterns**: Builder y Facade
- **API Rest**: POST a /lists con body sin id
- **Microservicios**: Colas aumentan confiabilidad
- **NoSQL**: Transaccionalidad requiere DB relacional
- **Horizontal Scaling**: N/2+1 nodos con info actualizada
- **Concurrency**: Proceso tiene múltiples threads, puede haber concurrencia sin paralelismo
- **Caching**: Cachés locales pueden ser inconsistentes, no usar como almacenamiento principal
- **Complejidad espacial**: O(n) por recursión
- **Pure Functions**: Sin efectos secundarios ni mutación de estado
- **Seguridad**: Caché, rate limiting por IP, debouncing