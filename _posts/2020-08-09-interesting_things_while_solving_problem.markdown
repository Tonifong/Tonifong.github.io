---
layout: post
title:      "Interesting things while solving problem"
date:       2020-08-10 03:29:27 +0000
permalink:  interesting_things_while_solving_problem
---


#####  I compiled 7 examples that I think you’ll enjoy. I’m also going to give you some commentary on each example to help you get the most out of this.Sum Of Two Numbers In this example we want to find out if given an array of unique numbers, there is a combination of two numbers which adds up to a target number.

#### Code:

##### def sum_eq_n?(arr, n)
  ##### return true if arr.empty? && n == 0
  ##### arr.product(arr).reject { |a,b| a == b }.any? { |a,b| a + b == n }
###### end
##### This is interesting because I’m using the product method here.

###### When you use this method is like having a loop inside a loop that combines all values in array A with all values in array B.

##### Counting, Mapping & Finding
##### Let’s say that you want to find the missing number in an arithmetic sequence, like (2,4,6,10). We can use a strategy where we calculate the difference between the numbers.                                                                                                                                                                                                                                                                              [2, 2, 4]. Our goal here is to find out what the sequence is. Is it increasing or decreasing?By how much?This code reveals the sequence:                                                                                                                                                                                                                                                                                                                                                       differences = [2, 2, 4] differences.max_by { |n| differences.count(n) }
# 2
# This is the increase between numbers in the sequence
##### Once we know the sequence we can compare all the numbers to find the missing one.

##### Here’s the code:

##### def find_missing(sequence)
  ##### consecutive     = sequence.each_cons(2)
  ##### differences     = consecutive.map { |a,b| b - a }
  ##### sequence        = differences.max_by { |n| differences.count(n) }
  ##### missing_between = consecutive.find { |a,b| (b - a) != sequence }
  ##### missing_between.first + sequence
##### end
##### find_missing([2,4,6,10])
# 8
##### Count how many Ruby methods we’re using here to do the hard work for us 🙂

##### Regular Expression Example
##### If you are working with strings & you want to find patterns then regular expressions are your friend.

##### They can be a bit tricky to get right, but practice makes mastery!

##### Now:

##### Let’s say that we want to find out if a given string follows a pattern of VOWEL to NON-VOWEL characters.

##### Like this:

##### "ateciyu"
##### Then we can use a regular expression, along with the match? method to figure this out.

##### Here’s the code example:

##### def alternating_characters?(s)
 ##### type = [/[aeiou]/, /[^aeiou]/].cycle
 #####  if s.start_with?(/[^aeiou]/)
    ##### type.next
 #####  end
 #####  s.chars.all? { |ch| ch.match?(type.next) }
##### end
##### alternating_characters?("ateciyu")
# true
##### Notice a few things:

##### We use the cycle method so we can keep switching between the VOWEL regex & the NON-VOWEL regex.
##### We convert the string into an array of characters with chars so that we can use the all? method.
##### Recursion & Stack Example
##### Recursion is when a method calls itself multiple times as a way to make progress towards a solution.

##### Many interesting problems can be solved with recursion.

##### But because recursion has its limits, you can use a stack data structure instead.

##### Now:

##### Let’s look at an example where we want to find out the “Power Set” of a given array. The Power Set is a set of all the ##### subsets that can be created from the array.

##### Here’s an example with recursion:

##### def get_numbers(list, index = 0, taken = [])
  ##### return [taken] if index == list.size
  ##### get_numbers(list, index+1, taken) +
  ##### get_numbers(list, index+1, taken + [list[index]])
##### end
##### get_numbers([1,2,3])
##### Here’s the same problem solved using a stack:

##### def get_numbers_stack(list)
  ##### stack  = [[0, []]]
  ##### output = []
  ##### until stack.empty?
    ##### index, taken = stack.pop
    ##### next output << taken if index == list.size
    ##### stack.unshift [index + 1, taken]
    ##### stack.unshift [index + 1, taken + [list[index]]]
  ##### end
  ##### output
##### end
##### The idea here is that on each pass of the algorithm we are either taking a number or not taking a number.

##### We branch out & try both outcomes so we can produce all the possible combinations.

##### Imagine a tree where each leaf is one of the solutions.

##### A few things to notice:

##### The recursion solution is shorter
##### The actual "making progress" part of the algorithm (index + 1) is almost the same
##### The stack we're using is just an array because there isn't a Stack class in Ruby
##### Method Chaining Example
##### This is my favorite example because it shows how powerful Ruby is.

##### Combining methods allows you to take the output produced by one method & pass it into another.

##### ##### just like a factory production line!

##### You start with some raw materials (input), then through the process of calling these methods, you slowly transform the ##### raw materials into the desired result.

##### Here's an example:

##### def longest_repetition(string)
 #####  max = string
          ##### .chars
          ##### .chunk(&:itself)
          ##### .map(&:last)
          ##### .max_by(&:size)
#####  ##### max ? [max[0], max.size] : ["", 0]
##### end
##### longest_repetition("aaabb")
# ["a", 3]
##### Given a string, this code will find the longest repeated character.

##### Note:

##### How this code is formatted to maximize readability
##### Use of the Symbol#to_proc pattern (&:size)
##### Btw, don't confuse this with the "Law of Demeter".

##### That "law" is about reaching out into the internals of another object.

##### Here we are only transforming objects.

##### With Index Example
##### Would you like to have the current index while iterating over a collection of items?

##### You can use the with_index method.

##### Here's an example:

##### def reverse_alternate(string)
  ##### string.gsub(/[^\s]+/).with_index { |w, idx| idx.even? ? w : w.reverse }
##### end
##### reverse_alternate("Apples Are Good")
##### # "Apples erA Good"

#### Notice:

##### We combine with_index & even? to find if we have to reverse the current word
##### Gsub without a block returns an Enumerator object, which allows you to chain it with other methods
##### Each With Object Example
##### Another interesting method is each_with_object, and its friend with_object.

##### You can use these two methods when you need an object to hold the results.

##### Here's an example:

##### def clean_string(str)
 #####  str
   #####  .chars
   #####  .each_with_object([]) { |ch, obj| ch == "#" ? obj.pop : obj << ch }
   #####  .join
##### end
##### clean_string("aaa#b")
##### In this example, we want to delete the last character when we find a # symbol.

##### Notice:

##### each_with_object takes an argument, which is the object we want to start with. This argument becomes the 2nd block parameter.
##### We are converting the string into an array of characters (char) & then back to a string when we are done (join).
##### We are using a ternary operator to decide what to do, this makes the code shorter.
##### Summary
##### In this article, you have seen some interesting code examples with my commentary & explanations to help you write ##### better Ruby code.
