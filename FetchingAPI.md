# Fetch API

## API - Application Programming Interface (One of the most powerful things a web developer can do)
- what makes possible all the interactivity
- API is the messenger that takes request and tells the system what you want to do and then return those response back to you
- Think of API as a waiter. You tell the waiter you want this delicious meal, who takes your request, and then communicates to the kitchen (the server) what to do and finally returning that response back to you. 
	- You know how there are websites that track all the airplane companies that offer flights or all the stocks? That is API right there. You use a website that then request through the many airline API, collecting and returning that data back to you so then you can have a display of multiple air flight choices. The same with stocks I assume. It is pushing back to you thousands and thousands of API data so you can track everything in one area 
- APIs are accessed through URLS, and the specifics of how to query depending on the specific service you use. 
	- OpenWeatherMap API has several types of data you can request. To request, you would do something like api.openweathermap.org/data/2.5/weather?q=London,uk (can switch out the location


One major caveat 
- Because requesting data costs money (although relatively cheap), it can quickly get very expensive and costly if you have thousands of people accessing your data every minute. That means, services like OpenWeatherApp would use API keys and make you have to create a account first, with limitations of the amount of request base on your account type, so you don't abuse and take advantage of the system. 
- It is as simple as creating a account and following its direction to getting a free API request


Fetching Data
- AJAX - Asynchronous Javascript and XML (don't have to use XML, its old term)
- `let promise = fetch(url, [options])`
	- A simple get request and downloads the content of the url
	- First the promise, returned by fetch, resolves with an object of the built-in Response class as soon as the server responds with headers. 
	- promise rejects if fetch was unable to make HTTP request (network problems, no such sites)
	- Secondly, you will need to use more methods to collect the data from `response`
		- `response.text()` – read the response and return as text,
		- `response.json()` – parse the response as JSON,
		- `response.formData()` – return the response as FormData object (explained in the next chapter),
		- `response.blob()` – return the response as Blob (binary data with type),
		- `response.arrayBuffer()` – return the response as ArrayBuffer (low-level representation of binary data),
	- You can only choose one body response method. if you are chose response.json() then having another response.text() will not work. 
- browsers restrict HTTP request to outside sources (which is what you are doing with requesting APIs) 
- To overcome this: simply add `mode: 'cors'`
  
- You first need to actually get the resolve promise back. And from there, you need to use of the methods such as `response.json()` to access further the values you really need. 
	- OR
- `response.headers` allow you to get individual headers by name or iterate over them
- differs from `ajax()`
	- promise return from `fetch()` won't reject on HTTP error status and instead will resolve normally, with response OK set to false
	- won't send cross-origin cookies


- GET - used to request data from a specified resource
	- GET requests can be cached
	- GET requests remain in the browser history
	- GET requests can be bookmarked
	- GET requests should never be used when dealing with sensitive data
	- GET requests have length restrictions
	- GET requests are only used to request data (not modify)
- POST - Used to send data to a server to create / update a resource
	- POST requests are never cached
	- POST requests do not remain in the browser history
	- POST requests cannot be bookmarked
	- POST requests have no restrictions on data length
- PUT - The same as POST but the difference is that if you call the same PUT request multiple times, it will always produce the same results. In contrast, calling POST request multiple times may have side effects of creating the same resource multiple times
- HEAD - Identical to GET but without response body. 
	- For example, GET /users may return a list of users but the HEAD /users will make same request but not return the list of users
	- Useful to check what  GET request will return
- DELETE - deletes the specified resource

- TO make POST request - the image below saves the object "user" into a json file saved to the server. 
	- body - the request body
	- By default, the content-type header is set to text/plain... since you are sending JSON, you need to send application/json instead. 
	- 
- To send a image
	- 
- use `.then()` so along with fetch even within a async/await so that you can get them to start reading the .json files right away. 
