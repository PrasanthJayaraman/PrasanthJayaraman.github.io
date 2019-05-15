---
title:  "Javascript - Map(), Filter() and Reduce() Simplified"
categories: 
  - Javascript
tags:
  - Javascript
comments: true
---

If you’re starting in JavaScript, maybe you already heard of ```.map()```, ```.reduce()```, and ```.filter()```. You may confused what the use of it and how does it work. Once you start working on large applications you will encounter number of scenarios where you need to you these functions.

I will explain in simple way you can understand these and will give you some examples when you can use these. Okay, Let's move on and start the article.. I

## map()

The ```map()``` method creates a new array with the results of calling a provided function on every element in the calling array.

```javascript
    const numbers = [25, 50, 75, 100];
    const halves = numbers.map(x => x / 5);
    // halves is [5, 10, 15, 20]
```

Map is used when you have an array of values and you want to do something for every item in that array.

### Example:

* Think of processing each element with given formula.

```javascript
    const celsius = [-15, -5, 0, 10, 16, 20, 24, 32]
    const fahrenheit = celsius.map(t => t * 1.8 + 32);
    // fahrenheit is [5, 23, 32, 50, 60.8, 68, 75.2, 89.6]
```

## filter()

The ```filter()``` method creates a new array with all elements that pass the test implemented by the provided function.

```javascript
    const words = ["spray", "limit", "elite", "exuberant", "destruction", "present"];
    const longWords = words.filter(word => word.length > 6);
    // longWords is ["exuberant", "destruction", "present"]
```

Filter is simple as ```map()```, It's going over values in ‘words’ array checking which values pass the test, in this case which value has length greater than 6 and then returns a new array with those values.

### Example:

* Filtering users with admin access

```javascript
    var officers = [
        { id: 11, name: 'Adam', age: 23, group: 'editor' },
        { id: 47, name: 'John', age: 28, group: 'admin' },
        { id: 85, name: 'William', age: 34, group: 'editor' },
        { id: 97, name: 'Oliver', age: 28, group: 'editor' }
    ];
    const admins = officers.filter(user => user.group === 'admin' );
    // admins is [{ id: 47, name: 'John', age: 28, group: 'admin' }]
```

## reduce()

The reduce() method applies a function against an accumulator and each element in the array (from left to right) to reduce it to a single value.

```javascript
    const total = [0, 1, 2, 3].reduce((sum, value) => sum + value, 1);
    // total is 7
```

The really cool thing about reduce is that it passes the result of one callback function invocation to the next one allowing us to do things easy.

### Example:

* Find sum of same age people from array

```javascript
    const users = [
        { id: 11, name: 'Adam', age: 23, group: 'editor' },
        { id: 47, name: 'John', age: 28, group: 'admin' },
        { id: 85, name: 'William', age: 34, group: 'editor' },
        { id: 97, name: 'Oliver', age: 28, group: 'editor' }
    ];
    const groupByAge = users.reduce((acc, it) => {
        acc[it.age] = acc[it.age] + 1 || 1;
        return acc;
    }, {});
    // groupByAge is {23: 1, 28: 2, 34: 1}
```
Hope, you are clear with the methods and usage now. If you’ve stumbled upon some interesting ways map, filter or reduce were used be sure to put it in the comments, I’m sure there are heaps of practical ways these 3 can be used!

