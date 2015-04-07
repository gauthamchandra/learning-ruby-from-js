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


##Objects/Dictionaries

Objects in JavaScript are called dictionaries in Ruby. 

###Traditional/JS-y way to define it:

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

####When to use parentheses
See the [Coding Conventions](coding_conventions.md#parentheses) page for more details.


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

###```unless``` keyword

This is similar to the if condition except it will execute the statement if the result is **false**

```
i = 0
unless i == 3
	print "Hooray it's not 3!" #will execute if the i == 3 is false which it is.
end
# => "Hooray it's not 3!"
```

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

###Using ```times``` iterator for simple looping

If you want to make a block of code execute a fixed number of times, the best way to do it would be through a "times" loop

```
3.times { puts "Hello!" } #Prints "Hello!" 3 times.
```
