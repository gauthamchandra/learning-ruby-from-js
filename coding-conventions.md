#Coding Conventions

##Curly Braces vs do...end

Most style guides and best practices state that:

* curly braces {} should be used for one liners
* and do...end should be used for multiline statements

There is also a subtle difference between the two:

**Braces have a high precedence; do has a low precedence. If the method invocation has parameters that are not enclosed in parentheses, the brace form of a block will bind to the last parameter, not to the overall invocation. The do form will bind to the invocation**

The examples below make it more clear:

This:

```
task :rake => pre_rake_task do
  something
end
```
Really means:

```
task(:rake => pre_rake_task){ something }
```

And this with curly braces:

```
task :rake => pre_rake_task {
  something
}
```

really means:

```
task :rake => (pre_rake_task { something })
```

See [Stackoverflow](http://stackoverflow.com/questions/5587264/do-end-vs-curly-braces-for-blocks-in-ruby)  here for more details
