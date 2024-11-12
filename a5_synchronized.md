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

<br />

### 🤡 Exercici 3: Non-performant messy database

Esta """base de datos""" debería crear un fichero nuevo en cada nueva ejecución del programa, y se supone que en el fichero debería aparecer la línea "hola que tal" 100 veces...

Arréglalo, pero:
* **Solo** puedes tocar el método `writeHolaquetal` y la clase `Database`. 
* El método `write` debe seguir escribiendo el string 'caracter a caracter' en el fichero.

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;
import java.time.LocalDateTime;
import java.util.concurrent.Executors;

public class Main {

    static class Database {
        static Path path;

        Database() {
            path = Path.of("db.sqlito-" + LocalDateTime.now());
            System.out.println("Database path: " + path);
        }

        void write(String data) {
            // Debe escribir el string en el fichero 'caracter a caracter'
            for (int i = 0; i < data.length(); i++) {
                try {
                    Files.writeString(path, data.substring(i, i + 1), StandardOpenOption.CREATE, StandardOpenOption.APPEND);
                } catch (Exception _) {
                }
            }
        }
    }

    public static void main(String[] args) throws Exception {
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (int i = 0; i < 100; i++) {
                executor.submit(Main::writeHolaquetal);
            }
        }

        // mostrar la """base de datos""" 
        try (var lines = Files.lines(Database.path)) {
            System.out.println("Total: " + lines.peek(System.out::println).count() + " lineas");
        }
    }

    static void writeHolaquetal() {
        Database database = new Database();
        database.write("hola que tal\n");
    }
}
```

<br />

### 🤑 Exercici 4: El típico ejemplo de _Banking Transaction System_

El siguiente real-life-example ™️, es un sistema de transacciones de dinero entre cuentas bancarias.

Hay dos simulaciones que se ejecutan cada una 100 veces:

* En la primera simulación se crean dos cuentas de 500$ y se transfieren tres veces de forma simultánea 200$ de la cuenta 1 a la 2.
* En la segunda se crean cien cuentas de 500$ y se realizan mil transacciones de cantidades random entre cuentas random.

Se supone que hay una comprobación de fondos antes de hacer la transaccion, y que una cuenta nunca debería quedarse con fondos negativos, pero como el progama no está sincronizado es un desastre.

Arréglalo, pero ten en cuenta que se deben poder realizar transacciones simultáneas, siempre que no se utilicen las mismas cuentas.

```java
import java.util.List;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadLocalRandom;
import java.util.stream.IntStream;

class Cuenta {
    int fondos;
    final int numero;

    Cuenta(int numero) {
        this.numero = numero;
        this.fondos = 500;
    }

    void sumar(int cantidad) {
        fondos += cantidad;
    }

    void restar(int cantidad) {
        fondos -= cantidad;
    }
}

class Banco {
    public void transferirDinero(Cuenta origen, Cuenta destino, int cantidad) {
        if (origen.fondos > cantidad) {
            destino.sumar(cantidad);
            origen.restar(cantidad);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            simularDosCuentas();
        }

        for (int i = 0; i < 100; i++) {
            simularCienCuentas();
        }
    }

    static void simularDosCuentas() {
        Cuenta cuenta1 = new Cuenta(1);
        Cuenta cuenta2 = new Cuenta(2);

        Banco banco = new Banco();

        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            executor.submit(() -> banco.transferirDinero(cuenta1, cuenta2, 200));
            executor.submit(() -> banco.transferirDinero(cuenta1, cuenta2, 200));
            executor.submit(() -> banco.transferirDinero(cuenta1, cuenta2, 200));
        }

        if (cuenta1.fondos < 0) {
            System.out.println("\uD83D\uDCA9 " + cuenta1.fondos);
        }
    }

    static void simularCienCuentas() {
        List<Cuenta> cuentas = IntStream.range(0, 100).mapToObj(Cuenta::new).toList();

        Banco banco = new Banco();

        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (int i = 0; i < 1000; i++) {
                executor.submit(() ->
                        banco.transferirDinero(
                                cuentas.get(ThreadLocalRandom.current().nextInt(cuentas.size())),
                                cuentas.get(ThreadLocalRandom.current().nextInt(cuentas.size())),
                                ThreadLocalRandom.current().nextInt(500)
                        ));
            }
        }

        cuentas.stream()
                .filter(c -> c.fondos < 0)
                .forEach(c -> System.out.println("\uD83E\uDD2E " + c.fondos));
    }
}

```
