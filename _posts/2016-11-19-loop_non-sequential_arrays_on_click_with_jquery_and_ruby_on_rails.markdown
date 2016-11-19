---
layout: post
title:  "Loop non-sequential arrays on click with jQuery and Ruby on Rails"
date:   2016-11-19 13:16:51 -0500
---

I recently learned how to scroll through objects with an event listener in jQuery. When I went to implement this idea in my code that consisted of objects with non-sequential id's however, I kept hitting an error page saying it couldn't find the next id in my database. Well obviously, because it didn't exist! But id's after the number it was searching for did exist and I wanted to make sure those were found. Here's what I mean:

```
data = [1, 2, 3, 32, 33, 34, 35, 36, 37]
```

When scrolling through the pages with my 'Next' button, I want to be able to jump from data[2] to data[3] while routing to the correct instance id. Beyond just that, I want the scrolling to be continuous so that when I reach the end of the array, it loops me back to the beginning and cycles through once again. Here's how I made that happen...

First I created a method in my controller that would allow me to access the array of sequence id's:

```
*app/controllers/sequences_controller.rb*

def ids
    @ids = Sequence.all.map { |sequence| sequence.id  }
    render json: @ids
  end
```

Then I had to make a route so that I could access that data:
```
*config/routes.rb*

  get '/sequence_ids', to: 'sequences#ids'
```

In my JavaScript file I already have the id set to the current sequence I have loaded in my browser:

```
*app/assets/javascripts/script.js*
var id = parseInt(window.location.pathname.split("/")[2])
```

In my document ready function I create the event listener to trigger when 'Next Sequence' is clicked:

```
*app/assets/javascripts/script.js*

$(".js-next").on("click", function() {
    scrollSequence();
  });
```

Now I can access the array of sequence id's using jQuery:

```
*app/assets/javascripts/script.js*

function scrollSequence(){
  $.get('/sequence_ids', function(data) {
    var idIndex = data.indexOf(id);
    if (data[idIndex + 1] === undefined) {
      nextId = data[0];
    } else {
      var nextId = data[idIndex + 1];
    }
    window.location.href = '/sequences/' + nextId;
    $(".js-next").attr("data-id", nextId);
  })
};
```

'data' returns the array of sequence id's that currently exist. I then find the index of the id I am currently on, check to make sure the next index exists (and if not it means we're at the end of the array so we loop it back to the first index), then increment 'nextId' to the next index of our array. Lastly we route the browser window to the route of the next id, and set our event listener data-id attribute to the current id. 
