# typescript-notes

## What exactly is typescript?
Typescript is a superset of of Javascript, meaning that anything that you can type valid Javascript code in Typescript without any problems. Typescripts creates a defined shape and structure of an variable, object, function, and so on. Typescript essentially puts more strictness onto your code to help you avoid any silly errors. Although the syntax will lead to writing a bit more code, it will save you from making insanity-driven errors in production mode. You can start playing around Typescript and getting familiar quickly because it is just adding a few more features onto Javascript.

## Defining Types in Variables 
When you write ```let helloWorld = "Hello World"``` the variable ```helloWorld``` becomes of the type ```string.``` . This means that if you want to redefine the variable in future, it **must** be a string. Redefining with other primitives like number or boolean or objects will return error. 
```
let helloWorld = "Hello World"
helloWorld = 34;

// Type 'string' is not assignable to type 'number'
```


