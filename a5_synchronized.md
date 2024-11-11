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

### 🧨 Exercici 1: Counter Crash

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
