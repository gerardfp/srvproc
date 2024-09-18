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

### 🧘 Exercici 1

Afegeix una acció a cada botó utilitzant el seu mètode `addActionListener()`. L'acció consisteix en imprimir el text `S'ha polsat el botó Mega ButtonX` (on `X` és el numero corresponent):

* `button1`: pasa-li al mètode `addActionListener()` per paràmetre una expresió lambda
* `button2` i `button3`: asigna una expressió lambda a una variable, i pasa la mateixa variable als dos botons. Observa que quan el botó faça la crida a la expressió lambda li passa un paràmetre `ActionEvent` que pots utlitzar per saber quin dels dos botons s'ha polsat.

```java
import javax.swing.*;
import java.awt.*;

public class Main extends JFrame {
    public static void main(String[] args) {
        new Main().start();
    }

    public void start() {
        JButton button1 = new JButton("Mega Button1");
        add(button1);

        JButton button2 = new JButton("Mega Button2");
        add(button2);

        JButton button3 = new JButton("Mega Button3");
        add(button3);

        setSize(600, 200);
        setLayout(new GridLayout(1, 3));
        setVisible(true);
    }
}
```

<br />

### 🏋️‍♀️ Exercici 2
La [Sucesión de Fibonacci](https://es.wikipedia.org/wiki/Sucesi%C3%B3n_de_Fibonacci) es la secuencia de números _0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ..._ en la que cada número es la suma de los números anteriores.
El siguiente programa pide repetidamente una posición al usuario y calcula el número que está en esa posición en la Sucesión.

El problema es que la implementación recursiva clásica del método `int fibonacci(int n)` es muy ineficiente y a partir de la posición `40` le empieza a costar calcular el número. Y claro, hasta que no acaba el cálculo de un número no nos pide el cálculo del segundo (desaprovechando, así, núcleos de la CPU). 

Podemos solucionar esto haciendo que se hagan los cálculos en segundo plano, y mientras se van haciendo calculos se le pueden ir pidiendo al usuario nuevas posiciones para calcular.
Conforme se vayan acabando los cálculos ya se irán imprimiendo...

Podemos hacer esto de de forma sencilla creando un **_hilo de ejecución virtual_** con el método `Thread.ofVirtual().start()`, pasándole un objeto que implemente el interface `Runnable`. Por suerte este interface solo tiene un método (`run()`) y podemos usar una expresión lambda para crear un objeto que lo implemente. Simplemente deberás poner el `System.out.println` dentro de la implementación del método `run()`.

```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        while(true) {
            System.out.println("Dime un numero: ");
            int posicion = scanner.nextInt();

            System.out.println("El número en la posicion " + posicion + " de la sucesión de fibonacci es: " + fibonaci(posicion));
        }
    }

    static int fibonaci(int n){
        if (n==0 || n==1) return n;
        return fibonaci(n-1) + fibonaci(n-2);
    }
}
```

<br />

### 🏄‍♀️ Exercici 3 

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

<br />

### 🤺 Exercici 4 

Este ejercicio es muy similar al ejercicio anterior. Solo que en lugar de una lista de numeros haremos el filtrado a una lista de objetos de clase `Estudiante`.

1. Crear una clase `Estudiante` con los atributos: `nombre` (String) y `nota` (int)
2. Crear una lista de estudiantes con sus nombres y notas.
3. Crea una clase MostradorDeAlumnos con un método `mostrar()` que reciba como parámetros la lista de alumnos y el `Filtro`
4. Modifica adecuadamente el interfaz `Filtro`
5. Utiliza el filtrado para mostrar los estudiantes:
    * Con nota superior o igual a 5
    * Cuyo nombre contenga la letra C
    * Cuyo nombre tenga más de cinco letras
    * Todos los alumnos (_sin filtro_)

<br />

### 🤹‍♂️ Exercici 5

Las `List` en java tiene un método `forEach` que permite realizar una acción _para cada_ uno de los elementos.
Implementa tu propio método `paraCada` en la clase `Almacen`.

1. Define un interface `Accion` con un método `hacerAccion` al que se le pasa un producto.
2. Define el método `paraCada` en la clase `Almacen` al que se le pase un objeto `Accion`. Este método harà un `for` de los productos de la `List` y por cada producto llamará al método `hacerAccion` de la `Accion` y le pasará el producto.
3. Utiliza el método `paraCada` en el objeto `almacen` del `main` para imprimir sus productos.


Extra Points: Haz que al método `paraCada`, además de la Accion se le pueda pasar un `Filtro`, y realice la acción solo a los elemento que pasen ese filtro.

```java

record Producto(String nombre, int precio) {}

class Almacen {

    List<Producto> productos;

    Almacen(List<Producto> productos) { this.productos = productos; }
}


public class Main {
    public static void main(String[] args) {

        Almacen almacen = new Almacen(List.of(new Producto("lapiz", 5), new Producto("boli", 6), new Producto("libro", 10)));

    }
}
```

<br />

### 🧗 Exercici 6

Encuentra algún método en la extensa API de Java en el que se pueda usar una expresión lambda, y pon un ejemplo. 

Si no encuentras ninguno te doy un ejemplo: utiliza el método `Collections.sort()` para ordenar una lista de strings de forma alfabética por el segundo caracter.
