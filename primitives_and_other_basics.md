#Basic Primitive Types and Reading/Writing to Screen

**Disclaimer**: This doesn't go through every single primitive type and explain what it is. It just explains the differences between how it is implemented and used in JS vs Ruby.

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

##Semicolons

Semicolons are not necessary and not recommended to be used unless you have multiple statements of code onto one line.

```
#bad use of semicolons!
puts 'hello world';
puts 'goodbye cruel world';
```

```
#good use of semicolons
puts 'hello world'; puts 'goodbye cruel world';
```

##Reading and Writing to the Screen/Console

###Print vs puts
In JavaScript, to print something to the console, you can use the ```Console``` object via ```console.log``` but in Ruby, it is differentiated to two different functions: ```puts``` and ```print```

The differences are **exactly the same as** Java's ```System.out.println``` and ```System.out.print``` respectively in that:

```
puts 'Hello'  # prints and adds a newline at the end (Similar to Java's System.out.println())
print 'Hello' # prints but no newline is added (Similar to Java's System.out.print())

=begin
The following yields:
Hello
Hello
=end
puts 'Hello'
puts 'Hello'

=begin
The following yields:
HelloHello
=end
print 'Hello'
puts 'Hello' 
```

###Referencing Variables in Puts/Print

To reference variables already defined, use the ```#{variable_name}```. For example:

```
name = 'Gautham'
puts 'Hello #{name}'
```

###Getting User Input from console
In Ruby, to read input from the user, its with the ```gets``` keyword (puts for output, gets for input).

There is one caveat to the ```gets``` keyword and that it returns the User's input WITH a newline character.

```
puts "Hello. What is your name?"
name = gets       # If you entered 'Jake', then name='Jake\n'
```

####Removing the newline character from ```gets```
To remove the newline character added from ```gets```, use the ```chomp``` function.

```
name = gets.chomp  # With chomp, it strips the '\n' and so name='Jake'
```


##Hashes (i.e JS-y objects)

Objects in JavaScript are called hashes in Ruby. 

###Traditional/JS-y way to define it:

In JS and Ruby, an empty hash can be defined like so:

```
stuff = {}
```

JS and Ruby both can use ```:``` to seperate key value pairs in hashes like so:

```
#the same in both JS and Ruby
a = { status: 'zombie', name:'Katie', id: 5 }
```

**Every key in a hash is a [symbol](#symbols) and can be accessed as such:**

```
a[:status] #Same as a['status']
``` 

####Alternate notation for key value seperators using the hashrocket: ```=>```

Instead of using ```:``` to seperate key value pairs, In Ruby, one can also use ```=>``` instead.


```
stuff = {
	'name' => 'Bilbo Baggins',
	'survives' => true
}
```
Equivalent JS:

```
stuff = {
	name: 'Bilbo Baggins',
	survives: true
};
```

####Ommitting the ```{}```

When passing a hash to a function, if the hash is the last parameter, the ```{}``` can be ommitted. 

So if there is a method ```foo``` that takes in 1 hash as a parameter:

```
foo({ 'status' => 'zombie' }) # This...
foo('status' => 'zombie')     # is the same as this
```

###via Hash.new:

Hash.new takes in an optional parameter for the default value. If set, anytime a non-existent key is accessed or an operation is done on a key that has no value, this default value is returned instead.

```
x = Hash.new        #Creating a blank hash with the default value as nil
stuff = Hash.new(0) #Creating a new object and setting the default value to 0. 
stuff[a] = 3; # => 3

puts stuff[a] # prints out "3"
puts stuff[b] # prints out "0"
```

##Strings

Strings works pretty much the same way in JS as it does in Ruby with a few nice enhancements. 

###String Interpolation
Aside from standard string concatenation, Ruby allows something called String interpolation. This allows you to refer to variables from inside a string using the ```#{<variable_name>}``` syntax:

```
name = 'Batman'
puts "#{name}" # => Batman
```

You can even execute functions from inside the ```#{}```

```
puts "#{name.upcase}" # => BATMAN
```

###String Interpolation and Single Quotes
String interpolation is not possible with single quotes. If single quotes are specified, Ruby will take the input **literally**. 

**You MUST use double quotes for string interpolation to be done**

```
name = 'Batman'
puts 'I am #{name}' # => 'I am #{name}'
puts "I am #{name}" # => 'I am Batman'
```

##Arrays

Arrays work the same as in JS with one or two small tweaks.

In Ruby, there is an alias for the push function using the ```<<``` operator. It can be used like so:

```
[3,2] << 5    # => [3,2,5]
[3,2].push(5) # same as above statement and the JS-y way to do it but NOT conventional
```


##Comparing/Converting/Checking different objects

###Comparing two different objects

###```==``` operator
The JS ```===``` is the same as ```==``` in that it checks **both** the type and the **value** **BUT does not care if they are in two different locations in memory (unlike Java's ==)**.

###```===``` operator

This is the same as ```==``` except it can be overriden by objects for their own custom equals logic. It would be the equivalent to overriding the equals function in Java.

###Combined Comparison Operator

Aside from the usual ```==```, ```>=```, ```<=```, ```!=```, Ruby has another comparison operator called the **Combined Comparison Operator** designated by ```<=>```. 

This returns,

* 0 if the two objects are equal
* 1 if the 1st object is greater
* -1 if the 2nd object is greater

In this regard, you can think of the combined comparison operator as a glorified shorthand for the ```compareTo``` function in JS and Java

```
'Hello' <=> 'Hello' # => 0
3 <=> 2             # => 1
3 <=> 9				 # => -1 
```

###```equal?``` function

To check to make sure that the two objects point to the same point in memory, you need to use the ```equal?```

**See this [StackOverflow answer on == vs === vs equal? vs eql?](http://stackoverflow.com/a/7157051)**


###Checking Types 
To check the type of an object, use ```is_a?``` function

```
greeting = 'Hello'
greeting.is_a? Integer # Checking if greeting is an Integer. => false
greeting.is_a? String  # Checking if greeting is a String. => true
```

###Converting

####To a string
Converting to a String involves using the ```to_s``` function

```
3.to_s # => "3"
```

####To an integer
Involves using the ```to_i``` function

```
'Hello'.to_i # => 0
```

####To a proc
Involves using the ```to_proc``` function

Procs are covered [here](functions.md#blocks-procs-and-lambdas).


##Symbols<a name="symbols"></a>
**Symbols are a special object that contains names and strings inside the Ruby Interpreter.**

There is no standard JS equivalent to Symbols. The closest Java equivalent to this would probably interned Strings. This is because symbols are defined as static things which are used once and are immutable. Referencing them again will refer to the same copy in memory. 

Symbols can only be values set to other objects.

They are defined via ":". Here are some examples:

```
:hello 
:'Hello World'                    # If you want to use symbol names with spaces
:"Hello World"                    # Ruby doesn't care whether you use single or double quotes like JS
:'Hello World'.to_s               # => 'Hello World'
books['some-book'] = :hello       # Sets the object's value at that key to the symbol ':hello'
books['some-other-book'] = :hello # Refers to the SAME object as books['some-book']
```

**WARNING: *For Ruby versions < 2.2*, SYMBOLS TAKE UP MEMORY AND CANNOT BE GARBAGE COLLECTED. YOU CAN CAUSE DoS PROBLEMS IF YOU AREN'T CAREFUL**


##Decrementing and Incrementing

Unlike JS, Ruby has no preincrement ```++i```, postincrement ```i++```, predecrement ```--i``` and postdecrement ```i--``` for the same reasons as python:

* Makes implementation simpler
* ++ and -- are NOT reserved operator in Ruby.
* C's increment/decrement operators are in fact hidden assignment. They affect variables, not objects. You cannot accomplish assignment via method. 

So instead, Ruby uses +=/-= operator

```
i++     # throws an error in Ruby
i += 1  # Right way
```
