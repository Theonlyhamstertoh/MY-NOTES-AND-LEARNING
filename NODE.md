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
 
### Explain why you might need a back-end for your project.
### Explain when you wouldn’t need a back-end for a project.
### Explain the event loop.
### Understand the origin of the Node.js runtime.
### Write a simple “hello world” application and run it in the console of your machine.
### Understand what Node.js really is.
