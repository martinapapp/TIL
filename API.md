# APIs
## Index
- servers and clients
- request response cycles
- APIs
- JSON
- fetch syntax
- HTTP request

---

## Server and Clients
- Server: Basically a computer. accepts requests from clients asking for something then responds to the client with that thing.
- Clients: Any devices that connect to the internet to get data from somewhere requests something. (a server can be a client too.)

## Request / Response cycle
- Request: When  a device asks for a “resource”. Requires internet connection(wired, wifi, phone data), if the server and the client are on different devices.
- Response: Reply to the request. Could contain the resource asked for the client (200 OK). Could contain a response saying the client isn’t authorized to receive the resource (403 Forbidden). Internal server error (500 Internal Server Error).

## APIs = Application Programming Interface
Any tool that helps connect one program with another’s program.
```
exapmle: **Restaurant analogy**
    my table = my program
    kitchen = another’s program
    menu = docs
    waiter/waitress = messenger deliveres req and res
getting data from a server:
    Server hosts “an API” - exposed “endpoints” we can access for getting data from server.
    The server doesn’t give access to everything, just the thing they want us to have. (security issues)
pre-written code that does cool stuff:
    DOM API (.getElementById), Array methods, localStorage, WebGL
    3rd party packages
    We don’t need to know how these work but can use them. (can check out the how in opensource)
```
## JSON = JavaScript Object Notation
language of the data of the web
mostly have an object `{}` or objects in an array `[{}, {}, ..]`
    * ` {“key”:”value”} `
    * key always have “”
    * `value = ”string”` || boolean || number || [array of items]
in devtools: 
    * Inspect/Network/XHR/click one item/Preview/open arrow that shows a numeratic key and value
    * XHR: AJAX request which gets a json as a response
fetch syntax and Asynchronous JS
```
fetch(“url”)
    .then(response => response.json())
    .then(data => console.log(data))
```

response.json: changes the body into JS
console.log(data): here comes the function
runs after everything else has run.
Asynchronous: happening out of order/time.
they don’t block other codes

---

## HTTP Request
Hypertext Transfer Protocol.

Protocol: an agreed-upon, standard way of doing something.

HTTP is a protocol for determining how Hypertext should be transfered over the internet.

Compontents of a request:

    1. Path (url)
    2. Method (default GET, POST, DELETE, Patch, OPTIONS, etc.)
    3. Body
    4. Headers

### Path

Address where the desired resource 'lives'. When clicking on a url is like making a get request and is it is ok, it opens - browser does the same it will load the json's response.

**base url**:
    portion of the URL that wil not change no matter what kind of resource is getting from the current API
    *e.g. https://apis.scrimba.com/jsonplaceholder*

**endpoint**: 
    Specifies which resource is requested, this can be changes and it represents contents.
    *e.g. /posts*

**full url**: https://apis.scrimba.com/jsonplaceholder/posts

### Methods
Telling the server what kind of request want to be make, what's the intention behind the request.
```
fetch("url", {method: "GET"})
```
- GET - getting data
- POST - adding new data
- PUTT - update existing data (edit/modify *e.g. typo*, adding new data if it is not already exist)
- DELETE - removing data
- others : PATCH, OPTIONS, etc.

### Body
The data we want to send to the server.

Only makes sense with: POST, PUT.

Needs to be turned into JSON first.
```
fetch("url", {
    method: "POST",
    body: JSON.stringify({ title: "Buy milk", completed: false})
})
```

### Headers
Extra/meta info about the request: authority, body info, client info, etc.
```
fetch("url", {
    mehtod: "POST",
    body: JSON.stringify({ title: "Buy milk", completed: false}),
    headers: {"Content-Type": "application/json"} //telling server that sending json and do its thing with it
})
```
or
```
const options = {
    mehtod: "POST",
    body: JSON.stringify({ title: "Buy milk", completed: false}),
    headers: {"Content-Type": "application/json"}
}
fetch("url", options)
```
---

## REST
Representational State Transfer

Set of rules for how devices talk to each other.
Commntication style between computers.
Design pattern to provide standard way for clients and servers to communicate.

### REST API Design Principles
**1. Client and Server separation**

The problem it is solves: a device (like a smartwatch), which doesn't have a browser, but wants some data in JSON format.
With REST API any device can display data if it's connected to the internet

**2. Statelessness**

It forgets the interaction after the response is sent.
Server doesn't maintain any memory of the requests.
Session state = if need more info, it has to sen it everytime when makkes a request

**3. Accessing "Resources"**
```
example: **Bycicle shop API**

baseurl: https://bikes.com/api

endpoints(/nouns - verbs): 
    /bikes - GET, POST
    /bikes/:id - GET, PUT, DELETE

nested(max 4 levels) endpoints and url parameters:
    /bikes/:id/reviews

query strings:
    /bikes?type=mountain&brand=trek
```

