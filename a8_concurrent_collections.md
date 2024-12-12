# Concurrent collections

* [Exercicis Concurrent collections](#exercicis-concurrent-collecions)

## Overview

https://docs.oracle.com/javase/tutorial/essential/concurrency/collections.html

El paquete `java.util.concurrent` añade una serie de implementaciones al **Java Collections Framework**:

    LinkedBlockingQueue
    ArrayBlockingQueue
    PriorityBlockingQueue
    
    DelayQueue
    SynchronousQueue
    LinkedTransferQueue
    
    LinkedBlockingDeque
    
    CopyOnWriteArrayList
    
    CopyOnWriteArraySet
    ConcurrentSkipListSet
    
    ConcurrentHashMap
    ConcurrentSkipListMap

Estas colecciones ayudan a evitar errores de consistencia de memoria cuando son manipuladas por threads, al definir una relación de "sucede antes" entre una operación que **agrega** un objeto a la colección y operaciones posteriores que **acceden** o **eliminan** ese objeto.




### 🏮 ConcurrentHashMap

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html


<br />

## Exercicis Concurrent collections

### 📊 Exercici 1: Histograma

Haz un programa que cuente el número de veces que aparece cada fruta en el array.
Para que sea más rápido y se aprovechen los núcleos, haz cuatro threads y que cada thread se encargue de un cuarto el array:

```java
        String[] palabras = {
                "manzana", "pera", "naranja", "uva", "manzana", "naranja", "naranja", "uva",
                "naranja", "manzana", "naranja", "uva", "manzana", "naranja", "naranja", "uva",
                "manzana", "naranja", "naranja", "uva", "naranja", "manzana", "naranja", "uva",
                "pera", "naranja", "pera", "naranja", "uva", "naranja", "naranja", "manzana",
                "pera", "pera", "naranja", "manzana", "naranja", "naranja", "pera", "manzana",
                "uva", "naranja", "manzana", "pera", "pera", "naranja", "naranja", "manzana",
                "pera", "naranja", "uva", "pera", "manzana", "naranja", "naranja", "naranja",
                "pera", "uva", "manzana", "naranja", "pera", "uva", "naranja", "manzana",
                "naranja", "manzana", "uva", "pera", "pera", "naranja", "uva", "manzana",
                "naranja", "uva", "manzana", "naranja", "pera", "naranja", "manzana", "pera",
                "naranja", "uva", "manzana", "pera", "manzana", "naranja", "manzana", "pera",
                "naranja", "manzana", "manzana", "naranja", "uva", "naranja", "manzana", "naranja"
        };
```

Demuestra que usando un `HashMap` se pueden producir _race conditions_. Y utiliza luego un `ConcurrentHashMap` para prevenirlos.

El resultado debería ser:

```
pera: 18
manzana: 24
uva: 16
naranja: 38
```

<br />

### 🏨 Exercici 2: Reservas

Implementa un sistema de reservas para habitaciones de un hotel. Los usuarios deben poder reservar habitaciones de manera concurrente. La aplicación debe garantizar que las reservas en una misma habitación no se solapen.

Requisitos:
* Cada habitación se puede reservar desde una hora de inicio hasta una hora de fin. **La fecha no se tendrá en cuenta**.
* Si un usuario intenta reservar una habitación en una franja ya ocupada, la reserva se debe rechazar.
* El sistema debe registrar para cada reserva el usuario que la ha realizado, la franja y la habitación.

Implementación:
* Utiliza un `ConcurrentHashMap` para almacenar las reservas.
* Implementa un método que permita reservar una habitación en una franja horaria y un usuario. Este método debe retornar `true` o `false`, según si la reserva se pudo realizar o no.
* Haz un programa principal que simule solicitudes concurrentes de reserva (con diversas habitaciones, usuarios y franjas) y comprueba que no se solapan las reservas.

<br />


### 🎎 Exercici 3: Grupos de chat

Programa un sistema para gestionar grupos de chat de forma concurrente. El sistema debe permitir añadir/eliminar grupos, y añadir/eliminar usuarios a dichos grupos. Los grupos y usuarios se identifican por su nombre.

1. Crea la clase `GroupsManager` con los siguientes métodos:
    * `boolean addUserToGroup(String user, String group)`

        Añade un usuario a un grupo, si el grupo no existe lo crea. 

        Si el usuario ya pertenecia al grupo retorna `false`

    * `boolean removeUserFromGroup(String user, String group)`

        Quita a un usuario de un grupo, si el grupo queda vacío, lo elimina. 

        Si el usuario no pertenecía al grupo retorna `false`

    * `boolean deleteGroup(String group)` 
    
        Elimina un grupo. 
        
        Si el grupo no existía retorna `false`

1. Haz una simulación en el `main` donde diversos _threads_ van añadiendo y quitando usuarios de grupos _random_ :

```java
public class Main {
    public static void main(String[] args) {

        var users = List.of("usera", "userb", "userc", "userd", "usere", "userf", "userg", "userf");
        var groups = List.of("group1", "group2", "group3", "group4");


        // anyade y quita usuarios random de los grupos en diversos threads
    }
}
```

<br />

### 🤪 Exercici 4: new WrongList<>()

Haz un progama que cree dos _threads_ y cada uno de ellos añada un string a un `ArrayList`. Demuestra que pueden ocurrir _race conditions_ (aka interferencia de hilos).

Corrige el programa utilizando un `CopyOnWriteArrayList`.

<br />

### 🫂 Exercici 5: Messenger

Diseña un sistema para el procesamiento y almacenamiento de mensajes para una aplicación de mensajería.

Cada conexión de usuario se maneja un _thread_, y los mensajes enviados se almacenan en una lista compartida. De esa manera, la lista puede ser accedida por varios _threads_ simultáneamente. Además, hay un _thread_ de servicio que revisa la lista permanentemente y elimina los mensajes antiguos. 

Realiza un simulación del sistema:

* Lanza diversos _threads_ que simulen las conexiones de usuario. Cada _thread_ debe ir "enviando" (añadir a la lista) mensajes aleatorios a intervalos de tiempo aleatorios entre 2 y 4 segundos, y obtener los mensajes cuando se añada un mensaje a la lista (simplemente imprimir el número de mensajes es suficiente).

* Por otra parte, lanza el thread de servicio que cada 100 milisegundos revise los mensajes y elimine los que sean más antiguos a 3 segundos.


Comprueba que el número de mensajes se mantiene alrededor del número de usuarios (ya que cada usuario los envia cada 2-4 segundos y se eliminan a los 3 segundos).

Puedes usar el siguiente `record` para almacenar un mensaje:

```java
record Mensaje (String text,LocalDateTime date){
    public Mensaje(String text) {
        this(text, LocalDateTime.now());
    }
}
```

<br />

### 🤼‍♀️ Exercici 6: Gestor de tareas colaborativo



```java
record Tarea(String description, AtomicReference<State> state) {
    enum State {
        TODO, IN_PROGRESS, DONE
    }

    public Tarea(String text) {
        this(text, new AtomicReference<>(Tarea.State.TODO));
    }

    void setState(State newState) {
        
    }
}
```