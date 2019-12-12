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

## 2) What does the following code print?

```ruby
h = Hash.new []
h[:one] << 1
h[:two] << 2
puts h[:two].to_s
puts h.keys.to_s
```

## 3) What is the difference between

```ruby
super
```

```ruby
super()
```

## 4) `private` is a

* Keyword
* Variable
* Constant
* Method
* None of the above

## 5) What does the following code do?

```ruby
obj&.foo
```

## 6) For which of these things is the `<<` operator NOT used?

* Concatenating strings
* Writing to a file
* Bitwise shifting a number
* Composing Methods or Procs
* Adding an element to an Array
* Adding an element to a Set
* It is used for all of these

## 7) What is the difference between the `redo` and `retry` keywords?

## 8) What are the three ways to mix in contents of a `module` into other objects and how do they differ?

## 9) What are the three ways to execute the block in a `Proc` or `Method` object?

## 10) How to run a piece of code whenever a certain class is subclassed?

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

## 12) Can `next` be used with an argument?

* Yes, it accepts any number of arguments
* Yes, it accepts any number of arguments and a block
* Yes, it accepts up to one argument
* Yes, it accepts up to one argument and a block
* No, it cannot be used with an argument

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

## 14) Explain the three operators used in the parameter list

```ruby
def foo(*a, **b, &c)
  # ...
end
```

## 15) What does the following code do?

```ruby
class Foo
  class << self
    def bar
    end
  end
end
```

## 16) What is the superclass of the `Class` class?

## 17) What does `throw` and `catch` mean?

## 18) How to define a private constant?

## 19) What are the differences between public, protected and private methods?

## 20) What is the problem with class variables (related to inheritance) and what can be used instead of them?
