##Ruby OOP Design

##Classes

Unlike JS which needs to use closures to create class-like structures (at least until EcmaScript 6 is finalized and comes out), Ruby has an actual structure designated by the ```class``` attribute. 

Here are the basic syntaxes for a class:

1. ```class``` defines the class (pretty obvious)
2. ```initialize``` function is the constructor for the class. The name matters. The constructor MUST have this name if you are overriding the default empty constructor!
3. An instance variable must have the ```@``` prefixed to it (Example: _@name_)
4. To instantiate an instance of a class, unlike JS which uses ```new <Type-name>(...)```, Ruby uses a syntax with this format: ```<Class>.new(...)```
5. ```self``` is a term to refer to the actual class itself NOT the instance of the class.


Here is a simplistic example:

```
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

```
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

###Overriding inherited functions

This is just like all other languages, just name the function the same as in the parent class to override it.

###Calling super (superclass version) function

If you have overriden a parent class function but now still need access to the parent class function, you can use the ```super``` keyword like you would in Java.

```
class Dog
	def speak
		return "Woof"
	end
end

class Pitbull < Dog
	def speak
		puts 'Instead of woof...'
		super                    #If there were any arguments, it would be super(...)
	end
end
```

##Getters and Setter functions
Getter and setter functions can be created via standard function definitions but this is suboptimal. We don't want to create getters and setters manually for a lot of the instance variables.

Ruby allows a nice shortcut for this using the ```attr_reader```, ```attr_writer``` and ```attr_accessor``` classes! They take in the instance variable name structured as a symbol (see example below).

Instead of writing your getter and setter function like so:

```
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

```
class Foo
	attr_accessor :name       #The name of the attribute written as a symbol. 
end

a = Foo.new
a.name = 'Billy'
a.name # => 'Billy'
```

```attr_accessor``` will generate the getter and setter for the variable while ```attr_reader``` and ```attr_writer``` will only generate the getter or the setter function respectively.

If you are confused about the ```name=(value)``` then please see [the naming convention entry in the Coding Conventions page](coding_conventions.md#setter-naming-convention)



##Scope

JavaScript is a wonderful language in the way we can manipulate access to variables via closures. In Ruby, the scoping rules completely different.

Unlike JS which is all based on a function scope, this has 4 different scopes:

1. **global variables (prefixed with ```$```)** - are available everywhere. _If you don't prefix the variable with ```$``` but it is outside of any function or class, then it is still a global variable. 
2. **local variables** - are available only to the methods they are defined in
3. **class variables (prefixed with ```@@```)** - are available to all members of the class and between instances of the class. Adding a class variable is essentially like attaching a variable to a JavaScript object's prototype. As soon as its defined, it is shared and viewed by all instances of the object.
4. **instance variables (prefixed with ```@```)** - are available to a particular instance of the class

**NOTE:** To access a global variable defined with ```$```, it **MUST** be accessed with ```$``` as the prefix.

**NOTE 2**: Another way to think of class variables is to think that they are like Java's ```static``` variables or functions. They are called the same way too.

A good example of using class variables is for counting the instances of a class

```
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






