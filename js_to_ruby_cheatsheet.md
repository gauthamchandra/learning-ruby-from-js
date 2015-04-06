#JavaScript <--> Ruby Cheatsheet

##Comments

Single line comments in Ruby are defined via:

```
# This is a comment
```

Multiline comments are enclosed via ```=begin``` and ```=end```

```
=begin
Hello World
This is a multiline
comment
=end
```

##Printing to the Screen/Console
In JavaScript, to print something to the console, you can use the ```Console``` object via ```console.log``` but in Ruby, it is differentiated to two different functions: ```puts``` and ```print```

The differences are **exactly the same as** Java's ```System.out.println``` and ```System.out.print``` in that:

```
puts 'Hello'  # Prints to console and adds a newline at the end (Similar to Java's System.out.println())
print 'Hello' # Prints to console but no newline is added (Similar to Java's System.out.print())
#
=begin
The following yields:
Hello
Hello
=end
2.times { puts 'Hello' } 
#
#The following yields 'HelloHello'
2.times { print 'Hello' }
```



##Objects/Dictionaries

Objects in JavaScript are called dictionaries in Ruby. 

###Traditional JS-y way to define it:

In JS and Ruby, they can be defined like so:

```
stuff = {}
```

###via Hash.new:
```
stuff = Hash.new(0) #Creating a new object and setting any newly specified key with the default value 0
stuff[a] = 3; # => 3
puts stuff[a] # prints out "3"
puts stuff[b] # prints out "0"
```

##Functions and Parameters

###Functions with no parameters

In JavaScript, it is mandatory to execute function with no arguments with parentheses but in ruby, parentheses aren't required and so most devs don't use them:

```
SomeClass.foo  #This is equivalent to SomeClass.foo() in JS
```

###Functions with one or more parameters
Unlike JS, in Ruby, parameters don't need to be enclosed by parentheses. The Ruby intepreter just looks for the parameters to be seperated by commas.

```
puts 'Hello' 'World' 'Goodbye' # Similar to console.log('Hello', 'World', 'Goodbye')
puts('Hello', 'World')         # Also correct in Ruby but less used as its unneccessary
```

###Functions as arguments

In Ruby, functions can be passed blocks via {} which act similar to anonymous functions in JavaScript (see below)

```
books = {}
books['stuff'] = 'hello' #setting a key and value is the same as in JS
books.values.each { puts 'Hello' } //equivalent to books.values.each(function(){ console.log('Hello') }); in JS
```

###Function parameters to anonymous functions

In Ruby, parameters to anonymous functions are enclosed via | or "pipe" characters like so:

```
books.values.each { |book| puts 'Gotta read ' + book } 
```

which is equivalent to in JavaScript:

```
books.values.each(function(book) {
	console.log('Gotta read ' + book);
});
```

##Converting to different types

###To a string
Converting to a String involves using the `to_s` function

```
3.to_s # => "3"
```

###To an integer
Involves using the `to_i` function

```
'Hello'.to_i # => 0
```


##Symbols
**Symbols are a special object that contains names and strings inside the Ruby Interpreter.**

There is no standard JS equivalent to Symbols. The closest Java equivalent to this would probably ```Enums```. This is because symbols are defined as static things which are used once and are immutable. Referencing them again will refer to the same copy in memory. 

Just like a Java Enum, they cannot be set to any value like a string or integer. They can only be values set to other objects.

They are defined via ":". Here are some examples:

```
:hello 
:'Hello World'                    # If you want to use symbol names with spaces
:"Hello World"                    # Ruby doesn't care whether you use single or double quotes like JS
:'Hello World'.to_s               # => 'Hello World'
books['some-book'] = :hello       # Sets the object's value at that key to the symbol ':hello'
books['some-other-book'] = :hello # Refers to the SAME object as books['some-book']
```

##Equality Comparison





