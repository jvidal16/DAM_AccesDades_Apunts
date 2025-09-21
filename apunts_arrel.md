# RA1 Accès a Arxius i Gestió d'Interrupcions

## Objectius

* Gestió d'arxius Seqüencials i Aleatoris
* Gestió d'excepcions

## El Registre de Dades

Un registre serà una **unitat de dades lògica**. Per exemple, les dades personals d'una persona com (nom, cognom1, cognom2, adreça, telèfon) conformen un registre de dades.

Els registres poden tenir una **longitud fixa** o una **longitud variable**. Això afectarà al mètode d'accedir a les dades.

## Accès Seqüencial vs. Aleatori

Segons els tipus de registre l'accés a les dades podrà ser seqüencial o aleatori.

L'accés **seqüencial** vol dir que per llegir el registre `n`, s'han hagut de llegir tots els registres anteriors, `1..n-1`.

En canvi, en l'accés **aleatori**, accedir al registre `n`, es pot calcular seguint un càlcul matemàtic.

El càlcul seria: size\_register \* (n-1).

## Arxius en Java

En Java hi ha moltes classes per a poder manipular arxius. A més a més, hi ha unes classes "Clàsiques" unes de "Modernes" i també de "Legacy" o obsoletes.

Tambés cal diferenciar entre manipulació d'arxius, directoris i contingut.

## Classes basades en Caràcters
* FileRreader - caràcter a caràcter
* BufferedReader - més eficient, permet llegir línies
* InputStreamReader
* StringReader
* CharacterArrayReader
  
## Classes orientades al byte
* FileInputStream
* BufferedInputStream
* DataInputStream - llegeix tipus primitius de dades
* ObjectInputStream
* ByteArrayInputStream

## Classes Modernes NIO
* Files - mètodes statics readAlllines(), lines()
* Paths - crea objectes Path
* FileChannel - High-Performance
* ByteBuffer
* CharBuffer

## Classe "parser"
* Scanner
* StreamTokenizer

## Legacy
* RandomAccesFile
* LineNumberReader
* PushbackReader
* FilterReader/FilterInputStream

### Buffering

Per a millorar l'eficiencia de la lectura, es poden carregar en memoria un nombre de bytes predeterminat, en un buffer, així es minimitza el nombre d'accesos al disc --- degut al principi de localitat de dades.

En la pràctica és tradueix en que una classe d'accès basic a arxiu esta *aniuada* en una classe *buffer*.

```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))){
    String line;
    while(line = br.readLine(){
        //processa
    })
}
```

### Exemple d'accès amb classes NIO

```java
try (BufferedReader br = Files.newBufferedReader(
            Paths.get("src/prova1.csv")))
    {
        String linea;

        while(br.ready()) {
            linea = br.readLine();
            System.out.println(linea);
        }
    } catch (IOException e) {
        System.err.println("⚠ Error en llegir l'arxiu:" + e.getMessage());
    }
```

### Exemple Avançat amb Stream

La utilització d'Stream és una caràcterística relativament recent de Java. Permet treballar d'una manera molt diferent amb les dades.

```java
//Processar totes les linies
List<String> lines = Files.readAllLines(Paths.get("file.txt"));

//Processar l'Stream
try (Stream<String> stream = Files.lines(Paths.get("file.txt"))) {
    stream.forEach(System.out::println)
}
```

### Accés Seqüencial i Aleatòri

La lectura d'un arxiu és per defecte seqüencial.
Per canviar la posició de lectura dins l'arxiu utilitzem el mètodes '''skip(long n)''', que desplaça el cursor n bytes, positius o negatius.

