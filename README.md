# Ruby Errors

## Learning Goals

- Read the three different parts of an error message.
- Identify four error types- name errors, syntax errors, type errors, and division errors- and fix them

## Introduction

This lesson provides a closer look at some common types of errors. Right now, it
may be the case that not all of these error messages make sense or seem
meaningful to you. After all, we've only handled a few code examples at this
point.

This lesson is meant to be a resource for you to refer back to as you start
building more complicated programs and have to debug them. **There are a few
important takeaways from this lesson:**

1) Don't be afraid of broken programs! It's easy to get frustrated when your
   program breaks. A lot of beginners tend to jump right back into the code when
   a test fails, or an error comes up as a consequence of running a program,
   *without reading the error messages*. Error messages are there to guide you.
   They contain important information about the location and type of problem you
   are encountering. Embrace them and get comfortable reading them––don't run
   away from them.

2) Pay attention to the helpful part of error messages. Check out the line
   number, file name, and the type of error that you're receiving. This will
   point you in the right direction. Let's take a look at an example from an
   earlier lab.

There are four files in this lab, each with a particular error. The video below
discusses each error and how to resolve them. Everything you need to pass this
lab can be found in the video, but additional examples are also provided further
down this page.

<iframe width="960" height="720" src="https://www.youtube.com/embed/L_eoziYKLXw?rel=0&showinfo=0" frameborder="0" allowfullscreen></iframe>

## Reading Error Messages

Error messages have 3 parts:

```text
lib/a_name_error.rb:3:in `<main>': undefined local variable or method `hello_world' for main:Object (NameError)
```

1) The location of the error, the "where".

```text
lib/a_name_error.rb:3:in `<main>':
```

* `lib/a_name_error.rb` is the file the error occurred in.
* `3` is the line of code with the error.
* `<main>` is the scope of the error.

2) The description (the "why").

```text
undefined local variable or method `hello_world' for main:Object
```

The interpreter does the best job it can to tell you what it thinks went wrong.

3) The type of error, the "who".

```text
(NameError)
```

This is a [Ruby Error Type](http://www.ruby-doc.org/core-2.2.0/Exception.html).

You've solved games of *Clue* with less information. This is one of the best
parts of programming: debugging and fixing errors. It's like you're a detective
solving a crime. The only bad thing is that more often than not, you're also the
criminal that caused the error in the first place.

Errors are clues, and reading them is the interpreter telling you what to do to
fix the program and move on.

## Four Common Error Types

### Name Errors

`NameError`s are caused when a given name is invalid or undefined. Whenever the
Ruby interpreter encounters a word it doesn't recognize, it assumes that word is
the name of a variable or a method. If that word was never defined as either a
variable or a method, it will result in a `NameError`.

In `lib/a_name_error.rb`, Ruby sees `hello_world` on line 3 is not a word it
recognizes, and it isn't a defined variable or method. One common cause of this
is forgetting to put a `String` in quotations.

It is also possible our code is attempting to use a variable before it is
defined. The following code will work fine:

```rb
greeting = "hello there"
greeting
 # => "hello there"
```

But if we try to call `greeting` before it is defined, we get a `NameError` and
Ruby will error out before we get to the definition.

```rb
greeting # NameError
greeting = "hello there"
```

### Syntax Errors

Syntax errors are pretty self-explanatory: they're the result of incorrect
syntax. Thankfully, they're usually followed by a guess about the location of
the error. For instance, running `ruby lib/a_syntax_error.rb` in its original
state will produce this error:

```text
lib/a_syntax_error.rb:3: syntax error, unexpected end-of-input
```

Something is wrong and it has something to do with line 3. In the file, line
three looks to be an incomplete variable assignment:

```rb
x = 1

x =
```

Removing this code or finishing the assignment will resolve the error. A very
common cause of syntax errors is from forgetting to include `end` on loops and
methods. If you were to paste the following into `lib/a_syntax_error.rb`:

```ruby
2.times do
  puts "hi"
```

You would get an error similar to the last:

```text
syntax error, unexpected end-of-input, expecting keyword_end
```

Ruby sees `do`, and expects there to be an end. Similarly, if Ruby sees `def`,
it will also expect an `end`.

Always read the full details of syntax errors and look for line numbers, which
usually appear at the beginning of the error message.

### Type Errors

When you try and do a mathematical operation on two objects of a different type,
you will receive a TypeError. For example, if you try and add a string to an
integer, Ruby will complain.

```ruby
1 + "1"
```

Will produce the following error:

```text
TypeError: String can't be coerced into Fixnum
```

This is the issue we see in `lib/a_type_error.rb`. The culprit is this line:

```rb
1 + "is the loneliest number"
```

One possible solution to this would be to make sure all values being added
together are the same data type. We can't convert `"is the loneliest number"`
into a number, but we can convert `1` into a string:

```rb
1.to_s + "is the loneliest number"
```

### Division Errors

DivisionErrors are caused when a given number is divided by 0.

```bash
Failures:

  1) Not having any errors and being all green ZeroDivisionError raises a ZeroDivisionError for dividing by zero
     Failure/Error: expect{
       expected no Exception, got #<TypeError: nil can't be coerced into Fixnum> with backtrace:
         # ./lib/a_division_by_zero_error.rb:3:in `/'
         # ./lib/a_division_by_zero_error.rb:3:in `<top (required)>'
         # ./spec/no_ruby_errors_spec.rb:30:in `load'
         # ./spec/no_ruby_errors_spec.rb:30:in `block (4 levels) in <top (required)>'
         # ./spec/no_ruby_errors_spec.rb:29:in `block (3 levels) in <top (required)>'
     # ./spec/no_ruby_errors_spec.rb:29:in `block (3 levels) in <top (required)>'
```

## Additional Errors

While these are not part of the lab files, they are worth reviewing to avoid
future frustration.

### No Method Errors

Let's say you want to calculate the amount that each coworker owes for a lunch
order on Seamless. The order came to $64.25 for you and your three coworkers and
you all agreed to split the total evenly, so you write:

```ruby
total = "64.25"
num_of_people = 4
price_per_person = total / num_of_people
```

And you get:

```text
3:in `<main>': undefined method `/' for "64.25":String (NoMethodError)
```

This error happened because we defined `total` as a string, not as a number,
and Ruby doesn't know how to divide a string by a number. It's like telling Ruby
to divide the word "lemon" by 7—it has no idea what to do. There are two ways
around this error:

- Option One: You could change `total` into a float from the start by removing
  the quotes:

```ruby
total = 64.25
num_of_people = 4
price_per_person = total / num_of_people

# => 16.0625
```

- Option Two: You could change `total` from a string to a float in the division
  step:

```ruby
total = "64.25"
num_of_people = 4
price_per_person = total.to_f / num_of_people

# => 16.0625
```

The `NoMethodError` will often occur when you have a variable set to `nil`
(essentially, nothing or no value), without realizing it. Most attempts to use a
method on something that equals `nil` will result in the `NoMethodError`.  This
is because you are asking an object, `nil` in this case, to do something it does
not know how to do. In other words, you are calling a method that is not defined
for that particular object.

For example: 

```ruby
missing_value = nil

missing_value.length
```

This will produce the following error

```ruby
NoMethodError: undefined method `length' for nil:NilClass
```

### Argument Errors

Argument errors occur when methods are passed either too few or too many
arguments. For instance, let's say you have a simple method, called
`calculate_interest` which takes the value of a loan and finds the amount of
interest that accumulates over the loan's first year given that the annual
interest rate is 5.25%:

```ruby
def calculate_interest(loan_amount)
  loan_amount * 0.0525
end
```

You want to see what happens so you call it below:

```ruby
def calculate_interest(loan_amount)
  loan_amount * 0.0525
end

puts calculate_interest
```
What results is:

```text
1:in `calculate_interest': wrong number of arguments (0 for 1) (ArgumentError)
```

This is because you called on the method `calculate_interest`, which takes one
argument, `loan_amount`, without passing it a value for `loan_amount`. To call
on this method, you must pass it a number. For instance, let's say you're
considering taking out a loan of $9,400 to buy a fancy La Marzocco espresso
machine for your cafe.

```ruby
def calculate_interest(loan_amount)
  loan_amount * 0.0525
end

puts calculate_interest(9400)
```

Now that you passed the method a value for `loan_amount`, it will calculate the
first year's interest, $493.50.

## Conclusion

Many other errors can occur in Ruby, this just covered the most common errors
encountered when beginning to code in Ruby. Ruby errors are pretty descriptive
and they are there to help you out, so always see if the error is offering you a
hint about your code.
