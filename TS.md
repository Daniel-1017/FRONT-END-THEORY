# TYPESCRIPT

## Getting Started

TypeScript is a JavaScript superset. ( a programming language built up on JavaScript )

1. #### What are TypeScript advantages?
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

## Classes and Interfaces

_Classes_<br>
Classes are blueprints for objects, they allow us to define how a object should look like.

```ts
class User {
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  printUserInfo() {
    console.log(this.firstName, this.lastName);
  }
}
```

Classes are compiled do constructor functions behind de scenes.

_this keyword_<br>
The `this` typically refers back to the concrete instance of the class.

```ts
printUserInfo() {
    console.log(this.firstName, this.lastName);
}
```

_`private` and `public` access modifiers_<br>

```ts
class User {
  private password: string = "ed2612e4-5c96-4c15-af47-db0bf6ee12c3";
  // public is the default, available outside

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}
```

_Shorthand initialization_<br>

```ts
class User {
  private readonly id: string = "d6140cfd-7a9c-4c46-a097-90d8ae344cd7";

  constructor(public firstName: string, public lastName: string) {}
}
```

_`readonly` properties_<br>

```ts
class User {
  private readonly id: string = "d6140cfd-7a9c-4c46-a097-90d8ae344cd7";

  constructor(public firstName: string, public lastName: string) {}
}
```

With `readonly` should be pretty clear that a property should be initialized once and shouldn’t change thereafter.

_Inheritance_<br>
Allows to inherit methods and properties from the parent class ( super class / base class )

```ts
class Department {
  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {}
}

class ITDepartment extends Department {
  constructor(id: string, public admins: string[]) {
    super(id, "IT"); // Calls the constructor of the parent class
  }
}

class Accounting extends Department {
  constructor(id: string, private reports: string[]) {
    super(id, "Accounting"); // Calls the constructor of the parent class
  }

  addReport(text: string) {
    this.reports.push(text);
  }
}
```

_`protected` modifier_<br>
Doesn’t allow to access property from outside but allows to access properties in child classes.

```ts
class Department {
  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {}
}
```

_`getters` and `setters`_<br>
Can be great for encapsulating logic and for adding extra logic when you try to read a property or when you try to set a property.

```ts
class Accounting extends Department {
  private lastReport: string;

  // GETTER
  get mostRecentReport() {
    if (this.lastReport) return this.lastReport;
    throw new Error("No report found!");
  }

  // SETTER
  set mostRecentReport(value: string) {
    if (!value) throw new Error("Please pass in a valid value!");
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
    this.lastReport = reports[0];
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }
}
```

_`static` methods/properties_<br>
Static methods a available directly on the class not on the instance. Used for utility functions that you want to group to a class or global constants that you want to store in a class.
For example: `Math`

```ts
class Department {
  // STATIC PROPERTY
  static fiscalYear = 2020;

  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {}

  // STATIC METHOD
  static createEmployee(name: string) {
    return { name: name };
  }
}
```

_`abstract` classes_<br>
When you want to force developers working with a certain class to implement/overwrite a certain method or whenever you want to ensure that a certain method is available in all classes based on some base class but when you also know that the exact implementation will depend on the specific version.

```ts
abstract class Department {
  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {}

  abstract describe(): void;
}
```

Can’t be instantiated, its just a class that is there to be inherited from. The inheriting classes are forced to provide concrete implementation for the methods.

_Singletons and `private` constructors_<br>
The `singletons` pattern is about ensuring that you always only have exactly one instance of a certain class. Useful when you can’t use `static` methods/properties or when you want to make sure that you can’t create multiple objects based on a class but you always have one object based on a class.

```ts
class AccountingDepartment extends Department {
  private static instance: AccountingDepartment;

  private constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
  }

  static getInstance() {
    if (this.instance) {
      return this.instance;
    }
    this.instance = new AccountingDepartment("id", []);
    return this.instance;
  }
}
```
