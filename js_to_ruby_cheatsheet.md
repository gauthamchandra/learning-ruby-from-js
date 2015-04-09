#JavaScript --> Ruby Cheatsheet

This is a simple cheatsheet/short guide of sorts for a JavaScript developer who wants to learn Ruby from scratch.

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

##Reading and Writing to the Screen/Console

###Print vs puts
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

But where JS and Ruby differ is in their notation. JSON uses ```:``` to seperate key value pairs but Ruby uses ```=>``` instead.

**IMPORTANT: The keys HAVE to be defined as strings in Ruby unlike JS which doesn't have to be unless it uses invalid symbols like spaces or "-"**

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


##Functions and Parameters

###Defining functions

To define a function, the ```def``` keyword is used and is wrapped with an ```end``` to finish off the code block.

For example: 

```
#Function that takes in no args
def foo
	puts 'Hello World'
end
```

or:

```
#function that takes in 1 argument
def foo(name)
	puts "Hello #{name}!"
end
```

####Returning from a function

If you want to return something, you can use the ```return``` keyword. **HOWEVER, if the last line of a function is the return statement, then the ```return``` keyword isn't needed. Ruby will automatically return the last variable set or retrieved without the return keyword. See below:

```
#bad
def foo
	name = 'hello'
	return name #unnecessary return
end
#
#good way to do it
def foo
	name = 'hello' #Ruby will auto return this variable value.
end
```

Another example:

```
def times_2(n):
	n * 2    #Ruby will auto return this value without the  return statement.
end
```

A ```return``` IS needed if this is **NOT** the last line of the function


```
def foo(x):
	return "hello" if x == "friend"
	return "hater!" if x == "hater"
	"stranger"
end
```




####Functions with an arbitrary number of arguments
In Java, you would define an arbitrary number of arguments of the same type via the ```...``` keyword like so:

```
#in Java
public void function(int ... numbers) { ... }
```

In Ruby, the splat arguments keyword ```*``` is used. This tells ruby that *we don't know how many arguments there are. **We just know there is at least one***

For example:

```
def todd_five(greeting, *bros)
  bros.each { |bro| puts "#{greeting}, #{bro}!" }
end
 
todd_five("Innuendo Five!", "J.D", "Turk")
```


###Executing functions with no parameters

In JavaScript, it is mandatory to execute function with no arguments with parentheses but in ruby, parentheses aren't required and so most devs don't use them:

```
SomeClass.foo  #This is equivalent to SomeClass.foo() in JS
```

###Executing functions with one or more known parameters
Unlike JS, in Ruby, parameters don't need to be enclosed by parentheses. The Ruby intepreter just looks for the parameters to be seperated by commas.

```
puts 'Hello' 'World' 'Goodbye' # Similar to console.log('Hello', 'World', 'Goodbye')
puts('Hello', 'World')         # Also correct in Ruby but less used as its unneccessary
```


####When to use parentheses
See the [Coding Conventions](coding_conventions.md#parentheses) page for more details.

**tl;dr**: Omit parentheses for anything with keyword status in Ruby and explicitely use parentheses for everything else.

###Functions as arguments

In Ruby, functions can be passed blocks via {} which act similar to anonymous functions in JavaScript (see below)

```
books = {}
books['stuff'] = 'hello' #setting a key and value is the same as in JS
books.values.each { puts 'Hello' } //equivalent to books.values.each(function(){ console.log('Hello') }); in JS
```

###Functions doing changes inplace
Some JS functions do their changes to an object in place and some don't. Ruby, however, makes this matter very clear.

Executing the function and ending with ! makes the change in-place. For example:

```
name = "Gautham"
puts name.upcase  # => "GAUTHAM"
puts name         # => "Gautham" (the name wasn't changed)
puts name.upcase! # => "GAUTHAM"
puts name         # => "GAUTHAM" (the name was changed when ! was specified in prev statement)
```

For functions with parameters, the ```!``` comes before the parameters

```
str = "sea shells by the sea shore"
str.gsub!(/s/, "th")        # Note how the ! is before the parameters
print str                   # Returns "thea thhellth by the thea thhore"
```

Note that you cannot use the '!' to try to do an inplace change if the original type and resulting type from the operation are different. 

```
str = 'Hello'
str.to_i!      #This will return an error. String and integer are not the same type
str = str.to_i #The workaround
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

##Comparing/Converting/Checking different objects

###Comparing two different objects

**Unlike JS, there is no "strict" type checking and type coercion in Ruby**

The JS === is the same as ==. 

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


###Checking Types 
To check the type of an object, use ```is_a?``` function

```
greeting = 'Hello'
greeting.is_a? Integer # Checking if greeting is an Integer. => false
greeting.is_a? String  # Checking if greeting is a String. => true
```

###Converting

####To a string
Converting to a String involves using the `to_s` function

```
3.to_s # => "3"
```

####To an integer
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

##Looping and control statements

###Conditional assignment

In JS, we sometimes want to only set the value of a variable if its not already defined. To do that in JS, we would do something like this:

```
some_var = some_var || {}
```

In Ruby, there is an even cooler and more terse way (Yey for terseness!). Here is how to write the aforementioned statement in Ruby:

```
some_var ||= {} #This is similar to a += or /= operator
```

###```if``` keyword

This works just like the JavaScript version but with one small enhancement: it can be used inline with other statements (in a very cool readable way). See below:

```
puts "Hello Bruce" if name == 'Bruce'
```

###```unless``` keyword

This is similar to the if condition except it will execute the statement if the result is **false**

```
i = 0
unless i == 3
	print "Hooray it's not 3!" #will execute if the i == 3 is false which it is.
end
# => "Hooray it's not 3!"
```

You could also use the unless statement inline with other statements (in an almost backwards if statement). See below:

```
#Doing a type check to see if its an integer. The ? is just a naming convention to indicate it returns a boolean
puts "That's not an integer bro!" unless some_int.is_a? Integer 
```

###```switch```/```case``` statements
What is called Switch statements in JS and Java and other languages is called simply case statements in Ruby.

This in Ruby:

```
case language
when "JS"
	puts "Dat frontend scripting language!"
when "Java"
	puts "Aw man. The kingdom of nouns!"
else
	puts "Some cool language I don't know!"
end
```

or a more terse way to write it using ```when...then```:

```
case language
	when "JS" then puts "Dat frontend scripting language!"
	when "Java" then puts "Aw man. The kingdom of nouns!"
	else puts "Some cool language I don't know!" 
end
```

is equivalent to in JS:

```
switch(language) {
	case "JS":
		puts "Dat frotnend scripting language";
		break;
	case "Java":
		puts "Aw man. The kingdom of nouns!";
		break;
	default:
		"Some cool language I don't know!"
}
```

So switch  keyword in JS translates to case in Ruby
   case    keyword in JS translates to when in Ruby
   default keyword in JS translates to else in Ruby


###```for``` loops

unlike JS, Ruby has a different ```for``` syntax where it defines the range:

There are two forms for this:
* with two dots used in between the range (to be max bound inclusive)
* with three dots used in between the range (to be max bound exclusive)

See example below:


```
for i in 0..10 # same as "for(var i = 0; i <= 10; i++)" in JS. NOTE THE TWO DOTS FOR 10 inclusive
	#some code
end
for i in 0...10 # same as "for(var i = 0; i < 10; i++)" in JS. NOTE THE THREE DOTS FOR 10 exclusive
	#some code
end
```

###Using the ```loop``` keyword (i.e do..while statement in JS)

In Ruby, there is another way to loop through code called the ```loop``` statement. It takes in a code block and asks for a condition to exit out of the loop if the condition is met with the ```break``` keyword.

**Like the do..while loop in other languages, the loop keyword always executes at least once**

For example:

```
#Ruby way to do it
i = 0
loop do
	i += 1
	puts 'Hello Person #{i}'
	break if i > 9
end
```

This is equivalent to this do..while in JS:

```
var i = 0;
do {
	i += 1;
	console.log('Hello Person ' + i);
} while (i > 9);
```

###Using ```until``` keyword in a ```while``` loop

Just like how the ```unless``` keyword is the opposite of the ```if``` statement, the ```while``` also has its opposite: ```until```. It is used in a similar way to ```unless```.

```
#The following code loops until j = 0 and then breaks out of the loop.
j = 3
until j == 0 do
	puts j
	j -= 1
end
```


####Using the ```next``` keyword in loop

You can use the 'next' keyword to skip the loop if a condition is met. It is equivalent to the ```continue``` keyword used in combination with an if.

For example:

```
for i in 1..5
	next if i % 2 == 0 #skip if its an even number
	puts i
end
``` 

Equivalent to JS's continue used with an if statement:

```
for (var i = 0; i <= 5; i++) {
	if (i % 2 == 0) {
		continue;
	}
	console.log(i);
}
```

###Using the ```each``` iterator for looping

The each iterator is similar to the Array's each() or Underscore/Lodash's _.each function. 

You specify an object to loop through and then specify a code block to run for each item in the object. 

For example, the following in Ruby:

```
object = ['hello', 'goodbye']
object.each { |str| puts str }

#or written with do..end instead of {}
object.each do |str| 
	puts str
end
```
is equivalent to in JS:

```
var object = ['hello', 'goodbye'];
object.each(function(str) {
	console.log(str);
});
```

To iterate over hashes (i.e JS objects), you can specify 1 parameter (where the parameter can be both the key and value) or 2 parameters (one for key and one for value):

```
person = {
	'name' => 'Goku',
	'power_level' => 9000000000
}
#
#Using 2 parameter prints:
# name => Goku
# power_level => 9000000000
person.each { |key,val| puts "#{key} => #{val}" } 
#
# Using 1 parameter prints:
# name
# Goku
# power_level
# 9000000000
person.each { |key_or_val| puts |key_or_val| }
```

The reason for this is due to the way the iterator is setup for the each.


###Using ```times``` iterator for simple looping

If you want to make a block of code execute a fixed number of times, the best way to do it would be through a "times" loop

```
3.times { puts "Hello!" } #Prints "Hello!" 3 times.
```




