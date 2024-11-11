# Synchronized

- [Exercicis Synchronized](#exercicis-synchronized)

## Overview

https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html

Los _threads_ se comunican principalmente compartiendo el acceso a variables. Esto hace posibles dos tipos de errores: 
* 💔 [_thread interference_](https://docs.oracle.com/javase/tutorial/essential/concurrency/interfere.html)
* 💔 [_memory consistency errors_](https://docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html)

La herramienta para evitar estos errores es la **sincronización** (`synchronized`).

Sin embargo, la sincronización puede introducir nuevos tipos de errores: 
* 💔 [_deadlock_](https://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html)
* 💔 [_starvation_](https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html)
* 💔 [_livelock_](https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html)

<br />

Java proporciona dos tipos básicos de sincronización: _synchronized methods_ y _synchronized statements_.

### ⏩ Synchronized methods

Para hacer un método sincornizado, simplemente añade la palabra clave `synchronized` en su declaración:

```java
    synchronized void método() {

    }
```

Esto tiene dos efectos:

* Primero, imposibilita que dos invocaciones del método en el mismo objeto se solapen. Cuando un _thread_ está ejecutando el método de un objeto, los otros _threads_ que invocan al método del mismo objeto se bloquen hasta que el primero finalice.
* Segundo, cuando finaliza un método `synchronized` se garantiza que los cambios en las variables del objeto serán efectivas para el resto de _threads_.

<br />

### ⏩ Synchronized statements

La sentencia `synchronized` permite crear un bloque de código sincronizado. Se debe especificar el objeto que proporciona el bloqueo.

```java
    synchronized(object) {

    }
```

Se asegura que, al mismo tiempo, solo podrá haber un _thread_ ejecutando una sentencia `synchronized` con el mismo objeto.

<br />

## Exercicis Synchronized

<br />

### 🧨 Exercici 1: Counter Crash

Analiza este programa y corrígelo.

```java
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        for (int i = 0; i < 100000; i++) {
            wtf();
        }
    }

    static void wtf() {
        Counter counter = new Counter();

        try(var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            executor.submit(counter::increment);
            executor.submit(counter::decrement);
        }
//        var t1 = Thread.startVirtualThread(counter::increment);
//        var t2 = Thread.startVirtualThread(counter::decrement);
//        t1.join();
//        t2.join();

        if (counter.value != 0)
            System.out.println(counter.value);
    }
}

class Counter {
    int value = 0;

    public void increment() {
        value++;
    }

    public void decrement() {
        value--;
    }
}
```

<br />

### ♥️♦️ vs ♠️ ♣️ Exercici2: Rojo y negro

El siguiente programa lanza un _thread_ que imprime 10x10 cada símbolo del poker. Cuando descomentamos los tres _threads_ restantes, los cuatro threads empiezan a imprimir simultáneamente y se muestran
una serie de líneas con los símbolos mezclados.

Está bien que se mezclen símbolos en una línea, pero eso sí, **solo los del mismo color!!!**

```java
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {

        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            executor.submit(Main::rojoynegro);
            executor.submit(Main::rojoynegro);
            executor.submit(Main::rojoynegro);
            executor.submit(Main::rojoynegro);
        }
    }

    static void rojoynegro() {

        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                System.out.print("♥️");
            }
            System.out.println();
        }

        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                System.out.print("♠️");
            }
            System.out.println();
        }

        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                System.out.print("♦️");
            }
            System.out.println();
        }

        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                System.out.print("♣️");
            }
            System.out.println();
        }
    }
}
```

### 🤡 Exercici 3: Non-performant messy database

Se supone que en el fichero debería aparecer la línea "hola que tal" 100 veces...

Arréglalo, pero **solo** puedes tocar el método `writeHolaquetal` y la clase `Database`.

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;
import java.util.concurrent.Executors;

public class Main {

    static class Database {
        static Path path = Path.of("db.sqlito");
        
        void write(String data) {
            for (int i = 0; i < data.length(); i++) {
                try {
                    Files.writeString(path, data.substring(i, i+1), StandardOpenOption.CREATE, StandardOpenOption.APPEND);
                } catch (Exception _) {
                }
            }
        }
    }

    public static void main(String[] args) throws Exception {
        Files.deleteIfExists(Database.path); // fresh start
    
        try(var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (int i = 0; i < 100; i++) {
                executor.submit(Main::writeHolaquetal);
            }
        }
        
        Files.lines(Database.path).forEach(System.out::println); // check print
    }

    static void writeHolaquetal() {
        Database database = new Database();
        database.write("hola que tal\n");
    }
}
```
