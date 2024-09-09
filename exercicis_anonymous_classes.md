# Exercicis Anonymous classes

## 1. 
Este mètode `imprimirJerarquiaDeClasses()` imprimix la jerarquia de classes d'una classe, és a dir, la seua superclasse, i la superclasse de la seua superclasse, i la superclasse de la superclasse de la seua superclasse, i així __recursivament__.

```java
    static void imprimirJerarquiaDeClasses(Class<?> c) {
        System.out.print(c.getName() + (c.getSuperclass() != null ? "  >  " : "\n"));
        if (c.getSuperclass() == null) return;
        mostrarClasses(c.getSuperclass());
    }
```

En el següent programa pots vore el mètode en acció:

```java
class Animal {
    // ...
}

class Gos extends Animal {
    // ...
}

class Pastor extends Gos {
    // ...
}

class PastorBelga extends Pastor {
    // ...
}

public class Main {

    static void imprimirJerarquiaDeClasses(Class<?> c) {
        System.out.print(c.getName() + (c.getSuperclass() != null ? "  >  " : "\n"));
        if (c.getSuperclass() == null) return;
        imprimirJerarquiaDeClasses(c.getSuperclass());
    }

    public static void main(String[] args) {

        PastorBelga pastorBelga = new PastorBelga();

        imprimirJerarquiaDeClasses(pastorBelga.getClass());
    }
}
```

Quan s'executa, el programa imprimix:

```
PastorBelga  >  Pastor  >  Gos  >  Animal  >  java.lang.Object
```

<br />

a. Escriu un programa que quan s'execute, el mètode `imprimirJerarquiaDeClasses()` mostre el següent:
 
```
$AnonymousClass$ > Alumne > Persona > java.lang.Object
```

Has d'utilitzar una classe anònima que extenga (redeclare) una classe.

<br />

b) Escriu un programa que quan s'execute, el mètode `imprimirJerarquiaDeClasses()` mostre el següent:

```
$AnonymousClass$ > java.lang.Object
```

Has d'utilitzar una classe anònima que implemente un interface.

## 2.

Modifica el següent programa, per a que l'objecte `notification` siga d'una classe anònima que extenga la classe `Notification`, de forma que quan el paràmetre `num` siga major a 99, imprimisca `You have +99 new messages`, i quan el número siga 1, ho diga en singular.

```java
class Notification {
    void show(int num) {
        System.out.println("You have " + num + " new messages");
    }
}

public class Main {
    public static void main(String[] args) {

        Notification notification = new Notification();

        notification.show(1);
        notification.show(10);
        notification.show(99);
        notification.show(100);
        notification.show(135);
    }
}
```

## 3. 

Modifica i completa el següent programa:

```java
class Executable {
    void executar(){}
}

class Executor {
    void executarXVegades(int x, Executable executable){
        for (int i = 0; i < x; i++) {
            executable.executar();
        }
    }
}

public class Main {
    public static void main(String[] args) {

        Executor executor = new Executor();

        executor.executarXVegades(10, new Executable());
    }
}
```

1. Fes que l'objecte de classe `Executable` que se passa al mètode `executarXVegades()` siga d'una classe anònima que redeclare el mètode `executar()`, de forma que el programa mostre per pantalla 10 vegades el text `Hello Executor`

2. Transforma la classe Executable en un __interface__, i després en una __abstract class__.


## 4. 

```java
import java.util.ArrayList;
import java.util.List;


interface Alerta {
    String getMissatge();

    default int getMinuts() {
        return 0;
    }

    int getSegons();
}

class Temporitzador {
    List<Thread> threadList = new ArrayList<>();

    Temporitzador programar(Alerta alerta) {
        threadList.add(Thread.ofVirtual().start(() -> {
            try {
                Thread.sleep(alerta.getSegons() * 1000);
            } catch (Exception _) {
            }
            System.out.println("¡ALERTA! " + alerta.getMissatge());
        }));

        return this;
    }

    public void esperarQueAcabenLesAlertes() throws InterruptedException {
        for (Thread thread : threadList) thread.join();
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {

        new Temporitzador()
                .programar(new Alerta() {
                    @Override
                    public String getMissatge() {
                        return "Hora d'anar al cole";
                    }

                    @Override
                    public int getSegons() {
                        return 5;
                    }
                })
                .programar(new Alerta() {
                    @Override
                    public String getMissatge() {
                        return "Hora de desdejunar";
                    }

                    @Override
                    public int getSegons() {
                        return 1;
                    }
                })
                .esperarQueAcabenLesAlertes();

    }
}
```
