# Asynch Javascript
## Index
- Callback function
- Promises
- Asynch/Await
- Error handling

## Synchronous JS
Each command msut complete before the next command executes.
No two command can be running at the same time.

## Asynchronous JavaScript
Code that can run 'out of order'.
Allows a lengthy operation to start, but finish at a later time wothout blocking other code from running at the meantime.
```
exapmle: **Boiling pasta**
1. grab a pot
2. fill it with water
3. set it to the stove
4. turn the stove on
5. put pasta to the boiling water
*6. asynch step: com to boil is long (doing other stuff meanwhile)*
7. ...continue synchronous steps
```
"asynchronous javascript": it rather has **callback mechanism** in place to run commands in different order to make things more efficient. Javascript is single-threaded, meaning only 1 command can run at a time (for true asynchronus behavour needs multiple threads).

## Callback Functions
Functions are a first-class/citizen Objects in Javascript.
Making a copy:
```
let myFn = function(param){}
let myFnCopy = myFn(arg)
```

## Promises
Promises are opertions that normally take a bit of time will be eventually finish runinning. (I owe you with a response).
Fetch requests returns a promise object
example: **Job interview**
promise: we will get back to you within a week
Stages:
1. **pending**: yet to be completed
2. **fulfilled/resolved**: wa completed as promised
3. **rejected**: was not completed as promised

```
const promise = fetch("url")
promise.then(if promise is fulfilled do this)
```
chaining promises: `promise.then(function).then(function)`
```
fetch("url")
    .then(res => return "Hello")
    .then(whatever => {
        console.log(whatever)
        return "World"
    }) // so far: "Hello"
    .then(another => console.log(another))  /** "Hello"
                                                "World"*/
```

Fetching data:
```
fetch("url")
    .then(res => res.json())
    .then(data => console.log(data))
```

## Asynch/Await
Introduced in ECMASCript 2017 (ES8).
Makes asynch code appear to be synch.
Asynch goes before functions.
Await goes before a method/function that  returns promise.
Keywords: **asynch**, **await**

```
asynch function handleCLick(){
    const res = await fetch('url')
    const data = await res.json()
    cosnole.log(data)
}
```
## Error Handling
A promise is rejected when :
- if an error is thrown inside any of the .then() blocks
```
try{
    fetch("url")
        .then(res => {
            if(!res.ok){                // true / false
            throw Error("Error")        // shortcut to .catch(err){}
        }
        console.log(res.status)         //404 / 200 / 501 etc.
        return res.json
        })
        .then(data => console.log(data))
}catch(err){
    console.error(err)
}finally{
    //code here
}
```

or 

- when programmer manually calls: `Promise.reject()`





