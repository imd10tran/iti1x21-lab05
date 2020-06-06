
# ITI1521 - Lab 05 - Interface, classe abstraite et héritage

## Soumission

Veuillez lire les [instructions junit](JUNIT.fr.md) pour obtenir
de l'aide avec l'exécution des tests pour ce laboratoire.

Veuillez lire attentivement les [Directives de soumission](SUBMISSION.fr.md).
Les erreurs de soumission affecteront vos notes.

Soumettez les réponses aux

* Book.java
* Library.java
* BookComparator.java
* Series.java
* AbstractSeries.java
* Arithmetic.java
* Geometric.java

# Première partie

## Objectifs d'apprentissages

* **Créer** soi-même une méthode **equals(Object o)**
* **Implémenter** un type paramétré, ici une interface
* **Utiliser** l'interface **java.util.Comparator\<T>**

Section 1: la méthode **equals**
--------------------------------

Comme vous le savez, la classe **Object** déclare une méthode **[equals](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#equals-java.lang.Object-)**.

```java
public class Object {
  // ...
  public boolean equals(Object obj) {
    return this == other;
  }
}
```


Puisqu'en Java, toutes les classes héritent des caractéristiques de la classe **Object.** Toutes les classes possèdent donc une méthode **boolean equals(Object obj)**. C'est vrai pour les classes prédéfinies, mais aussi pour toutes les classes que l'on crée soi-même.

On utilise la méthode **equals** afin de comparer les objets désignés par des **variables références.** Étant donné deux variables référence **a** et **b**, et les objets désignés:

* Pour comparer l'identité des objets (est-ce que **a** et **b** désignent le même objet ou non?), on utilise l'opérateur de comparaison «==» (donc **a == b** );
* Pour comparer le **contenu** des deux objets (est-ce que le **contenu** des objets désignés est logiquement équivalent?), on utilise la méthode **equals**" (donc **a.equals(b)**).

Pour cette première partie, vous devez créer la classe **Book**. Son implémentation est très simple. Voici les éléments que vous devrez intégrer:

* Une variable d'instance **author** de type **String**;
* Une variable d'instance **title** de  type **String**;
* Une variable d'instance **year** de type **int** (l'année de publication du livre);
* Un contructeur **Book(String author, String title, int year)**, qui reçoit en paramètre l'auteur, le titre, l'année de publication;
* Une méthode **toString()**, qui retourne une chaîne de caractère ayant le format suivant: «author: title (year);
* Une méthode **equals(Object o)**.

Votre implémentation doit être robuste. En particulier, une méthode **equals** ne devrait jamais produire une erreur d'exécution. Les objets sont logiquement équivalents ou ils ne le sont pas.

### Question 1.1:

Implémentez la classe **Book**. Utilisez le [gabarit fourni] () comme point de départ.

* Book.java

Section 2: l'interface **java.util.Comparator\<T>**
----------------------------------------------------

Pour votre dernier laboratoire, vous avez utilisé l'interface [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html) pour creér des objets de la classe **Post**. Un objet qui implémente cette interface doit redéfinir la méthode **compareTo** et ainsi être capable de se comparer à un autre objet.

Contrairement à Comparable, une classe implémentant l'interface **Comparator** est externe au type d'élément que nous comparons. Une telle classe doit implémenter les méthodes **compare** et **equals** pour définir et imposer l'ordre de tri.

Ainsi, la méthode **sort** de la classe **[Collections.sort](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-java.util.Comparator-)** or **[Arrays.sort](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-java.util.Comparator-)** utilisera la méthode compare du comparateur (objet réalisant l'interface Comparator). Il nous faut donc une méthode compare qui puisse traiter le type d'éléments se trouvant dans la liste (objet de la classe [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html), par example).

Puisque **Comparator** est une interface, il lui faut une implémentation concrète.

Notez que la classe **ArrayList** possède un paramètre de type. Pour ce laboratoire, nous déclarons des variables de type **ArrayList<Book>**. Ainsi, l'instance ne reçoit que des objets de la classe **Book**. Du coup, la méthode sort doit recevoir un comparateur pour des objets de la classe **Book**. Ainsi, **BookComparator** réalisent l'interface **Comparator<Book>**. Le grand avantage de cette structure de programme, c'est que l'on peut comparer les objets selon plusieurs critères (par author, par title, par year) et donc trier les éléments en fonctions de critères variés.

Vous devez créer une classe **Library** afin de sauvegarder des objets de la classe **Book**. Pour ce faire, nous utilisons à nouveau **ArrayList**. De plus, on souhaite trier les éléments de la liste à l'aide d'un objet **Comparator**. Vous devez trier les éléments de la liste d'abord par auteur, ensuite par titre, et finalement selon l'année de publication (bien sûr, dans une application complète, il y aurait plusieurs comparateurs).

La classe **Library** a les caractéristiques suivantes:

* **void addBook (Book b)**: ajoute un livre à la bibliothèque (ajout d'un objet de la classe **Book** à l'aide de la méthode **addBook** de l'objet de la classe **Library**);
* **void sort()**: trie les éléments à l'aide d'un objet **Comparator**;
* **void printLibrary()**: affiche tous les livres dans l'ordre actuel de la liste.

### Question 1.2:

Vous devez compléter l'implémentation de la classe **Library** ainsi que **BookComparator**. Utilisez les gabarits fourni pour Library et BookComparator comme point de départ.

* Library.java
* BookComparator.java

# Deuxième partie

## Objectifs d'apprentissage

* **Déclarer** et **implémenter** une interface
* **Créer** classe abstraite
* **Mieux comprendre** l'héritage en Java

Classes abstraites et interfaces
-------------------------------

En mathématique, une série est une séquence infinie de termes qui sont additionnés les uns aux autres. La **somme partielle d'une série**, <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n">, est la somme des <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20n"> premiers termes..

<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20a_i">
</div>

Vous devez concevoir une hiérarchie de classes, conforme au diagramme UML ci-bas, de sorte que toutes les séries possèdent une méthode **next**, qui retourne la prochaine valeur <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n">. Le premier appel à la méthode **next** retournera <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_1">, soit <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a_1">. le prochain appel à la méthode **next** retournera <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_2">, soit <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a_1&plus;a_2">. le prochain appel à la méthode **next** retournera <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_3">, soit <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a_1&plus;a_2&plus;a_3"> et ainsi de suite. L'implémentation de la méthode **next** dépend du type de série, **Arithmetic** ou **Geometric** pour cette question. Cette hiérarchie comprend l'interface **Series**, une classe abstraite nommée **AbstractSeries**, ainsi que deux implémentations concrètes, **Arithmetic** et **Geometric**.

![UML display](uml_series-720.png)

Voici un programme test pour illustrer le résultat attendu pour:

* une class pour la somme partielle de l'Arithmetic série avec 1 comme le premier élément and le raison 1;
* une class pour la somme partielle de la Geometric série avec 1 comme le premier élément and le raison 0.5.

```java
public class TestSeries {

  public static void main(String[] args) {

    AbstractSeries sn;
    double[] tuple;

    sn = new Arithmetic();

    System.out.println("The first five terms of the arithmetic series are:");

    for (int n=0; n<5; n++) {
      System.out.println(sn.next());
    }

    sn = new Geometric();

    System.out.println();
    System.out.println("The first five terms of the geometric series are:");

    tuple = sn.take(5);

    for (int n=0; n<5; n++) {
      System.out.println(tuple[n]);
    }

  }
}
```


Here is the expected output:

```java
The first five terms of the arithmetic series are:
1.0
3.0
6.0
10.0
15.0
The first five terms of the geometric series are:
1.0
1.5
1.75
1.875
1.9375
```

La première boucle affichera les valeurs 1.0, 3.0, 6.0, 10.0, 15.0, alors que la deuxième affichera, 1.0, 1.5, 1.75, 1.875, 1.9375.

### Question 2.1: Series

Vous devez concevoir une interface nommée **Series**. Elle doit déclarer une méthode nommée **next** dont le type de la valeur de retour est **double**.

Utilisez le **gabarit fourni** comme point de départ.

* Series.java

### Question 2.2: AbstractSeries

Donnez l'implémentation d'une classe abstraite nommée **AbstractSeries**. Elle doit réaliser l'interface **Series**. La classe implémente la méthode **take** qui retourne un tableau contenant les prochaines **k** sommes partielles de cette série, où **k** est le paramètre formel de la méthode **take**.

Utilisez le **gabarit fourni** comme point de départ.

* AbstractSeries.java

### Question 2.3: Arithmetic

Donnez l'implémentation de la classe **Arithmetic**. Elle doit être une sous-classe de la classe **AbstractSeries**. Pour cette implémentation, le premier appel à la méthode **next** retournera la valeur 1.0, le second appel retournera la valeur 3.0, le troisième appel retournera la valeur 6.0, le quatrième appel retournera la valeur 10.0, et ainsi de suite. De façon générale, le <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20i">ème appel de la méthode retournera <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_%7Bi-1%7D&plus;i">, pour  <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20i%5Cin1%2C2%2C3..."> et <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_%7Bi-1%7D"> est la valeur de l'appel précédent de la méthode **next**.

<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20i">
</div>

Voici les 5 premières sommes partielles de la série arithmétique:
<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_1%20%3D%201"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_2%20%3D%201&plus;2%3D%203"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_3%20%3D%201&plus;2&plus;3%20%3D%206"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_4%20%3D%201&plus;2&plus;3&plus;4%20%3D%2010"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_5%20%3D%201&plus;2&plus;3&plus;4&plus;5%20%3D%2015"><br/>
</div>

Utilisez le **gabarit fourni** comme point de départ.

* Arithmetic.java

### 2.3 Geometric

Vous devez implémenter la classe **Geometric**. Cette classe est une sous-classe de **AbstractSeries**. Chaque appel de la méthode **next** produit la prochaine somme partielle de cette série selon l'équation ci-bas. Le premier appel produira la valeur 1.0, le second appel produira la valeur 1.5, le troisième appel produira la valeur 1.75, etc. Vous pouvez utiliser la méthode **Math.pow( a, b )**, qui retourne <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20a%5Eb">, pour vos calculs.

<div align="center">
<img src = "https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n%20%3D%20%5Csum_%7Bi%3D0%7D%5E%7Bn%7D%5Cfrac%7B1%7D%7B2%5Ei%7D">
</div>

Voici les 5 premières sommes de la série géométrique:

<div align="center">
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_1%20%3D%201"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_2%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D%3D%201.5"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_3%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D&plus;%5Cfrac%7B1%7D%7B4%7D%3D%201.75"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_4%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D&plus;%5Cfrac%7B1%7D%7B4%7D&plus;%5Cfrac%7B1%7D%7B8%7D%3D%201.875"><br/>
<img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_5%20%3D%201&plus;%5Cfrac%7B1%7D%7B2%7D&plus;%5Cfrac%7B1%7D%7B4%7D&plus;%5Cfrac%7B1%7D%7B8%7D&plus;%5Cfrac%7B1%7D%7B16%7D%20%3D%201.9375"><br/>
</div>

Chaque appel à la méthode **next** produit la prochaine somme partielle, c'est-à-dire la prochaine valeur de <img src="https://latex.codecogs.com/gif.latex?%5Cfn_cm%20S_n">.

Utilisez le **gabarit fourni** comme point de départ

* Geometric.java

## Ressources

* [https://docs.oracle.com/javase/tutorial/getStarted/application/index.html](https://docs.oracle.com/javase/tutorial/getStarted/application/index.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html](https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html)
