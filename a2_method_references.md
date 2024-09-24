# Method references

- [Exercicis Method references](#exercicis-method-references)

## Overview

https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html

Un **method reference** en Java és una forma simplificada de referir-se a un mètode sense haver d'escriure una lambda completa. En lloc d'escriure un bloc de codi anònim que únicament invoca a un mètode, pots utilitzar una referència directa a aquest mètode. S'utilitza l'operador `::`

Per exemple, en lloc d'escriure una lambda com `str -> System.out.println(str)`, pots usar una method reference: `System.out::println`.

<br />

<!--
### Tipus de Method references

N'hi han quatre tipus de _method references_:

| Tipus | Sinstaxi | Exemples |
|-|-|-|
| Referencia a un mètode static |	`ContainingClass::staticMethodName` |	`Person::compareByAge` <br /> `MethodReferencesExamples::appendStrings` |
| Referencia a un metode d'un objecte | `containingObject::instanceMethodName` |	`myComparisonProvider::compareByName` <br /> `myApp::appendStrings2` |
| Referencia a un metode de l'objecte que se passa com a primer paràmetre |	`ContainingType::methodName` | `String::compareToIgnoreCase` <br /> `String::concat` |
| Referencia a un constructor | `ClassName::new` | `HashSet::new` |
-->

### 🥹 Referencia a un mètode static

Si dintre d'una lambda només fem una crida a un métode static, i els parámetres que rep la lambda li'ls passem **_en el mateix ordre_** a aquest mètode static, aleshores podem
transformar la lambda en un _method reference_.


En el següent exemple, l'objecte `myLambda` implementa el mètode `doMyLambda(int a)` i només fa una crida al mètode static `mètode(int a)`, i a més a més, el paràmetre `int a` que rep li'l passa al mètode en el mateix ordre:

```java
interface MyLambda {
    void doMyLambda(int a);
}

class MyClass {
    static void metode(int a) {
        System.out.println("The number is " + a);
    }
}

public class Main {
    public static void main(String[] args) {

        MyLambda myLambda = MyClass::metode;

        myLambda.doMyLambda(7);
    }
}
```

### 😢 Referencia a un mètode d'un objecte

Si dintre d'una lambda només fem una crida a un métode d'un objecte, i els parámetres que rep la lambda li'ls passem **_en el mateix ordre_** a aquest mètode, aleshores podem
transformar la lambda en un _method reference_.

En el següent exemple, l'objecte `myLambda` implementa el mètode `doMyLambda(int a)` i només fa una crida al mètode `mètode(int a)` de l'objecte `myObject`, i el paràmetre `int a` que rep li'l passa al mètode en el mateix ordre:

```java
interface MyLambda {
    void doMyLambda(int a);
}

class MyClass {
    void metode(int a) {
        System.out.println("The number is " + a);
    }
}

public class Main {
    public static void main(String[] args) {

        MyClass myObject = new MyClass();

        MyLambda myLambda = myObject::metode;
        
        myLambda.doMyLambda(7);
    }
}
```

### 😭 Referencia a un mètode de l'objecte que se passa com a primer paràmetre

Si dintre d'una lambda només fem una crida a un métode del objecte que se passa per primer paràmetre, i la resta de parámetres li'ls passem **_en el mateix ordre_** a aquest mètode, aleshores podem
transformar la lambda en un _method reference_.

En el següent exemple, l'objecte `myLambda` implementa el mètode `doMyLambda(int a)` i només fa una crida al mètode `mètode(int a)` de l'objecte `myObject` que se li passa com a primer paràmetre, i el paràmetre `int a` que rep li'l passa al mètode en el mateix ordre:

```java
interface MyLambda {
    void doMyLambda(MyClass myObject, int a);
}

class MyClass {
    void metode(int a) {
        System.out.println("The number is " + a);
    }
}

public class Main {
    public static void main(String[] args) {
        
        MyLambda myLambda = MyClass::metode;

        myLambda.doMyLambda(new MyClass(), 7);
    }
}
```

### 🤯 Referencia a un constructor

Si dintre d'una lambda només fem una crida a un métode constructor, i els parámetres que rep la lambda li'ls passem **_en el mateix ordre_** a aquest mètode constructor, aleshores podem
transformar la lambda en un _method reference_.

En el següent exemple, l'objecte `myLambda` implementa el mètode `doMyLambda(int a)` i només fa una crida al constructor de la classe `MyClass`,  i el paràmetre `int a` que rep li'l passa al constructor en el mateix ordre:

```java
interface MyLambda {
    void doMyLambda(int a);
}

class MyClass {
    MyClass(int a) {
        System.out.println("The number is " + a);
    }
}

public class Main {
    public static void main(String[] args) {

        MyLambda myLambda = MyClass::new;

        myLambda.doMyLambda(7);
    }
}
```

<br />

## Exercicis Method references

<br />

### 🥾 Exercici 1 

```java
import java.util.ArrayList;
import java.util.List;

interface MyLambda<A> {
    A doMyLambda(String name);
}

class CreadorDeObjetos {
    static <T> List<T> crear(MyLambda<T> lambda, List<String> names) {
        List<T> objects = new ArrayList<>();

        for(String name : names) {
            objects.add(lambda.doMyLambda(name));
        }

        return objects;
    }
}

record Ciudad (String name) {}

class Album {
    String titulo;
    Album(String titulo){ this.titulo = titulo; }
    public String toString() { return "Album{titulo='" + titulo + "'}"; }
}

class Coche {
    String marca;
    Coche(String marca){ this.marca = marca; }
    public String toString() { return "Coche{marca='" + marca + "'}"; }
}


public class Main {
    public static void main(String[] args) {

        var coches = CreadorDeObjetos.crear(Coche::new, List.of("Seat", "Fiat", "Audi"));
        var asignaturas = CreadorDeObjetos.crear(Album::new, List.of("Abbey Road", "Nevermind", "The Wall"));
        var ciudades = CreadorDeObjetos.crear(Ciudad::new, List.of("Paris", "Londres", "Burriana"));

        coches.forEach(System.out::println);
        asignaturas.forEach(System.out::println);
        ciudades.forEach(System.out::println);
    }
}
```

### 👢 Exercici 2

```java
record Estudiant (String nom, Integer edat, Boolean matriculat) {

    public boolean comparaPerEdat(Estudiant b) {
        return edat > b.edat;
    }

    public boolean comparaPerNom(Estudiant b) {
        return nom.charAt(0) > b.nom.charAt(0);
    }

    public boolean comparaPerMatriculat(Estudiant b){
        return !matriculat;
    }
}

interface Comparador<T> {
    boolean comparar(T a, T b);
}

class MyFixedSizeArrayList<T> {
    T[] elements;

    MyFixedSizeArrayList(T ...elements) {
        this.elements = elements;
    }

    void ordenar(Comparador<T> comparador) {
        for (int i = 0; i < elements.length - 1; i++) {
            for (int j = 0; j < elements.length - 1 - i; j++) {
                if (comparador.comparar(elements[j], elements[j + 1])) {
                    T temp = elements[j];
                    elements[j] = elements[j + 1];
                    elements[j + 1] = temp;
                }
            }
        }
    }

    void mostrar() {
        for (T element : elements) System.out.println(element);
    }
}

public class Main {
    public static void main(String[] args) {

        var estudiants = new MyFixedSizeArrayList<>(
                new Estudiant("Annia", 43, true),
                new Estudiant("Lluis", 65, false),
                new Estudiant("Manel", 21, true),
                new Estudiant("Tania", 30, false),
                new Estudiant("Ramon", 18, true),
                new Estudiant("Carla", 80, false),
                new Estudiant("Joana", 73, false),
                new Estudiant("Patri", 65, true),
                new Estudiant("Vanne", 10, false)
        );

        System.out.println("ORDENATS PER NOM");
        estudiants.ordenar(Estudiant::comparaPerNom);
        estudiants.mostrar();

        System.out.println("ORDENATS PER EDAT");
        estudiants.ordenar(Estudiant::comparaPerEdat);
        estudiants.mostrar();

        System.out.println("ORDENATS PER MATRICULAT");
        estudiants.ordenar(Estudiant::comparaPerMatriculat);
        estudiants.mostrar();
    }
}
```
