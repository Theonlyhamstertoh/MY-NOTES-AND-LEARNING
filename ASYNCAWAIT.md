# Async and await notes
## How do you declare an async function?
- You can declare them in arrays, in function expression

## What does the async keyword do?
- Lets javascript engine know you are declaring an asynchronous function which is required if you want to use await  inside any function.
- Just syntactical sugar for promises
## What does the await keyword do?
- Tells the javascript to wait for an asynchronous action to finish before continuing the function.
- basically saying: Pause until done
- Instead of calling .then after waiting for the async request to be done, you assign the await keyword and then use the result in your code as you normally would in synchronous code.
- Does not work with global scope. It has to be inside a async function 
## What is returned from an async function?
- If a function declares async, it automatically returns a promise. Returning in a async function is the same as resolving a promise!!! 
	- `promise.resolve("yes") === async () => { return 'yes'}`
- Throws error if rejected
## What happens when an error is thrown inside an async function?
## How can you handle errors inside an async function?
- There are two ways of handlign errors. 
	- First: Simply add a `.catch()` after the async function. Because async function always return a promise, it is just like adding a .catch() at the end of a promise chain
- Second: using try/catch like you normally would in synchronous code
- can use the returned value async function as a await in another async function
- write `JSON.stringify(profile)` to turn your object into a string.
- You have to wrap a anonymous async function to run await in top level code


## try and catch
- The lesson is: be careful when you have any function inside your async function. The await will only pause its parent function, so check that it's doing what you actually think it's doing.
- If you return `Promise.resolve()` , it will execute after all the synchronous code has been completed. 
- If you doing multiple await requests, it is really dumb to wait for the first one to complete then the second one when you don't need to be depending on each other. In that case, return a `promise.all([..])` instead which allow you to run concurrently. You don't want to pause a function uncessary.
	- `const smoothie = await Promise.all([...])`
	- Allow you to use try...catch.. block
- If you throw a error inside your catch block from try..catch, it will simply ignore the error and send it to the next closest `.catch `

## Tricks
- do `for await (const emoji of smoothie) {}`
	- By doing this, it will automatically wait for the item to resolve and then loop over them immediately
- `If (await getFruit('peach') === this.value)`
- write a for loop inside a async. This way you can pause. 
