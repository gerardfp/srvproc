# Method references

- [Exercicis Method references](#exercicis-method-references)

## Overview

https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html

Un **method reference** en Java és una forma simplificada de referir-se a un mètode sense haver d'escriure una lambda completa. En lloc d'escriure un bloc de codi anònim que únicament invoca a un mètode, pots utilitzar una referència directa a aquest mètode. S'utilitza l'operador `::`

Per exemple, en lloc d'escriure una lambda com `str -> System.out.println(str)`, pots usar una method reference: `System.out::println`.

<br />

### Tipus de Method references

N'hi han quatre tipus de _method references_:

| Tipus | Sinstaxi | Exemples |
|-|-|-|
| Referencia a un mètode static |	`ContainingClass::staticMethodName` |	`Person::compareByAge` <br /> `MethodReferencesExamples::appendStrings` |
| Referencia a un metode d'un objecte | `containingObject::instanceMethodName` |	`myComparisonProvider::compareByName` <br /> `myApp::appendStrings2` |
| Referencia a un metode de l'objecte que se passa com a primer paràmetre |	`ContainingType::methodName` | `String::compareToIgnoreCase` <br /> `String::concat` |
| Referencia a un constructor | `ClassName::new` | `HashSet::new` |

### 🥹 Referencia a un mètode static

> [!WARNING]
> Només podem simplificar una lambda amb una referència a un mètode static quan els paràmetres de la lambda se passen **_en el mateix ordre_** al mètode que crida la lambda:
>
>* ```(a, b) -> algun.mètode(a, b)```  sí ho podem simplificar amb ```algun::metode```
>* ```(a, b) -> unaltre.metode(b)``` no ho podem simplificar
>* ```(a, b) -> unmes.metode(a)``` no ho podem simplificar
>* ```(a, b) -> other.metode()``` no ho podem simplificar

En el següent exemple, l'objecte `myLambda` implementa el mètode `doMyLambda(int a)` i només fa una crida al mètode static `mètode(int a)`, i el paràmetre `int a` que rep li'l passa al mètode en el mateix ordre:

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

En el següent exemple, l'objecte `myLambda` implementa el mètode `doMyLambda(int a)` i només fa una crida al mètode `mètode(int a)` de l'bjecte `myObject`, i el paràmetre `int a` que rep li'l passa al mètode en el mateix ordre:

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
