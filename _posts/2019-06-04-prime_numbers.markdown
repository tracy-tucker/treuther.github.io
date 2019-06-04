---
layout: post
title:      "Prime Numbers"
date:       2019-06-04 23:55:30 +0000
permalink:  prime_numbers
---

![]( http://res.cloudinary.com/tracyr/image/upload/c_scale,w_960/v1559688697/tracy-reuther-numbers_pjszkg.jpg)

### Back to the basics

I'd like to start off by saying that back in my schooling days, I was actually really good at math. But, just like with coding, if you don't use it, you lose it!

So when it came down to tackling the Flatiron School Prime lab, I had to take myself back to the basics and remember what the heck a prime number even is! It seems elementary, but if you don't practice those types of skills on the daily, you honestly forget all about it!


Back to the lab...

The lab states the following:

> You'll be defining a method, prime?(), that takes in an integer argument and returns a boolean of whether or not that integer is a prime number.

Ok, based on the first line of instructions, we can concur that I need to *define* a method, and *take in* an argument of some type of integer. We also know that this will be a ? type of method, based on the part of the instructions that states "boolean" (AKA, If/Elsif) I must confess, I originally used the word "integer" in my syntax and decided to go with "number" instead.  Don't ask, I just changed my mind.

Based on the first part of the instructions we get the following:

```
def prime?(number)

end
```

Alright! Off to a good start!

### What is a prime number?

The lab also left me with a few things to think about:

> What defines an integer as a prime number? Research algorithms for how to determine if a number is prime.
How do you create a range of numbers? How do you turn a range into an array so that it can be iterated over?

This is the part where I had to remind myself of what the heck a prime number even is.

*What is a prime number:*

> A **prime number** is a whole **number** greater than 1 whose only factors are 1 and itself. A factor is a whole **number** that can be divided evenly into another **number**. The first few prime **numbers** are 2, 3, 5, 7, 11, 13, 17, 19, 23 and 29. Numbers that have more than two factors are called composite numbers.

After that, I also did the due diligence of researching into how to use ranges in Ruby.
This is what I learned:

> The first and perhaps the most natural use of ranges is to express a sequence. Sequences have a start point, an end point, and a way to produce successive values in the sequence.

```
(1..5)        #==> 1, 2, 3, 4, 5
```

OK gotcha! So 1 thru 5 can be simply stated as (1..5). This is more efficient, as the coder is not needing to list out every single number. Think of it this way, what if you have 1 thru 100?! (1..100) is a far more efficient syntax.

The next portion asks how to turn a range into an array that can be "iterated" over. Well in Ruby, there is the #to_a method. This would appear to be the more feasible method to incorporate in my code.

### Let's recap:

Ok, let's back track for a moment on this code. So far we have:

```
def prime?(number)

end
```

And, based on the definition of **prime**, we know that prime numbers must be greater than 1. So, let start at 2!

```
def prime?(number)
     start = 2
end
```

Now remember, we don't know what range of numbers are being tested, so we have to write our code in a way that considers this fact. And, we also know that we are dealing with a Boolean so we need to implement some type of if/elsif syntax in there. And, we know that we need to take the range of numbers put those in an array, as stated in the instructions.

```
def prime?(number)
     start = 2
		 if number > 1
		 range = (start..number-1).to_a
end
```

Ok, so far we have a defined **method** that takes in an argument of **number**. We have defined the variable, **start**, to represent the number 2. Now, we need to start our Boolean. If the number is greater than 1, we need to put those numbers in an array, but we cannot include the number itself because a prime number can divide evenly into ANOTHER number, not itself. Lastly, wecreate a new array with the numbers that fit the criteria.

The lab also states that we push the numbers into an array in order to "iterate" over them. Well, that must mean we are going to be doing some iterating! But we have to pick the right method.

### none?

This took me a bit to figure out which method to utilize for this lab. I chose #none enumerator because of the very definition:

> None? - It will return true if NONE of the values are returning what you are asking.

```
def prime?(number)

     start = 2
		 
		 if number > 1
		 
		 range = (start..number-1).to_a
		 range.none? do |num_to_test|
		 number % num_to_test == 0
end
     else false
		 
end
end
```

So what did I do here? I *iterated* over my new **range** of numbers (that does not include any number less than 2 because we are starting at 2. Primes have to be greater than 1).

Once the iteration is complete, I want to take my original **number** and divide it by the newly iterated number from **range** and if the remainder does not **strictly equal to 0**, then the statement would be true, that it actually is a prime number!

And if it's not. Well... then it should return false.

If you run **learn** you'll see that you must take into consideration for negative numbers. But, not worry! That was covered in the initial **start** variable AND **If number > 1**.

I know code is harder to explain than it is  to actually understand.

I hope this makes sense to those whoe seek out a solution to this lab.

> ;lkjafd
