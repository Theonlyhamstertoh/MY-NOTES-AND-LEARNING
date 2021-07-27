# Jotai
Jotai, a atom based state managment system. You know how in Zustand, you had only giant state managament system? Now with Jotai, you use what are called atoms which acts like a useState but can be accessed in any function by calling the `useAtoms` hook. 

## Basic Example
For example, say you want to create a counter. The following below will use the `countAtom` state and make changes inside of it. This way, you can have multiple different components but can have shared states simply by calling `useAtom(countAtom)`. Now, if you want to have different states, simply create another atom. 
```
const countAtom = atom(0)

function Counter() {
  const [count, setCount] = useAtom(countAtom);
  return (
    <div>
      <input type="button" value="+" onClick={() => setCount((state) => state + 1)} />
      <span>{count}</span>
    </div>
  );
}
```

## Naming doesn't matter
Another way to share state with two different variable name is just like how you would normally do it in javascript. If you console.log `newCount`, it will show the `countAtom` values. 
``` 
const countAtom = atom(0)
const newCount = countAtom;
```
## Creating read-only atoms
Let's say, you have a atom that stores all the users you have online and now you want to read the data. One way to go about doing it is to simply attach a `length` to the atom. Another way is to create a special atom that only reads the value. For example, take a look at the following below. We use a special (get) atom that only returns the read value of the atom. 

```
const dotsAtom = atom([]);
const numberOfDotsAtom = atom((get) => get(dotsAtom).length);
```

