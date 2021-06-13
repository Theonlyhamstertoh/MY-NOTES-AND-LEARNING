# React notes
Please be aware the notes written here are on React Class Component.  

## You use `super()` whenever you use a constructor in ES6 class
- Required if you need it to access some information of the parent variables
- In react, it will make props available across the component 
- `super()` is needed to initialize props.
- The only place where you can assign `this.state` is in the constructor
- You bind the `this` keyword when passing a function is that so the context of this stays the same. 
	- Here, the `looseClick()` return undefined because the meaning of this has been changed. When you bind the this as the `myButton`, it will instead see this as `mybutton` and not a globalThis
	- You have to bind for all functions in class components when passing them to other components so they know what context to oprate in.
	- It returns a new function. See it as just `cool()` but with an extra `.bind(this)` that sets the this value to the argument in place.   
	- Deconstruct `title`  and `onButtonClicked` by doing this
		- 
## `State()`
- All values that can be change over time are handled through state
- State should never be changed directly. Always use setStateto modify the the state. Never do `this.state.count = 3`.
- The only place where you can assign `this.state` is the constructor.
- Because this.props and `this.state` can run async, you shouldn't rely on their values to calculate the next state. Think of them as async. If you have a lot of these `this.setState`, react may batch them up . This causes a problem if any of the state depend on the current state, not the previous. If you want to incerment by 1 and call `this.setState` twice, it may not be incremented. 
- Top to bottom. All flows down. 

## JSX
- allow you to use your own XML-like tags 
- Self-closing tags must end in a slash `/>`
- You can embed javascript expressions
- When you write `<welcome name='Sara' />`
- The JSX attribute name and its children is passed to the Welcome component as a single object called props . To access the attribute, you would write props.name. This is a really interesting concept because it is basically like parameters in a way. 
- Allow embedding any expression in curly braces. 
React Notes
- You do not need paranthesis if your JSX only contains one line
- className refers to the DOM class. 
- Always use keys in React when making lists to help you identify each list item. Use a random ID or index. 

REACT WILL MERGE IN BATCHING MODE
- If you call multiple set states, react will simply decide to do Object.assign(), which means the last key of the same value overwrites all others. 
- If you want to calculate the next state from the previous state, do not insert objects. You need to use function setstate. 
	- When you do function setState, the updates will be queued and execute later in the order they were called. When react see multiple functional `setstate()` it will queue instead of mergin objects together. 
	- After that, React then pass in the previous setstate value 
	- Also accepts a callback. If there is a value in it, it will call that after updating the state. 

Event Handling
- You pass a function as event handler and not a string.`onClick={ASD}`
- Uses camel casing onClick
- must explicity say `e.preventDefault()`
-  Don't need to call addEventListener . 
- Generally, if you refer to a method without () after it, such as `onClick={this.handleClick}`, you should bind that method.

Rendering
- If you need to hide anything, return null
- You can use arrays and `map()` to return for each `<li>{el}`</li>  and runs through the entire list
	- Must always include a special key string attribute when creating lists of elements
		- Helps react identity which items have changed, added, or ad removed. 
		- Give them a stable identity. 
	- A good rule of thumb is that elements inside the map() call need keys.
	- Put keys in the surrounding context that makes sense

Advice
- Don't do `prevState.counter++`  as this mutates prevState. Do `revState.counter + 1`
Thoughts
- You are a beginner and you may feel like you are not understanding a single thing, you will eventually come to a understanding. You will eventually master React. Just like Webpack; just like Async and await, you will eventually understand React. And you already pretty much do. You just need a bit more information and you are comfortable. Trust the process. It will guide you to light. (notes to myself)
