# Streams

https://docs.oracle.com/javase/tutorial/collections/streams/

## Overview

Los Streams de Java, proporcionan una forma ágil de trabajar con colecciones de datos, como listas o arrays, permitiendo realizar operaciones co nun estilo funcional (lambdas y method references).

Un `Stream` es una secuencia de elementos que admite diversas operaciones realizadas _en cadena_ (conocido como _pipeline_). Esta cadena o _pipeline_ se divide en tres fases:

* **Fuente**: El stream se crea a partir de una fuente de datos, como un array o una lista
* **Operaciones intermedias**: Son operaciones como `filter`, `map` o `sorted` que transforman el Stream. Estas operaciones son **perezosas** (_lazy_), lo que significa que solo se ejecutan cuando se invoca una operación terminal
* **Operaciones terminales**: Operaciones como `forEach`, `reduce` y `collect`. Estas operaciones producen un resultado o aplican una acción a cada uno de los elementos del stream.

Veamos cada una de estas fases:

### ⛲️ Fuente: creación de un Stream

Hay múltiples maneras de crear un Stream, veremos algunos ejemplos. Es importante saber que una vez creado, no se puede modificar la fuente de datos, es decir, no se pueden añadir nuevos elementos al Stream.

#### 🟢 A partir de un array

```java
String[] array = new String[]{"e1", "e2", "e3"};

Arrays.stream(array);
```

#### 🟢 A partir de una Collection (List, Set)

```java
List<String> lista = new ArrayList<>(); 

lista.stream();
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

Permite crear secuencias consecutivas de números en un rango

```java
IntStream.range(1,5);  // 1 2 3 4
```

#### 🟢 Files.lines()

Permite crear un stream con las líneas de un fichero

```java
Files.lines(Paths.get("/ruta/a/fichero");
```

### 🛠 Operaciones intermedias: transformación de un Stream

<br />

### Exercici 0,000001

```java
import java.time.LocalDateTime;
import java.time.temporal.ChronoUnit;
/*
Un algoritmo Proof-of-Work (PoW) es el que se utiliza en algunas criptomonedas, como Bitcoin, para validar transacciones y crear nuevos bloques
en la cadena de bloques. Básicamente, PoW consiste en encontrar un valor que, cuando se combina con los datos de un bloque y se pasa por una
función hash (como SHA-256), produce un hash que cumple con ciertas condiciones, como tener un número determinado de ceros al inicio.
Para implementar un algoritmo de Proof-of-Work sencillo sin utilizar librerías de cifrado como SHA-256, podemos utilizar un hash casero basado
en operaciones simples como la suma y manipulación de caracteres. Aunque este no es seguro ni eficiente como SHA-256, es útil para fines
de ilustración.
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
