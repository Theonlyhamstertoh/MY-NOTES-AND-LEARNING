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
* `res.send()` = to send string response
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
* Middleware can perform any operation, execute any code, make changes to the request and response objects, also end the request-response cycle
* If not end, you must call `next()` to pass control to next middleware function
* Be aware of the order you place your middleware. If session middleware depend on cookie middleware, then cookie handler must be added first. 
* OFten the case that middleware are called before setting routes
* 
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
