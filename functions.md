# Blocks, Procs, Lambdas and Functions

## Blocks, ```proc``` and ```lambda```<a name="blocks-procs-and-lambdas"></a>

In Ruby, there is a reusable piece of code called a block. It is designated by curly braces: ```{}```

Many functions such as `sort`, `each` and `times` allow you to specify a code block to execute.

For example, in Ruby, if you wanted to print "Hello" 5 times, you could do:

```ruby
# The code inside the curly braces is a block that is passed to the function 'times'
5.times { puts "Hello" }
```

### Passing in code blocks as parameters to functions
If you want to pass in a code block as a valid parameter to a function, you need to specify to Ruby that it is a function/code block by using the ```&``` prefix. See below:

**IMPORTANT:** Like the [splat arguments](#splat-arguments), this needs to be defined as a parameter of the function **last**

```ruby
# Note the '&' before the 'func' argument in the method definition
def foo(&func)
	func.call
end
foo { puts "Hello World" }
```

### Isn't a block just an anonymous function?
Not exactly. Blocks look VERY similar at a first glance but an anonymous function is more like a Ruby [lambda function](#lambda-functions)

### So...what is a ```proc```?
Ruby is built on an "everything is an object" philosophy and that ironically has a few exceptions, one of which is that **a block is NOT an object**.

So how do we reuse blocks? We give it a name but a name can only be given to an object. This is where ```proc``` comes in. It is a container object that holds blocks.

To declare a ```proc```, you can use ```Proc.new```. 

To reference a proc inside a function, you use the ```&``` prefix with the name of the proc. When the prefix is attached, Ruby essentially unpacks/unwraps the block and passes it to the function.  

See below:

```ruby
times_2 = Proc.new { |num| num * 2 }  #define the proc
[1,2,3].map(&times_2)                 #note how the proc is prefixed with the '&'
									   # => [2,4,6]
```

### Calling Procs directly with ```call```

To call procs directly, use the ```call``` method like so:

```ruby
hello = Proc.new { puts 'Hello World' }
hello.call # => Hello World
```

**WARNING:** using ```return``` inside a proc will cause it to not only exit out of the proc but return outside of the calling function. See below:

```ruby
def foo
	some_proc.call    #The return in this proc will kick us out of function foo
	return 'Goodbye'  #This never gets executed :'(
end

some_proc = Proc.new { return 'Hello' }

foo
```

So:

```ruby
'Goodbye' #Expected
'Hello'   #Actual :(
```


### Converting Symbols to Procs for shorthand blocks
In Ruby, there is a bit of shorthand notations which will, for most JS developers not familair with Ruby, look like black magic.

Converting symbols to Procs is one such example. See this example below:

```ruby
["1", "2", "3"].map(&:to_i) # => [1,2,3]
```

Most devs will probably be like...

![](http://replygif.net/i/311.gif)

Everything is fairly normal until we hit the ```&:to_i```


What is happening is:

* ```to_i``` is a function and when combined with ```:``` gets turned into a symbol referencing the ```to_i``` function (which just converts the object to an integer).
* Recall that the ```&``` prefix for a proc usually unwraps/unpacks the block for a function to consume. In this case, since it's a symbol and not a block, Ruby calls the ```to_proc``` function to convert it to a block. 
* This newly formed block is fed into the map function.

So...:

```ruby
["1", "2", "3"].map(&:to_i) # => [1,2,3]
["1", "2", "3"].map { |arg| arg.to_i } # same as the above statement
```

**NOTE: This notation and referencing the method via ```&``` works with ```lambda``` (see next section) too.**

### What are lambdas?
A lambda is a ```proc``` but with a few small additions. It is declared almost the same way as a proc. 

```ruby
foo = Proc.new { puts "Hello World" }
bar = lambda { puts "Goodbye Cruel World" } 
```

#### Then aren't lambdas and procs the same?

Nope. Lambdas and procs have 2 major differences:

1. Lambdas check the number of arguments passed to it while procs do not. This means if a lambda has 3 arguments and is passed 2, it throws an error. Procs on the other hand just assign ```nil``` to any missing arguments.
2. Unlike procs, when a return statement is hit inside a lambda, it only exits out of the lambda and not out of the calling method.

```ruby
foo = lambda { |x, y| x + y }

def bar
	foo.call(5) #Insufficient args so Ruby throws an 'ArgumentError' 
	return 'Hello'
end

bar
```

```ruby
foo = lambda { |x,y| return x + y } #The return only exits out of lambda, not calling method

def bar
	foo.call(5, 6)
	return 'Hello'
end

bar # Returns 'Hello' not 11
```

#### Why can't you call Procs and Lambda functions like regular functions with `()`

If a lambda is defined like so:

```ruby
foo = lambda { |x,y| x + y }
```

Then invoking it like below will throw an error:

```ruby
foo(5) # => NoMethodError: undefined method `foo` for Object
```

This is better explained at [StackOverflow here](http://stackoverflow.com/questions/1686844/why-isnt-the-ruby-1-9-lambda-call-possible-without-the-dot-in-front-of-the-pare)

### Passing Procs/Lambdas as parameters to functions

To pass procs/lambdas as parameters to functions for actions such as filtering, use the ```&``` to refer to the proc/lambda. This essentially unpackages/unwraps the code block from the proc/lambda

```ruby
stuff = [:hello, :goodbye, 'Hello', 'Goodbye']
is_symbol = Proc.new { |val| val.is_a? Symbol } #Lets filter out anything that isn't a symbol

stuff.select!(&is_symbol)                       # => [:hello, :goodbye]
```


## Functions and Parameters

### Defining functions

To define a function, the ```def``` keyword is used and is wrapped with an ```end``` to finish off the code block.

For example: 

```ruby
#Function that takes in no args
def foo
	puts 'Hello World'
end
```

or:

```ruby
#function that takes in 1 argument
def foo(name)
	puts "Hello #{name}!"
end
```

#### Returning from a function

If you want to return something, you can use the ```return``` keyword. **HOWEVER**, if the last line of a function is the return statement, then the ```return``` keyword isn't needed. Ruby will automatically return the last variable set or retrieved without the return keyword. See below:

```ruby
#bad
def foo
	name = 'hello'
	return name #unnecessary return
end

#good way to do it
def foo
	name = 'hello' #Ruby will auto return this variable value.
end
```

Another example:

```ruby
def times_2(n):
	n * 2    #Ruby will auto return this value without the  return statement.
end
```

A ```return``` IS needed if this is **NOT** the last line of the function


```ruby
def foo(x):
	return "hello" if x == "friend"
	return "hater!" if x == "hater"
	"stranger"
end
```


#### Functions with an arbitrary number of arguments using splat (```*```)<a name="splat-arguments"></a>
In Java, you would define an arbitrary number of arguments of the same type via the ```...``` keyword like so:

```java
// In Java
public void function(int ...numbers) { /* do something */ }
```

In Ruby, the splat arguments keyword ```*``` is used. This tells ruby that *we don't know how many arguments there are. **We just know there is at least one***

For example:

```ruby
def todd_five(greeting, *bros)
  bros.each { |bro| puts "#{greeting}, #{bro}!" }
end
 
todd_five("Innuendo Five!", "J.D", "Turk")
```


### Executing functions with no parameters

In JavaScript, it is mandatory to execute function with no arguments with parentheses but in ruby, parentheses aren't required and so most devs don't use them:

```ruby
SomeClass.foo  #This is equivalent to SomeClass.foo() in JS
```

### Executing functions with one or more known parameters
Unlike JS, in Ruby, parameters don't need to be enclosed by parentheses. The Ruby intepreter just looks for the parameters to be seperated by commas.

```ruby
puts 'Hello' 'World' 'Goodbye' # Similar to console.log('Hello', 'World', 'Goodbye')
puts('Hello', 'World')         # Also correct in Ruby but less used as its unneccessary
```


#### When to use parentheses
See the [Coding Conventions](coding_conventions.md#parentheses) page for more details.

**tl;dr**: Omit parentheses for anything with keyword status in Ruby and explicitly use parentheses for everything else.

### Functions as arguments

In Ruby, functions can be passed blocks via {} which act similar to anonymous functions in JavaScript (see below)

```ruby
books = {}
books['stuff'] = 'hello' #setting a key and value is the same as in JS
books.values.each { puts 'Hello' } #equivalent to Object.values(books).forEach(function(){ console.log('Hello') }); in JS
```

### Functions doing changes inplace
Some JS functions do their changes to an object in place and some don't. Ruby, however, makes this matter very clear.

Executing the function and ending with `!` makes the change in-place. For example:

```ruby
name = "Gautham"
puts name.upcase  # => "GAUTHAM"
puts name         # => "Gautham" (the name wasn't changed)
puts name.upcase! # => "GAUTHAM"
puts name         # => "GAUTHAM" (the name was changed when ! was specified in prev statement)
```

For functions with parameters, the ```!``` comes before the parameters

```ruby
str = "sea shells by the sea shore"
str.gsub!(/s/, "th")        # Note how the ! is before the parameters
print str                   # Returns "thea thhellth by the thea thhore"
```

Note that you cannot use the '!' to try to do an inplace change if the original type and resulting type from the operation are different. 

```ruby
str = 'Hello'
str.to_i!      #This will return an error. String and integer are not the same type
str = str.to_i #The workaround
```


### Function parameters to anonymous functions

In Ruby, parameters to anonymous functions are enclosed via `|` or "pipe" characters like so:

```ruby
books.values.each { |book| puts 'Gotta read ' + book } 
```

which is equivalent to in JavaScript:

```javascript
Object.values(books).forEach(function(book) {
	console.log('Gotta read ' + book);
});
```

### ```yield``` and executing code blocks inside functions
In Ruby, you can yield control to run a block of code inside another function. This has a variety of uses.

As a refresher, a block is just a reusable piece of code enclosed by braces ```{}```

Using the ```yield``` keyword you can invoke code blocks and pass it arguments as if they were functions. **Unless the blocks are specified as arguments, they are outside the parentheses. See below:**

```ruby
def foo(greeting)
	puts 'Inside the function foo'
	yield(greeting)
	puts 'Back inside the function foo'
end
foo("Top of the mornin to ya") { |arg| puts "#{arg} John" }
```

This prints out:

```
Inside the function foo
Top of the mornin to ya John
Back inside the function foo
```

