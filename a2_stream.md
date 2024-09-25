```java
import java.time.LocalDateTime;
import java.time.temporal.ChronoUnit;
/*
Un algoritmo Proof-of-Work (PoW) es el que se utiliza en algunas criptomonedas, como Bitcoin, para validar transacciones y crear nuevos bloques en la cadena de bloques. Básicamente, PoW consiste en encontrar un valor que, cuando se combina con los datos de un bloque y se pasa por una función hash (como SHA-256), produce un hash que cumple con ciertas condiciones, como tener un número determinado de ceros al inicio.
Para implementar un algoritmo de Proof-of-Work sencillo sin utilizar librerías de cifrado como SHA-256, podemos utilizar un hash casero basado en operaciones simples como la suma y manipulación de caracteres. Aunque este no es seguro ni eficiente como SHA-256, es útil para fines de ilustración.
 */
public class Main {

    public static void main(String[] args) {
        String data = "0.000001 btc for you";  // Datos que queremos incluir en el bloque
        int difficulty = 3;  // Número de ceros iniciales requeridos en el hash
        System.out.println("Iniciando el proceso de minado...");
        var start = LocalDateTime.now();

        String hash = mineBlock(data, difficulty);

        var end = LocalDateTime.now();
        System.out.println("Hash encontrado: " + hash);
        System.out.println("Tiempo tardado: " + ChronoUnit.MILLIS.between(start, end));
    }

    public static String mineBlock(String data, int difficulty) {
        String target = String.format("%0"+difficulty+"d", 0);
        System.out.println(target);


        for (int nonce = 0; ; nonce++) {
            String hash = simpleHash(data + nonce);

            //System.out.println(hash);
            // Verifica si el hash comienza con el número deseado de ceros
            if (hash.startsWith(target)) {
                System.out.println("Bloque minado! Nonce: " + nonce);
                return hash;
            }
            nonce++;
        }
    }

    // Un algoritmo de hashing sencillo basado en operaciones de suma y manipulación de caracteres
    public static String simpleHash(String input) {
        int hash = 0;

        for (int i = 0; i < input.length(); i++) {
            hash += input.charAt(i) * (i + 1) ;  // Acumulamos el valor ASCII del carácter multiplicado por su posición
            hash = Integer.rotateRight(hash, i);
        }

        return String.format("%08x", hash); // 16 caracteres
    }
}

```
