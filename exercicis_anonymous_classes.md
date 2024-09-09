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

```bash
PastorBelga  >  Pastor  >  Gos  >  Animal  >  java.lang.Object
```
