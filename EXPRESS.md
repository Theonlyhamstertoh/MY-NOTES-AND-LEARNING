# Express
After using NodeJS, you can see that it requires a lot of complexity and can be get really messy just to do a few things. 

### What is Express?
* A really fast, very basic at the core and have full control of how you handle the request and responds. 
* A server-side framework but can be used in combination.

### Why use Express?
* Makes building web application 100x times easier
* Used for both API and microservices
* Most popular Node Framework
* Write handlers for request with different HTTP routes
* generates responses by inserting data into templates

### Express Core
* `res.send()` = to send string response. Will also end the request cycle. 
* `res.json()` = to send JSON response
* `res.sendFile()` = to send a file
* `app.all()` = will be called in response to any HTTP method. 

Has all these methods: 
```
checkout(), copy(), delete(), get(), head(), lock(), merge(), mkactivity(), mkcol(), move(), m-search(), notify(), options(), patch(), post(), purge(), put(), report(), search(), subscribe(), trace(), unlock(), unsubscribe().
```
An example:
```
app.all('/secret', function(req, res, next) {
  console.log('Accessing the secret section ...');
  next(); // pass control to the next handler
});
```
* Routing allow you to match the specific URL name you want and to do action with it

### Adding folders in Routing
* Type `const router = express.Router()` and use that instead of `app.get()` would be how you add routes. The route `/app` would then become ``host/router/app"
* You will have a new file specifically called say `wiki.js` and export the `router`. Then to use it, you will simply write in the main file `app.use("/wiki", wiki)`

### Using Middleware
* An express application is really just a series of middleware function calls
* Middleware can perform any operation, execute any code, make changes to the request and response objects, also end the request-response cycle
* If not end, you must call `next()` to pass control to next middleware function
* Be aware of the order you place your middleware. If session middleware depend on cookie middleware, then cookie handler must be added first. 
* If `next()` is not called, then the request is left hanging.
* OFten the case that middleware are called before setting routes
* The difference between a `middleware function` and a `route handler callback` is that the middleware function must have a third argument `next` which are expected to call to complete the request cycle.

``` 
// this will be used by all routes
app.use(middlewareFunction)

// specify a specific route to be used
app.use("/someroute", middlewareFunction")

// can add middleware for specific HTTP verb
app.get(middlewareFunction)

// can specify the specific route for HTTP verb
app.get("/route", middlewareFunctiom)

```

* Application-level middleware: using the `app` 
* writing `next("route")` will automatically skip to the next route stack. 


### Middleware as a array
Declare the middleware substack as a array to allow for resuability
```
const logOriginalURL = (req, res, next) => {
  console.log("original:", req.originalUrl);
  next();
};
const logMethod = (req, res, next) => {
  console.log("method:", req.method);
  next();
};
const middlewareArray = [logOriginalURL, logMethod];
app.get("/about", middlewareArray, (req, res, next) => {
  res.send("GAME OVER");
});
```

### Error Handling
Define your middleware function as usual but now with four arguments:
```
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

### Basic Route Handling
* Handling requests/routes is simple
* app

Describe Express and Node’s main benefits.
Describe the relationship between Node and Express.
Explain what a module is and how Express fits in.
Import and create modules.
Describe asynchronous APIs.
Describe and create route handlers.
Describe and use middleware.
Describe error handling in Express.
Describe what the main parts of an Express app might look like.


#### Setting up a Node development environment
Describe Express development environment.
Import Express into an application using NPM.
Create and run applications using the Express application generator tool.
Set up a development environment for Express on your computer.
