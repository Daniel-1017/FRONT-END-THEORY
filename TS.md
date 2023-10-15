# TYPESCRIPT

## Getting Started

- TypeScript is a JavaScript superset. ( a programming language built up on JavaScript )

2. #### What are TypeScript advantages?
   - Types
   - Next-Gen JavaScript features
   - Nan-JavaScript features _interfaces_ and _generics_
   - Meta-Programming features like _decorators_
   - Rich configuration object
   - Modern tooling that helps even in Non-TypeScript projects

## TypeScript Basics & Basic Types

1.  ##### Core Types

        - number **all numbers, no difference between integers or floats**
        - string **all text**
        - boolean **truthy or falsy values**
        - object **any JavaScript object**
        - array **any JavaScript array, type can be flexible or strict ( regarding the element types )**
        - tuple **added by TypeScript: fixed-length and fixed-type array**
        - enum **added by TypeScript: automatically enumerated global constant identifiers**
        - any **any kind of value, no specific type assignment**

2.  #### Union Types

    - use union types to be more flexible regarding what you do in your code

3.  #### Type Alias
    - use to save code and write code quicker

```ts
type Combinable = number | string;
```

4. #### Function return type & "void"
   - it is a good idea to let TypeScript infer the type if there is no good reason to set it manually
   - use void when the function doesn't return

```ts
const add = (n1: number, n2: number): number => {
  return n1 + n2;
};

const print = (value: string): void => {
  console.log(value);
};
```

5. #### Functions as types
   - allow us to describe which type of function we want to use

```ts
let combineValue = (a: number, b: number) => number;
```

6. #### Unknown type
   - use when you don't know what type of value will be stored but you know what you want to do with it eventually, just add an extra check to make sure that what you want to do can be done

```ts
let userInput: unknown;
let userName: string;

userInput = 5;
userInput = "Max";

if (typeof userInput === "string") {
  userName = userInput;
}
```

7. #### Never types
   - use when function never produces a return value

```ts
const generateError = (message: string, code: number): never => {
  throw { message, errorCode: code };
};
```
