Asynchronous JavaScript
---

## Problem Statement

Browsers have to manage a lot. They're animating a `gif`, they're displaying
text, they're listening for clicks and scrolls, they're running JavaScript
programs, they're streaming a SoundCloud demo in a tab.

To efficiently do all that work, browsers use an _asynchronous_ execution model.
That's a fancy way of saying "they do little bits for lots of things until
they're done."

In this lesson we'll build a foundation of understanding around the
asynchronous execution model of JavaScript.

## Objectives

1. Establish a metaphor for synchronous versus asynchronous work
2. Describe a synchronous code bloc
3. Describe an asynchronous code bloc
4. Identify a synchronous code bloc
5. Identify an asynchronous code bloc

## Establish a Metaphor For Synchronous Versus Asynchronous Work

Let's imagine a chef in a kitchen preparing a big meal. There's only one chef
in this kitchen.  The chef could prepare a boiled goose, then prepare
some potatoes, then prepare some biscuits, then prepare a salad, and then serve
it.

![Boiled Goose](https://media.giphy.com/media/zE0zR9u9oR54Y/giphy.gif)

Our diners would be treated to cold boiled goose, hardened potatoes, cold
biscuits, and a fresh salad! This is not the goal. This meal was prepared in a
_synchronous_ model: one-thing-after-the-other. Whatever happened "blocked" the
rest of things that were waiting for work.

_Instead_, our chef should move between each of these tasks quickly. The chef
should use the _asynchronous_ execution model browsers use. They should stuff
the goose, they should measure the ingredients for the biscuits, they should
peel the potatoes, etc. in a loop, _as fast as possible_ so that all the tasks
_seem_ to be advancing at the same time. If the chef were to adopt this
_asynchronous_ model of work, the diners would be treated to piping-hot
boiled goose, steaming potatoes, soft biscuits, and a fresh salad.

![Stuffing](https://media.giphy.com/media/AGc4wSw993mOQ/giphy.gif)

## Describe a Synchronous Code Bloc

Thus far in JavaScript, we've written _synchronous_ code and the execution
model didn't matter.

```js
let sum = 1 + 1; // Line 1
let lis = document.querySelectorAll("li"); // Line 2
```

In this case, when we hit the definition of `sum`, this work doesn't rely on
any "questionable" or "unknowably long" process. As soon as the work of `Line
1` is done, JavaScript will then go to work finding elements and assigning them
to `lis` in Line 2.

But let's consider a "blocking" operation. Imagine we had a synchronous function
called `synchronousFetch("URL STRING")` that fetches data from the network.

```js
let tooMuchData = synchronousFetch("http://genome.example.com/..."); // Line 1
let lis = document.querySelectorAll("li"); // Line 2
console.log(tooMuchData);
```

That work in Line 1 could take a long time (e.g. slow network), or might fail
(e.g. failed login), or might retrieve a ***huge*** amount of data (e.g. The Human
Genome).

It's possible that the `let lis` in Line 2 _will never execute_! While
JavaScript is executing `synchronousFetch` it will not be able to animate gifs,
you won't be able to open a new tab, it will stop streaming SoundCoud, it will
appear "locked up." Recall our chef metaphor, while they prepare the biscuits
the mashed potatoes grow cold and the boiled goose congeals. Gross.

## Describe an Asynchronous Code Bloc

Asynchronous code in JavaScript looks a lot like event handlers. And if we
think about it, that makes sense. You tell JavaScript:

> Hey, do this thing. _And then_ go do whatever you maintenance you need:
> animate that gif, play some audio from SoundCloud, whatever. But when that
> first thing has an "I'm done" event, go **back** to it and do some work that
> I defined in a function when I called it.

Let's imagine a function called `asynchronousFetch` that takes as arguments:

1. A URL String
2. An arrow function that will have the fetched data passed into it as its
   first argument when the `asynchronousFetch` work is done

```js
asynchronousFetch("http://genome.example.com/...", tonOfGeneticData => sequenceClone(tonOfGeneticData)); // Line 1
let lis = document.querySelectorAll("li"); // Line 2
```

In this case, JavaScript _starts_ the `asynchronousFetch` in Line 1, and then
sets `lis` in Line 2.  Some time later (who knows how long?), the fetch of data
finishes and _that_ data is passed into the "callback" function as
`tonOfGeneticData` &mdash; back on Line 1.

Most asynchronous functions in JavaScript have this quality of "being passed a
callback function." It's a helpful tool for spotting asynchronous code "in the
wild."

Let's try seeing how synchronous versus asynchronous works in real JavaScript
code.

## Identify a Synchronous Code Bloc

As we have experienced in JavaScript, our code execute top-to-bottom,
left-to-right.

```js
function getData(){
  console.log("2. Returning instantly available data.")
  return [{name: "Dobby the House-Elf"}, {name: "Nagini"}]
}

function main(){
  console.log("1. Starting Script")
  const data = getData()
  console.log(`3. Data is currently ${JSON.stringify(data)}`)
  console.log("4. Script Ended")
}

main();
```

We can copy and paste this into a DevTools console to see the result. But it
matches our default model of "how code runs."

## Identify An Asynchronous Code Bloc

The easiest asynchronous wrapper function is `window.setTimeout()`. It takes as
arguments:

* a `Function` (the "callback" function)
* a `Number` representing milliseconds

The `setTimeout()` will wait the number of milliseconds and then execute the
callback.

```js
setTimeout(() => console.log('Hello World!'), 2000)
```

This says "Hello World!"... in 2 seconds. Try it out in the DevTools console!

Since this code is in an _asynchronous_ container, JavaScript can do other work
and _come back_ when the work "on the back-burner is done." If JavaScript
_didn't_ have an asynchronous model, while you waited those 2 seconds, no gifs
would animate, streaming audio might stall. Asynchronous execution makes
browsers the exceedingly useful tools they are.

What do you think the output will be here?

```js
setTimeout(() => console.log('Hello World!'), 2000)
console.log("No, me first")
```

Sure enough:

```text
No, me first
Hello World!
```

JavaScript is so committed to trying to squeeze in work when something else
when it gets a chance that this has the exact same output!

```js
setTimeout(() => console.log('Hello World!'), 0) // 0 Milliseconds!!
console.log("No, me first")
```

The browser has < 0 milliseconds (i.e. nanoseconds) to see if it can find any
work to do!

## Conclusion

JavaScript in the browser has an asynchronous execution model. This fact has
very little impact when you're writing simple code, but when you start
doing work that might block the browser you'll need to leverage asynchronous
functions. Be advised, these functions can be surprising and nearly every
JavaScript developer is sooner or later bitten by a bug where they forgot to
reckon with asynchrony.

While working asynchronously can be a bit of a headache for developers, it
allows JavaScript to do other work whenever it has opportunity. Important
methods which require us to think asynchronously are `setTimeout()`, `fetch()`,
among others.
