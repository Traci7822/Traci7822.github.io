---
layout: post
title:  "Bubble Sort: The gateway algorithm"
date:   2017-02-22 13:33:36 -0500
---


Bubble sort is a gateway algorithm. It’s mild, slow, easy to implement, and best used when you aren’t looking for for anything miraculous to happen. It will however leave you wanting more….
From everything I’ve gathered, bubble sort is best used as an introduction to algorithms and only really recommended to check if data is already sorted or if you already know that a list is almost sorted.
The following video comes Cracking The Coding Interview by Gayle Laakmann McDowell via HackerRank and does a great job of breaking down how bubble sort works:

<iframe width="560" height="315" src="https://www.youtube.com/embed/6Gv8vg0kcHc?ecver=1" frameborder="0" allowfullscreen></iframe>

Here is a Ruby example that came out of a bubble sort study group I participated in the other day to solve the accompanying HackerRank challenge ([https://www.hackerrank.com/challenges/ctci-bubble-sort](https://www.hackerrank.com/challenges/ctci-bubble-sort))

```
n = gets.strip.to_i
a = gets.strip
a = a.split(' ').map(&:to_i)

def bubble_sort(a,n)
    count = 0
    loop do
       sorted = false
        (n-1).times do |i|
            if a[i] > a[i+1] 
               a[i], a[i+1] = a[i+1], a[i]
               count += 1
               sorted = true
            end
           end
        break if not sorted
       end
    puts "Array is sorted in #{count} swaps." #where  is the number of swaps that took place.
    puts "First Element: #{a[0]}" #where  is the first element in the sorted array.
    puts "Last Element: #{a[n-1]}" #where is the last element in the sorted array.
end

bubble_sort(a,n)
```

Essentially, what this algorithm does is accept an array of numbers, as well as the count of items in the array and organize them by pushing smaller numbers to the front and leaving bigger numbers behind. The algorithm loops through the array and if the current index is bigger than the next sequential index, it will swap the two values. The count will increase when this occurs so that a record is kept of how many swaps have occurred. This will repeat until there are no more swaps to be made.

Performance of Bubble Sort is ranked very low because of it’s worst/average case of O(n²). This means that its performance is directly proportional to the square of the size of the input data set, meaning it can get really big really fast. In a best case scenario, bubble sort has a performance of O(n) but only if the list is already sorted.

I’ve had a taste of bubble sort and knowing there’s better, more efficient algorithms to be learned, I am now left wanting more…

<iframe width="560" height="315" src="https://www.youtube.com/embed/lyZQPjUT5B4?ecver=1" frameborder="0" allowfullscreen></iframe>
