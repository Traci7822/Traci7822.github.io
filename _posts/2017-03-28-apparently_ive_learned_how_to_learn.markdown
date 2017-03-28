---
layout: post
title:  "Apparently I've learned how to learn"
date:   2017-03-28 21:19:26 +0000
---


I found a job that would be a perfect fit, if only I knew [Clojure](https://clojure.org/) . Since the job required no experience with the language (although definitely preferred it) I decided to apply anyway. I had my initial interview and then they sent over a Clojure code challenge. Looking over it the challenge seemed pretty easy and I knew I could write it in Ruby or JavaScript so that's where I began. 

Here is the challenge I was given:

> Double Booked
> 
> When maintaining a calendar of events, it is important to know if an event overlaps with another event.
> 
> Given a sequence of events, each having a start and end time, write a program that will return the sequence of all pairs of overlapping events.


I wrote out the code in JavaScript so that I knew how I wanted the code to work and here is what I came up with: 

```
function overlap(sequence) {
  var overlap_pairs = [];
  for (var i = 0; i < sequence.length; i++) {
    for (var j = 0; j < sequence.length; j++) {
      if ((sequence[i].start < sequence[j].end) && (sequence[j].start < sequence[i].end)) {
        overlap_pairs.push([sequence[i], sequence[j]]);
        return overlap_pairs;
      }
			
	    }
  }
}

var Timestamps = [
    { start: 1360454400000, end: 1360540800000 },
    { start: 1360454400000, end: 1360574700000 },
    { start: 1372668447947, end: 1372668447947 }]

overlap(Timestamps);
```
	
Then began the tumultuous journey of trying to figure out what Clojure is and how to write *anything* let alone an operational function in it. Without a ton of documentation available online, there was a lot of trial and error. Working in https://repl.it/languages/clojure on and off for several days, this is what I finally came up with: 

```
(defn overlap
  [events]
  (def overlap_matches (vector))
  (dotimes [i (count events)]
    (def first_event (get events i))
    (if
      (and
        (some? (get events (+ i 1)))
        (def second_event (get events (+ i 1)))
        )
      (do   
        (if
          (and
            (< (get first_event :start) (get second_event :end))
            (< (get second_event :start) (get first_event :end))
          )
          (do
            (def overlap_matches (conj overlap_matches (array-map (str "Overlap #" (+ i 1)) [:first_event first_event :second_event second_event])))
            true)
          (do () :error)
        )
      )
      (do () :error) 
    )
  )
(println (str "The following event pairs overlap: " overlap_matches))
)


(def event1 {:start 1360454400000 :end 1360540800000})
(def event2 {:start 1360454400001 :end 1360540800001})
(def event3 {:start 1360454400003 :end 1360540800003})
(def event4 {:start 1360454300001 :end 1360454300003})

(def events [event1 event2 event3 event4])
    
(overlap events)
```
	
	
The code seems to do what I wanted it to and was worth all the frustration to get there. I guess it's safe to say at this point that given the time and the motivation, I can learn anything at this point. 
