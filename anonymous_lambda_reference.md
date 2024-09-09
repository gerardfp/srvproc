# Anonymous classes, Lambda expressions i Method references

https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html

## Anonymous classes

Les classes anònimes és quan creem un objecte i redeclarem alguns mètodes de la seva classe/interface al mateix temps. Java crea, al vol, una nova classe sense nom, que extén la orginal, per a aquest objecte.

### Classe anònima que exten una classe

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
### Classe anònima que implementa un interface

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

### Classe anònima que exten una classe abstracta

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
