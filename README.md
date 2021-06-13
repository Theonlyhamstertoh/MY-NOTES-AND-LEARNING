# typescript-notes
The very basics of basics from notes of a complete beginner. Be aware of any errors.

## What exactly is typescript?
Typescript is a superset of of Javascript, meaning that any Javascript code are valid Typescript code. Typescripts creates a defined shape and structure of an variable, object, function, and so on. Typescript essentially puts more strictness onto your code to help you avoid any silly errors. Although the syntax will lead to writing a bit more code, it will save you from solving insanity-driven errors in production mode. You can start playing around with Typescript and getting familiar quickly because it is just adding a few more features onto Javascript.

## Defining Types in Variables 
When you write ```let helloWorld = "Hello World"``` the variable ```helloWorld``` becomes of the type ```string``` . This means that if you want to redefine the variable in future, it **must** be a string. Redefining with other primitives like number or boolean or objects will return error. 
```
let helloWorld = "Hello World"  ---- equals to ----> let helloWorld:string = "Hello World" 
helloWorld = 34; //ERROR 
// Type 'string' is not assignable to type 'number'
```

Typescript infers from the value assigned to the variable what the type of it should be. If however, you are simply intiailizing the variable with no value but you want to define a type, you would write: ```let helloWorld:string;```

## Defining Type: ```Any```
If you were to write ```let icecream;``` the ```icecream``` variable will be of the type ```any```. This means the variable will work like a normal Javascript one and can be redefined as any primitives or objects. 
```
let icecream; // infer as any type
icecream = "25";
icecream = 25;
icecream = [];
icecream = {};
```
## Interface and Custom Types 
A Interface defines the shape of the object you can create.
```
interface Person {
  first: string,
  last: string,
}
```
Any objects that uses the interface of ```Person```  **must** only have the two properties: first and last, and they both must all be string values. 
```Person``` also becomes a custom type you created with your own structure. 

### Using Interface 
To use ```Person```, you would specify the same way you do with any other primitive ```:string```, ```:number```, etc...
```  
const person1: Person = {
  first: "weibo",
  last: "zhang",
};
```
This will check if ```person1``` has the right shape defined in the **Person** interface. If not, it will result in ERROR. 

### Dynamic Interface Property
The above example is very strict to what properties of the object ```person1``` can have. But what if you want more flexibility? What if you want to add another property of ```age``` or ```middleName```? You will need to use ```[key: string]: any,```

```
interface Person {
  first: string,
  last: string,
  // the **key** must be of the type string. The value assigned to the **key** is now the :any type.
  [key: string]: any,

}
```

Now, if you were to add a new property, it will not result in error. 
```
const person1: Person = {
  first: "weibo",
  last: "zhang",
  tired: false,
};
```

## Types 
Types are almost the same as Interface. The only difference is in the syntax and that interface only accepts **objects**. Types however can be used for other types such as: primitives, unions, and tuples. 

Read this Stackoverflow for more information on the difference between types and interface: https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types;

To create a type: 
```
type Point = {
  x: number;
  y: number;
};
```

From the Stack Overflow link, to create other types, you will write: 
```
// primitive
type Name = string;

// object
type PartialPointX = { x: number; };
type PartialPointY = { y: number; };

// union
type PartialPoint = PartialPointX | PartialPointY;

// tuple
type Data = [number, string];
```

## Extending Interfaces and Types
Extending other interfaces and types mean that you inherit all the properties specified while allowing you to make changes, add, or delete properties from them. 

### Interface
To extend a interface, you will write
```
// you can extend **Types** the same way. The **Person** can be created by **Type** instead and it will work.
interface extendPerson extends Person {
   location: string,
}
```

The ```extendPerson``` will now become
```
{
 first: string,
 last: string,
 [key: string]: any,
 location: string,
}
```

### Types
To extend in Type, you will use the syntax style below.
```
type typeExtendPerson = Person & {
  [key: string]: any,
}
```


## Functions
## Array
## Anonymous Interface




