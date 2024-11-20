# Atomic Variables

* [Exercicis Atomic Variables](#exercicis-atomic-variables)

## Overview

https://docs.oracle.com/javase/tutorial/essential/concurrency/atomicvars.html

La biblioteca `java.util.concurrent.atomic` proporciona clases -como `AtomicInteger`- para realizar operaciones atómicas sobre valores.

Las operaciónes atómicas se ejecutan de forma indivisible: no pueden ser interrumpidas ni observadas en un estado intermedio por otros threads. Es decir, se ejecutan completamente de forma _synchronizada_.

Cada clase proprciona métodos para hacer las operaciones típicas según su tipo de datos.

## AtomicInteger



<br />

## Exercicis Atomic Variables

<br />

### 🐦 Exercici 1: DeepPurpleSky

En el siguiente programa, cada `Post` tiene tiene un contador de _megustas_ y otro de _republicaciones_.

Mandamos 5.000.000 de operaciones `megusta`y 5.000.000 de operaciones `republicar` sobre un post... y los contadores se vuelven locos.

Arréglalo con `syncrhonized`, `lock` y `atomic` a ver cuál es más rápido.

PD: _¿Crees qué es fiable medir así la velocidad de un programa?_

```java
public class Main {

    static class Post {
        int meGustas = 0;
        int rePublicaciones = 0;

        void meGusta() {
            meGustas++;
        }

        void rePublicar() {
            rePublicaciones++;
        }
    }

    public static void main(String[] args) {
        var startTime = LocalDateTime.now();

        Post post = new Post();

        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (int i = 0; i < 5_000_000; i++) {
                executor.submit(post::meGusta);
                executor.submit(post::rePublicar);
            }
        }

        System.out.println(post.meGustas);
        System.out.println(post.rePublicaciones);

        System.out.println("Tiempo tardado: " + ChronoUnit.MILLIS.between(startTime, LocalDateTime.now()));
    }
}
```