# typescript-notes

## What exactly is typescript?
Typescript is a superset of of Javascript, meaning that anything that you can type valid Javascript code in Typescript without any problems. Typescripts creates a defined shape and structure of an variable, object, function, and so on. Typescript essentially puts more strictness onto your code to help you avoid any silly errors. Although the syntax will lead to writing a bit more code, it will save you from making insanity-driven errors in production mode. You can start playing around Typescript and getting familiar quickly because it is just adding a few more features onto Javascript.

## Defining Types in Variables 
When you write ```let helloWorld = "Hello World"``` the variable ```helloWorld``` becomes of the type ```string``` . This means that if you want to redefine the variable in future, it **must** be a string. Redefining with other primitives like number or boolean or objects will return error. 
```
let helloWorld = "Hello World"  ---- equals to ----> let helloWorld:string = "Hello World" 
helloWorld = 34; //ERROR 
// Type 'string' is not assignable to type 'number'
```

Typescript infers from the value assigned to the variable what the type of it should be. If however, you are simply intiailizing the variable with no value but you want to define a type, you would write: ```let helloWorld: string;```

## Defining Type: ```Any```
If you were to write ```let icecream;``` the ```icecream``` variable will be of the type ```any```. This means the variable will work like a normal Javascript one and can be redefined as any primitives or objects. 
```
let icecream; // infer as any type
icecream = "25";
icecream = 25;
icecream = [];
icecream = {};
```
## Interface
A Interface defines the shape of the object you can create.
```
interface Person {
  first: string,
  last: string,
}
```
Any objects that uses the interface of ```Person``` below **Must** only have the two properties: first and last, and they both must all be string values. 

### Using Interface 
To use ```Person```, you would specify the way you do with primitive ```: string```, ```:number```, etc...
```
const person: Person = {
  first: "weibo",
  last: "zhang",
};
```
## Anonymous Interface




