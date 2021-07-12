# Node
Node is a "javascript runtime". When javascript was first created, it was designated to only run in the browser. But with node, you can now run this out of the browser. You can use javscript in the server to compete with other serverside languages like ruby. This means that you really only have to learn one language to do both instead of two separate ones. 

node has added some new functionalities that is only accessible in Node. Example: the ability to read and write local files, create http connections, and listen to network requests. 

NodeJS is a asynchronous event driven javascript runtime. This means, instead of defining exactly what will happen line after line, you break them into functions that runs when the event before it has completed. This is what async means. Remember your library project and how you first fetched data, then use it to display. The same here in Nodejs. You don't want to be blocking all the code after it so that's why it is asynchronous. Asynchronous allows your sync code to run normally. Node will then wait on a event and when something occurs (file is read) then it will do the next step. 

### What is a web server?
A web server is a computer that stores the web server software and the component files (html and css and js). A web server when connected to internet can then exchange these files and data to other devices connected to the web server. Imagine a bar. The bar has all the drinks and dishes. When the customer comes to the bar and sit down (they are connected to the web sever or bar in this case) they will have access to the bar's dishes and drinks. They can then order a drink (fetching a file by request via HTTP) and the web server will respond with the 40% vodka they wanted. Throughout the night, the customer and the bar will continue making request and send back the files. 

### Static Server VS Dynamic Server
A static server is one that returns your files as it is to your browser. Meaning they just send without updating. A dynamic server however is a combination of a static server and a lot of extra stuff. Usually, a server and a database. That means, your data will be updated overtime instead of just being a static file. think about wikipedia. They can't be making millions of static html pages. They instead have a few html template and have a giant database. An analogy to make is: a static server is buying a new car and a dynamic is one that can fix the car or upgrade it and make changes to it. The static server can't make changes to the new car while the dynamic one can. 

### Describe the purpose of a server.
* To dynamically display different data when needed by simply pulling out of a database. 
* It allows you to tailor website content for individual users. You can send notifications or updates through email to really deeply engage with the users. 

### What is  server-side programming?
* Web brwoser communicates with web servers through **HyperText Transfer protocol** or HTTP. When you click a link on the webpage or submit a form, you are submitting a HTTP request to the web server. 
### Describe the differences between static and dynamic sites.
### Explain why you might need a back-end for your project.
### Explain when you wouldn’t need a back-end for a project.
### Explain the event loop.
### Understand the origin of the Node.js runtime.
### Write a simple “hello world” application and run it in the console of your machine.
### Understand what Node.js really is.
