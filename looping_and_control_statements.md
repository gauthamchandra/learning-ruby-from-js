# Looping and control statements

### Conditional assignment

In JS, we sometimes want to only set the value of a variable if it's not already defined. To do that in JS, we would do something like this:

```js
some_var = some_var || {}
```

In Ruby, there is an even cooler and more terse way (Yey for terseness!). Here is how to write the aforementioned statement in Ruby:

```ruby
some_var ||= {} #This is similar to a += or /= operator
```

### ```if``` keyword

This works just like the JavaScript version but with one small enhancement: it can be used inline with other statements (in a very cool readable way). See below:

```ruby
puts "Hello Bruce" if name == 'Bruce'
```

### ```unless``` keyword

This is similar to the if condition except it will execute the statement if the result is **false**

```ruby
i = 0
unless i == 3
	print "Hooray it's not 3!" #will execute if the i == 3 is false which it is.
end
# => "Hooray it's not 3!"
```

You could also use the unless statement inline with other statements (in an almost backwards `if` statement). See below:

```ruby
#Doing a type check to see if it's an integer. The `?` is just a naming convention to indicate it returns a boolean
puts "That's not an integer bro!" unless some_int.is_a? Integer 
```

### ```switch```/```case``` statements
What is called Switch statements in JS and Java and other languages is called simply case statements in Ruby.

This in Ruby:

```ruby
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

```ruby
case language
	when "JS" then puts "Dat frontend scripting language!"
	when "Java" then puts "Aw man. The kingdom of nouns!"
	else puts "Some cool language I don't know!" 
end
```

is equivalent to in JS:

```js
switch (language) {
	case "JS":
		console.log("Dat frotnend scripting language");
		break;
	case "Java":
		console.log("Aw man. The kingdom of nouns!");
		break;
	default:
		console.log("Some cool language I don't know!");
}
```

So **switch**  keyword in JS translates to **case** in Ruby
   **case**    keyword in JS translates to **when** in Ruby
   **default** keyword in JS translates to **else** in Ruby


### ```for``` loops

unlike JS, Ruby has a different ```for``` syntax where it defines the range:

There are two forms for this:
* with two dots used in between the range (to be max bound inclusive)
* with three dots used in between the range (to be max bound exclusive)

See example below:


```ruby
for i in 0..10 # same as "for(var i = 0; i <= 10; i++)" in JS. NOTE THE TWO DOTS FOR 10 inclusive
	#some code
end
for i in 0...10 # same as "for(var i = 0; i < 10; i++)" in JS. NOTE THE THREE DOTS FOR 10 exclusive
	#some code
end
```

### Using the ```loop``` keyword (i.e do..while statement in JS)

In Ruby, there is another way to loop through code called the ```loop``` statement. It takes in a code block and asks for a condition to exit out of the loop if the condition is met with the ```break``` keyword.

**Like the do..while loop in other languages, the loop keyword always executes at least once**

For example:

```ruby
#Ruby way to do it
i = 0
loop do
	i += 1
	puts 'Hello Person #{i}'
	break if i > 9
end
```

This is equivalent to this `do..while` in JS:

```js
var i = 0;
do {
	i += 1;
	console.log('Hello Person ' + i);
} while (i > 9);
```

### Using ```until``` keyword in a ```while``` loop

Just like how the ```unless``` keyword is the opposite of the ```if``` statement, the ```while``` also has its opposite: ```until```. It is used in a similar way to ```unless```.

```ruby
#The following code loops until j = 0 and then breaks out of the loop.
j = 3
until j == 0 do
	puts j
	j -= 1
end
```


#### Using the ```next``` keyword in loop

You can use the 'next' keyword to skip the loop if a condition is met. It is equivalent to the ```continue``` keyword used in combination with an if.

For example:

```ruby
for i in 1..5
	next if i % 2 == 0 #skip if its an even number
	puts i
end
``` 

Equivalent to JS's continue used with an if statement:

```js
for (var i = 0; i <= 5; i++) {
	if (i % 2 == 0) {
		continue;
	}
	console.log(i);
}
```

### Using the ```each``` iterator for looping

The each iterator is similar to the Array's `.forEach()` or Underscore/Lodash's `_.each` function. 

You specify an object to loop through and then specify a code block to run for each item in the object. 

For example, the following in Ruby:

```ruby
object = ['hello', 'goodbye']
object.each { |str| puts str }

#or written with do..end instead of {}
object.each do |str| 
	puts str
end
```
is equivalent to in JS:

```javascript
var object = ['hello', 'goodbye'];
object.forEach(function(str) {
	console.log(str);
});
```

To iterate over hashes (i.e JS objects), you can specify 1 parameter (where the parameter can be both the key and value) or 2 parameters (one for key and one for value):

```ruby
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


### Using ```times``` iterator for simple looping

If you want to make a block of code execute a fixed number of times, the best way to do it would be through a "times" loop

```ruby
3.times { puts "Hello!" } #Prints "Hello!" 3 times.
```

