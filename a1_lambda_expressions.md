# Lambda expressions

- [Exercicis Lambda exressions](#exercicis-lambda-expressions)

## Overview

https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html

Una expresió lambda en Java permet crear objectes d'un interface **_d'un únic mètode_** d'una forma compacta. És molt semblant a les classes anònimes, però d'un forma més compacta.

Igual que amb les classes anònimes, creem un objecte i al mateix temps redeclarem un mètode del seu interface.


En el següent exemple definim l'interface `Animal`, amb **un únic mètode** (`parlar()`) i després instanciem l'objecte `gos` al mateix temps que redeclarem el mètode `parlar()`  de l'itnerface `Animal`. L'objecte `gos`, aleshores, __no__ és de classe `Animal` (de fet, `Animal` no és una classe) sinò d'una classe nova _sense nom_. La superclasse d'aquesta classe anònima és la classe `Object`.

```java
interface Animal {
    void parlar();
}

public class Main {
    public static void main(String[] args) {

        Animal gos = () -> System.out.println("Guau");

        gos.parlar();  // Guau
    }
}
```

La expressió lambda és `() -> System.out.println("Guau")`. 

<br />

Anem a vore com una expressió lambda és equivalent a una classe que implementa l'interface, o una classe anònima que implementa l'interface.

En este exemple creem la classe `Gos` que implementa l'interface i redeclara el mètode `parlar()`:

```java
interface Animal {
    void parlar();
}

// Creem una classe que implementa Animal
class Gos implements Animal {
    public void parlar(){
        System.out.println("Guau");
        System.out.println("Guau");
    }
}

public class Main {
    public static void main(String[] args) {

        Animal gos = new Gos();

        gos.parlar();   // Guau  Guau
    }
}
```

En este exemple, fem l'objecte `gos` d'una classe anònima que implementa l'interface _in situ_:
```java
interface Animal {
    void parlar();
}

public class Main {
    public static void main(String[] args) {

        // Creem una classe anonima que implementa Animal
        Animal gos = new Animal(){
            public void parlar(){
                System.out.println("Guau");
                System.out.println("Guau");
            }
        };

        gos.parlar();  // Guau  Guau
    }
}
```

Tornem a vore l'exemple amb una expressió lambda que implenta l'interface _in situ_:

```java
interface Animal {
    void parlar();
}

public class Main {
    public static void main(String[] args) {

        // Creem l'objecte gos a partir d'un lambda que redeclara el únic mètode parlar()
        Animal gos = () -> {
            System.out.println("Guau");
            System.out.println("Guau");
        };

        gos.parlar();    // Guau  Guau
    }
}
```

En definitiva, fer una expressió lambda és com una classe anònima pero llevant el nom de classe i del mètode, i deixant només els parèntesis del mètode.

<br />





### ⚽️ Omitir els parèntesis `( )` del mètode

Quan s'implementa un mètode que **_només té un paràmetre_** se poden llevar inclús els parèntesis del mètode:

```java
interface Saludador {
    void saludar(String persona);
}

public class Main {
    public static void main(String[] args) {

                                    // Aci estem implementant el metode saludar(String persona)
        Saludador saludadorAngles = (persona) -> System.out.println("Hello " + persona);
        Saludador saludadorItalia = persona -> System.out.println("Ciao " + persona);


        saludadorAngles.saludar("Gerard");  // Hello Gerard
        saludadorItalia.saludar("Gerard");  // Ciao Gerard
    }
}
```
### 🏀 Omitir les claus `{ }`del bloc del mètode

Si el mètode a implementar **_només té una instrucció_**, es poden omitir les claus `{ }`:

```java
interface Symbol {
    void imprmir();
}

public class Main {
    public static void main(String[] args) {

        Symbol euro = () -> {
            System.out.println("€");
        };

        Symbol dolar = () -> System.out.println("$");


        euro.imprmir();     // €
        dolar.imprmir();    // $
    }
}
```


### 🥎 Omitir la paraula `return`

Si el mètode a implementar **_només té una instrucció return_**, es pot omitir la paraula `return` (i les claus `{ }`):
```java
interface Operacio {
    int realitzar(int numero1, int numero2);
}

public class Main {
    public static void main(String[] args) {

        Operacio suma = (numero1, numero2) -> { return numero1 + numero2; };
        Operacio resta = (numero1, numero2) -> numero1 - numero2;

        suma.realitzar(4,2);   // retorna 6
        resta.realitzar(7, 2);  // retorna 5
    }
}
```



<br />

## Exercicis Lambda expressions

<br />

#### 🏄‍♀️ Exercici 1 

La classe `MostradorDeNumeros` té el mètode `mostrar()` que rep una llista de números i un `Filtre`. Aquest métode fa un `for` dels números i només imprimeix els que
passen el filtre.

Fes tres crides al mètode `mostrar()` passant-li la llista `números` i tres filtres diferents.
* Que el número siga major a 4
* Que el número siga parell
* Que el número siga múltiple de 3

**Utilitza expressions lambda per a implementar els filtres**

```java
import java.util.List;

interface Filtre {
    boolean filtrar(int numero);
}


class MostradorDeNumeros {
    void mostrar(List<Integer> numeros, Filtre filtre) {
        for (var numero : numeros) {
            if (filtre.filtrar(numero)) {
                System.out.print(numero + " ");
            }
        }
        System.out.println();
    }
}


public class Main {
    public static void main(String[] args) {

        var numeros = List.of(2, 6, 4, 8, 1, 9, 3, 7, 5);

        MostradorDeNumeros mostradorDeNumeros = new MostradorDeNumeros();
        
    }
}
```