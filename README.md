JavaScript XHR
---

## Objectives

1. Explain how JavaScript fetches data from remote resources
2. Explain how XHR helps us write dynamic programs
3. Practice initializing an XHR request
4. Practice handling an XHR response

## Introduction

As we have noticed with JavaScript, our code runs one step at a time.  One line of code will execute, and when it is complete the next line of code executes.  Let's throw a wrench into that right now with the introduction of a JavaScript function called `setTimeout`.  Let's first explain the function by way of example.

```js
  setTimeout(function(){
      alert('Hello world!')
  }, 1000)
```  

If you copy and paste the code into a browser, you may get a sense of how it works: the alerting of "Hello World!" occurs a second after the code is run.  This is because `setTimeout` takes two arguments.  The first argument is a function to eventually be run, and the second argument is an integer indicating how long to wait (in milliseconds) before running the function.  So here, our code says to wait 1000 milliseconds and then run the function that alert's with the text "Hello World".  Ok, nothing is that new.  Let's see some more code.    

```js
  setTimeout(function(){
      alert('Hello world!')
  }, 1000)

  console.log('Hola')
```  

Ok, so our question with the above code is the following: which will occur first, the alerting or the logging.  That is, will the `console.log` statement wait for the `setTimeout` function to complete before running?  If you run this code in the browser, you will see that the `console.log` occurs first - JavaScript did not wait.  

This feature, that the `setTimeout` function runs, and while it is executing the next line runs, is called *asynchronous code*.  By that we simply mean that while a previous part of the code is executing, JavaScript continues executing the next piece of code.  We showed one function that takes some time, with `setTimeout`, but stay tuned for us to run into this feature of JavaScript again in the future.  

Note that not all languages behave this way.  For example, let's type "irb" in your terminal to open the interactive ruby shell and run the following ruby code.

```ruby
  def wait_then_print
    sleep 3
    puts 'hello world'
  end

  wait_then_print; puts 'Hola'
  # hello world
  # Hola
```

As you see from the code, when we use the `sleep` function in ruby.  Ruby properly waits the correct number of seconds before completing the function.  However, ruby does not run any other code while it is waiting.  

If we think about it, it makes a bit of sense that JavaScript's default behavior is to keep running the next lines of code while an earlier chunk waits to complete.  After all, JavaScript is the language of the browser right?  So can you imagine if while JavaScript finished completing an early part of code, the browser just politely paused and waited, no matter how much you clicked or hovered over items?  It doesn't feel right.  So it's a good thing that JavaScript just keeps going right?  Yes, but sometimes we want JavaScript to wait before continuing to execute it's code.  We'll discuss that in the next lesson.

## Summary

JavaScript has asynchronous features.  By this, we mean that while JavaScript is waiting for one piece of code to run, it will continue on with the next line of code.  Other languages like ruby, politely wait for a procedure to execute before moving onto the next function.  And because we sometimes would like JavaScript to wait as well, we need to keep on trucking.  

## Resources

<p class='util--hide'>View <a href='https://learn.co/lessons/javascript-xhr'>XHR</a> on Learn.co and start learning to code for free.</p>
