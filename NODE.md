# Node
Node is a "javascript runtime". When javascript was first created, it was designated to only run in the browser. But with node, you can now run this out of the browser. You can use javscript in the server to compete with other serverside languages like ruby. This means that you really only have to learn one language to do both instead of two separate ones. 

node has added some new functionalities that is only accessible in Node. Example: the ability to read and write local files, create http connections, and listen to network requests. 

NodeJS is a asynchronous event driven javascript runtime. This means, instead of defining exactly what will happen line after line, you break them into functions that runs when the event before it has completed. This is what async means. Remember your library project and how you first fetched data, then use it to display. The same here in Nodejs. You don't want to be blocking all the code after it so that's why it is asynchronous. Asynchronous allows your sync code to run normally. Node will then wait on a event and when something occurs (file is read) then it will do the next step. 

### What is a web server?
A web server is a computer that stores the web server software and the component files (html and css and js). A web server when connected to internet can then exchange these files and data to other devices connected to the web server. Imagine a bar. The bar has all the drinks and dishes. When the customer comes to the bar and sit down (they are connected to the web sever or bar in this case) they will have access to the bar's dishes and drinks. They can then order a drink (fetching a file by request via HTTP) and the web server will respond with the 40% vodka they wanted. Throughout the night, the customer and the bar will continue making request and send back the files. 

### Static Server VS Dynamic Server
A static server is one that returns your files as it is to your browser. Meaning they just send without updating. A dynamic server however is a combination of a static server and a lot of extra stuff. Usually, a server and a database. That means, your data will be updated overtime instead of just being a static file. think about wikipedia. They can't be making millions of static html pages. They instead have a few html template and have a giant database. An analogy to make is: a static server is buying a new car and a dynamic is one that can fix the car or upgrade it and make changes to it. The static server can't make changes to the new car while the dynamic one can. 

#### Static
![image](https://user-images.githubusercontent.com/75579372/125340300-2a5dd200-e307-11eb-94df-5763ce41c0a6.png)

#### Dynamic 
![image](https://user-images.githubusercontent.com/75579372/125340317-3053b300-e307-11eb-877c-f5cc0b53396f.png)

### Describe the purpose of a server.
* To dynamically display different data when needed by simply pulling out of a database. 
* It allows you to tailor website content for individual users. You can send notifications or updates through email to really deeply engage with the users. 

### What is  server-side programming?
* Web brwoser communicates with web servers through **HyperText Transfer protocol** or HTTP. When you click a link on the webpage or submit a form, you are submitting a HTTP request to the web server. 
* It is very useful because you can efficiently deliver information tailored to the individual users. 
* You can make a online live game! If you do this, you can make the multiplayer JENGA! IMAGINE HOW COOL, REALLY JUST HOW COOL THAT CAN BE.
* A deeper analysis of user habits can be used to anticipate their interests and further customize responses and notifications, for example providing a list of previously visited or popular locations you may want to look at on a map.
* Can allow you to restrict access to only authorized users! Like a game! Like a game room! That would be really cool.
* Can store your user sessions! Like know when the user plays and not playing. know when they last read.
* Can make data analysi!

![image](https://user-images.githubusercontent.com/75579372/125340724-abb56480-e307-11eb-89e1-bf88529d14b0.png)

### Client-Server overview
* When you click a link, the browser sends an HTTP request. This request can include: 
   * URL identifying the target resource 
   * Method that defines the required actions: 
      * `GET` Gets a specific resource (an html file or a list of products)
      * `POST` Create a new resource (add a new article to the wiki)
      * `HEAD` Get metadata information without getting the "BODY". Remember, the BODY is basically all the data. You can use `HEAD` to scout ahead for enemies and if found, call `GET` to send the troops over. 
      * `PUT` Updates an existing resource or create new one if doesn't exist
      * `DELETE` delets specific resource
    * Addition request information encoded in the URL. `http://mysite.com?name=Fred&age=11`
* `200` success, `404` not found, `403` forbidden, unauthorized

#### A Request
* `HEADER` - Just like how the HTML Head contains useful information about the HTML document, the `HEADER` of a request contains useful metadata. 
```
GET https://developer.mozilla.org/en-US/search?q=client+server+overview&topic=apps&topic=html&topic=css&topic=js&topic=api&topic=webdev HTTP/1.1
Host: developer.mozilla.org
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Referer: https://developer.mozilla.org/en-US/
Accept-Encoding: gzip, deflate, sdch, br
Accept-Charset: ISO-8859-1,UTF-8;q=0.7,*;q=0.7
Accept-Language: en-US,en;q=0.8,es;q=0.6
Cookie: sessionid=6ynxs23n521lu21b1t136rhbv7ezngie; csrftoken=zIPUJsAZv6pcgCBJSCj1zU6pQZbfMUAT; dwf_section_edit=False; dwf_sg_task_completion=False; _gat=1; _ga=GA1.2.1688886003.1471911953; ffo=true
```
 
```
* Type of request (end of first line identify the specific HTTP protocol version)
* target resource URL
* URL Parameter
* Target host website
* Information of the browser used 
* sort of responses browser can handle.
* indeciate the address of the web page that contained the link to the resource
* Final line contains a cookie to manage session id.
```

In a `POST` request, the URL dosen't have paramter but is instead inside the body of the request
### The Response
```
* first line include the response code if the request succeeded
* content-type: shows the response is `text/html` formatted
* content-length: tells us how big the it is
* We then see the body
* the X-Frame-Options: DENY line tells the browser not to allow this page to be embedded in an <iframe> in another site
```
```
HTTP/1.1 200 OK
Server: Apache
X-Backend-Server: developer1.webapp.scl3.mozilla.com
Vary: Accept,Cookie, Accept-Encoding
Content-Type: text/html; charset=utf-8
Date: Wed, 07 Sep 2016 00:11:31 GMT
Keep-Alive: timeout=5, max=999
Connection: Keep-Alive
X-Frame-Options: DENY
Allow: GET
X-Cache-Info: caching
Content-Length: 41823

<!DOCTYPE html>
<html lang="en-US" dir="ltr" class="redesign no-js"  data-ffo-opensanslight=false data-ffo-opensans=false >
<head prefix="og: http://ogp.me/ns#">
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=Edge">
  <script>(function(d) { d.className = d.className.replace(/\bno-js/, ''); })(document.documentElement);</script>
  ...
```

#### For Post
* Status code `302 found` tells the browser the post suceeded and must issue a second HTTP request to load the page specified in`LOCATION` 

### Explain why you might need a back-end for your project.
### Explain when you wouldn’t need a back-end for a project.
### Explain the event loop.
### Understand the origin of the Node.js runtime.
### Write a simple “hello world” application and run it in the console of your machine.
### Understand what Node.js really is.

# Node Core
* Doesn't have any WEBAPIS (documents, windows...)
* Another another object called `global`. Instead of `window.console.log` it becomes `global.console.log` 
* However, variables you defined are not added to `global` object. This is because they are only scoped to the file and cannot access outside of it
* Avoid defining variables and functions in the global scope else you will have conflicts
* Every file in a node application is considered a module. 
* IN OOP, it means they are PRIVATE. They are scoped only to that file. If you want to use it, you need to explicitly export it. 
* Each will have a main module
* `process` object gives information about, and control over, the current NODE.JS process
* `global` provides a system for accessing and setting global variables.

For example, if you do `global.something = true` in one module, in another module you can access `something` and it will be true (without having to export it).

### Node Modules
* Here is the thing, in node, everything is in modules. If you create another folder right, your code can only be access with 
```
///// myfirstmodule.js /////
function log(message) {
  // send http request
  console.log(message);
}

module.exports.log = log;


///// app.js /////
const logger = require("./logger")
logger.log("yo") <--- see here

```

`module.exports.log = log;` ---> creates a object
`module.exports = log;` -----> makes log the only function with no scope. 

### Module Wrapper function
Node.js under the hood wraps your code in an IFFE function and pass the require and exports parameters of your file. This way, it keeps top level code scoped to the specific file.
```
(function (exports, require, module, __filename, __dirname) {  


 });
 ```
### Path
 just get file name and get rid of the extension
 ```
console.log(path.basename(__filename, ".js"));
 just get directory name
console.log(path.basename(__dirname));
 get only the extension
console.log(path.extname(__filename));
 join path
console.log(path.join(__dirname, "cooldog.js"));
 parse returns object of the properties separated
console.log(path.parse(__filename));
┌─────────────────────┬────────────┐
│          dir        │    base    │
├──────┬              ├──────┬─────┤
│ root │              │ name │ ext │
"  /    home/user/dir / file  .txt "
└──────┴──────────────┴──────┴─────┘
```

# Filesystem

```
// create folder 
fs.mkdir(path.join(__dirname, "test2"), { recursive: false }, (err) => console.log(err));

// create and write to file to the newly created folder
// writeFile will overwrite whatever file is inside
fs.writeFile(path.join(__dirname, "test2", "test5.md"), "testing....", (err) => {
  if (err) throw err;
  console.log("THE FILE HAS BEEN SAVED");
});

// append file will not overwrite
fs.writeFile(path.join(__dirname, "test2", "new.md"), "testing....", (err) => {
  if (err) throw err;
  console.log("THE FILE HAS BEEN SAVED");
  fs.appendFile(path.join(__dirname, "test2", "new.md"), "second testing but without overwriting", (err) => {
    if (err) throw err;
  });
});

// read file. You have to include the `encoding type` or else it will just return you a bunch of numbers.
fs.readFile(path.join(__dirname, "test2", "new.md"), "utf8", (err, data) => {
  if (err) throw err;
});

fs.rename(path.join(__dirname, "test2"), path.join(__dirname, "nomoretest"), (err) => {
  if (err) throw err;
});


```

# OS
```
const os = require("os");
// get the platform
os.platform();
// get CPU architecture 
os.arch();
// get data of all the cpus
os.cpus();

// Returns the amount of free system memory in bytes as an integer.
os.freemem()

// return all the memory
os.totalmem();

// Returns the string path of the current user's home directory.
os.homedir();

// return computer uptime
os.uptime();

// return computer hostname (ex.laptop-qqs98kh)
os.hostname()

//


