# Anonymous classes

- [Exercicis Anonymous classes](#exercicis-anonymous-classes)

## Overview

https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html

Les classes anònimes és quan creem un objecte i, al mateix temps, redeclarem alguns mètodes de la seva classe/interface. Java crea, al vol, una nova classe sense nom, que extén la orginal, per a aquest objecte.

#### Classe anònima que extén una classe

En el següent exemple definim la classe `Animal`, i després instanciem l'objecte `gos` al mateix temps que redeclarem el mètode `imprimirNom()`  de la classe `Animal`. L'objecte `gos`, aleshores, __no__ és de la classe `Animal` sinò d'una classe nova _sense nom_. La classe `Animal` és la __superclasse__ d'aquesta classe anònima (o el que és el mateix: la classe anònima extén la superclasse `Animal`).

```java
class Animal {
    void imprimirNom(){
        System.out.println("Soc un animal");
    }
}

public class Main {
    public static void main(String[] args) {
        
        Animal gos = new Animal(){
            @Override
            void imprimirNom() {
                System.out.println("Soc un gos");
            }
        };

    }
}
```
#### Classe anònima que implementa un interface

El següent exemple és similar, però instanciem l'objecte `falco` a partir de l'interface `Volador`. L'objecte `falco` serà d'una classe __anònima__ i en aquest cas `Volador` __no__ serà la seva superclasse (ja que Volador no és una classe). La classe anònima que s'ha creat per a l'objecte `falco` implementa l'interface `Volador`.

```java
interface Volador {
    void imprimirAltura();
}

public class Main {
    public static void main(String[] args) {

        Volador falco = new Volador() {
            @Override
            public void imprimirAltura() {
                System.out.println("30 metres");
            }
        };

    }
}
```

#### Classe anònima que exten una classe abstracta

En este últim exemple es creen dues classes anònimes que extenen la classe abstracta `Personatge`, i s'instancien dos objectes (`golum`i `aragorn`) d'aquestes dues classes anònimes.

```java
abstract class Personatge {
    void imprimirSalutacio(){
        System.out.println("Hola");
    }

    abstract void imprimirNom();
}

public class Main {
    public static void main(String[] args) {

        Personatge golum = new Personatge() {
            @Override
            void imprimirNom() {
                System.out.println("GOLUM");
            }
        };

        Personatge aragorn = new Personatge() {
            @Override
            void imprimirNom() {
                System.out.println("ARAGORN");
            }
        };

    }
}
```

<br />

## Exercicis Anonymous classes

<br />

#### 🍎 Exercici 1 
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

a) Escriu un programa que quan s'execute, el mètode `imprimirJerarquiaDeClasses()` mostre el següent:
 
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

<br />

#### 🌻 Exercici 2

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

<br />

#### 💼 Exercici 3 

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

<br />

#### ☕ Exercici 4 

Modifica el següent programa per a que vaja demanant a l'usuari (Scanner i println) les alertes que vol posar. Quan l'usuari deixe en blanc la resposta, el programa finalitzarà.
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

<br />

#### ✨ Exercici 5

Programa el mètode `ferSalutacio()`. Crea tres objectes per a Sauldar en tres idiomes:

1. Anglés: Crea una classe de nom `HolaMonAngles` que extenga la classe `HolaMon`. Esta classe redefinirà la variable `frase` amb la salutació en angles (`"Hello World"`). Crea una variable amb un objecte d'aquesta classe. crida al mètode `saludar()` sobre aquesta variable.
2. Frances: Crea un objecte d'una classe anònima que extenga la classe `HolaMon`. Esta classe redefinirà la variable `frase` amb la salutació en frances (`Salut tout le monde`). Crida al mètode `saludar()` directament sobre aquest objecte, sense crear cap variable.
3. Español: Crea una variable d'un objecte d'una classe anònima que extenga la classe `HolaMon`. Esta classe redefinirà el mètode `setFrase()`, que establirà la variable `frase`. Crida al mètode `saludar()` sobre la variable.

```java
public class Main {

    abstract class HolaMon {
        private String frase;

        public abstract void setFrase();

        public void saludar(){
            setFrase();
            System.out.println(frase);
        }
    }

    public void ferSalutacio() {


    }

    public static void main(String... args) {
        Main myApp = new Main();
        myApp.ferSalutacio();
    }
}
```

<br />

#### 💒 Exercici 6

Afegeix una acció a cadascun dels botons utilitzant el mètode `addActionListener()` sobre cadascun d'ells. L'acció pot ser simplement un `print`.

- Button1: crea una classe `MyActionListener` que extenga la classe necessaria. Passa-li al mètode `addActionListener()` un objecte d'aquesta classe (sense crear cap variable).
- Button2: crea un variable amb un objecte d'una classe anònima i passa-li-la al mètode `addActionListener()`
- Button3: passa-li un objecte al mètode `addActionListener()` directament.

```java
import javax.swing.*;

public class Main extends JFrame {
    public static void main(String[] args) {
       new Main().start();
    }

    public void start() {
        JButton button1 = new JButton("Mega Button1");
        button1.setBounds(0,0, 400,200);
        add(button1);

        JButton button2 = new JButton("Mega Button2");
        button2.setBounds(0,200, 400,200);
        add(button2);

        JButton button3 = new JButton("Mega Button3");
        button3.setBounds(0,400, 400,200);
        add(button3);

        setSize(400,600);
        setLayout(null);
        setVisible(true);

    }
}
```
