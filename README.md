---
tags: readme
language: ruby
resources: 0
track: web development
topic: ruby
unit: debugging
lesson: errors
---

# Ruby Errors

## Name Errors
NameErrors are caused when a given name is invalid or undefined. Whenever the Ruby encounters a word it doesn't recognize, it assumes that word is the name of a variable or a method. If that word was never defined as either a variable or a method, it will result in a name error.

## Syntax Errors
Syntax errors are pretty self explanatory and the result of incorrect syntax. Thankfully, they're usually followed by a guess as to the location of the error. For instance:
```ruby
2.times do
  puts "hi"
```

Will result in:
```text
2: syntax error, unexpected end-of-input, expecting keyword_end
```
Here, Ruby is saying that on line 2, there is a missing `end`. Always read the full details of syntax errors and look for line numbers, which usually appear at the beginning of the error message.

## No Method Errors
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

A common error is when you have a nil for a variable when you don't expect it.  This commonly happens when you iterate and one of the elements you pass to a block evaluates to nil and you're calling some method on each element.

```ruby
person = {:blake => "cool"}
person2 = {:steven => "not as cool"}
[person, person2].each do |item|
     item[:blake].reverse
end
```

This will produce the following error

```ruby
NoMethodError: undefined method `reverse' for nil:NilClass
```

## Argument Errors

Argument errors occur when methods are passed too few or too many arguments. For instance, let's say you have a simple method, called `calculate_interest_over_first_year`, which takes the value of a loan and find the amount of interest that accumumlates over the loan's first year given that the annual interest rate is 5.25%:

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

## Conclusion

There are many other errors that can occur in Ruby, this just covered the most common errors encountered when beginning to code in Ruby. Ruby errors are pretty descriptive and there to help out so always see if the error is offering you a hint about your code.
