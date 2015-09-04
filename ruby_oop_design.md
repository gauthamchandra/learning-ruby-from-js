#Ruby OOP Design

#Everything is an object

One of the fundamental tenets of Ruby is that *EVERYTHING* is an object. You will see this further in the classes section.

#Classes

Unlike JS which needs to use closures to create class-like structures (at least until EcmaScript 6 is finalized and comes out), Ruby has an actual structure designated by the ```class``` attribute. 

Here are the basic syntaxes for a class:

1. ```class``` defines the class (pretty obvious)
2. ```initialize``` function is the constructor for the class. The name matters. The constructor MUST have this name if you are overriding the default empty constructor!
3. An instance variable must have the ```@``` prefixed to it (Example: _@name_)
4. To instantiate an instance of a class, unlike JS which uses ```new <Type-name>(...)```, Ruby uses a syntax with this format: ```<Class>.new(...)```
5. ```self``` is a term to refer to the actual class itself NOT the instance of the class.

Finally, Class names are written in `CamelCase` not ```snake_case```


Here is a simplistic example:

```ruby
class Superhero
	def initialize(pseudonym, name)
		@pseudonym = pseudonym
		@name = name
	end
	
	def description
		puts "I'm #{@pseudonym} but my real name is #{@name}!"
	end
end

batman = Superhero.new('Batman', 'Bruce Wayne')
batman.description # => "I'm Batman but my real name is Bruce Wayne"
```

###Inheritence

Unlike JS which uses a nonstandard prototypal inheritence, Ruby has a more classic inheritence akin to Java. It uses the ```<``` to designate what the class inherits from.

Here is a basic example:

```ruby
class Agent
	def hijack_person
		puts 'I got them. Intercepting them now.'
	end
	
	def order_sentinels
		puts "Send the sentinels. They can't use EMP yet"
	end
end

class Smith < Agent
	def give_monologue_to_neo
		puts 'Mr...Anderson. Humanity is the virus. We are the cure'
	end
end

Smith.new.order_sentinels        # => 'Send the sentinels. They can't use EMP yet'
Smith.new.give_monologue_to_neo  # => 'Mr...Anderson. Humanity is the virus. We are the cure'

```

As you can see, the Agent class has two functions and by using the ```child_class < parent_class``` syntax, we were able to have Smith class inherit from the Agent class and call not only the Agent's own functions but the inherited functions as well.

**Ruby only allows single parent inheritence. To get around this, see [here](#mixins-with-include-and-extend)

###Overriding inherited functions

This is just like all other languages, just name the function the same as in the parent class to override it.

###Calling super (superclass version) function

If you have overriden a parent class function but now still need access to the parent class function, you can use the ```super``` keyword like you would in Java.

```ruby
class Dog
	def speak
		puts "I'm a dog."
	end
end

class Pitbull < Dog
	def speak
		super                    #If there were any arguments, it would be super(...)
		puts 'But I'm also a pitbull...'
	end
end
```

###Getters and Setter functions
Getter and setter functions can be created via standard function definitions but this is suboptimal. We don't want to create getters and setters manually for a lot of the instance variables.

Ruby allows a nice shortcut for this using the ```attr_reader```, ```attr_writer``` and ```attr_accessor``` classes! They take in the instance variable name structured as a symbol (see example below).

Instead of writing your getter and setter function like so:

```ruby
class Foo
	@name 			#just creating the instance var. currently 'nil'
	
	def name
		@name
	end
	
	def name=(value)
		@name = value
	end
end

a = Foo.new
a.name = 'Billy'
```

you can just write this:

```ruby
class Foo
	attr_accessor :name       #The name of the attribute written as a symbol. 
end

a = Foo.new
a.name = 'Billy'
a.name # => 'Billy'
```

```attr_accessor``` will generate the getter and setter for the variable while ```attr_reader``` and ```attr_writer``` will only generate the getter or the setter function respectively.

If you are confused about the ```name=(value)``` then please see [the naming convention entry in the Coding Conventions page](coding_conventions.md#setter-naming-convention)

###Class Definitions (Gotchas when coming from other languages)

In Ruby, code that defines a class is no different from any other code. This means you can place the class definition code almost anywhere. For example:

```ruby
# this is actually valid
# yields: 
#  Hello
#  Hello
2.times do
  class Foo
    puts 'Hello'
  end
end
```

#####Classes can be defined multiple times

In Ruby, the first time the interpreter encounters a class definition, it creates the class with the specific members that have been specified. If it encounters a definition for the same class again, it actually *extends the existing definition*

```ruby
# Defines class Foo
class Foo
  def bar
    2
  end
end

# extends already defined Foo
class Foo
  def baz
    3
  end
end

# The above 2 definitions is equivalent to this
class Foo
  def bar
    2
  end
  
  def baz
    3
  end
end
```

This metaprogramming feature is a pretty powerful way to extend classes at runtime and this is how the Rails's `ActiveRecord` classes work (see the rails doc for details).

The JS equivalent of this would be overriding the `prototype` of an existing type. 

######Careful in overriding existing functions!

This kind of extending classes on existing types inherent to the language is known by JS devs as `monkeypatching` and is dangerous to do on existing types. For example:

```
class Foo
  def bar
    3
  end
end

# somewhere else in the code
# The following actually overwrites existing bar definition
class Foo
  def bar
    5
  end
end

Foo.new.bar # => 5
```

This example might seem silly but it is very important to note. In JavaScript, developers are warned not to overwrite existing types like `String.prototype` or `Array.prototype` that might unintentionally affect other functionality or libraries used in the app. Ruby code has the same implications and so the same care must be taken.

###Scope

JavaScript is a wonderful language in the way we can manipulate access to variables via closures. In Ruby, the scoping rules completely different.

Unlike JS which is all based on a function scope, this has 4 different scopes:

1. **global variables (prefixed with ```$```)** - are available everywhere. _If you don't prefix the variable with ```$``` but it is outside of any function or class, then it is still a global variable. 
2. **local variables** - are available only to the methods they are defined in
3. **class variables (prefixed with ```@@```)** - are available to all members of the class and between instances of the class. Adding a class variable is essentially like attaching a variable to a JavaScript object's prototype. As soon as its defined, it is shared and viewed by all instances of the object.
4. **instance variables (prefixed with ```@```)** - are available to a particular instance of the class

**NOTE:** To access a global variable defined with ```$```, it **MUST** be accessed with ```$``` as the prefix.

**NOTE 2**: Another way to think of class variables is to think that they are like Java's ```static``` variables or functions. They are called the same way too.

A good example of using class variables is for counting the instances of a class

```ruby
class Foo
	@@foo_instance_count = 0
	
	def initialize
		@@foo_instance_count += 1
	end
	
	def self.get_instance_count
		@@foo_instance_count
	end
end

a = Foo.new
puts Foo.get_instance_count #Note we are accessing via the class itself and not the instance
							 # => 1
b = Foo.new
puts Foo.get_instance_count # => 2
```

###Everything is an object (even a class)

If you define a class Foo:

```ruby
class Foo; end
```

Then the class `Foo` inherits from `Class`.

But what exactly is a class? In Ruby, it's really just an object! This object sets up all of the methods that you associate with classes including `new`, `superclass` and `allocate`. 

But wait...if `Class` is an object, then does it inherit from something? Yes. Here is the inheritance chain:

```
BasicObject <--- Object <-- Module <-- Class
```

So technically... all classes are modules and `BasicObject` is the root of the Ruby class heirarchy.

###Class Reflection and Metaprogramming

Unlike other languages like C where much of the metadata on an object or class is stripped away, Ruby keeps almost all of it. 

Some useful information that can be available are:

#####Class and Superclass via `.class` and `.superclass`

```ruby
class Foo
end

x = Foo.new
x.class      # => Foo
x.superclass # => Class
```

#####Getting a list of methods via `.methods`, `.public_methods`, `.protected_methods`, `.private_methods`

```ruby
class Foo
  def bar
  end
end

x = Foo.new
x.methods(false) # false tells the function to only return non-inherited methods.
                 # => [:bar]

#To get a subset by searching through the methods, calling grep() is useful
x.methods.grep(/ba/) # => [:bar]
```

#####Getting a list of instance variables via `.instance_variables`

```ruby
class Foo
  @a = 3
  def initialize
    @b = 5
  end
end

Foo.instance_variables     # => [:@a]
Foo.new.instance_variables # => [:@b]
```

Why is `@a` not available for `Foo.new`? Its because` @a` is an instance variable of Foo (since class Foo is technically an object). When `Foo.new` is executed, the `initialize` function is called, creating a new instance variable `@b`.


#Module

A module in Ruby is a data structure for holding methods and constants.

Just like with classes, they have a ```CamelCase``` naming convention.

```ruby
module Foo
	SOME_CONSTANT = 9
end
```

##Namespace resolution using ``::``

If there are multiple functions with the same name in different modules, to make sure Ruby knows to pick the right function, we use ```<module-name>::<function/constant-name>``` format to be specific. 

For example. if you define a module with the constant PI

```ruby
module Foo
	PI = 3.1415
end
```

Then if you want to refer to the Math module's PI and not Foo's, you can specify it like so:

```ruby
Math::PI  #=> 3.141592653589793
```

You can also use the standard dot notation as well **IF it is a function**

```ruby
Date::Today 
Date.Today #same as Date::Today
```


##Importing a module with ```require``` and ```include```

There are two ways to import a module:

1. ```require 'module'``` - This imports the module but you still have to refer to it by namespace. For example, if you import Math library, to use the cos() function, you still need to say ```Math.cos```
2. ```include Module``` - This imports the module AND acts as if the module contents are now part of the class/module. This is very important as it allows you to use the module's constants and methods without the prefix. As a result, if you import the Math library, you can just say ```cos(34)``` instead of ```Math.cos(34)```. **For this reason, the include must be inside the class or module declaration.**


Importing a module in Ruby uses the ```require``` keyword like so:

```ruby
require 'date' #import the date module. Note how the name is surrounded by quotes.
Date.today # date.today
```

Unlike with ```require```, importing a module with ```include``` means the module must **NOT** be wrapped as a string. It also must be imported from within a module or class.

```ruby
class Foo
	include Math
	def cosine(radians)
		cos(radians)        #Look Ma! No Namespace prefix for the cos()!
	end
end
```

##Mixins<a name="mixins-with-include-and-extend"></a>

###Mixin with ```include```<a name="mixins-with-include"></a>
When you use ```include```, the module's methods act as if they are now part of the **instance of the** class/module and this has far reaching effects.

See below:

```ruby
module Gun
	def shoot(name)
		puts "Shot #{name}"
	end
end

class Man
	include Gun
end

joe = Man.new
joe.shoot("Pete")  # => 'Shot Pete'
```

As you can see, the Man class was essentially augmented with the module's functions. This is called a **mixin**.

Let's continue:

```ruby
class BadMan < Man; end

will = BadMan.new
will.shoot("Franz Ferdinand") # => 'Shot Franz Ferdinand'
```

This is how single inheritence can be side stepped. Classes can be mixed in and inherited and the child classes would then inherit the constants and methods of the module even if they aren't explicitely inclduing it themselves.

###Mixin with ```extend```<a name="mixins-with-extend"></a>
The mixin with ```extend``` works exactly the same way as the [mixin with include](#mixins-with-include) but it works on a **Class level**

See example below (thanks code academy for the example):

```ruby
module ThePresent
  def now
    puts "It's #{Time.new.hour > 12 ? Time.new.hour - 12 : Time.new.hour}:#{Time.new.min} #{Time.new.hour > 12 ? 'PM' : 'AM'} (GMT)."
  end
end

class TheHereAnd
  extend ThePresent
end

TheHereAnd.now
```

Note how the class itself can call ```now``` and not just instances of the class.
