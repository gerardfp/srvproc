# Cryptography

* [Exercicis Cryptography](#exercicis-cryptography)

## Overview

https://docs.oracle.com/en/java/javase/23/security/java-cryptography-architecture-jca-reference-guide.html


### 🔐 String ↔ byte[] ↔ Base64

```java
// String to byte[]
byte[] bytes = "un texto".getBytes();

// byte[] to String
String texto = new String(bytes);

// byte[] --> Base64
String enBase64 = Base64.getEncoder().encodeToString(bytes);

// Base64 --> byte[]
byte[] bytes = Base64.getDecoder().decode(enBase64);

```

### 🔐 SecureRandom

```java
SecureRandom random = new SecureRandom();
byte[] store = new byte[16];
random.nextBytes(store);
```

### 🔐 KeyGenerator

```java
KeyGenerator keyGenerator = KeyGenerator.getInstance("AES");
keyGenerator.init(256);
SecretKey secretKey =  keyGenerator.generateKey();
```

### 🔐 KeyPairGenerator

```java
KeyPairGenerator keyGen = KeyPairGenerator.getInstance("RSA");
keyGen.initialize(2048);
KeyPair keypair = keyGen.generateKeyPair();

PrivateKey privateKey = keypair.getPrivate();
PublicKey publicKey = keypair.getPublic();
```

<br>
<hr>
<br>

### 🔐 MessageDigest

```java
MessageDigest md = MessageDigest.getInstance("SHA-256");
byte[] hashedBytes = md.digest(bytes);
```

### 🔐 Signature

```java
Signature signature = Signature.getInstance("SHA256withRSA");

// firmar
signature.initSign(privateKey);
signature.update(bytes);
byte[] dataSignature = signature.sign();

// validar firma
signature.initVerify(publicKey);
signature.update(bytes);
boolean valid = signature.verify(dataSignature);
```

### 🔐 Cipher

#### 🥄 Symmetric

```java
Cipher cipher = Cipher.getInstance("AES");

// Crypt
cipher.init(Cipher.ENCRYPT_MODE, secretKey);
byte[] encryptedBytes = cipher.doFinal(bytes);

// Decypt
cipher.init(Cipher.DECRYPT_MODE, secretKey);
byte[] decryptedBytes = cipher.doFinal(encryptedBytes);
```

#### 🍴 Asymmetric

```java
Cipher cipher = Cipher.getInstance("RSA");

// Crypt
cipher.init(Cipher.ENCRYPT_MODE, publicKey);
byte[] encryptedBytes = cipher.doFinal(bytes);

// Decrypt
cipher.init(Cipher.DECRYPT_MODE, privateKey);
byte[] decryptedBytes = cipher.doFinal(encryptedBytes);

```


<br />

## Exercicis Cryptography

### 🧬 Exercici 1: Hashing seguro de contraseñas con SHA-256

Escribe un programa que permita registrar usuarios y almacenar sus contraseñas de forma """_segura_""" utilizando SHA-256. Luego, implementa una función que valide una contraseña ingresada contra su hash almacenado.

El programa debe mostrar el siguiente Menú:
```
1. Registrar usuario
2. Iniciar sesión
3. Salir
Seleccione una opción: 
```

Cuando un usuario inicie sesión se le debe mostrar este Menú:
```
1. Mostrar usuarios y passwords
2. Cerrar sesión
Seleccione una opción:
``

* El programa debe evitar que se dupliquen los nombres de usuario
* Los usuarios los puedes almacenar en un HashMap.

**Extra**: prueba a añadir un poco de sal 🧂🧂🧂 a los passwords

<br />

### 🪦 Exercici 2: Cifrado y descifrado simétrico con AES

Implementa un programa que permita al usuario cifrar y descifrar un mensaje de texto usando el algoritmo AES con una clave secreta.

<br />

### 🧪 Exercici 3: Cifrado y descifrado asimétrico con RSA

Implementa un programa que permita al usuario cifrar y descifrar un mensaje de texto usando usando un par de claves pública/privada.

<br />

### 🔭 Exercici 4: Generación y verificación de un hash seguro con SHA-256

Crea un programa que calcule el hash SHA-256 de un archivo y permita verificar si su contenido ha sido alterado comparándolo con un hash previamente almacenado.

<br />

### 🧽 Exercici 5: Cifrado de archivos con AES

Desarrolla un programa que solicite al usuario seleccionar un archivo y lo cifre utilizando AES. Luego, implementa una opción para descifrar el archivo con la clave secreta.

<br />

### 🪆 Exercici 6: Cifrado de archivos con RSA

Desarrolla un programa que solicite al usuario seleccionar un archivo y lo cifre utilizando la clave pública RSA. Luego, implementa una opción para descifrar el archivo con la clave privada.

<br />

### 🪅 Exercici 7: Firma digital con RSA

Desarrolla un programa que solicite al usuario seleccionar un archivo y genere una firma digital con RSA sobre dicho archivo. Luego, otro usuario podrá verificar si la firma es válida.
