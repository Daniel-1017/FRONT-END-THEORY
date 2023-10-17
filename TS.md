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
type Combinable = number | string
```

4. #### Function return type & "void"
   - it is a good idea to let TypeScript infer the type if there is no good reason to set it manually
   - use void when the function doesn't return

```ts
const add = (n1: number, n2: number): number => {
  return n1 + n2
}

const print = (value: string): void => {
  console.log(value)
}
```

5. #### Functions as types
   - allow us to describe which type of function we want to use

```ts
let combineValue = (a: number, b: number) => number
```

6. #### Unknown type
   - use when you don't know what type of value will be stored but you know what you want to do with it eventually, just add an extra check to make sure that what you want to do can be done

```ts
let userInput: unknown
let userName: string

userInput = 5
userInput = "Max"

if (typeof userInput === "string") {
  userName = userInput
}
```

7. #### Never types
   - use when function never produces a return value

```ts
const generateError = (message: string, code: number): never => {
  throw { message, errorCode: code }
}
```

## Classes and Interfaces

### Classes

1. Classes

- Classes are blueprints for objects, they allow us to define how a object should look like.
- Classes are compiled do constructor functions behind de scenes.

```ts
class User {
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
  }

  printUserInfo() {
    console.log(this.firstName, this.lastName)
  }
}
```

2. this keyword

- The `this` typically refers back to the concrete instance of the class.

```ts
printUserInfo() {
    console.log(this.firstName, this.lastName);
}
```

3. `private` and `public` access modifiers

```ts
class User {
  private password: string = "ed2612e4-5c96-4c15-af47-db0bf6ee12c3"
  // public is the default, available outside

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
  }
}
```

4. Shorthand initialization

```ts
class User {
  private readonly id: string = "d6140cfd-7a9c-4c46-a097-90d8ae344cd7"

  constructor(public firstName: string, public lastName: string) {}
}
```

5. `readonly` properties

- With `readonly` should be pretty clear that a property should be initialized once and shouldn’t change thereafter.

```ts
class User {
  private readonly id: string = "d6140cfd-7a9c-4c46-a097-90d8ae344cd7"

  constructor(public firstName: string, public lastName: string) {}
}
```

6. Inheritance

- Allows to inherit methods and properties from the parent class ( super class / base class )

```ts
class Department {
  protected employees: string[] = []

  constructor(private readonly id: string, public name: string) {}
}

class ITDepartment extends Department {
  constructor(id: string, public admins: string[]) {
    super(id, "IT") // Calls the constructor of the parent class
  }
}

class Accounting extends Department {
  constructor(id: string, private reports: string[]) {
    super(id, "Accounting") // Calls the constructor of the parent class
  }

  addReport(text: string) {
    this.reports.push(text)
  }
}
```

7. `protected` modifier

- Doesn’t allow to access property from outside but allows to access properties in child classes.

```ts
class Department {
  protected employees: string[] = []

  constructor(private readonly id: string, public name: string) {}
}
```

8. `getters` and `setters`

- Can be great for encapsulating logic and for adding extra logic when you try to read a property or when you try to set a property.

```ts
class Accounting extends Department {
  private lastReport: string

  // GETTER
  get mostRecentReport() {
    if (this.lastReport) return this.lastReport
    throw new Error("No report found!")
  }

  // SETTER
  set mostRecentReport(value: string) {
    if (!value) throw new Error("Please pass in a valid value!")
    this.addReport(value)
  }

  constructor(id: string, private reports: string[]) {
    super(id, "Accounting")
    this.lastReport = reports[0]
  }

  addReport(text: string) {
    this.reports.push(text)
    this.lastReport = text
  }
}
```

9. `static` methods/properties

- Static methods a available directly on the class not on the instance. Used for utility functions that you want to group to a class or global constants that you want to store in a class.
- For example: `Math`

```ts
class Department {
  // STATIC PROPERTY
  static fiscalYear = 2020

  protected employees: string[] = []

  constructor(private readonly id: string, public name: string) {}

  // STATIC METHOD
  static createEmployee(name: string) {
    return { name: name }
  }
}
```

10. `abstract` classes

- When you want to force developers working with a certain class to implement/overwrite a certain method or whenever you want to ensure that a certain method is available in all classes based on some base class but when you also know that the exact implementation will depend on the specific version.
- Can’t be instantiated, its just a class that is there to be inherited from. The inheriting classes are forced to provide concrete implementation for the methods.

```ts
abstract class Department {
  protected employees: string[] = []

  constructor(private readonly id: string, public name: string) {}

  abstract describe(): void
}
```

11. Singletons and `private` constructors

- The `singletons` pattern is about ensuring that you always only have exactly one instance of a certain class. Useful when you can’t use `static` methods/properties or when you want to make sure that you can’t create multiple objects based on a class but you always have one object based on a class.

```ts
class AccountingDepartment extends Department {
  private static instance: AccountingDepartment

  private constructor(id: string, private reports: string[]) {
    super(id, "Accounting")
  }

  static getInstance() {
    if (this.instance) {
      return this.instance
    }
    this.instance = new AccountingDepartment("id", [])
    return this.instance
  }
}
```

### Interfaces

1. _Interface_

- An interface describes the structure of an object. Interfaces are often used to share functionality amongst different classes not regarding their concrete implementation but regarding the structure a class should have. Interfaces are not compiled to JavaScript. Available only during development in TypeScript.

```ts
interface User {
  firstName: string
  middleName?: string
  lastName: string
  age: number

  greet(phrase: string): void
}
```

2. _Using `interfaces` with `classes`_

```ts
interface Greetable {
  name: string
  greet(phrase: string): void
}

class Person implements Greetable {
  constructor(public name: string) {}

  greet(phrase: string) {
    console.log(`${phrase}, ${this.name}`)
  }
}
```

3. _Extending `interfaces`_

```ts
interface Named {
  readonly name: string
}

interface Greetable extends Named {
  greet(phrase: string): void
}

class Person implements Greetable {
  constructor(public name: string) {}

  greet(phrase: string) {
    console.log(`${phrase}, ${this.name}`)
  }
}
```

4. _`interfaces` as function types_

```ts
type AddFn (a: number, b:number) => number

interface AddFn {
	(a: number, b: number): number
}

let add: AddFn
add = (n1: number, n2: number) => n1 + n2
```

## Advanced Types

1. Intersection types

- When you want ta combination of two types / interfaces.

```ts
type Admin = {
  name: string
  privileges: string[]
}

type Employee = {
  name: string
  startDate: Date
}

type ElevatedEmployee = Admin & Employee
```

2. Type Guards

```ts
type UnknownEmployee = Employee | Admin

function printEmployeeInformation(emp: UnknownEmployee) {
  console.log(`Name: ${emp.name}`)
  // TYPE GUARD
  if ("privileges" in emp) {
    console.log(`Privileges: ${emp.privileges}`)
  }
}

// OR

// When working with classes
class Car {
  drive() {
    console.log("Driving...")
  }
}

class Truck {
  drive() {
    console.log("Driving a truck...")
  }

  loadCargo(amount: number) {
    console.log("Loading cargo... " + amount)
  }
}

type Vehicle = Car | Truck

function useVehicle(vehicle: Vehicle) {
  vehicle.drive()
  // TYPE GUARD
  if (vehicle instanceof Truck) {
    vehicle.loadCargo(10)
  }
}
```

3. Descriminated union

- This is a discriminated union because we have a common property in every object that makes up our union which describes that object.

```ts
interface Bird {
  type: "bird"
  flyingSpeed: number
}

interface Horse {
  type: "horse"
  runningSpeed: number
}

type Animal = Bird | Horse

function moveAnimal(animal: Animal) {
  let speed
  switch (animal.type) {
    case "bird":
      speed = animal.flyingSpeed
      break
    case "horse":
      speed = animal.runningSpeed
      break
  }

  console.log(`Moving at speed ${speed}`)
}
```

4. Type Casting

```ts
const paragraph = <HTMLParagraphElement>document.getElementById("paragraph")!
// OR
const paragraph2 = document.getElementById("paragraph")! as HTMLParagraphElement
```

5. Index Properties

- Useful when we don’t know the name of the properties and how many properties we will have.

```ts
interface ErrorContainer {
  [prop: string]: string
}

const errorBag: ErrorContainer = {
  email: "Not a valid email!",
  message: "Must start with a capital character!",
}
```

6. Function Overloads

- You can have multiple functions with the same name but different parameter types and return type. However the number of parameters should be the same.

```ts
function add(a: number, b: number): number // Overload 1
function add(a: string, b: string): string // Overload 2
function add(a: Combinable, b: Combinable) {
  if (typeof a === "string" || typeof b === "string") {
    return a.toString() + b.toString()
  }
  return a + b
}
```

## Generics

1. Generic

- Generics enable you to write code that can work with different data types while preserving type information. They give us flexibility combined with type safety. We are flexible regarding the values we are passing in and we got full type support for what we then want to do with the class or with the result of a generic function.

2. Generic function

- In this example, `identity` is a generic function that takes a type parameter `T`. The type parameter `T` is a placeholder for the actual type that will be provided when the function is called. You can specify the type argument explicitly, as shown in the usage example, or let TypeScript infer it from the provided argument.

```ts
function identity<T>(arg: T): T {
  return arg
}

// Example
const result = identity<string>("Hello, TypeScript")
```

3. Constraints

- Constrains enable you to restrict the types of your `generic` types. Use `extends` keyword. Constrains allows us to narrow down the concrete types that may be used in a generic function.

```ts
function merge<T extends object, U extends object>(objA: T, objB: U) {
  return Object.assign(objA, objB)
}
```

4. `keyof` constraint

- We are saying the `T` should be an object and `U` should be any `key` in that object.

```ts
function extractAndConvert<T extends object, U extends keyof T>(
  obj: T,
  key: U
) {
  return `Value: ${obj[key]}`
}
```

5. Generic classes

```ts
class DataStorage<T> {
  private data: T[] = []

  addItem(item: T) {
    this.data.push(item)
  }

  removeItem(item: T) {
    this.data.splice(this.data.indexOf(item), 1)
  }

  getItems() {
    return [...this.data]
  }
}

// Example with strings
const textStorage = new DataStorage<string>()
textStorage.addItem("Max")
textStorage.addItem("Jack")
textStorage.removeItem("Manu")

// Example with numbers
const numberStorage = new DataStorage<number>()
numberStorage.addItem(10)
numberStorage.addItem(20)
numberStorage.removeItem(20)
```

6. Utility types

```ts
// Partial
interface Person {
  name: string
  age: number
  email: string
}

// Create a type representing a partial Person
type PartialPerson = Partial<Person>

// Usage
const partialPerson: PartialPerson = {
  name: "Alice",
  // Other properties are optional
}

// Readonly
const names: Readonly<string[]> = ["Max", "Jack"]
// names.push("Anna") is allowd but TypeScript will show an error
// because of Readonly
```

7. Generic Types vs Union Types

- With generic types we are saying that you got to choose once and then you are only allowed to add that exact type of data. Generic types are great if you want to lock in a certain type. Use the same type throughout the entire `class`/`function`.
- With union types we are saying that the storage can have different types mixed. Are great when you want to have a function which you can call with on of the types available every time you call it.

```ts
// Generic Types
class DataStorage<T extends string | number | boolean> {
  private data: T[] = []
}

// Union Types
class DataStorage {
  private data: (string | number | boolean)[] = []
}
```

## Decorators

1. Decorators

- A `decorator` is an instrument to write code which is then easier to use by other developer. Decorators execute when your class is defined not when it is instantiated.

2. Class decorators

```ts
function Logger(constructor: Function) {
  console.log("Logging...")
  console.log(constructor)
}

@Logger
class Person {
  name = "Max"

  constructor() {
    console.log("Creating person object...")
  }
}
```

3. Decorators factory

- With factories we can pass arguments which will be used by that inner decorator function.

```ts
function Logger(logString: string) {
  return function (constructor: Function) {
    console.log(logString)
    console.log(constructor)
  }
}

@Logger("LOGGING - PERSON")
class Person {
  name = "Max"

  constructor() {
    console.log("Creating person object...")
  }
}
```

4. More advanced decorators

```ts
function WithTemplate(template: string, hookId: string) {
  return function (constructor: any) {
    const hookEl = document.getElementById(hookId)

    const p = new constructor()

    if (hookEl) {
      hookEl.innerHTML = template
      hookEl.querySelector("h1")!.textContent = p.name
    }
  }
}

@WithTemplate("<h1>My Person Object</h1>", "app")
class Person {
  name = "Max"

  constructor() {
    console.log("Creating person object...")
  }
}
```

5. Adding multiple decorators

- The creation of the actual decorator functions happens in the order in which we specify the factory functions but the execution of the decorator functions then happens bottom up.

```ts
function Logger(logString: string) {
  return function (constructor: Function) {
    console.log(logString)
    console.log(constructor)
  }
}

function WithTemplate(template: string, hookId: string) {
  return function (constructor: any) {
    const hookEl = document.getElementById(hookId)

    const p = new constructor()

    if (hookEl) {
      hookEl.innerHTML = template
      hookEl.querySelector("h1")!.textContent = p.name
    }
  }
}

@Logger("LOGGING")
@WithTemplate("<h1>My Person Object</h1>", "app")
class Person {
  name = "Max"

  constructor() {
    console.log("Creating person object...")
  }
}
```

6. Accessor decorator

- On a accessor decorator we can return a new property descriptor.

```ts
function Log(target: any, name: string, descriptor: PropertyDescriptor) {
  console.log("Accessor Decorator")
  console.log(target)
  console.log(name)
  console.log(descriptor)
}

class Product {
  title: string
  private _price: number

  @Log
  set price(val: number) {
    if (val > 0) {
      this._price = val
    } else {
      throw new Error("Invalid price - should be positive!")
    }
  }

  constructor(t: string, p: number) {
    this.title = t
    this._price = p
  }
}
```

7. Property decorator

- Decorators on properties can return but TypeScript will ignore it.

```ts
function Log(target: any, propertyName: string | Symbol) {
  console.log("Property Decorator")
  console.log(target, propertyName)
}

class Product {
  @Log
  title: string
  private _price: numbe

  constructor(t: string, p: number) {
    this.title = t
    this._price = p
  }
}
```

8. Method decorator

- On a method decorator we can return a new property descriptor.

```ts
function Log(
  target: any,
  name: string | Symbol,
  descriptor: PropertyDescriptor
) {
  console.log("Method Decorator")
  console.log(target)
  console.log(name)
  console.log(descriptor)
}

class Product {
  title: string
  private _price: number

  constructor(t: string, p: number) {
    this.title = t
    this._price = p
  }

  @Log
  getPriceWithTax(tax: number) {
    return this._price * (1 + tax)
  }
}
```

9. Parameter decorator

- Decorators on parameters can return but TypeScript will ignore it.

```ts
function Log(target: any, name: string | Symbol, position: number) {
  console.log("Parameter Decorator")
  console.log(target)
  console.log(name)
  console.log(position)
}

class Product {
  title: string
  private _price: number

  constructor(t: string, p: number) {
    this.title = t
    this._price = p
  }

  getPriceWithTax(@Log tax: number) {
    return this._price * (1 + tax)
  }
}
```

10. Returning and chaining a `class` in a `class` decorator

- Now we are able to add logic that does not run when the class is defined but when the class is instantiated.

```ts
function WithTemplate(template: string, hookId: string) {
  return function <T extends { new (...args: any[]): { name: string } }>(
    originalConstructor: T
  ) {
    return class extends originalConstructor {
      constructor(..._: any[]) {
        super()
        const hookEl = document.getElementById(hookId)
        if (hookEl) {
          hookEl.innerHTML = template
          hookEl.querySelector("h1")!.textContent = this.name
        }
      }
    }
  }
}
```

11. Auto bind decorator

```ts
function Autobind(_: any, _2: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value
  const adjustedDescriptor: PropertyDescriptor = {
    configurable: true,
    enumerable: false,
    get() {
      const boundFn = originalMethod.bind(this)
      return boundFn
    },
  }
  return adjustedDescriptor
}
```
