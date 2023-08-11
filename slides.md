# FP in Python

Codestar ✕ Pythoneers R&D Hackathon

11 aug 2023

---

- Is Python multi paradigm? (probably)
- Are functions a first class citizen? (lambda expressions?)
- How do some of the functional operators work?
- Where to get immutable data structures?
- Do we want to use Coconuts? [coconut-lang.org](coconut-lang.org)

Note: 

Why "Functional Operators" and not FP? Because FO is 2 hours and FP is a CS degree...

Goals for today, find out:

---

## Pure functions

- deterministic
- no side effects

> Pure functions help prevent runtime errors and are transparent and easily testable.

Note: But first, why do we want FP?

- deterministic ("predictable", argument -> return value)
- no side effects (side effects e.g. DOM, console, mutate global)

---

not pure (non deterministic)

```python
def add_random(x):
    return x + random.random()
```

Note:

```python
import random

def add_random(x):
    return x + random.random()

print(add_random(3))
```

---

not pure (deterministic, but side effect)


```python
thisdict = {
  "z": 0
}

def add(x, y):
    sum = x + y;
    thisdict["z"] = sum
    return sum
```

Note:

```python
import random

thisdict = {
  "z": 0
}

def add(x, y):
    sum = x + y;
    thisdict["z"] = sum
    return sum

print(add(3, 2), thisdict["z"])
```

---

pure

```python
def add(x, y):
    return x + y;
```

---

Compare math:

```
f(x) = 2 * x ^2 + 3

def f(x):
    return 2 * pow(x, 2) + 3;
```

Note:

Pure functions can improve readability: they should be concise, high cohesion. 

Mathematically Provable (Kyle Simpson): if a lot of your code is mathematically provable, you have less code to worry about.

---



## Immutability

```python
heroes = ["Dent", "Prefect"]

def add_hero(h):
    heroes.append(h)

add_hero("Marvin")
```

Note:

help prevent side effects (mutate global)

take:

<!-- this is `const`, but can be mutated. The value of `heroes` has now changed on the global scope, not just in `addHero`!

```typescript
const heroes: readonly string[] = ["Dent", "Prefect"];

const addHero = (h: string) => {
  heroes.push(h); // not allowed
}

addHero("Marvin");
``` -->

---

functional style:

```python
heroes = ["Dent", "Prefect"]

def add_hero(hs, h):
    new_hs = hs.copy()
    new_hs.append(h)
    return new_hs

new_heroes = add_hero(heroes, "Marvin")

print(heroes, new_heroes)
```

---

The End

---

---

TODO rewrite examples to python


Spread operator: `[...hs, h]` does `[hs[0], hs[1], h];`
Copy vs mutate.

Similar for objects:

```typescript
interface Hero {
  readonly name: string;
}

const dent: Hero = {
  name: "Dent"
}

const updateName = (h: Hero, n: string) => {
  // Can't mutate:
  // h.name = n;
  // return h;
  return {
    ...h,
    name: n
  }
}

const newDent = updateName(dent, "Arthur");
```

ImmutableJS

More about immutability: [https://github.com/immutable-js/immutable-js/blob/master/README.md#the-case-for-immutability](https://github.com/immutable-js/immutable-js/blob/master/README.md#the-case-for-immutability)

## Iteration

iterable objects: 
    * string: `const x = "123";` 
    * array:  `const x = [1, 2, 3];`
    * array-like objects: e.g. NodeList `const x = document.querySelectorAll('div');`
    * Map, Set: `const x = new Set([1,2,3]);`

`for` [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)

```typescript
const str = "123";
for (let i = 0; i < str.length; i++) {
    console.log(str[i]);
}
```

Same for array, but not for Set, because it has no "get by index".

You can convert to array: `Array.from(new Set([1,2,3]))`

## Get the square of each item in an array

What is wrong with?

```typescript
const sourceArr = [1,2,3];
const squareArr = [];
for( const item of sourceArr ) {
    squareArr.push(item * item);
}
console.log(squareArr);
```

Mutability of squareArr.

Using .forEach would not solve this. Exercise: Implement in forEach to show why.

## map operator (the first functional operator)

Imperative (how) vs Declarative (what), readability because the function will describe what will be the result instead
of how it is obtained.

```typescript
const sourceArr = [1,2,3];

const mapSquare = (arr) => {
    const squareArr = [];
    for( const item of arr ) {
        squareArr.push(item * item);
    }
    return squareArr;
}

console.log(mapSquare(sourceArr));
```

More abstract

```typescript
const sourceArr = [1,2,3];

const map = (arr, fn) => {
    const resultArr = [];
    for( const item of sourceArr ) {
        resultArr.push(fn(item));
    }
    return resultArr;
}

const square = item => item * item;

console.log(map(sourceArr, square));
```

Note:

* returns a value, unlike the imperative for loop
* `square` is a HOF!
* `square` is pure and trivial to unit test

Underscore, LoDash, ES6: `.map`

```typescript
const sourceArr = [1,2,3];
const square = item => item * item;
console.log(sourceArr.map(x => square(x)));
```

Can be written in point free (ish) / equational reasoning style as:

```typescript
console.log(sourceArr.map(square));
```

typed:

```typescript
const sourceArr: number[] = [1, 2, 3];
const square = (item: number): number => item * item;
console.log(sourceArr.map<number>(square));
```
