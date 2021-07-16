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
* When you use route, the `router.get("/")` will now be based on that route folder. So if you have a route at `/icecream` then any future route `/` will become the `/icecream` route homepage.

```
router.get("/:id", (req, res) => {
  // this is a great method since filter returns the one and we then directly send it.
  const found = users.some((user) => user.id.toString() === req.params.id);
  console.log(found);
  if (found === false) {
    res.status(400).json({ message: "Member not found" });
  } else {
    res.json(users.filter((user) => user.id.toString() === req.params.id));
  }
});

```
### Using Middleware
* An express application is really just a series of middleware function calls
* Middleware can perform any operation, execute any code, make changes to the request and response objects, also end the request-response cycle
* If not end, you must call `next()` to pass control to next middleware function
* Be aware of the order you place your middleware. If session middleware depend on cookie middleware, then cookie handler must be added first. 
* If `next()` is not called, then the request is left hanging.
* OFten the case that middleware are called before setting routes
* The difference between a `middleware function` and a `route handler callback` is that the middleware function must have a third argument `next` which are expected to call to complete the request cycle.
* Examines incoming request and prepare it for further processing. 
* For example, check if the user is trying to access a portion that requires authentication and if good continue or show error

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

### Serving Static Files
* Use this to serve static files, including images, CSS, and Javascript. 
`app.use(express.static('public'));`

### Can specify a folder instead of going by the base URL && serve static files out of a specific folder!
```
// this one line right here makes it so that the files are all served from that one folder you specified.
app.use(express.static(path.join(__dirname, "public")));
// add a prefixed URL folder name
app.use("/pages", express.static(path.join(__dirname, "public")))
### Middleware as a array
Declare the middleware substack as a array to allow for resuability
```
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
### Give status to have better error handling
```
res.status(400).json({ message: "Member not found" });
```
### res.render("index")
Renders a view and sends the rendered HTML string to the client.

### res.json
```
// get all json data
app.get("/api/members", (req, res) => res.json(users));
```

### Error Handling
Define your middleware function as usual but now with four arguments:
```
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

### POST Parsing
```
// parse data passed through the body. By adding this, the req.body will no longer be undefined and data can actually be parsed as JSON.
app.use(express.json());
```
### Templating engines
* There are many templating engines out there like: pug, ejs, handlebars, and so on. pug will have some getting used to because it feels significantly different from normal HTML. As for me, I think I want to spend some time learning ejs. That will be good for me. 


### Express Engine
Write `express [name] --view=ejd --css=[]` to create a skeleton project.
You can then run `npm start` `DEBUG=express-locallibrary-tutorial:* npm start`. The difference is the latter enable debugging. 

Middleware
**cookie-parser**: Used to parse the cookie header and populate req.cookies (essentially provides a convenient method for accessing cookie information).
**debug**: A tiny node debugging utility modeled after node core's debugging technique.
**morgan**: An HTTP request logger middleware for node.
**http-errors**: Create HTTP errors where needed (for express error handling).

### crud and mvc

Explain CRUD and how it correlates to HTTP methods in Express.
* CRUD stands for `create, read, update, delete`. These are the four basic actions a user should expect to do. add things and delete things. These correlates to the `app.get() = read`, `app.delete() = delete`, `app.post() = add`, `app.put() = update`.   
Describe MVC and how it correlates to Express.
* MVC is simply a coding architecture. The structure of the system that separates the logic from one another. 
* MVC stands for `model`, `view`, and `controller`. The `view` is what will render your data. In this case, `ejs` will be what show the data. In react case, `react` is the view. `Controller` is what decides should be rendered or displayed. While `models` is the data and data-managment of the application. The `model` enables a internal inference (API) to allow for the program to interact internally. While the `view` and `controller` enable interaction on the `GUI or the web` to allow for the users to communicate. 
* **All interaction flows from the VIEW/CONTROLLER to the MODEL. THE MODEL does not ever tell what the view/controller should do**
Describe databases and ORMs as well as how to use them with Node/Express apps.
Design and create your own models using Mongoose.
Declare object schema and models.
Describe the main field types and basic validation.
List a few ways to access model data.
Test models by creating a number of instances (using a standalone script).


