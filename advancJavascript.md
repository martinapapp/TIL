# Advanced JavaScript

## Index

- [Foundation](#foundation)
    - [Runtime Fundamentals](#runtime-fundamentals)
    - [Operators](#operators)
    - [Timers](#timers)
    - [Modules & Built-ins](#modules--built-ins)
- [Methods and Loops](#methods-and-loops)
- [Function Exressions and Parameters](#function-expressions-and-parameters)

## Foundation

### Runtime Fundamentals 

JS is a single-threaded, non-blocking language.

Get help from: WebAPI, Task Queue, Event Loop, Call Stack.

JS runtime environment is V8 Engine.

**Heap**: handles memory allocation

**V8 Engine**: V8 Engine is Google's open-source JavaScript engine, written in C++. It's what powers both Chrome and Node.js.
When it runs your JS code, it does two key things:

1. Parses & compiles — converts JS into machine code directly (no intermediate bytecode step), using a technique called JIT (Just-In-Time) compilation. This is why JS is fast despite being interpreted.
Manages memory — handles the Heap (allocation) and runs the Garbage Collector, which automatically frees memory that's no longer referenced.
2. It also contains the Call Stack and works alongside the browser's WebAPI and queues — it handles only the JS execution part, not the async infrastructure.

**Call Stack**: it can execute one piece of code at a time in its single thread from top to bottom,
but it can handle multitasks like setTimeout, setInterval, fetching data, promises

**WebAPI**: 
- provided by the browser 
- not part of JS
- have functionality for:
    - DOM manipulation
    - data requests
    - timers (can delay if in Task Queue there are more tasks or there's a heavier function on Call Stack)

**Microtask Queue**:
- holds callbacks from Promises (`.then`, `.catch`, `.finally`) and `queueMicrotask()`
- has **higher priority** than the Task Queue
- fully drained before any Task Queue item is allowed to run

**Task Queue** (also called Macro Task Queue):
- holds callbacks from `setTimeout`, `setInterval`, and DOM events
- only processed when the Call Stack is empty **and** the Microtask Queue is fully drained

**Event Loop**:
- constantly monitors the Call Stack and the queues
- if the Call Stack is empty, it first drains the entire Microtask Queue, then picks the next task from the Task Queue and pushes it onto the Call Stack
- this cycle repeats indefinitely



*How JavaScript works*
```javascript
console.log("first")                                // road: 1. call stack 2. executes
setTimeout(() => console.log("second"), 5000)       // road: 1. call stack 2. WebAPI (timer) 3. Task Queue (waits here until call stack is empty) 4. call stack 5. executes
console.log("third")                                // road: 1. call stack 2. executes

/**CONSOLE
> first
> third
> second
*/
```

**Hoisting**:

- order doesn't matter with function declaration or function invocation
- order does matter between variables and their use cases (JS knows it exists or is not defined. if it exists and has an accessing/functioning problem it is called: Temporal Dead Zone)
- variables and function declarations are moved to the top of their containing scope during the compilation phase, before code execution

```javascript
// var is hoisted and initialized to undefined — no error
console.log(a)  // undefined
var a = 10

// let/const are hoisted but NOT initialized — Temporal Dead Zone
console.log(b)  // ReferenceError: Cannot access 'b' before initialization
let b = 10

// function declarations are fully hoisted — works fine
greet()         // "Hello"
function greet() {
    console.log("Hello")
}

// function expressions are NOT hoisted
sayBye()        // TypeError: sayBye is not a function
var sayBye = function() {
    console.log("Bye")
}
```

### Operators

**Ternary operator:**

```javascript
const route = isLoggedIn ? "/dashboard" : isAdmin ? "/admin" : "/login"
```

**Switch statement**:

```javascript
switch(role) {
    case "admin":
        access = true
        break
    default:
        return "access denied"
}
```

**Object destructuring:**

```javascript
const obj = { k1: v1, k2: v2 }

const { k1, k2 } = obj
```

### Timers

**setTimeout:**

```javascript
setTimeout(function, delay)
setTimeout(function, delay, parameters)
clearTimeout(setTimeout function declaration)
```

**setInterval:**

```javascript
setInterval(function, delay)
clearInterval(setInterval function)
```

### Modules and Built-ins

**Import-Export:**

a) named version

- alias: `import {oldName as newName} from 'path'`
- import multiple stuff: `import {oldName as newName, anObj} from "path"`
- multiple imports can break to new lines
- export multiple stuff: `export {fnName, anObj}`

b) default version

- export: `export default fnName(){}`
- import, in this case, doesn't need `{}`: `import fnName from "path"`
- can change import name, but don't
- best practice is to give the same name for the *file && export && import*

**Date constructor:**

- built-in object
- date snapshot: `new Date()`
- human readable: `(dateSnapshot).toDateString()`
- common properties: `.getFullYear()` // 2025, `.getYear()` // 25
- common package: **luxon**

**Error constructor:**

- built-in object
- example: `new Error('custom error msg')`
- difference with `throw new Error('custom error msg')`: it will stop any code after when executes

**Custom constructors:**

 |`String()` |`Number()` |`Array()` |`Boolean()` |`Object()` |

- most rarely used is `Number()` constructor

**Pre-increment:**

```javascript
let count = 0

function incrementCount() {
    count++
}

// or

function incrementCount() {
    ++count                             // valid too (also with --count)
}
```

**Numeric Separator:**

```javascript
const numSeparated = 9_007_012_535_000
console.log(typeof numSeparated)

/**CONSOLE
> number
*/
```
- separator: **_**
- maximum safe number in JS: 9_007_199_254_740_991
- for higher numbers, use `BigInt()` -> `9_007_199_254_740_991_345n`
- can't do math operations on it easily
- common use cases:
    - precise handling of large integers
    - cryptography
    - interacting with a DB that uses large integer "id"

## Methods and Loops

**For ... of || For ... in**

| | `for...of` | `for...in` |
|---|---|---|
| **General** | iterates over values | iterates over keys |
| **Best for** | arrays, strings, iterables | objects |
| **Difference** | can't access index by default | can't access values directly |
| **Examples**| let(const letter of string) | let(let property of character) |

- Array is a type of object
- both iterates over obj daata structures

**.forEach()**

- C-style for loop == For ... of == .forEach
- cleaner and easier way of iterating
- access to an inner array by nesting

```Javascript
//regular
characters.forEach( character => console.log(character))

//nested and accessing index
characteres.forEach( character => character.powers.forEach( (power, index) => console.log(index, power) ))
```

**.includes()**

- method for check if an array holds a given *value*
- returns : true || false
- common to use with **.filter()**

**.map()**

- returns : **new array**
- iterating over each element in arrays
- common to store it in variable
- can access index too by parameter

```Javascript
const miles = [100, 345, 856, 203]
const milesToKm = 1.6

const km = miles.map( mile => console.log(mile * milesToKm))

/**CONSOLE
 * > [160, 555, 1377, 326]
*/
```

**.join()**

- concats elements of array into a string
- choose how to separate: `.join("") || .join(".") || .join("-") || .join(" ")`
- returns : new string

**.filter()**

- getting only the elements we want
- test each element
- returns: new array
- common to use with objects and **.includes()**

```Javascript
const ages = [8, 12, 34, 45]
const adult = []

const adults = ages.filter(age => {
    if(age >= 18) adult.push(age)
})

console.log(adult)

/**CONSOLE
 * > [34, 45]
*/
```

**.reduce()**

- give me just one thing
- add **0** as second parameter to overwrite first total: `.reduce(function, 0)`
- add empty arr `[]` as second parameter to flatten nested array: `.reduce(function, [])`

```Javascript
//regular
const rain=[10, 22, 0, 122]
const totalRain = rain.reduce((toal, current) => total + current)

//accessing value in obj and overwrite first total to be 0
const gardes = [{id: "6780BA", subject: "Physics", grade: "5"}, ...other students]
const totalGrades = grades.reduce((total, current) => { total + current.grade}, 0)

//flattening nested array
const data = [{}, {}, {}, ...]
function getHosts(data){
    return data.reduce((toal, current) => {
        return [...total, ...current.hosts]
    }, [])
}
```

**for loop + break + continue**

- chronology order matters

```Javascript
 // code here
```

**Other methods:**

1. `.every()` : `arr.every(param => condition)`, returns boolean
2. `.some()` : `arr.some(param => condition)`, returns boolean
3. `.find()` : `arr.find(param => condition)`, returns value of the first match
4. `.findIndex()` : `arr.findIndex(param => condition)`, returns index of the first match || -1
5. `.indexOf()` : `arr.findIndex(item)`, returns index
6. `.at()` : `arr.at(index)`, returns value
7. `.replace()` : `string.replace(pattern, replacement/function)`, replace first instance
8. `.replaceAll()`: `string.replaceAll(pattern, replacement/function)`, replace every instance

```Javascript
let string = "I love⁠ you with all my heart⁠!"
// replace


//replaceAll
string.replaceAll(pattern(love|heart), (match) => {
    console.log(match) //represents each of the matches to regex and returns string with replacement
    return `${match}❤︎⁠`
})

/**CONSOLE
 * > I ❤︎⁠ you with all my ❤︎⁠!
*/
```

**Regex flags**

- g = global
- i= case sensitive

```Javascript
const text = "... Wifi"
const regex = /wifi/            // false
const regexFlags = /wifi/gi     // true

const isMatch = regex.test(text)

```

## Function Exressions and Parameters

### Function Exressions vs. Function Declarations vs. Arrow Functions

Function Declarations:
- Hoisted, can be called anywhere
- `function functionName(){}`

Function expressions:
- NOT hoisted
- cleaner and easier to read (arguably)
- depends on the dev team prefernces
- `const functionName = function(){}`

Arrow Functions:
- ultra concise function
- mostly with function expressions for methods (`const cards = arr.map( param => {})`)
- no param : `() => {//complex logic}` or `() => //simple logic`
- 1 param : `(param) => {}` or `param => {}`
- 2 or param : `(param1, param1) => {}`


### Function Parameters

- defalut parameters : `function createProfile(name = "Anonymous"){}`
- rest parameter : `function users(permissionLevel, ...names){}` - returns array, always the last param

















