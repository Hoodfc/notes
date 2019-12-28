# Big O Notation

## Intro

The Big O Notation is fundamental for understanding Algorithms and Data Structures. It involves some maths, necessary to analyze algorithms.

### Why Big O?

What's the **best** solution to a challenge? How can one compare two different implementations to the same problem in order to decide what's the best to use in our application?

It would be useful to have a systems for classifying different implementations: not only `GOOD` or `BAD`, as things are usually more nuanced than that. The more generalized and math-y, the better.

### Who cares? 

If I find a solution to the problem, isn't it enough? In some occasions, it is. Not every bit of your codebase will have to be always the best, performance-wise. But, **performance matters**, especially in large codebases with lots of users (also, Big-O related questions are quite often used during job interviews).

Such a system is also useful to discuss trade-offs: often there isn't a perfect implementation for all context, but one for each context: a generalized system can easily show that.

## Timing our code

Let's make an example: write a function that calculates the sum of al numbers from 1 up to some number `n`.

```javascript
// 1
function addUpTo(n) {
    let total = 0;
    for (let i=1; i <= n; i++){
        total += i;
    }
    return totale
}
```

```javascript
// 2
function addUpTo(n){
    return n * (n+1) / 2;
}
```

### What does better mean?

- **Faster code**: should we measure the performance time of our code is seconds or milliseconds to declare which one is better? And against which type of tests? Big ones (in the order of billions, for example) or little ones?
- **Less memory-intensive**: how much the memory is used during the running time of our solutions? How many variables are used, how much storage?
- **More readable**: is a more readable code better than a smaller, more confused code?

Efficient code is often not so much readable, which is one of the trade-offs.

### Using timers - the easy way

- `1` **1.2416**... seconds 

- `2` **0.0001**... seconds 

`2` seems the more efficient one, but timers are not so good as a system,

### The problems with time

- different machines will record different times: based on their specifics, two machines can give different results; which usually doesn't mean opposite results, but it makes difficult to say how much a solution is better than the other if the measurements are so volatiles.
- same machine, different results: even in the same machine, two tests can - and very often will - record different times - due to the state of the machine in the moment the code running.
- in very fast algorithms, speed test may not be enough: because of their speed, the nuance of their difference can be lost.

