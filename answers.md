# Ruby 'hard as fuck' quiz

All questions are based on the behaviour of Ruby 2.6.  
Multiple choice questions have exactly 1 correct answer.

## 1) What is the difference between

```ruby
x = nil || 3
```

```ruby
x = nil or 3
```

### Answer

The first sets `x` to `3` and returns `3`.  
The second sets `x` to `nil` and returns `3`.  
The `or` keyword has lower priority than the `=` operator, while `||` has higher.  
Therefore, `x` is set to be `nil` first, which returns `nil` and the resulting expression `nil or 3` returns `3`.

## 2) What does the following code print?

```ruby
h = Hash.new []
h[:one] << 1
h[:two] << 2
puts h[:two].to_s
puts h.keys.to_s
```

### Answer

`[1, 2]`  
`[]`  
The first line creates a `Hash` with an empty `Array` as the default value.  
When accessing a key that is not present in the `Hash`, the default value is returned.
`h[:one]` returns the empty `Array`. Then `1` is added to the `Array`. On the next line, `h[:two]` returns the `Array`, which now contains `1`. Then `2` is added to it.  
Because nothing was actually assigned to the keys `:one` and `:two`, those keys still don't exist in the `Hash`.  
Therefore, printing the keys only prints an empty `Array`.  
In order to create a new empty `Array` and set it to the accessed key every time, a block would have to be given to the `Hash`, like this:

```ruby
Hash.new { |hash, key| hash[key] = [] }
```

## 3) What is the difference between

```ruby
super
```

```ruby
super()
```

### Answer

Calling `super` without parentheses implicitly passes all arguments the current method received into the `super` method.  
Calling `super` with empty parentheses calls the `super` method with no arguments, as expected.

## 4) `private` is a

* Keyword
* Variable
* Constant
* Method
* None of the above

### Answer

Although it looks like a keyword, `private` is a method. Some text editors will show this by coloring it differently from keywords.  
When used without arguments, it makes all methods defined afterwards private.  
It can also be used with an argument, the name of the method you want to make private.

```ruby
def foo
end
private :foo
```

This also works, because method definition returns a symbol with the method's name.

```ruby
private def foo
end
```

## 5) What does the following code do?

```ruby
obj&.foo
```

### Answer

The safe navigation operator only calls the `foo` method if `obj` is not nil.  
This can mostly replace code like this:

```ruby
obj && obj.foo
```

The difference is that the safe navigation operator checks that `obj` is not `nil`, while the other syntax checks whether `obj` is truthy. Both `nil` and `false` are falsey, therefore with this syntax, the `foo` method will not be called if `obj` is `nil` or `false`.

## 6) For which of these things is the `<<` operator NOT used?

* Concatenating strings
* Writing to a file
* Bitwise shifting a number
* Composing Methods or Procs
* Adding an element to an Array
* Adding an element to a Set
* It is used for all of these

### Answer

All are possible. The newest addition is the use as a functional composition operator. This was added in Ruby 2.6 and can be used to compose two `Method`s or `Proc`s into one. Both `<<` and `>>` can be used for this.  
`f1 >> f2` creates a new `Proc`. When it is called, it runs `f1` and then uses its return value as an argument for `f2`.

```ruby
f1 = ->(x) { x + 1 }
f2 = ->(x) { x * 2 }
f3 = f1 >> f2
f4 = f1 << f2
f3.call 5 # (5 + 1) * 2 => 12
f4.call 5 # (5 * 2) + 1 => 11
```

## 7) What is the difference between the `redo` and `retry` keywords?

### Answer

The `redo` keyword can be used inside a loop to restart the current iteration. The iteration will start over without checking any condition to determine whether the loop should end.

```ruby
# This will sometimes print a number multiple times
10.times.each do |n|
  puts n
  redo if n == (rand * 10).round
end
```

The `retry` keyword can be used inside a `rescue` to rerun the block this `rescue` is associated with.

```ruby
# This will keep running the block over and over
begin
  raise StandardError
rescue StandardError
  retry
end
```

## 8) What are the three ways to mix in contents of a `module` into other objects and how do they differ?

### Answer

`include` can be used to mix in methods from a `module` into a `class` as instance methods. This places the `module` above the `class` in the hierarchy that is used for method lookup. Therefore, if the `class` defines a method with the same name as a method from the `module`, the implementation from the `class` will be used.

`prepend` works just like `include`, but the `module` is placed in front of the `class` in the method lookup hierarchy. This means that methods from the `module` will be used even if the `class` tries to override them.

`extend` can be used to mix in method from a `module` into the object `extend` is called on. Typically, `extend` is used in a `class` to add methods from the `module` as class methods. However, any object can be extended this way.

## 9) What are the three ways to call a `Proc` or `Method` object?

### Answer

```ruby
f = -> { puts 'Called' }

f.call
f.()
f[]
```

`.()` is just an alias for the `call` method, so overriding `call` changes the behaviour of both. `[]` is a separate method and will still work the same, though.  
Arguments for the `Proc` can be passed into the `call` method's arguments, into the parentheses when using the `.()` syntax, or into the square brackets when using `[]`.

## 10) How to run a piece of code whenever a certain class is subclassed?

### Answer

By defining the `inherited` class method. The subclass is passed as an argument to the method. The same can be done with a `module` by implementing `included`, `prepended` or `extended`, based on the way the `module` is mixed into another object.

```ruby
class Foo
  def self.inherited(child)
    # ...
  end
end
```

## 11) How to make an object evaluate to false in a condition like this

```ruby
if obj
  # ...
end
```

* Override the nil? method to return true
* Override the ! operator to return true
* Override both the nil? method and the ! operator to return true
* Override the is_a? method to return true if the argument is NilClass or FalseClass
* This is not possible

### Answer

This is not possible. Only `nil` and `false` are falsey and no other object can be made falsey.

## 12) Can `next` be used with an argument?

* Yes, it accepts any number of arguments
* Yes, it accepts any number of arguments and a block
* Yes, it accepts up to one argument
* Yes, it accepts up to one argument and a block
* No, it cannot be used with an argument

### Answer

Yes, it accepts any number of arguments. The given argument will be returned from the current iteration of the loop. Multiple arguments can be passed, they will be put into an `Array`, which will then be returned.

```ruby
10.times.map do |n|
  next -1 if n > 5
  n
end
# => [0, 1, 2, 3, 4, 5, -1, -1, -1, -1]
```

## 13) What are the differences between

```ruby
rescue
```

```ruby
rescue StandardError
```

```ruby
rescue Exception
```

### Answer

`rescue` and `rescue StandardError` are the same. They both rescue anything that is a `StandardError` or a subclass of it. This rescues any error that normally gets raised in a program.  
`Exception` is the base class for all errors, including ones that the program cannot do anything about, like `NoMemoryError` or `SystemStackError`.

## 14) Explain the three operators used in the parameter list

```ruby
def foo(*a, **b, &c)
  # ...
end
```

### Answer

The splat operator `*` takes any number of arguments and puts them into an `Array`, which will be accessible as `a` inside the method.  
The double splat operator `**` takes any named arguments and puts them into a `Hash`, which will be accessible as `b` inside the method.  
The `&` operator converts the block this method is given into a `Proc` and puts it into `c`.

## 15) What does the following code do?

```ruby
class Foo
  class << self
    def bar
    end
  end
end
```

### Answer

It defines `bar` as a class method of `Foo`. It is the same as the following code:

```ruby
class Foo
  def self.bar
  end
end
```

The `class << obj` syntax evaluates code in the context of the `obj`'s singleton class. The most common use is `class << self`. This allows us to define methods for the `class` itself and use macros like `attr_accessor` for class instance variables.

## 16) What is the superclass of the `Class` class?

### Answer

The `Class` class inherits from `Module`.  
This means that every class is technically also a module. Unlike `Module`, instances of `Class` implement the `new` method. On the other hand, classes cannot be included, prepended or extended, which breaks the Liskov substitution principle.

## 17) What does `throw` and `catch` mean?

### Answer

Unlike in other languages, `throw` and `catch` in Ruby are not used for dealing with exceptions. Instead, they can be used for flow control like in the following example:

```ruby
# Using throw anywhere in the block (or in methods called inside the block) will return control to the place where catch is used.
catch :my_label do
  # ...
  throw :my_label if some_condition
  # ...
end
```

This is similar to rescuing an error raised inside the block, but unlike errors, `throw` and `catch` are specifically meant for flow control. Since this also behaves similarly to the goto keyword in other languages, it easily can make the execution path of the code confusing.

## 18) How to define a private constant?

### Answer

`private` only works on methods. For constants, it is necessary to use `private_constant` instead.

```ruby
FOO = 1
private_constant :FOO
```

## 19) What is the difference between

```ruby
render json: values.map { |value| value * 2 }
```

```ruby
render json: values.map do |value|
  value * 2
end
```

### Answer

In the first one, a json containing doubles of the values gets rendered. The block belongs to the map method as expected.
In the second one, the block belongs to the render method, which ignores it and renders the enumerator returned from map that did not receive a block.

## 20) What is the problem with class variables (related to inheritance) and what can be used instead of them?

### Answer

A class variable is shared by all classes in the hierarchy.

```ruby
# Both classes refer to the same variable, so its value will be rewritten by Bar
class Foo
  @@my_var = 1
end

class Bar < Foo
  @@my_var = 2
end
```

The solutiuon is to use class instance variables. These are regular instance variables, but defined for the class itself, not for its instances.

```ruby
# Each class has its own variable
class Foo
  @my_var = 1
end

class Bar < Foo
  @my_var = 2
end
```
