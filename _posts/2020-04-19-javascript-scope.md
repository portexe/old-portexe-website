---
layout: post
title: "Scope in JavaScript"
description: How does scope work in JavaScript?
image: 'https://i.imgur.com/bQL0cJ4.jpg'
category: 'code tutorial'
twitter_text: How does scope work in JavaScript?
introduction: How does scope work in JavaScript?
caption: Scope in JavaScript
---

# Scope

## Scope can be thought of as the area of code in which a variable is visible.

#### Please note that the following post is under the assumption that you are not using strict mode. If you aren't sure what that means, don't worry about it for now. Just understand that this is how scope works naturally in JavaScript without strict mode turned on.

#### I also challenge you to run these code examples for yourself as you read through.

Let's look at the following function:

```
function addTwenty(num) {
  var add = 20;
  return num + add;
}
console.log(add);
```

If we ran this code we would end up with a ```ReferenceError```. This is because ```add``` is bound by the scope of the function, and we are trying to access it outside of it's scope. In other words ```add``` is not visible outside of it's scope.

Let's look at another example:

```
console.log(num);
let num = 5;
```

This works as you'd intuitively expect. Since the variable ```num``` has yet to be defined you end up with a ```ReferenceError``` from trying to access a variable that does not yet exist in memory.

However this does not work the same. Declarations made using ```let``` and ```const``` are evaluated at runtime but ```var``` declarations are not, so we end up with ```undefined``` here instead of a ```ReferenceError```:

```
console.log(num);
var num = 5;
```

The fact that the interpreter knows about the existence of these variables before they are declared is due to something known as hoisting.

# Hoisting

## Variable declarations are ***figuratively*** hoisted to the top of it's scope.

What does this mean? It means that due to the way the JavaScript interpreter works, you can imagine that variable declarations are moved to the top of it's scope, and then are ***set*** at the point that you defined them; either at or before runtime depending on if you are using ```var``` or ```let/const```. Sounds kind of confusing, but it's really quite simple. Let's looks at an example.

I want you to notice that this example gives us ```undefined```:

```
console.log(char);
var char = 'a';
```

but this example gives us a ```ReferenceError```:

```
console.log(char);
let char = 'a';
```

Both variables are hoisted, however the difference is that variables defined with ```const``` and ```let``` are evaluated at runtime. Don't believe me that ```let``` declarations are hoisted? Let's prove it really quick for the sake of completion:

```
let char = 'a';
if(true) {
  console.log(char);
  let char = 'b';
}
```

Even though ```char``` is defined in the Global scope, and the local version of ```char``` is declared after our ```console.log()```, we still get a ```ReferenceError```. This means that our local ```let``` declaration is being hoisted and the ```console.log()``` is aware of it, but due to the way the JavaScript interpreter works, we are simply unable to access it at this point. The time in which a ```let``` or ```const``` declaration is not yet available at runtime is known as the ***Temporal Dead Zone***. This can be confusing but it will be helpful to understand in certain debugging scenarios.

Now let's move on.

A variable's declaration is hoisted, but the definition is not. So essentially the following two examples will produce the same outcome:

```
console.log(char);
var char = 'a';
```

and

```
var char;
console.log(char);
char = 'a';
```

One last thing on hoisting for now.

Function declarations are also hoisted to the top of it's current scope, but differently than variables. The function definition is also hoisted with it. 

For example:

```
console.log(squareNumber(4));
function squareNumber(num) {
  return num **2;
}
```

This function will work fine because the interpreter loads the function and it's definition into memory before running through and executing the code.

# Global Scope

## There are different types of scope in JavaScript to be aware of.

Let's start with the Global scope. There are a few different ways to utilize the Global scope in JavaScript.

Let's look at the most simple way:

```
var globalVar = 'I am global!';

function concatString(str) {
  return str + ' ' + globalVar;
}
console.log(concatString('I am hungry!'));
```

We call this Global scope because every other scope within this module has access to ```globalVar```. Notice that the function ``` concatString ``` has access to the ```globalVar```.

Let's take a look at another method of accessing the global scope. Instead of returning the string in concatString, let's set it as a property on the window object. Let's then see if we can access it inside of the logString function:

```
var globalVar = 'I am global!';

function concatString(str) {
  window.concat = str + ' ' + globalVar;
}
function logString() {
  console.log(window.concat);
}

// First we'll call concat string..
concatString('I am hungry!');

// Then let's call logString()
logString();
```

As you can see, variables in the global scope can be accessed by both of these functions.

It is often best practice to avoid setting variables in the Global scope if possible. This is because we often want to avoid other parts of our code accessing and changing values that are used elsewhere in the code. That can lead to some unexpected behavior. Something to keep in mind.

