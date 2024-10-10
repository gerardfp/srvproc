# Streams

- [Exercicis Stream](#exercicis-stream)
 

## Overview

https://docs.oracle.com/javase/tutorial/collections/streams/

Los Streams de Java, proporcionan una forma ágil y expresiva de trabajar con colecciones de datos, como listas o arrays, permitiendo realizar operaciones con un estilo funcional (lambdas y method references).

Un `Stream` es una secuencia de elementos que admite diversas operaciones realizadas _en cadena_ (conocido como _pipeline_). Esta cadena o _pipeline_ se divide en tres fases:

* **Fuente**: El stream se crea a partir de una fuente de datos, como un array o una lista
* **Operaciones intermedias**: Son operaciones como `filter`, `map` o `sorted` que transforman el Stream. Estas operaciones son **perezosas** (_lazy_), lo que significa que solo se ejecutan cuando se invoca una operación terminal
* **Operaciones terminales**: Operaciones como `forEach`, `reduce` y `collect`. Estas operaciones producen un resultado o aplican una acción a cada uno de los elementos del stream.

Veamos cada una de estas fases:

<br />

### ⛲️ Fuente: creación de un Stream

Hay múltiples maneras de crear un Stream, veremos algunos ejemplos.

#### 🟢 A partir de un array

```java
String[] array = new String[]{"e1", "e2", "e3"};

Arrays.stream(array);
```

#### 🟢 A partir de una Collection (List, Set)

```java
List<String> lista = List.of("e1", "e2", "e3");

lista.stream();
```

#### 🟢 Stream.of
```
Stream.of("e1", "e2", "e3");
```


#### 🟢 Stream.generate()

Genera un Stream infinito a partir de una función

```java
Random random = new Random();
Stream.generate(random::nextInt);

Scanner scanner = new Scanner(System.in);
Stream.generate(scanner::nextInt);
```

#### 🟢 Stream.iterate()

```java
Stream.iterate(100, n -> n + 10);   // 100 110 120 130 ...
```

El primer parámetro (100) es el primer elemento del Stream, cada nuevo elemento se crea aplicando la función al elemento anterior.

#### 🟢 IntStream, LongStream, DoubleStream

Permite crear secuencias consecutivas de números en un rango. Los elementos del Stream son de tipos primitivos (`int`, `long` o `double`) y no se pueden transformar en otra clase.

```java
IntStream.range(1,5);  // 1 2 3 4
```

#### 🟢 Files.lines()

Permite crear un stream con las líneas de un fichero

```java
Files.lines(Paths.get("/ruta/a/fichero");
```

<br />

### 💅🏻 Operaciones intermedias: transformación de un Stream

En esta fase se pueden quitar elementos del Stream, ordenarlos o transformarlos.

#### ❌ Quitar elementos

##### 🟢 skip

Permite saltarse los primeros elementos del Stream.

```java
Stream.of("e1", "e2", "e3", "e4", "e5").skip(2);  // e3 e4 e5
```

##### 🟢 limit

Permite quedarse solo con los primeros elementos del Stream.
```java
Stream.of("e1", "e2", "e3", "e4", "e5").limit(2);   // e1 e2
```

##### 🟢 distinct
Permite eliminar los elementos duplicados del Stream

```java
Stream.of("e1", "e2", "e3", "e2", "e3").distinct();  // e1
```

##### 🟢 dropWhile
Permite eliminar los primeros elementos mientras se cumpla una condición. Cuando la condición no se cumple deja de eliminar elementos
```java
Stream.of(2, 4, 6, 7, 8, 10, 3).dropWhile(n -> n%2 ==0);  // 7 8 10 3
```

##### 🟢 takeWhile
Permite quedarse solo con los primeros elementos mientras se cumpla una condición. Cuando la condición no se cumple elimina el resto de elementos
```java
Stream.of(2, 4, 6, 7, 8, 10, 3).takeWhile(n -> n%2 ==0);  // 2 4 6
```

##### 🟢 filter
Elimina todos los elementos que **no** cumplen una condición.
```java
Stream.of(2, 4, 6, 7, 8, 10, 3).filter(n -> n % 2 == 0);  // 2 4 6 8 10
```

#### 🔄 Ordenar elementos

##### 🟢 sorted

Ordena los elementos acorde al orden _natural_, o acorde a la funcion de ordenación proporcionada

```java
Stream.of(2, 4, 5, 3, 1).sorted();  // 1 2 3 4 5
Stream.of("bbbbb", "aaaa", "eee", "cc", "d").sorted();  // aaaa bbbbb cc d eee
```

La función de comparación debe retornar un número negativo, cero o positivo, según el primer argumento sea menor, igual o mayor al segundo.

```java
Stream.of(2, 4, 5, 3, 1).sorted((a, b) -> a % 2 == 0 ? 1 : a.equals(b) ? 0 : -1);   // 1 3 5 2 4
Stream.of("bbbbb", "aaaa", "eee", "cc", "d").sorted(
    (a,b) -> a.length() > b.length() ? 1 : a.length() == b.length() ? 0 : -1
);  //  d cc eee aaaa bbbbb
```

#### 🥸 Transformar elementos

##### 🟢 map

Toma cada elemento y hace una acción sobre él, retornando un elemento de la misma clase o de otra distinta.

```java
Stream.of(1,2,3).map(n -> n*10);   // 10 20 30
Stream.of(1, 2, 3).map(n -> n + "º");   // "1º" "2º" "3º"
```

##### 🟢 mapMulti

Toma cada elemento, y permite añadir al Stream diversos elementos nuevos a partir de el.

```java
 Stream.of(1,2,3).mapMulti((e, d) -> {
    d.accept(e + "º");
    d.accept(e*10);
});   // "1º" 10 "2º" 20 "3º" 30
```

##### 🟢 flatMap

Transforma cada elemento de un Stream en otro Stream, y luego junta estos Streams resultantes en un único Stream. Esto es particularmente útil cuando tenemos varias listas y queremos unirlas en una única lista:

```java
List<Integer> lista1 = List.of(1,2,3);
List<Integer> lista2 = List.of(4,5,6);
List<Integer> lista3 = List.of(7,8,9);

Stream.of(lista1, lista2, lista3).flatMap(List::stream);  // 1 2 3 4 5 6 7 8 9
```

##### 🟢 peek

En realidad, `peek` no transforma los elementos de un Stream, sino que realiza una acción con cada uno de ellos, y los vuelve a meter en el Stream.

<br />

### 🚧 Operaciones terminales: producir un resultado o hacer una acción

Estas operaciones finalizan el stream. Después de ellas ya no se puede seguir haciendo acciones sobre el Stream. O bien retornan un resultado, o hacen una acción con cada elemento.

##### 🟢 count

Retorna un `long` con el número de elementos en el Stream

```java
long numeroDeElementos = Stream.of(45,67,89).count();   // 3
```

##### 🟢 min/max

Retorna un `Optional` con el elemento mínimo/maximo encontrado (si había algun elemento).

```java
Optional<Integer> minimoA = Stream.of(1, 3, 5, 7).min(Integer::compare);
System.out.println(minimoA.get());  // 1

Optional<Integer> minimoB = Stream.of(1, 3, 5, 7).filter(n -> n % 2 == 0).min(Integer::compare);
System.out.println(minimoB.isPresent());  // false
```

##### 🟢 findFirst

Retorna un `Optional` con el primer elemento del Stream, o un `Optional` vació si no había ningún elemento en el Stream

```java
Optional<Integer> minimoA = Stream.of(4,2,7).findFirst();
System.out.println(minimoA.get());  // 4
```

##### 🟢 anyMatch

Retorna un `boolean` indicando si algun elemento cumple la condición proporcionada

```java
boolean hayAlgunPar = Stream.of(4,2,7).anyMatch(n -> n % 2 == 0);  // true
boolean hayAlgunoQueEmpiecePorA = Stream.of("hola", "adios", "que tal").anyMatch(n -> n.startsWith("a"));  // true
```

##### 🟢 allMatch

Retorna un `boolean` indicando si todos los elementos cumplen la condición proporcionada

```java
boolean todosContienenLaA = Stream.of("hola", "adios", "que tal").allMatch(n -> n.contains("a"));   // true
```


##### 🟢 noneMatch

Retorna un `boolean` indicando si ningún elemento cumple la condición proporcionada

```java
 boolean ningunoEsMayorACinco = Stream.of(3,2,4,1).noneMatch(n -> n > 5);   // true
```


##### 🟢 reduce

Si ninguna de las operaciones anteriores sirve para obtener el resultado que deseamos, siempre podemos escribir nuestra propia funcion que produce un resultado con `reduce`.


Hay tres variaciones del método `reduce()`:

1. `Optional<T> reduce(BinaryOperator<T> accumulator);`
2. `T reduce(T identity, BinaryOperator<T> accumulator);`
3. `<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);`

<br />

👉🏼 1.  `Optional<T> reduce(BinaryOperator<T> accumulator);`

Retorna un `Optional`, con el resultado de acumular todos los elementos, según la función `accumulator` proporcionada.

**El `Optional` resultante es de la misma clase que los elementos del Stream.**

```java
Optional<Integer> result = Stream.of(3,2,4,1).reduce((a,b) -> a + b);   // 10

Optional<Integer> result = Stream.of(3,2,4,1).reduce((a,b) -> a * b);   // 24
```
<br />

👉🏼 2. `T reduce(T identity, BinaryOperator<T> accumulator);`

Retorna un valor, resultado de acumular el primer parámetro pasado -`identity`- con todos los elementos, según la función `accumulator` proporcionada.

**El resultado es de la misma clase que los elementos del Stream.**




```java
Integer result = Stream.of(3,2,4,1).reduce(0, (a,b) -> a + b);   // 10
Integer result = Stream.of(3,2,4,1).reduce(1, (a,b) -> a + b);   // 11

Integer result = Stream.of(3,2,4,1).reduce(0, (a,b) -> a * b);   // 0
Integer result = Stream.of(3,2,4,1).reduce(1, (a,b) -> a * b);   // 24
```



👉🏼 3. `<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);`

Retorna un valor, resultado de acumular el primer parámetro pasado -`identity`- con todos los elementos, según las funciones `accumulator` y `combiner` proporcionadas.

 * `identity`: determina el primer valor resultado. Se aplicará la función `acumulator` con cada elemento del Stream y este valor _identity_.
 * `accumulator`: determina cómo se acumula cada elemento del Stream al valor `identity`.
 * `combiner`: determina cómo se combinan los valores resultado intermedios del `accumulator`.
   
El resultado no tiene porqué ser de de la misma clase que los elementos del Stream.

Solo se usa en Streams paralelos.

```java
String result = Stream.of(3, 2, 4, 1).parallel()
        .reduce(
                "a",
                String::repeat,
                (string, string2) -> string + " : " + string2
        );

// aaa : aa : aaaa : a
```



##### 🟢 collect

Permite **convertir** o **acumular** los elementos de un Stream en una colección o resultado final. 

El resultado obtenido por `collect` **no** tiene porqué ser de la misma clase que los elementos del stream.

Hay dos variaciones del método `collect`:

1. `<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner);`
2. `<R, A> R collect(Collector<? super T, A, R> collector);`

<br />

🫴🏼 1. `<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner);`

Es muy parecida a `reduce` solo que los métodos `accumulator` y `combiner` no retornan un valor resultado, sino que modifican el primer parámetro pasado.

```java
 StringBuilder result = Stream.of(3, 2, 4, 1).parallel()
         .collect(
                 () -> new StringBuilder("a"),
                 (string, integer) -> {
                     String repetidos = string.toString().repeat(integer);
                     string.delete(0, string.length());
                     string.append(repetidos);
                 },
                 (string, string2) -> string.append(" : ").append(string2)
         );
// aaa : aa : aaaa : a
```

<br />

🫴🏼 2. `<R, A> R collect(Collector<? super T, A, R> collector);`

Podemos encontrar _colectores_ predefinidos en la classe `Collectors`:

  * `Collectors.averagingInt`, `Collectors.averagingLong`, `Collectors.averagingDouble`: calculan la media de los elementos. Se le debe pasar una función que retorne cada elemento convertido en un   `int`, `long` o `double`. Siempre retorna un `Double`.

  ```java
  Double result = Stream.of(1, 2, 3).collect(Collectors.averagingInt(n -> n));   // 2.0
  ```
  
  * `Collectors.joining`: encadena un Stream de `String` usando el delimitador, prefijo y sufijo especificados.
  
  ```java
  String result = Stream.of("a", "b", "c").collect(Collectors.joining("-", "[","]"));
  ```


##### 🟢 toList

Retorna una `List` con los elementos del Stream

```java
List<String> result = Stream.of("a", "b", "c").toList()
```

##### 🟢 toArray

Retorna un `Array` con los elementos del Stream

```java
String[] result6 = Stream.of("a", "b", "c").toArray(String[]::new);
```

##### 🟢 forEach

Realiza un acción para cada uno de los elementos del Stream. Es una _operación terminal_, por lo tanto ya no retorna ningún Stream y no se pueden seguir realizando operaciones (en cambio, con `peek` sí se puede)

```java
Stream.of("a", "b", "c").forEach(System.out::println);
```

<br />

## Exercicis Stream

### 🎬 Exercici 1

Dado el siguiente Stream:

```java
import java.util.stream.Stream;

record Movie (String title, int duration, double rating, int year){}

public class Main {
    public static void main(String[] args) {

        Stream.of(
                new Movie("The Shawshank Redemption", 142, 9.3, 1994),
                new Movie("The Godfather", 175, 9.2, 1972),
                new Movie("The Dark Knight", 152, 9.0, 2008),
                new Movie("Pulp Fiction", 154, 8.9, 1994),
                new Movie("The Lord of the Rings: The Return of the King", 201, 8.9, 2003),
                new Movie("Schindler's List", 195, 8.9, 1993),
                new Movie("Fight Club", 139, 8.8, 1999),
                new Movie("Inception", 148, 8.8, 2010),
                new Movie("Forrest Gump", 142, 8.8, 1994),
                new Movie("The Matrix", 136, 8.7, 1999),
                new Movie("Goodfellas", 146, 8.7, 1990),
                new Movie("Star Wars: Episode IV - A New Hope", 121, 8.6, 1977),
                new Movie("Interstellar", 169, 8.6, 2014),
                new Movie("The Silence of the Lambs", 118, 8.6, 1991),
                new Movie("Seven", 127, 8.6, 1995)
        );
    }
}

```

1. Imprime el titulo de todas las peliculas
2. Encuentra la película maś larga
3. La más corta
4. Películas lanzadas después del 2000
5. Con un rating mayor a 9
6. Cuenta el número de películas lanzadas en los 60's
7. Calcula el rating medio de todas las películas
8. Lista las películas ordenadas alfabéticamente
9. Ordenadas por duración en orden descendente
10. La película con el rating más alto
11. Los títulos en mayúsculas
12. Encuentra alguna película lanzada antes de 1980
13. Comprueba si todas las peliculas tienen un duración mayor a 100 minutos
14. Comprueba si alguna película tiene un rating por debajo de 8
15. Obtén la duración acumulada de todas las películas 

<br />

### 🌊 Exercici 2

Haz un programa que lea un fichero con datos de estudiantes (nombre: string, nota: double) y escriba en otro fichero 
los datos:

* Ordenados por nombre
* Eliminando duplicados
* Con el nombre en mayúsculas

Ejemplo:

-- Entrada --
```
Juan:5.4
Ana:6.7
Manuel:8.6
Ana:6.7
```

-- Salida --
```
Manuel:8.6
Ana:6.7
Juan:5.4
```

Escribe una utilidad que genere los datos del fichero de entrada. Utiliza `ThreadLocalRandom.current().doubles()`  para generar números aleatorios. A partir de estos números genera el nombre y la nota.

<br />

### 🔋 Exercici 0,000001

```java
import java.time.LocalDateTime;
import java.time.temporal.ChronoUnit;

/*
Un algoritmo Proof-of-Work (PoW) es el que se utiliza en algunas criptomonedas, como Bitcoin, para validar
transacciones y crear nuevos bloques en la cadena de bloques. Básicamente, PoW consiste en encontrar un valor que,
cuando se combina con los datos de un bloque y se pasa por una función hash (como SHA-256), produce un hash que cumple
con ciertas condiciones, como tener un número determinado de ceros al inicio.

Para implementar un algoritmo de Proof-of-Work sencillo sin utilizar librerías de cifrado como SHA-256, podemos utilizar
un hash casero basado en operaciones simples como la suma y manipulación de caracteres. Aunque este no es seguro ni eficiente
como SHA-256, es útil para fines de ilustración.
 */

public class Main {

    public static void main(String[] args) {
        String data = "0.001 btc for you";  // Datos que queremos incluir en el bloque
        int difficulty = 3;  // Número de ceros iniciales requeridos en el hash
        System.out.println("Iniciando el proceso de minado...");
        var start = LocalDateTime.now();

        String hash = mineBlock(data, difficulty);

        System.out.println("Hash encontrado: " + hash);
        System.out.println("Tiempo tardado: " + ChronoUnit.MILLIS.between(start, LocalDateTime.now()));
    }

    public static String mineBlock(String data, int difficulty) {
        String target = String.format("%0"+difficulty+"d", 0);
        System.out.println(target);

        for (int nonce = 0; ; nonce++) {
            String hash = simpleHash(data + nonce);

            //System.out.println(hash);
            if (hash.startsWith(target)) {
                System.out.println("Bloque minado! Nonce: " + nonce);
                return hash;
            }
        }
    }

    public static String simpleHash(String input) {
        int hash = 0;

        for (int i = 0; i < input.length(); i++) 
            hash = Integer.rotateRight(hash += input.charAt(i) * (i + 1), i);
        
        return String.format("%08x", hash); // 8 caracteres
    }
}

```
