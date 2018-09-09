# Intro à la Programmation Orientée Objet dans Ruby

## Objectifs pédagogiques
* Expliquer la relation entre `.new()` et `def initialize()`
* Savoir si une donnée serait mieux stockée dans une variable locale, d'instance ou de classe
* Expliquer si l'accessibilité de cette donnée est définie au mieux par `attr_accessor`, `attr_reader`, `attr_writer` ou aucune de ces expressions
* Définir précisément les variables d'instance et de classe
* Proposer deux manière de définir des méthodes de classe

## Les bases: Qu'est-ce que la POO?
Ruby est un langage orienté objet. Cela veut dire que cela a été conçu sur l'idée que le développeur construira son application en passant essentiellement par des objets.

De la même manière que vous l'avez appris avec le JSOO (Javascript orienté objet), un objet est une collection d'attributs reliés (propriétés) et de méthodes (comportements). Imaginez un objet comme une petite machine avec des écrans affichant un texte et des boutons à pousser.

Lorsqu'on écrit une application orientée objet, on écrit les plans de ces machines, et ensuite on écrit une suite d'événements, qui sera lancée par les utilisateurs, et qui permettra aux machines d'interagir les unes avec les autres.

Une grande partie du code que vous allez voir aujourd'hui est très similaire à ce que vous avez vu la dans le cours de JSOO la semaine dernière. Le concept de classe existe depuis quelques années déjà, mais n'a été introduit dans Javascript que très récemment (ECMAScript 2016/ES6). Utilisez ces connaissances pour avancer dans le cours d'aujourd'hui!

## A FAIRE: Le Processus POO (10 minutes / 0:10)
Nous avons expliqué sommairement en quoi la poo est un paradigme, mais nous n'avons pas expliqué comment réduire un problème en un ensemble d'objets.

### Exemple: Le Monopoly
> "Le Monopoly est un jeu où les joueurs de faire fortune à travers le biens immobiliers et l'argent.
* plateau
* joueurs
* jetons
* cartes de propriété
* l'argent
### Exemple: Facebook
> "Facebook est une application où des utilisateurs peuvent poster des statuts et ajouter des amis.

* utilisateurs
* statuts
* amis

Décomposer ses idées te donnera une idée de ce que peuvent être les objets.

Prends 10 minutes avec un partenaire pour trouver au moins trois types d'objets qui pourraient t'aider à créer les exemples d'applications qui suivent. Ajoute tes réponses en commentaire à la fin de ce cours. Nous répondrons aux réponses comme dans un cours.


1. Amazon
2. Une application qui attribue des notes aux devoirs maisons
3. Une application qui note les présences.
3. Lyft
> Une approche utile serait de prendre les "noms" dans la description de l'application et de dire qu'ils sont des noms.

## Our First Object (10 minutes / 0:20)
Prenons une voiture? On a tous un modèle mental d'une voiture: elle a quatre roues, de l'essence, un guidon... Le plan de la voiture est comme une classe. Lorsque nous voyons une voiture, c'est comme une instance de classe, un véritable objet devant nous. Chaque objet a son propre plan, et est une instance de ce plan ou classe.

Une classe est un plan à partir duquel les objets sont fabriqués. En Javascript nous avons utilisé des classes qui fonctionnent de façon très similaire que dans Ruby. Chaque objet fait à partir d'une classe est une instance de cette classe. Chaque instance de classe est un objet.

Définissons une classe Utilisateur. Nous allons utiliser binding.pry pour tester notre code.

> Remarque: pry est une gem ruby qui nous permet de travailler avec ruby dans un IRB (interactive ruby shell). C'est comme travailler dans notre console de développeur dans le navigateur.

```
$ touch app.rb
$ gem install pry # faites ceci si vous n'avez pas encore installé pry
require "pry"
```
```ruby
class User

  def set_name_to(some_string)
    @name = un_string
  end

  def get_name
    return @name
  end

  def greet
    puts "Salut! Je m'appelle #{@name}!"
  end

end

binding.pry

puts "fin du fichier"
```

<details>
  <summary>Qu'en est-il de cette classe Ruby qui ressemble à une classe Javascript.
  </summary>
Le mot clef `classe`. La classe contient des méthodes.
</details>


Maintenant, générons des instances de cette classe...
```ruby
alice = User.new
alice.set_name_to("alice")
puts alice.get_name

madhatter = User.new
madhatter.set_name_to("Mad Hatter")
puts madhatter.get_name

alice.greet
madhatter.greet
```

#### Quelques questions
Le `User` est-il une...

* classe?
* instance?

`alice` est-elle une...

* classe?
* instance?

`User.greet` retourne une erreur. `alice.greet` marche correctement. Donc nous pouvons déduire que la méthode `greet` ne peut être appelée que sur...

* des instances de la classe User?
* la classe User elle-même?

Ainsi, est-ce que ça aurait un sens de nommer `greet` une..

* "une méthode d'instance"?
* "une méthode de classe"?

`User.new` marche bien. `alice.new` renvoit une erreur. Donc nous pouvons déduire que la nouvelle méthode ne peut qu'être appelée sur...

* des instances de la classe User?
* la classe User elle-même?

Ainsi, est-ce que ça aurait un sens de nommer `new` une..

* "une méthode d'instance"?
* "une méthode de classe"?
  
<details>
  <summary>
`class User` fonctionne. `class user` renvoit une erreur. Quelle est la règle que nous pouvons déduire sur les classes?</summary>
 > Les noms de classe commencent obligatoirement avec une majuscule.
</details>

<details>
  <summary>
 `class UserName` fonctionne. `class User Name` renvoit une erreur. Quelle est la règle que nous pouvons déduire sur les classes?</summary>
 > Les noms de classe n'ont pas d'espaces.
</details>

### Initialiser Users (10 minutes / 0:30)
What was the purpose of a constructor function in Javascript classes?
Ruby classes have an equivalent to Javascript constructors: the initialize method!

require 'pry'

class User

  def initialize
    puts "I'm a new User"
  end

  def set_name_to(some_string)
    @name = some_string
  end

  def get_name
    return @name
  end

  def greet
    puts "Hi! My name is #{@name}!"
  end

end

binding.pry

puts "end of file"
alice = User.new
alice.greet

madhatter = User.new
madhatter.greet


puts alice
puts madhatter
What can we conclude about the relationship of `def initialize` and `.new`? (Hint: it serves the same purpose as Javascript's constructor function)
How is this different from other User instance methods we've seen?
You Can Pass Arguments to initialize
initialize is a special method in its relationship to .new, but otherwise it behaves like any other method. This means you can pass arguments to it (again, just like Javascript's constructor)...

require "pry"

class User

  def initialize(firstname, lastname)
    puts "I'm a new User named #{firstname} #{lastname}"
  end

end

binding.pry

puts "end of file"
# pry
harry = User.new("Harry", "Potter")
# I'm a new User named Harry Potter
# => #<User:0x007f96f312b240>
Instance Variables (10 minutes / 0:40)
Let's create a method that prints the full name of the user.

In Ruby, normal variables are available only inside the method in which they were created.

If you put an @ before the variable's name, it becomes an instance variable and therefore available inside the entire instance in which it was created.

class User

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

end
# pry
harry = User.new("Harry", "Potter")
# => #<User:0x007faf3903f670 @firstname="Harry", @lastname="Potter">
harry.full_name
# => "Harry Potter"
Getting and Setting Properties
To get Harry's first name, we can't simply type harry.firstname. To set Harry's first name, we can't simply type harry.firstname = "Harry"

The only things available outside an instance are its methods. @firstname is a property, not a method. We can't access data inside of an instance unless it contains methods that let us do so.

To make a property "gettable" and "settable", we need to create "getter" and "setter" methods for it.

class User

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

  def get_firstname
    return @firstname
  end

  def set_firstname(firstname)
    @firstname = firstname
  end

end
# pry
harry = User.new("Harry", "Potter")
# => #<User:0x007faf3903f670 @firstname="Harry", @lastname="Potter">
puts harry.get_firstname
# "Harry"
harry.set_firstname("Ginny")
puts harry.get_firstname
# "Ginny"
attr_accessor
Recall how we couldn't simply type Harry.firstname = "some other name" the previous example.

class User

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

  def get_firstname
    return @firstname
  end

  def set_firstname(firstname)
    @firstname = firstname
  end

end
harry = User.new("Harry", "Potter")
harry.firstname = "Ginny"
# This throws an error
harry.set_firstname("Ginny")
puts harry.get_firstname
# =>
If only there were a way to define a class so that we don't have to define a getter and setter method for every single property...

class User

  attr_accessor :firstname, :lastname

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

end
nayana = User.new("Nayana", "Davis")
nayana.firstname = "Nayana"
puts nayana.firstname
We now can directly access properties on the User instance, so we can deduce that `attr_accessor` is a shortcut that does what?
attr_accessor is actually a shortcut that combines two other shortcuts
attr_accessor is attr_reader combined with attr_writer.
attr_reader makes an attribute readable, attr_writer makes an attribute writeable. attr_accessor makes an attribute both readable AND writeable.

To illustrate the difference between attr_reader and attr_writer, let's have a look at the code below.

class User
  attr_reader :firstname
  attr_writer :lastname

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

end
hermione = User.new("Hermione", "Granger")
hermione.firstname
# => "Hermione"
hermione.lastname
# => Error!
hermione.firstname = "Ginny"
# => Error!
hermione.lastname = "Weasley"
hermione.full_name
# => "Hermione Weasley"
attr_reader creates a getter method only. Trying to do hermione.firstname = "Ginny" will fail.

attr_writer creates a setter method only. Trying to do puts hermione.lastname will fail.

attr_accessor creates getters and setters.

You will most commonly use attr_accessor

Break (10 minutes / 1:00)
You Do: Monkies! (20 minutes / 1:20)
For the next exercise, clone down the repo linked below: https://github.com/ga-wdi-exercises/oop_monkey

Class Attributes / Variables (5 minutes / 1:25)
Let's come up with a way of keeping track of how many users have been created total...

class User
  attr_accessor :firstname, :lastname
  @@all = 0

  def count
    return @@all
  end

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
    @@all += 1
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end
end
harry = User.new("Harry", "Potter")
harry.count
# => 1
ron = User.new("Ron", "Weasley")
harry.count
# => 2
ron.count
# => 2
draco = User.new("Draco", "Malfoy")
harry.count
# => 3
ron.count
# => 3
draco.count
# => 3
But there's something weird going on here: note that we aren't counting the number of Rons, Harrys or Dracos. Think about what .count might be returning. More on this in a moment!

A variable name beginning with @@ is a class variable. Every instance of a class has the same value for this variable. It cannot be accessed with attr_accessor. You have to actually create a method to access it.

Class Attributes and Methods Together (10 minutes / 1:40)
A method name beginning with the class name is a class method. It is attached to the class itself, rather than to instances. There are also methods you call on User itself. So far we've only seen .new.It would make more sense if, in order to retrieve the total number of users, we ran User.count instead of harry.count...

class User
  attr_accessor :firstname, :lastname
  @@all = 0

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
    @@all += 1
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

  # You could also define this as `def self.count`, where self represents the class
  def User.count
    return @@all
  end

end
ginny = User.new("Ginny", "Weasley")
ginny.count
# => Error!
User.count
# => 1
Self (5 minutes / 1:45)
self is a special variable that contains the current instance of an object (like this in Javascript). It's how the object refers to itself.

self has another context as well: def self.all Here, self refers to class User. What does this mean? It means that the method .all is called on the class User, much like .new, and is therefore a class method.

class User
  attr_accessor :firstname, :lastname
  @@all = []

  def initialize(firstname, lastname)
    @firstname = firstname
    @lastname = lastname
    # here, `self` refers to the current instance
    puts "Creating #{self.firstname}"
    @@all.push(self)
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

  # Can also be written as `def User.all`
  # here, `self` refers to the class
  def self.all
    return @@all
  end

end
draco = User.new("Draco", "Malfoy")
# "Creating Draco"
luna = User.new("Luna", "Lovegood")
# "Creating Luna"
bellatrix = User.new("Bellatrix", "LeStrange")
# "Creating Bellatrix"
User.all
# => [#<User @firstname="Draco">, #<User @firstname="Luna">, #<User @firstname="Bellatrix">]
Break (10 minutes / 1:55)
You Do: Orange Tree (25 minutes / 2:20)
From Chris Pine's "Learn to Program - Second Edition": p 112, section 13.6

Make an OrangeTree class that has...

a height method that returns its height in feet
it's initial value should be determined by some input
hint: you don't necessarily have to define the method
a one_year_passes method that, when called, ages the tree one year. Start the age at 0.
Test your code.

Each year the tree grows taller by one foot
After 50 years the tree should "die" (its height goes to 0)
Test your code.

After the first 5 years, the tree bears 20 oranges
You should be able to count_the_oranges, which returns the number of oranges on the tree
Test your code.

You should be able to pick_an_orange, which reduces the number of oranges by 1
Ensure that your tree cannot have negative oranges
Ensure that after each year your tree has 20 total oranges again
Test your code.

The number of oranges the tree bears each year is equal to 20 plus the age of the tree
Bonus
Create an OrangeTreeOrchard class that manages multiple OrangeTrees. It can...

Age all the trees by one year
Pick and count all the fruit
Calculate average height and fruit of all orange trees
Closing / Questions (10 minutes / 2:30)
Sample Questions
Create a Ruby class for a student, initialized with a name and an age.
Write a getter for name and age, and a setter for name only
Create a new student and demonstrate using all the methods
Explain the difference between local and instance variables
Glossary
Class: a blueprint for objects
Instance: an object that is created using a class
Instance Variable: a property that is particular to an instance
Class Variable: a property that is accessible by all instances of a class
Instance Method: a method that can be called by an instance of a class (e.g., sample_user.reset_password)
Class Method: a method that can be called by a class (e.g., User.list_user)
initialize: a class method that, when triggered, creates an instance and assigns initial properties
.new: a class method that, when called, triggers its initialize method
attr_accessor: a setting that allows you to directly "get" or "set" an instance variable
Bonus: Public and Private (5 minutes / 1:50)
You Do
Draw a picture of a machine, real or imaginary, that has inputs (buttons, switches, keypads...) and displays (dials, lights, screens...). Label what they do.
Most machines have internal gauges or memories that help it make decisions: temperature monitors, voltage monitors, hard disks, and so on. These are visible only inside the machine: whoever's using the machine can't see them. Draw two of these on your machine and label them.
By default all instance and class methods are public, except for def initialize which is private. This means they're visible to other objects. An analogy: they're functions that have their own buttons on the outside of the machine, like a car's turn signal.

There may be methods that all other objects don't need to know about.

class User
  attr_accessor :firstname, :lastname
  @@all = []

  def initialize(firstname, lastname, password)
    @firstname = firstname
    @lastname = lastname
    @password = encrypt(password)
    @@all.push(self)
  end

  def full_name
    return "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

  def User.all
    return @@all
  end

  private
  def encrypt(input)
    return input.reverse
  end

end
harry = User.new("Harry", "Potter", "Expecto Patronum")
# #<User @firstname="Harry" @password="munortaP otcepxE">
harry.encrypt("Expecto Patronum")
# Error! Private method `encrypt`
Putting private in front of methods means they can be used inside the object, but are not available outside it. An analogy: they're functions that do not have their own buttons on the outside of the machine, like a car's air filter.

private is useful mostly for keeping things organized. Consider jQuery: It's already cluttered enough, with all these methods like .fadeOut and .css. It has lots of other methods hidden inside it that we don't really need to know about.

Review: Why OOP?
Easy to Understand
Objects help us build programs that model how we tend to think about the world. Instead of a bunch of variables and functions (procedural style), we can group relevant data and functions into objects, and think about them as individual, self-contained units. This grouping of properties (data) and methods is called encapsulation.

Managing Complexity
This is especially important as our programs get more and more complex. We can't keep all the code (and what it does) in our head at once. Instead, we often want to think just a portion of the code.

Objects help us organize and think about our programs. If I'm looking at code for a Squad object, and I see it has associated people, and those people can dance when the squad dances, I don't need to think about or see all the code related to a person dancing. I can just think at a high level "ok, when a squad dances, all it's associated people dance". This is a form of abstraction... I don't need to think about the details, just what's happening at a high-level.

Ensuring Consistency
One side effect of encapsulation (grouping data and methods into objects) is that these objects can be in control of their data. This usually means ensuring consistency of their data.

Consider the bank account example... I might define a bank account object such that you can't directly change it's balance. Instead, you have to use the withdrawl and deposit methods. Those methods are the interface to the account, and they can enforce rules for consistency, such as "balance can't be less than zero".

Modularity
If our objects are well-designed, then they interact with each other in well-defined ways. This allows us to refactor (rewrite) any object, and it should not impact (cause bugs) in other areas of our programs.

Extra Practice: Scrabble
Clone this exercise and follow the instructions in the readme.

Scrabble Word Scorer

Resources
Variables cheat sheet
Other exercises
Monkeys
Application Config
Superheroes
Screencasts
WDI8, Robin
Part 1
Part 2
Part 3
Part 4
