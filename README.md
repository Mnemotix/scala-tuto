# Apprendre Scala

## Installation

	brew install scala --with-docs
	
### Hello World !

Créer un fichier hello.scala

```scala
// hello.scala
object HelloWorld {	def main(args: Array[String]) {		println("Hello, world!")	}}
```

__Compiler__

	scalac hello.scala
	
__Exécuter__

	scala -classpath . HelloWorld

## Interactions avec Java

Il est possible d'importer n'importe quelle classe JAVA en SCALA avec la commande `import` qui fonctionne de manière très similaire à celle de JAVA.

```scala
import java.util.{Date, Locale}import java.text.DateFormatimport java.text.DateFormat._

object FrenchDate {	def main(args: Array[String]) {		val now = new Date		val df = getDateInstance(LONG, Locale.FRANCE)		println(df format now)	}}
```
En Scala, l'espace est équivalent au `.` et les parenthèses peuvent être omises, ainsi La commande `df format now` est équivalente à `df.format(now)`


## Les bases

### Les variables

Les variables en scala peuvent se déclarer avec `var`ou `val`. Dans le cas des `val` il s'agira d'une constante (équivalent à `final`en Java).

```scala
var i = 3
val j = 3

i = 4 // => 4
j = 4 // => error: reassignment to val
```

### Les types

![scala types](http://puu.sh/iQrUQ/5562eca36f.png =600x)

__Any, AnyVal et AnyRef__

La classe `Any` est à la racine de toutes les classes Scala, c'est une classe abstraite. Tous les objets scala héritent directement ou indirectement de cette classe. Les classes `AnyVal` ou `AnyRef` sont les classes filles du type `Any` et permettent de séparer les objets de type *valeur* des objets de type *reference*. Les objets de type *reference* sont définis comme étant tous les objets qui ne sont pas des valeurs.

Par exemple la classe String hérite de `AnyRef`

```scala
val str = "Coucou"
str.isInstanceOf[AnyRef] // => true
```

__Inférence de type__

Le compilateur Scala peut inférer le type de certaines variables tout seul. Ce n'est cependant pas toujours le cas. Il est parfois nécessaire de lui passer un type de manière explicite : 

```scala
var a:Int = 3     // => type explicite
var b = 3.3		  // => type implicite (Double)
var c = 3.3		  // => Double
var d = .3		  // => Double
var e = 'f'		  // => Char
var f = "chaine"  // => String
var g = 'chaine'  // => error: unclosed character literal
var h = 1 < 2	  // => Boolean : true
```

__Le type *Unit*__

Le type Unit est utilisé pour désigner les fonctions qui n'ont pas de valeur de retour. C'est l'équivalent du type `void` en Java. Par défaut, toutes les fonctions scala retournent le type `Unit`.

__Les types Nothing et Null__

Les types `Nothing` et `Null` sont tout en bas de la hiérarchie de type Scala. 

Le mot-clé `null` en Scala ne devrait jamais être utilisé, mais pour des raisons de compatibilité avec Java, il existe et Null est son type.

`Null` est un type réservé aux objets de type référence, il est impossible d'assigner une valeur `null` à une variable de type value.

```scala
scala> var s = null
s: Null = null

scala> var i:Int = null     // Error
scala> var i:String = null  // OK
```

Nothing est un type assignable à n'importe quel objet en Scala. C'est un trait qui est garanti comme n'ayant aucune instance possible. Ce type est en général utilisé pour signaler qu'un processus s'est terminé de façon inattendue.

En pratique, les valeures nulles ne devraient jamais être utilisées. Il est toujours préférable d'utiliser le type `Option` aux valeurs nulles.

### Le type String

En Scala, le type String est construit par dessus le type String Java et se déclarent uniquement avec des guillements doubles (double quotes), les guillemets simples étant réservés au variables de type `Char`. 

```scala
scala> var s = "Hello"
s: String = Hello

scala> var s = 'Hello'
<console>:1: error: unclosed character literal
var s = 'Hello'

scala> var c = 'M'
c: Char = M
```

Les String en Scala ajoutent quelques fonctionnalités utilitaires comme l'interpolation. L'interpolation est un équivalent de `String.format` en Java ou du `sprintf`en C et permet de passer des variables à l'intérieur d'une chaine.

L'interpolation en Scala se déclare en préfixant une chaine de l'opérateur `s` avant le premier guillemet. Ensuite les valeurs sont déclarées par des `$` à l'intérieur de la chaine.

```scala
val bookTitle = "Beginning Scala" // creating a Strings"Book Title is ${ bookTitle}" // String interpolation
// ou 
s"Book Title is $bookTitle" // String interpolation
```

### Les fonctions

Scala dispose à la fois des fonctions et des méthodes.
Les méthodes font partie des classes qui ont un nom et une signature alors que les fonctions sont des objets à part entière et peuvent être assignés à des variables.

#### Les fonctions sans paramètres

Les fonctions sont définies par le mot-clé `def` suivi par le nom de la fonction et le corps de la fonction.

```scala
scala> def hello() = {"Hello World!"}hello: ()String
```

Il est également possible de préciser le type de la valeur de retour : 

```scala
scala> def hello():String = {"Hello World!"}hello: ()String
```

En Scala, le mot-clé `return` n'est pas obligatoire, par défaut c'est la dernière ligne du corps de la fonction qui sera considérée comme étant la valeur de retour.

```scala
scala> def hello():String={
     | val str = "Hello world!"
     | println(str)
     | str
     | }
hello: ()String

scala> hello
Hello world!
res4: String = Hello world!
```

#### Les fonctions avec paramètres

Les fonctions avec paramètres fonctionnent exactement de la même manière que les autres

```scala
def square (i:Int) = {i*i}
def add(x: Int, y: Int): Int = { x + y }
def concat(prefix: String, body: String, suffix: String): String = s"$prefix $body $suffix"
```

### Les tableaux, les listes, les ranges et les tuples

#### Les tableaux
Les tableaux sont des structures qui contiennent une collection d'éléments de même type. Les éléments sont associés à un index qui permet d'accéder, de remplacer ou de supprimer un élément.

Les tableaux peuvent être affectés de deux manières : 

soit on spécifie le nombre total d'éléments et on assigne les valeurs ultérieurement 

```scala
var books:Array[String] = new Array[String](3)   // sans inference de type
var books = new Array[String](3)                 // inference de type
```

soit on assigne toutes les valeurs d'un coup.

```scala
var books = Array("Beginning Scala", "Beginning Java", "Beginning Groovy")
```

On accède aux éléments par l'index :

```scala
println(books(0))
```

#### Les listes
Les listes sont

```scala
// liste vide
val empty: List[Nothing] = List()
// ou 
val empty = Nil

// assignation à la volée
val books: List[String] = List("Beginning Scala", "Beginning Groovy", "Beginning Java")
```

Les listes nous permettent d'accéder au premier élément avec `head` et `tail` nous permet de récupérer tous les éléments sauf le premier.

```scala
scala> books.headres0: String = " Beginning Scala"scala> books.tailres1: List[String] = List(Beginning Groovy, Beginning Java)scala>
```

#### Les Ranges

Les ranges sont définis par un début, une fin et éventuellement la valeur d'un pas. Pour créer des ranges en scala il existe plusieurs opérateurs

```scala
scala> 1 to 5res0: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5)scala>

scala> 1 until 5res1: scala.collection.immutable.Range = Range(1, 2, 3, 4)scala>

scala> 1 to 20 by 4res2: scala.collection.immutable.Range = Range(1, 5, 9, 13, 17)scala>
```

Les ranges sont des objets qu'il est possible d'assigner à une variable.

```scala
scala> var r = 1 to 5
r: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5)
scala> r.reverse
res9: scala.collection.immutable.Range = Range(5, 4, 3, 2, 1)
```

#### Les Tuples
Les tuples sont des structures de données ordonnées qui, à la différence des tableaux et des listes, contiennent des valeurs de type potentiellement différent. Néanmoins, il est impossible d'itérer sur les valeurs d'un tuple. Le but est simplement de contenir des données. Il existe deux manières de créer des tuples en scala : 

En écrivant les valeurs entourées de parenthèses et séparées par des virgules ou en utilisant l'opérateur de relation `->`

```scala
val tuple = (1, false, "Scala")
val tuple2 = "title" -> "Beginning Scala"
```

Pour accéder aux valeurs d'un tuple, il faut utiliser l'index sachant que l'index d'un tuple commence à 1 et non à 0 comme dans les autres tableaux/listes.

```scala
val third = tuple._3
```


### Structures de contrôle
Les structures de contrôle en scala sont assez riches pour être équivalentes à leurs cousines dans les autres langages impératifs, mais de plus ces structures de contrôle renvoient toutes (à l'exception du while) des résultats, ce qui permet de les intégrer dans une approche fonctionnelle.

#### Les expressions "If, then, else"
Par défaut, les expressions `if` en scala renvoient un résultat de type `Unit` car les blocs associés aux opérateurs sont considérés comme des fonctions anonymes. 

```scala
scala> var i = if(true) println("yes")
yes
i: Unit = ()
```
Le résultat d'un `if/else` dépend de la valeur de renvoi de chaque partie de l'expression.

```scala
if (exp) println("yes") // inline

// multi-ligne
if (exp) {	println("Line one")	println("Line two")}
```

L'expression `if/else` fonctionne comme l'opérateur ternaire `?:` en java

```scala
val i: Int = if (exp) 1 else 3 // inline

// multi-ligne
val i: Int = if (exp) 1 else {	val j = System.currentTimeMillis	(j % 100L).toInt}
```

#### Les boucles "while"
Les boucles while et de do/while sont appelées boucles et non expressions car elles ne renvoient que la valeur `Unit`.

```scala
while (exp) println("Working...")while (exp) {	println("Working...")}
```

#### Les expressions "for"
La structure `for`en scala est extrêmement puissante. Elle ne permet pas seulement d'itérer sur des collections, mais elle permet également de filtrer ou de générer de nouvelles collections.

##### *for* de base

Itération sur une collection :

```scala
val books = List("Beginning Scala", 
	"Beginning Groovy", 
	"Beginning Java", 
	"Scala in easy steps", 
	"Scala in 24 hours")
	
for (book<-books) println(book)
```
L'opérateur `<-` est appelé générateur car il génère une valeur à partir d'une collection pour l'assigner à une variable.

##### Les filtres
Un filtre est une clause `if` incluse à l'intérieur de l'expression `for`. 

```scala
for(book<-books if book.contains("Scala")) println(book)
```

##### Assignation de variable
Il est possible de déclarer des variables à l'intérieur d'une expression `for`. Il n'est pas nécessaire de les déclarer en tant que `val` ou `var`, néanmoins il est nécessaire de les déclarer à l'intérieur d'un bloc fonctionnel (entouré d'accolades).

```scala
for {book <- books; bookVal = book.toUpperCase()} println(bookVal) // inline

// multiline
for {book <- books
	bookVal = book.toUpperCase()
} println(bookVal)

```

##### Yield
Le mot-clé `yield` permet de générer une nouvelle collection. Le type de la collection générée est inférée en fonction de type de la collection itérée.

```scala
var scalabooks = for{	book <-books	if book.contains("Scala")} yield book
```

#### Les expressions "try"
Les blocs `try`en scala fonctionnent sensiblement de la même manière qu'en Java.

```scala
try {	throw newException("some exception...")} finally{	println("This will always be printed")}
```
La seule différence réside dans la manière de gérer le catch qui repose en scala sur le principe de pattern matching

```scala
try {	file.write(stuff)} catch{	case e:java.io.IOException => // handle IO Exception	case n:NullPointerException => // handle null pointer}
```
Les expressions `try/catch` peuvent être assignées à des variables :

```scala
var a:Int = try{ Integer.parseInt("dog") } catch { case_ => 0 }
```

#### Les expressions "match"
Les expressions `match` sont utilisées pour le pattern matching en Scala et permettent de construire des tests très complexes avec très peu de code. Cette expression permet une grande flexibilité en permettant de comparer des éléments très variés. Le pattern matching en scala est l'équivalent du `switch/case`en Java mais la différence est qu'en scala il est permis de tester à peu près n'importe quoi. Comme toutes les expressions en scala, le pattern matching retourne une valeur qu'il est possible d'affecter à une variable :

```scala
var j:Boolean = 44 match {	case 44 => true// if we match 44, the result is true	case _ => false// otherwise the result is false}

var k:Int = "David" match {	case "David"=> 45 // the result is 45 if we match "David"	case "Elwood" => 77	case _ => 0}
```

## Programmation Orientée Objet

Scala est un langage multi-paradigme. Il est à la fois fonctionnel et profondément orienté objet. Cette approche laisse une grande liberté au développeur quant à la manière d'aborder un problème.

Scala en tant que langage orienté objet possède donc le concept de classe, d'héritage et de surchage de méthodes.

Par exemple

```scala
class Shape {	def area:Double = 0.0}

class Rectangle(val width:Double,val height:Double) extends Shape {	override def area:Double = width*height}class Circle(val radius:Double) extends Shape {	override def area:Double = math.Pi*radius*radius}
```

### Classes & Objets

La différence principale entre les classes Java et les classes Scala consiste en la manière dont elles sont déclarés.

Scala est un langage minimaliste et donc pour déclarer une classe *Book*, sans paramètres, il suffit d'écrire :

```scala 
scala> class Bookdefined class Book
scala> new Bookres0: Book = Book@181ba0scala> new Book()res1: Book = Book@d19e59scala>

```

#### Contructeurs
##### Contructeurs avec paramètres
__Paramètres déclarés en tant que *val*__

Quand les paramètres sont déclarés en tant que *val*, Scala génère automatiquement les *getters*, mais pas les setters puisque cette valeur est immutable.

```scala 
class Book( val title:String)
```

__Paramètres déclarés en tant que *var*__
Dans le cas des paramètres déclarés en *var*, les setters et les getters sont générés

```scala 
class Book( var title:String)

```
__Paramètres privés__
Si les paramètres (var ou val) sont déclarés comme privés, les setters/getters ne seront pas générés et les variables ne seront donc accessibles qu'à l'intérieur de la classe.

```scala 
class Book(private var title: String) {	def printTitle {println(title)}}
```
__Paramètres sans *var* ni *val*__

A première vue les classes qui déclarent des paramètres sans *val* ni *var* ressemblent aux classes qui les déclarent comme *private*. Néanmoins, il y a une différence importante entre les deux.

Par exemple dans le cas d'une classe *private*, cette déclaration de classe compile sans erreur.

```scala 
class Book(private val title: String) {	def printTitle(b: Book) {		println(b.title)	}}
```

Celle ci échoue à la compilation ar le champ *title* n'est accessible que par l'objet this.

```scala 
class Book(title: String) {	def printTitle(b: Book) {		println(b.title)	}}

>scalac Book.scalaBook.scala:3: error: value title is not a member of Bookprintln(b.title)```

__Paramètres par défaut et paramètres nommés__
En scala il est possible de donner des valeurs par défauts aux paramètres d'une classe : 

```scala
class Book (val title: String = "Beginning Erlang")
```

Il est également possible de préciser le paramètre que l'on est en train de renseigner en passant son nom au moment de l'affectation :

```scala
scala> val book = new Book(title="Beginning Scala")book: Book = Book@46aaf1d2scala> book.titleres0: String = Beginning Scala
```

##### Contructeurs auxiliaires
Il est possible de définir des constructeurs auxiliaires pour une classe de manière à proposer plusieurs façons de créer un même objet.
Les constructeurs auxiliaires sont définis en créant une ou plusieurs méthode(s) `this`. Comme en Java les constructeurs doivent avoir des signatures différentes.

```scala
class Book (var title :String, var ISBN: Int) {	def this(title: String) {		this(title, 2222)	}	def this() {		this("Beginning Erlang")		this.ISBN = 1111	}	override def toString = s"$title ISBN- $ISBN"}
```

#### Déclaration de méthodes
Les méthodes se déclarent comme les fonctions, avec le mot-clé `def`.

```scala
// sans param avec type de retour
def myMethod():String = "Moof" 

// sans param ni type de retour
def myOtherMethod() = "Moof"	

// avec param et type de retour
def foo(a: Int):String = a.toString	

// deux params + type de retour
def f2(a: Int, b:Boolean):String= if (b)a.toStringelse"false" 

// nombre de paramètres variable
def largest(as: Int*): Int = as.reduceLeft((a, b)=> a maxb) 

// Généricité
def list[T](p:T):List[T] = p :: Nil 

// Généricité + nb de params variable
def mkString[T](as: T*):String = as.foldLeft("")(_ + _.toString)

// Généricité avec contrainte + nb params variables
// Ici les paramètres doivent hériter de Number
// Equivalent  de <T extends Number> en Java
def sum[T <:Number](as:T*): Double = as.foldLeft(0d)(_ + _.doubleValue)
```

#### Blocs de code

Les méthodes et les définitions de variables peuvent se faire sur une seule ligne ou dans un bloc de code délimité par des accolades.

Les blocs de code peuvent être imbriqués. Le résultat d'un bloc de code est égal à la dernière ligne évaluée à l'intérieur du bloc : 

```scala
def meth():String = {	val d = new java.util.Date()	d.toString()}
```

La définition d'une variable peut également se faire grâce à un bloc de code :

```scala
val x3:String= {	val d = new java.util.Date()	d.toString()}
```

#### Call-by-name
En Java, les invocations de méthodes se font toujours par référence ou par valeur. Scala offre une nouvelle possiblité pour d'invocation de méthode par leur nom.

La différence principale par rapport à l'appel par référence en Java réside dans l'ordre dans lequel les fonctions seront appelées pour leur résolution.

On définit les fonctions suivantes :

```scala
// on définit une méthode nano
def nano() = {	println("Getting nano")	System.nanoTime}

// appel par réf
def notDelayed(t:Long) = {	println("In notdelayed method")	println("Param:"+t)	t}

// appel par "nom"
def delayed(t: => Long) = {	println("In delayed method")	println("Param:"+t)	
	t}
```

```scala
scala> nano()
Getting nano
res2: Long = 107035806029219

scala> notDelayed(nano())
Getting nano
In notdelayed method
Param:107065698468019
res3: Long = 107065698468019

scala> delayed(nano())
In delayed method
Getting nano
Param:107083288528528
Getting nano
res4: Long = 107083288552488
```

Dans le cas de la méthode `notDelayed`, nous voyons que la méthode `nano()` est appelée en premier et que sa valeur de retour est utilisée comme elle a été retournée.

Dans le cas de la méthode `delayed`, le fonctionnement est très différent. La variable `t` est liée à l'exécution de la méthode `nano()` et donc à chaque fois que la variable `t` est utilisée, la méthode `nano()` est invoquée à nouveau. C'est pourquoi nous voyons que la méthode `nano()` est appelée deux fois et que la valeur retournée par la fonction est égale au résultat de la dernière invocation.


#### Invocation de méthode
Scala permet quelques variations autour de l'invocation de méthode.

```scala
// invocation java standard
instance.method()

// par de param les () peuvent être supprimées
instance.method

// méthode avec param
instance.method(param)

// méthode avec un param en scala
instance.method param

// méthode avec 2 params
instance.method(p1, p2)

// méthode avec type générique
instance.method[T](p1,p2)
```

#### Objects
En scala le mot `object` est un mot-clé qui permet de désigner un type particulier de classe.

##### Singletons
Scala n'a pas de méthode ou de classe statique. A la place, Scala a des singletons. La définition d'un singleton ressemble trait pour trait à la déclaration d'une classe si ce n'est que le mot-clé `object` est utilisé à la place de `class`. Un singleton ne peut avoir qu'une seule instance.

```scala
object Car {	def drive { println("drive car") }}
```

Les méthodes des classes `object` sont appelées à la manière des méthodes statiques en Java : 

```scala
object Main extends App {	Car.drive}
```

Contrairement aux autres classes, les classes `object` ne peuvent pas prendre de paramètres. Les singletons peuvent être utilisés dans de nombreux cas, mais l'un des plus commun consiste à les utiliser comme point d'entrée pour l'exécution des applications Scala. Il existe deux façons de créer des classes exécutables en Scala, soit en créant un `object`qui implémente une méthode `main` à la façon de Java, soit en créant un `object` qui hérite du trait `App`.

```scala
object HelloWorld {	def main(args: Array[String]) {		println("Hello,World!")	}}
```

Ou 

```scala
object HelloWorld extends App {	println("Hello,World!")}
```


Scala propose un trait `App` qui permet l'exécution du bloc de code de la class `object` comme s'il s'agissait d'une fonct

##### Companion Objects

En Scala, un `object` et une classe peuvent partager le même nom. Dans ce cas, l'`object` est appelé le *companion object* de la classe. Un compagnon est un objet qui partage le même nom et le même fichier source que sa classe ou son trait de rattachement. Un objet compagnon est idéal pour implémenter des méthodes de type *helpers* ou des *factories*.

Par exemple :

```scala
trait Shape {	def area :Double}

object Shape {	private class Circle(radius: Double) extends Shape{		override val area = 3.14*radius*radius	}	private class Rectangle (height: Double, length: Double)extends Shape{		override val area = height * length	}	def apply(height :Double , length :Double ) : Shape = new Rectangle(height,length)	def apply(radius :Double) : Shape = new Circle(radius)}

```

A l'exécution : 

```scala
scala> val circle = Shape(2)circle: Shape = Shape$Circle@1675800scala> circle.areares0: Double = 12.56scala> val rectangle = Shape(2,3)rectangle: Shape = Shape$Rectangle@1276fd9scala> rectangle.areares2: Double = 6.0

```

Le fait d'appeler l'instance `Shape` avec un ou deux paramètres permet au compilateur scala de savoir quelle méthode `apply` invoquer.


### Packages & Imports
Les packages et imports en scala fonctionne de la même manière qu'en Java à quelques détails près.

Les déclarations `package` peuvent être placées en début de fichier :

```scala
package com.mnemotix

import scala.xml._	// import de toute une lib
import scala.collection.mutable.HashMap // import d'une classe spécifique
import scala.collection.immutable.{TreeMap, TreeSet} // import de plusieurs classes
import scala.util.parsing.json.{JSON=> JsonParser} // import et renommage de la classe JSON en JsonParser

class MyApp  extends App{
// code
}

```

Il existe d'autres nuances en scala. Il est possible de déclarer des imports à l'intérieur de n'importe quel bloc de code. Dans ce cas, l'import ne sera valide que dans le scope de ce bloc précis.

```scala
class Frog {	import scala.xml._	def n:NodeSeq = NodeSeq.Empty}
```

Il est également possible de déclarer plusieurs namespaces à l'intérieur d'un même fichier

```scala

package models {
	case class Person(id:Long, firstName:String, lastName:String)
}

package controllers {
	object Persons {
		def get(id:Long) = {
		// Code
		}
	}
}

```

### Héritage
En Scala comme en Java il est possible d'hériter d'une classe mais jamais de plus. La surcharge de méthode requiert l'utilisation du mot-clé `override` et seul le contructeur primaire peut passer des arguments au constructeur de la classe mère.

```scala
class Vehicle (speed : Int){	val mph :Int = speed	def race() = println("Racing")}

class Car (speed : Int) extends Vehicle(speed) {	override val mph: Int= speed	override def race() = println("Racing Car")}

class Bike(speed : Int) extends Vehicle(speed) {	override val mph: Int = speed	override def race() = println("Racing Bike")}
```


### Traits

Supposons que l'on veuille rajouter une autre classe à la hiérarchie de véhicules. Cette fois çi, il s'agit de la batmobile, un véhicule qui ne fait pas que rouler, mais qui peut voler et glisser sur l'eau. Dans ce cas, la méthode `race()` est insuffisante, mais cela ne veut pas dire que la classe `Vehicle` est mal définie pour autant. C'est dans ce cas précis qu'il est intéressant d'utiliser les *traits*. Les *traits* sont comme les interfaces en Java, elles permettent de compenser l'absence d'héritage multiple tout en laissant la possiblité de définir plus précisément une classe.

La différence entre les *traits* et les interfaces Java réside néanmoins dans le fait que les *traits* peuvent contenir du code.

Dans le cas d'un héritage de *traits*, le premier trait ou la première classe mère seront déclarés avec le mot-clé `extends` alors que les *traits* suivants seront ajoutés avec `with`.

```scala
trait flying {	def fly() = println("flying")}trait floating gliding {	def gliding() = println("gliding")}

class Batmobile(speed : Int) extends Vehicle(speed) with flying with gliding{	override val mph: Int = speed	override def race() = println("Racing Batmobile")	override def fly() = println("Flying Batmobile")	override def float() = println("Gliding Batmobile")}
```

En scala, les *traits* peuvent hériter de classes ou d'autres traits avec le mot-clé `extends` également.

### Case classes

En scala les case classes s'écrivent de la manière suivante :

```scala
case class Person(var name: String, var age: Int)
```
	
Les `case classes` en Scala diffèrent légèrement des classes standards :

* Le mot clé `new` n'est pas nécessaire pour les instancier, ainsi on peut écrire directement `Person("John Doe", 25)`.
* Les getters/setters sont automatiquement créés pour les paramètres du constructeur. C'est à dire qu'il est possible d'obtenir l'âge d'une instance p de Person en écrivant `p.age`
* Les méthodes `equals` et `hashCode` sont fournies par défaut.
* Une méthode `toString` par défaut est fournie
* Les instances de ces classes peuvent être décomposée à l'aide de *pattern matching*
 
Les case classes sont grosso modo l'équivalent Scala des *beans* en Java. L'avantage étant qu'elles permettent de faire l'économie d'une grande quantité de code.


### Value classes
Il est possible de surcharger les types Scala en créant des *value classes*.
Pour cela, il suffit de surcharger la classe `AnyVal` qui est la classe mère de tous les types de données Scala.

```scala
class SomeClass(val i: Int) extends AnyVal {	def twice() = i*2}
scala> val v = new SomeClass(9)v: SomeClass = SomeClass@9scala> v.twice()res5: Int = 18
```

## Programmation fonctionnelle





## Collections


## Gestion des erreurs

## Injection de dépendance

## Bases de données

## Logging

## SBT
