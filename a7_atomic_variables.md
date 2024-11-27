# Atomic Variables

* [Exercicis Atomic Variables](#exercicis-atomic-variables)

## Overview

https://docs.oracle.com/javase/tutorial/essential/concurrency/atomicvars.html

La biblioteca `java.util.concurrent.atomic` proporciona clases -como `AtomicInteger`- para realizar operaciones atómicas sobre valores.

Las operaciónes atómicas se ejecutan de forma indivisible: no pueden ser interrumpidas ni observadas en un estado intermedio por otros threads. Es decir, se ejecutan completamente de forma _synchronizada_.

Cada clase proprciona métodos para hacer las operaciones típicas según su tipo de datos.

### ⚛️ AtomicInteger 

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/AtomicInteger.html

### ⚛️ AtomicLong

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/AtomicLong.html

### ⚛️ AtomicRefrence 

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/AtomicReference.html


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

<br />

### 🌰 Exercici 2: Autoincrement

Implementa un generador de identificadores únicos en un entorno multihilo.

* Implementa una clase `UniqueIDGenerator` con un `AtomicLong` inicializado en `1`.
* Crea un método `generateID()` que devuelva un nuevo identificador único incrementando el valor del `AtomicLong`.
* Simula un sistema con 10 hilos donde cada hilo genera 500 identificadores únicos.
* Verifica que no hay duplicados en los identificadores generados.

<br />

### 👨‍👨‍👦‍👦 Exercici 3: Control de acceso

Simula un sistema para controlar el número máximo de usuarios simultáneos conectados.

* Crea una clase `AccessControl` con un `AtomicInteger` que represente el número de usuarios actuales conectados y un límite máximo de usuarios, por ejemplo, 5.  Implementa los métodos:
    * `connect()`: Incrementa el contador si el límite no se ha alcanzado; si ya está lleno, muestra un mensaje indicando que el acceso está bloqueado.
    * `disconnect()`: Decrementa el contador, asegurando que no sea negativo.
* Prueba la clase con un programa donde múltiples hilos intenten conectarse y desconectarse aleatoriamente.
* Asegúrate de que nunca se exceda el límite máximo de usuarios.

<br />

### 🗣 Exercici 4:  Actualizar perfil

Crea una clase para gestionar actualizaciones de un perfil de usuario.

* En primer lugar crea un record `UserProfile` con:
    * Un atributo `String` para el nombre.
    * Un atributo `int` para la edad.


* Diseña una clase `UserProfileUpdater` que contenga un atributo `AtomicReference<UserProfile>` inicializado con un perfil de usuario de ejemplo como `new UserProfile("Juan Doe", 25)`.

    * Implementa los métodos:
        * `updateProfile(String newName, int newAge)`:

        Usa `set()` para actualizar el perfil, *creando una nueva instancia* de `UserProfile` con los valores proporcionados.

        * `updateProfile(String expectedName, String newName, int newAge)`:
        
        User `getAndUpdate()` para actualizar el perfil solo si el nombre actual coincide con el paràmatro `expectedName`
    
        * `udpateProfile(UserProfile expected, UserProfile newProfile)`:
        
        Usa `compareAndSet()` para intentar cambiar el perfil actual solo si coincide con el perfil esperado (`expected`).

* En el programa principal, simula una actualización concurrente con múltiples hilos, usando los diferentes métodos de actualización. Cada hilo debe mostrar las operaciones realizadas.

* Al final, imprime el estado final del perfil.

<br />

### 🧎🏽‍♀️ Exercici 5: TaskState

Implementa la gestión del estado de una tarea. Los posible estados son `INIT`, `PROCESSING` y `COMPLETED`.

* Crea un clase `Task` con:
    * Un atributo `AtomicReference<TaskState>` para manejar el estado de la tarea de manera atómica. Define los estados posibles en un `enum TaskState`. El estado inicial debe ser `INIT`.
    * `getState()`: Devuelve el estado actual de la tarea.
    * `setState(TaskState newState)`: Cambia el estado de la tarea cumpliendo las siguientes reglas:
        * El estado puede cambiar de `INIT` a `PROCESSING`.
        * El estado puede cambiar de `PROCESSING` a `COMPLETED`.
        * Si no se cumplen las condiciones anteriores, el estado no debe cambiar, y el método debe devolver `false`.
    * `run()`:
        * Cambia el estado a `PROCESSING` si el estado actual es `INIT`.
        * Se ejecuta el método `perform()` cuando el estado cambia a `PROCESSING`.
        * Cambia el estado a `COMPLETED` al finalizar la ejecución de `perform()`.
    * `perform()`:
        * Método *abstracto* que define la tarea a realizar
               
* Implementa un programa principal donde varios hilos intenten ejecutar una misma tarea. Crea la tarea implementando una clase anónima.
