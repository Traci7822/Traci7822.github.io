---
layout: post
title:  "JavaScript Callbacks"
date:   2017-04-09 22:09:41 +0000
---


Callbacks in JavaScript (or 'higher-order functions') aren't a defined part of the JS language but are a convention commonly used to handle asynchronous events. An asynchronous event is when you've executed a piece of code but there's a response time to be accounted for and the rest of your code executes while that response is still happening in the background. An example of an asynchronous event is when you make an API call and while it is fetching the data from that call, code that follows and executes an output to the user will appear and then update when your API request completes and has the necessary data to update with. 

Callbacks don't have to be external requests, they can also be used to execute code in a certain order. If you want to use jQuery to show an element within its parent element a callback would be a great idea. In this scenario, you would use a callback to ensure that the parent element is being created before calling the function that shows the child element. This would look like the following:

```
$(".parentDiv").show(function() {
   #(".childDiv).show()
}
```

The anonymous function (or callback) is initiated once the parent div has completed its action and any code in the same scope as the outer function can run at the same time without waiting for the child div to complete its action. In other words, this convention enables the receiving function to call the callback function after the receiving function finishes a task while allowing for the continued execution of the rest of the code. You want to use a callback when you want the function only to execute following another functions action. 

Callbacks can either be named or anonymous functions. When you pass a callback you're not actually executing the function (as you typically do with `()`), but passing the function definition so that it is available when needed. One thing to consider when using callback functions is that it is good practice to ensure that a callback is actually a function before calling it (ex: `if (typof callback === "function"`). Here are some times when a callback might be a helpful addition to your code:

Callbacks are great to use when dealing with asynchronous events, event listeners and handlers and as a way to keep your code DRY and concise. 

