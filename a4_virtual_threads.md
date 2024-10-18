# Virtual Threads


## Overview

https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html

Un thread es un método que se ejecuta de forma simultánea (_concurrente_ o _paralela_) a otro thread.

La ejecución de métodos de forma simultánea benficia:
* que los programas se ejecuten con más _throughput_ (rendimiento), aprovechando el uso de CPU, ya que en numerosas ocasiones un método se pasa gran parte del tiempo esperando a que finalicen operacion I/O (disco, red, etc...).
* que los programas parezcan más _responsive_ ya que pueden ir avanzando varias tareas poco a poco, en lugar de esperar a que acabe una para empezar otra.
* que los programas sean más rápidos en sisemas multicore, ejecutando los threads en paralelo en los distintos núcleos

Sin embargo, la ejecución de Threads conlleva su problemática, en especial:
* Race conditions: cuando dos threads intentan acceder al mismo recurso (variable) al mismo tiempo, puede llevar a _bugs_ en los que el resultado depende del _timing_ de los threads.
* Deadlocks: si dos threads estan esperandose mútuamente que el otro termine, pueden quedarse atrapados y nunca proceder.
* Starvation: ejecutar diversos threads a la vez puede llevar a _bugs_ de agotamiento de recursos.

Java provee diversos mecanismos para ejecutar Threads y manejar sus problemáticas.

<br />

### 👹 Ejecución de Virtual Threads

#### 🟢 Thread.ofVirtual().start() y Thread.startVirtualThread()

El método `Thread Thread.ofVirtual().start(Runnable task)` permite lanzar la ejecución de un Thread. 

```java
var thread = Thread.ofVirtual().start(() -> {
    // thread code
});
```

De igual manera, el método `Thread Thread.Thread.startVirtualThread(Runnable task)` permite lanzar la ejecución de un Thread.

```java
var thread = Thread.Thread.startVirtualThread(() -> {
    // thread code
});
```

Ambos métodos retorna un objeto `Thread` para poder manejar la tarea.

* `join()`:
    espera a que termine el thread. Es importante saber que si no se usa `join()` para esperar que un thread termine, el thread terminará cuando el thread que lo lanzó termine.

```java
var thread = Thread.startVirtualThread(() -> {
    // thread code
    for(int a = 10; a--> 0;) System.out.println(a);
});

thread.join(long);  // esperar a que termine el thread

System.out.println("Program finished");
```

* `join(long milis)`:
    espera a que termine el thread un tiempo determinado, si el thread no termina el programa continua

```java
var thread = Thread.startVirtualThread(() -> {
    // thread code
    for(int a = 100000000; a--> 0;) System.out.println(a);
});

thread.join(1000);  // esperar 1 segundo a que termine el thread

System.out.println("Program finished");
```


#### 🟢 Executors.newVirtualThreadPerTaskExecutor() 

Los _Executors_ permiten manejar los threads de una forma más manejable.

El executor _newVirtualThreadPerTask_:

* Debe usarse en un bloque try-with-resources.
* El executor no se cerrarà hasta que no finalicen todos los threads en ejecución**
* Cada vez que se lanza un thread con `submit()` retorna un objeto `Future` para poder manejarlo.

```java
try (var executor = Executors.newSingleThreadExecutor()) {
    var future = executor.submit(() -> {
        // thread code
    });
}
```

Hay tres variaciones del método `submit()`:

* `<T> Future<T> submit(Callable<T> task)`

Envía una tarea-que-retorna-un-resultado para su ejecución y devuelve un `Future` que representa el resultado pendiente de la tarea. El método `get` del `Future` retornará el resultado de la tarea una vez completada con éxito.

* `<T> Future<T> submit(Runnable task, T result)`

Envia una tarea `Runnable` para su ejecución y retorna un `Future` representando dicha tarea. El método `get()` del _Future_ retornará el mismo valor `result` que se le proporcionó en la llamada a `submit`. Esto resulta útil para identificar cuál es la tarea que ha finalizado.

* `Future<?> submit(Runnable task)`

Envia una tarea `Runnable` para su ejecución y retorna un `Future` representando dicha tarea. El método `get()` del _Future_ retornará `null` cuando se haya completado.
