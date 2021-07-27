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


