# Ruby Errors

So far, we've been introduced to some common errors and learned up to read and understand error messages that appear as a consequence of running Ruby programs and test suites. This Readme provides a closer look at some common types of errors. Right now, it may be the case that not all of these error messages make sense or seem meaningful to you. After all, we've only handled a few real programs at this point. 

This is meant to be a resource for you to refer back to as you start building more complex programs and having to debug them. **There are a few important take-aways from this and the previous two lessons:**

* Don't be afraid of broken programs! It's easy to get frustrated when your program breaks. The tendency of a lot of beginners is to jump right back into the code when a test fails or an error comes up as a consequence of running a program, *without reading the error messages*. Error messages are there to guide you. They contain important information about the location and type of problem you are encountering. Embrace them and get comfortable reading them––don't run away from them. 
* Pay attention to the helpful part of error messages. Check out the line number and the type of error that you're receiving. This will point you in the right direction. Let's take a look at an example from an earlier lab. 

```ruby
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

This is some of the output we recieved after running out test suite with the `learn` or `rspec` command on this lab. There is a lot going on there! BUT––we know what to look for now. 

We pay attention to the texit right after the `Failure/Error:`. 

It reads: `expect{ expected no Exception, got #<TypeError: nil can't be coerced into Fixnum>` 

Now we know we are dealing with a TypeError and that *something* in our program is `nil`. But what? Well, let's take a look at the next line: 

` # ./lib/a_division_by_zero_error.rb:3:in `/'`

That is telling us that our error is likely originating on line 3 of this file, `lib/a_division_by_zero_error`. 

The rest of the error message is very likely just noise. 

## Error Types

### Name Errors
NameErrors are caused when a given name is invalid or undefined. Whenever the interpreter Ruby encounters a word it doesn't recognize, it assumes that word is the name of a variable or a method. If that word was never defined as either a variable or a method, it will result in a name error.

### Syntax Errors
Syntax errors are pretty self explanatory and the result of incorrect syntax. Thankfully, they're usually followed by a guess as to the location of the error. For instance:

```ruby
2.times do
  puts "hi"
```

Will result in:
```text
2: syntax error, unexpected end-of-input, expecting keyword_end
```
Here, Ruby is saying that on line 2, there is a missing `end` (every `do` keyword must be followed by some code and then an `end` keyword). Always read the full details of syntax errors and look for line numbers, which usually appear at the beginning of the error message.

### No Method Errors
  Let's say you want to calculate the amount that each coworker owes for a lunch order on Seamless. The order came to $64.25 for you and your three coworkers and you all agreed to split the total evenly so you write:

```ruby
total = "64.25"
num_of_people = 4
price_per_person = total / num_of_people
```

And you get:

```text
3:in `<main>': undefined method `/' for "64.25":String (NoMethodError) 
```

This error happened because you defined total as a string, not as a number, and Ruby doesn't know how to divide a string by a number. It's like telling Ruby to divide the word "lemon" by 7---it has no idea what to do. There are two ways around this error:

* Option One: You could change total into a Float from the start by removing the quotes:

```ruby
total = 64.25
num_of_people = 4
price_per_person = total / num_of_people

# => 16.0625
```

* Option Two: You could change total from a String to a Float in the division step:

```ruby
total = "64.25"
num_of_people = 4
price_per_person = total.to_f / num_of_people

# => 16.0625
```

The `NoMethodError` will often occur when you have a varialbe set to `nil` (essentially, nothing or no value), wihtout realizing it. Attemptint to use any method on something that equals `nil` will result in the `NoMethodError`. 

For example: 

```ruby
missing_value = nil

missing_value.length
```

This will produce the following error

```ruby
NoMethodError: undefined method `reverse' for nil:NilClass
```

### Argument Errors

Argument errors occur when methods are passed too few or too many arguments. For instance, let's say you have a simple method, called `calculate_interest_over_first_year`, which takes the value of a loan and finds the amount of interest that accumulates over the loan's first year given that the annual interest rate is 5.25%:

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

This is because you called on the method, `calculate_interest`, which takes one argument, `loan_amount`, without passing it a loan amount. To call on this method, you must pass it a number. For instance, let's say you're considering taking out a loan of $9,400 to buy a fancy La Marzocco espresso machine for your cafe.

```ruby
def calculate_interest(loan_amount)
  loan_amount * 0.0525
end

puts calculate_interest(9400)
```

Now that you passed the method a value for loan_amount, it will calculate the first year's interest, $493.50.

### TypeErrors

When you try and do a mathematical operation on two objects of a different type.  For example if you try and add a string and an integer, Ruby will complain.

```ruby
1 + "1"
```
Will produce the following error.
```
TypeError: String can't be coerced into Fixnum
```

Another common TypeError is when you try and index into an array with a variable that doesn't evaluate to an integer.

```ruby
index = "hello"
array = [1,2,3]
array[index]
```

Will produce the following error.
```ruby
TypeError: no implicit conversion of String into Integer
```

Ruby is telling you that it is trying to convert the string you passed as the index to the [] method into an integer but it can't.

## Conclusion

There are many other errors that can occur in Ruby, this just covered the most common errors encountered when beginning to code in Ruby. Ruby errors are pretty descriptive and there to help out so always see if the error is offering you a hint about your code.
