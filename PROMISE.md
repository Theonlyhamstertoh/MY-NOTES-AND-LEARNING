# Promises Notes

## What is a promise?
1. Imagine yourself as a little child again and you saw this very beautiful toy. You ask your parents if they can buy you this toy and your parents, after some reluctance, says that if you successfully do your chores for a week, they will promise you to buy a toy. 
	1. The request: Do your chores for a entire week 
2. Now, here begins the promise. your parents request you to do your chores for a entire week. If you manage to successfully do your chores, then you will be able to get the toy. If you failed, you will not be getting the toy. That is the promise. Promise doesn't happen right away. It takes some time. In this case, a week. But in javascript, it may take two three seconds to make the request. This means that your code wouldn't run right away and what a promise does is make sure your code only runs after the request has completed. 
	1. resolve if you succeed, reject if you fail
3. You might ask, how does .then() come into play?
	1. Well, say you succeeded right and your parents promise now needs to be acted upon. Your parents would have to first start driving, go to the store, buy you the toy, and give it to you. That is what .then() means. It is what you want to do with the successful request. If you made a request say to get the weather data from a API and it was successful, you would want to make changes to it wouldn't you?  You may want to display on the web so you have a .then() to say appendChild and create DOM element. You can then chain the .then() together, allowing you to break it into multiple steps: create element, then style, then append. Of course this may not be the best example but what is happening here is allowing you to create your own flow of what you wish to do. 


## Promise Chain
- `.then(alert)` is the same as `.then(result => alert(result));`
- A flow control, expressing multistep async sequence but also a message channel to propagate messages from step to step
- Easy to catch error at any point in the chain and that catching acts sort to "reset" chain back to normal operation
- If you don't have any **function to catch error in then()**, It will automatically substitute one for you **function(err)**
- If no proper fulfillment handler parameter is put in `then(fulfill, fail)`, then it will automatically substitute one for you too
- A `then()` call against one Promise automatically produces a new Promise to return from the call
- Inside the handlers, if you return a value or an exception is thrown, the new returned Promise is resolved accordingly. 
- When you return a promise, it is unwrapped, so that the resolution in it will become the next `then()` parameter


## Resolve, Fulfill, Reject
```
const p = new Promise((x, y) {
  // X() for fulfillment 
	// Y() for rejection 
} );
```
- The rejection, second parameter, will always mark the Promise as **rejected**. 
- The fulfillment, first, will usually mark the Promise as fulfilled. But y**ou want to call it resolve  because you don't know if it would be rejection or fulfillment**
	- You can choose to resolve a **rejection**. 
- Resolving a promise means either fulfilling or rejecting the Promise

`Promise.resolve(..)`  creates a Promise that's resolved to the value given to it. 
- If you had to unwrap a object and that object was a rejection, then `Promise.resolve()` would have to return a rejection (perfectly fit for its name)
`Promise.reject("Oops")`  creates the rejected promise for the reason "oops"
- Doesn't do the unwrapping `Promise.resolve()` does. It will be seen as the rejection reason. 

What about the call backs in then()?
- Should be normally called `fulfilled()` and `rejected()`


## Error Handling

- `try...catch()` does not work on async. It is only synchronous. 
- There are limitations with just using `fulfilled()` and `rejected()` in the `then()`
	- `rejected()` wouldn't be notified of an error in fulfilled() because they were error handlers made for the `p.then()`. If there was no error in the returned value, then it would not switch to `rejected()`.

- `Promise.reject()` and `Promise.resolve()` are shortcuts for promises that will already be resolved. 
- `Promise.resolve()` can also help you unwrap thenable values and won't do anything if the value is already genuine. So use this as a good normalizing mechanism. 
- write a `catch( handleErrors)` at the end to catch any errors. All errors will go to the last promise. 





### `then()`
- takes one or two parameters,**first: fulfilled; second: rejected callback**. If both are omitted, it will pass its own default callback to substitute. 
- Creates and return a new promise to allow promise chain flow control
-  .then(success => {}, error => {})
-   returns a new promise
	1. The first argument runs the success function if the promise was resolved
	2. The second argument runs the error function if the promise was rejected
	3. If you are only interested in completion, then just pass one value into the argument. 
	4.  If you are only interested in catching errors, you can do `.then(null, errorHandling)` or use `.catch(errorHandling)`

### `catch()`
- Takes only the rejection callback as parameter and automatically substitute the default fulfillment callback. It is the same as `then(null, rejected)`
- Use this if you are only interested in catching errors
- Put this at the very end of the async chain so it will catch errors. If you want to have custom error handlers, you can put the second parameter in the `.then()` to find it
- Creates and return a new promise to allow promise chain flow control

## `.finally()`
1. essentially .then(f, f) it will always run no matter if resolve or rejected. 
2. This is a good handler to help you perform cleanup such as stopping loading indicators. 
3. Has no arguments. Different from .then(f, f) in that it wouldn't know if the promise was successful or not. But that is alright since it is meant to do general cleanups
4. Use this only when you don't care about the arguments. 
5. It just passes the results and error through to the next handler which is awesome since it is meant to just do general work.


### `Promise.all`
For `Promise.all([ .. ])` , all the promises you pass in must fulfill for the returned promise to fulfill. If any promise is rejected, the main returned promise is immediately rejected, too (discarding the results of any of the other promises)

### `Promise.race`
For `Promise.race([ .. ])` , only the first promise to resolve (fulfillment or rejection) "wins," and whatever that resolution is becomes the resolution of the returned promise. This is called a latch. 
- If no values is put, it will hang forever and never resolve. 

### `Promise.allSettled`
`Promise.allSettled` will basically wait for every single one to settle first regardless if it succeeded or failed. So you have say 3 succeeded and one failed. 
- Which then means you will have to iterate through all of them, like results.forEach and using result.status
- However, if and only if an empty iterable is passed as an argument, `Promise.allSettled()` returns a Promise object that has already been resolved as an empty array.

### `Promise.any`
`Promise.any` waits only for the first fulfilled promise. It will ignore all rejected. If all promises were rejected, then it will return AggregateError - an error object that all you to access promise errors in its errors property. So something like `error.errors[0]` 


## Advices from Ice-RIMED RASPUTIN from ODIN DISCORD PROJECT
- `.then` statements assume a previous one returned another promise. And we can continue to chain them like that, so long as they continue to return promises. So fetch returns a promise, because it is asynchronous. `response.json()`  in that first then clause is also returning a promise. When that resolves, we pass that into the next .then in the chain.
- So we can pass functions to other functions, and they'll run them for us. This is what is meant when we say "in javascript, functions are first-class citizens." They are simply variables... that store a function. We can pass them in, we can pass them out, we can send them about like any other variable.

## Promise
- When you type  const promise = `new Promise((resolve, reject) => {  // executor (the producing code, "singer") });`
	- The executor runs automatically when you create it. 
- `resolve(value) `and `reject(error)` are both callbacks provided by javascript and are not variables. 
	- Sooner or later, you will have to call on one of these callbacks. 
- When you create a new Promise, 
	1. the executor, which is the code inside the promise, will run automatically. It will attempt to perform a job, fetch a API, or collect data which all takes async time
	2. Then, once the job is completed, you will call either resolve or reject callbacks. 
	3. The variable promisethat you defined will then contain the internal properties
		1. state: either fulfilled when resolved or rejectif there was a error
		2. result - initially undefined, then changes to value when of `resolve(value)` if called or errorwhen `reject(new Error("OOPS"))`
	4. resolve and reject tells that the job is done, that either the fetch was successful or failure which then can be passed onto the variable
		1. This promise is then called "settled" instead of "pending"
		2. If no value is being returned in `resolve()`, it won't return any value but just move to the next `.then()`. Please however, include a error for reject. 
			1. There can only be one single resolve / reject. If you already called resolve earlier, calls afterwards will all be ignored. See it as a return essentially. Anything below it won't matter. 
			2. resolve / reject will only expect only one argument and any additional will be ignored. 
			3. It is recommended to use Error objects for rejected
			4. You can have a immediately resolved promise if all fetching is already completed
		3. You cannot directly access the state or result properties. .then .catch and .finally can catch the values for you and do things with it



## Why do we return promises?
1. It is so that instead of writing a gigantic long promise initializer, you put it in a function and return it so  you can keep the cold very clean. Look at the example below, it makes everything much easier to find and to search.

Instead of this: 
```
const promise = new Promise((resolve, reject) {
    ... extremely
    huge
    and big
    code 
    block
    while
    being
    hard
    to
    read
})
```

We do: 
```
const loadScript = (src) => {
    return new Promise((resolve, reject) => { 
        ... write the super long code here
    })

}


const promise = loadScript(src);
```

From here, if you do a `.then(success)` , it will take the `resolve(takethevalueinhere)` and be able to use it and edit.  

Remember: 
`.then()` and all others will only run after promise has passed a resolve or reject value. 

```
function doAsyncTask() {
    return Promise.resolve("sucess");
    }
}

doAsyncTask().then(() => console.log(message) );

const message = “Hello I am a teapot”;
```

## `Promise.resolve()`
- use this as a checker to normalize in case the value is not a genuine promise


## Promise Chaining

1. When you return a `new Promise()`  within a `.then()` , what you are basically doing is allowing async task to be dealt in the `.then()` so that the next `.then()` would have to wait until the previous async task has been completed. the next `.then()` would be instead chained to a new promise. This is really awesome because it allows you to have async events and chain all of them together, meaning you can now fetch in the `.then()` too! That is really powerful
2. It will create two promises if you are returning a new Promise within a `.then()` function. But its best to see them as one instead
	1. Note: As described, technically there are two promises in that interchange: the 200ms-delay promise and the chained promise that the second `then(..)` chains from. But you may find it easier to mentally combine these two promises together, because the Promise mechanism automatically merges their states for you. In that respect, you could think of `return delay(200)` as creating a promise that replaces the earlier-returned chained promise. 
3. This is very useful if you have to make a network request several times. First to fetch user data general, then to get the specific  data, then make a netwrok request to github, and then remove it after some seconds. All of those require async time which by returning a new Promise or using the already made functions that returns a promise. 
4. 
   

`.then(f1, f2)` is not the safe as  `.then(f1).catch(f2)`
- In the second example, if there was a error, the `.catch(f2)` will handle the error because the error is passed down the chain. 
- They are basically the same with exception that the second example can have a different error handling. 
Practices
- An asynchronous action should always return a promise even if you don't plan on doing anything afterwards. 
	- Don't just do `setTimeout(()=>.., 2000)` at the end of a `.then()` . This will make you unable to continue the chain. 
	- Instead, You return a promise which will resolve inside the setTimeout so the next `.then()` would wait on it and be able to continue chaining. 
- Split your code into resuable code by returning `[fetch || new Promise]` . Remember what is happening if you put a return new Promise in a `function(url)`. With this, you can split your code and be organized. 
	- Such as functions for: `loadJson(url) loadGithubuser(name) showavatar(githubUser) etc..`
- Append a `.catch` to the end of the chain to catch any errors


## Error Handlings
1. When a promise rejects, it jumps to the closest rejection handler. That is convenient as you can have a `.catch()` at the very bottom to fetch the errors. 
2. a `throw new Error("whoops!")` is the exact same as `reject(new Error("Whoops!"));` . This is because of the invisible try...catch that automatically catches the error and turns it into a rejected promise. this also happens in any handlers (.then) as well. 
3. You can rethrow the error such as `throw New Error("whoops!")` in the the `.catch` and it will then go to the next **closest error handler.** 
	1. If the `.catch` block finishes normally, it will then continue to the next closest .then handler
	2. If you don't return anything in a `.catch` , it will execute normal way 
	3. By throwing a error, you can have `.catch` handlers just for URL or have one for undefined. 
	4. If there are no error handlers for any errors, it will then die, sending a global error which triggers the event unhandledrejection
	6. Should have a unhandledrejection event handler that inform the error to user so the app don't "just dies"
	7. Will `.catch` trigger? Just don't use throw new Error. Instead use the reject normally. 
		1. 

## Promise.resolve/reject
- They are rarely needed in modern code due to async/await syntax.

## Promisification
- Conversion of a function that accepts a callback and returns a promise instead. 
- This is very necessary to have because many functions are libraries are callback-based

## Async/Await
- A special syntax that allow you to use promise in a more fashionable way just like class constructors
- Very easy to understand!!
- there is a async keyword which you place before a function. 
  
	- When you have the async keyword, it means the function always returns a promise. 
	- Other values are wrapped in a resolved promise automatically


await only works inside the function ofasync.  
- `let value = await promise`;
- Makes JS wait until that promise settles and returns its result. 
- So it literally suspend function execution until promise settles, and then resume when the promise is completed. 
- More of a elegant way than to use promise.then
- Cannot be used INSIDE A non-async function

