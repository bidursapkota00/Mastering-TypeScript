# TypeScript Complete Guide

![Bidur Sapkota](https://www.bidursapkota.com.np/_next/image?url=%2Fimages%2Fprofile3.png&w=48&q=75 "Bidur Sapkota - Developer")&nbsp;[Bidur Sapkota](https://www.bidursapkota.com.np/)

![TypeScript Complete Guide by Bidur Sapkota](/5-ts-post.jpg "TypeScript Complete Guide – Blog by Bidur Sapkota")

## Table of Contents

1. [Installation](#installation)
2. [Why Types?](#why-types)
3. [Primitive Types](#primitive-types)
4. [Object Types](#object-types)
5. [Variables and Type Annotations](#variables-and-type-annotations)
6. [Type Inference](#type-inference)
7. [Functions](#functions)
8. [Practice Exercises - Functions](#practice-exercises---functions)
9. [Quiz - Variables and Functions](#quiz---variables-and-functions)
10. [Objects in TypeScript](#objects-in-typeScript)
11. [Arrays in TypeScript](#arrays-in-typeScript)
12. [Union Types](#union-types)
13. [Narrowing the Type with typeof](#narrowing-the-type-with-typeof)
14. [Type Assertions](#type-assertions)
15. [Literal Types](#literal-types)
16. [Exercises](#exercises)
17. [Tuples](#tuples)
18. [Enums](#enums)
19. [Interfaces](#interfaces)
20. [Generics](#generics)
21. [Type Narrowing](#type-narrowing)
22. [Index Signatures](#index-signatures)
23. [TypeScript Classes](#typescript-classes)

## Installation

You can use npm to install TypeScript globally, this means that you can use the `tsc` command anywhere in your terminal.

To do this, run `npm install -g typescript`. This will install the latest version.

```bash
npm install -g typescript
```

##### Verify installation

```bash
tsc -v
```

##### Compile Typescript to Javascript

```bash
tsc file.ts
tsc    # compiles all ts files including in nested dir
```

##### Run Compiled Javascript

```bash
node file.js
```

##### Compile Typescript to Javascript in watch mode

```bash
tsc -w file.ts
tsc -w    # watches every ts file
```

##### Generate tsconfig.json

```bash
tsc --init
```

##### Some options for updating tsconfig file

```json
{
  "compilerOptions": {
    "target": "ES2020",
    // "lib": [], list of types for DOM, ES2020; better remain commented and just control by target
    "module": "commonjs", // ES5 or something from browser, commonjs is for node
    "outDir": "./dist",
    "rootDir": "./src",
    "noEmitOnError": true,
    "strict": true,
    // "strictNullChecks": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "src/tests/**/*"]
}
```

##### Side Note: "use strict" directive in JavaScript

- Enables strict mode. Eg:

```js
"use strict";
apple = 5; // throws error since apple not defined yet
console.log(apple);
```

## Why Types?

TypeScript brings static typing to JavaScript, providing several key benefits:

- **Error Detection**: Helps us find errors during development
- **Code Analysis**: Analyzes our code as we type
- **Development Tool**: Only exists in development - compiles to regular JavaScript

Types help catch common mistakes before runtime, making your code more reliable and maintainable.

## Type System Overview

TypeScript has two main categories of types:

### Primitive Types

- `number` - All numeric values
- `string` - Text values
- `boolean` - True/false values
- `null` - Intentional absence of value
- `undefined` - Uninitialized value
- `void` - No return value
- `any` - Any type (escape hatch)
- `never` - Values that never occur
- `unknown` - Type-safe any

### Object Types

- `object` - General object type
- `Array` - Collections of values
- `Function` - Function types
- `Tuple` - Fixed-length arrays
- `Enum` - Named constants

## Primitive Types

### Strings

Strings represent character values and text.

```typescript
// Explicit type annotation
let movieTitle: string = "Amadeus";
movieTitle = "Arrival";
movieTitle = 9; // Error: Type 'number' is not assignable to type 'string'

// String methods work as expected
movieTitle.toUpperCase(); // "ARRIVAL"
```

### Numbers

In TypeScript (like JavaScript), all numbers are just numbers or floating-point values.

```typescript
// Number with explicit annotation
let numCatLives: number = 9;
numCatLives += 1; // 10
numCatLives = "zero"; // Error: Type 'string' is not assignable to type 'number'
```

### Booleans

Boolean values represent simple true/false states.

```typescript
// Explicitly typed boolean
let gameOver: boolean = false;
gameOver = true; // ✅ Valid
gameOver = "true"; // Error: Type 'string' is not assignable to type 'boolean'
```

### Any Type

The `any` type is an escape hatch that disables type checking.

```typescript
let thing: any = "hello";
thing = 1; // No error
thing = false; // No error
thing(); // No error (but might fail at runtime)
thing.toUpperCase(); // No error (but might fail at runtime)
```

**Note**: Use `any` sparingly as it defeats the purpose of TypeScript's type safety.

## Variables and Type Annotations

### Basic Syntax

To assign a type to a variable, use the colon syntax:

```typescript
let myVar: type = value;
```

### Examples

```typescript
let name: string = "Alice";
let age: number = 25;
let isStudent: boolean = true;
```

## Type Inference

TypeScript can automatically infer types based on the assigned values:

```typescript
// TypeScript infers these types automatically
let tvShow = "Olive Kitteridge"; // inferred as string
tvShow = "The Other Two"; // ✅ Valid
tvShow = false; // Error: Type 'boolean' is not assignable to type 'string'

let isFunny = false; // inferred as boolean
isFunny = true; // ✅ Valid
isFunny = "maybe"; // Error: Type 'string' is not assignable to type 'boolean'
```

## Functions

### Parameter Types

You can specify types for function parameters:

```typescript
function greet(person: string) {
  return `Hi, ${person}!`;
}

// Multiple parameters with different types
const doSomething = (person: string, age: number, isFunny: boolean) => {
  console.log(`${person} is ${age} years old`);
};
```

### Return Types

Specify what type a function returns:

```typescript
// Explicit return type annotation
function square(num: number): number {
  return num * num;
}

// Arrow function with return type
const add = (x: number, y: number): number => {
  return x + y;
};

// Function with default parameter
function greet(person: string = "stranger"): string {
  return `Hi there, ${person}!`;
}
```

### Anonymous Functions and Contextual Typing

TypeScript can infer parameter types for anonymous functions based on context:

```typescript
const colors = ["red", "orange", "yellow"];

// TypeScript knows 'color' is a string based on the array context
colors.map((color) => {
  return color.toUpperCase(); // String methods available
});
```

### Void Type

Functions that don't return anything have a `void` return type:

```typescript
function printTwice(msg: string): void {
  console.log(msg);
  console.log(msg);
  // No return statement needed
}
```

### Never Type

The `never` type represents values that never occur:

```typescript
// Function that always throws an error
function makeError(msg: string): never {
  throw new Error(msg);
}

// Function that never finishes executing
function gameLoop(): never {
  while (true) {
    console.log("GAME LOOP RUNNING!");
  }
}
```

**Key Difference**: `void` functions complete execution but return nothing, while `never` functions never complete execution.

## Practice Exercises - Functions

### Exercise 1: Two For One

Write a function that creates a sharing message:

- Write a function called "twoFer" that accepts a person's name
- It should return a string in the format "one for <name>, one for me"
- If no name is provided, it should default to "you"
- twoFer() => "One for you, one for me"
- twoFer("Elton") => "One for Elton, one for me"

```typescript
function twoFer(person: string = "you"): string {
  return `One for ${person}, one for me.`;
}

// Usage examples:
console.log(twoFer()); // "One for you, one for me."
console.log(twoFer("Elvis")); // "One for Elvis, one for me."
```

### Exercise 2: Leap Year Calculator

Determine if a year is a leap year using these rules:

- Year is divisible by 4 AND not divisible by 100, OR
- Year is divisible by 400

```typescript
const isLeapYear = (year: number): boolean => {
  return (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0;
};

// Test cases:
console.log(isLeapYear(2012)); // true (divisible by 4, not by 100)
console.log(isLeapYear(2013)); // false (not divisible by 4)
console.log(isLeapYear(1900)); // false (divisible by 100, not by 400)
console.log(isLeapYear(2000)); // true (divisible by 400)
```

## Quiz - Variables and Functions

[Variables Quiz Link](https://bidursapkota00.github.io/Mastering-TypeScript/03-variables/3.1%20Super%20Quick%20Quiz!.html "Variables Quiz Link")

[Inference Quiz Link](https://bidursapkota00.github.io/Mastering-TypeScript/03-variables/8.2%20Inference%20Quiz.html "Inference Quiz Link")

[Function Parameter Quiz Link](https://bidursapkota00.github.io/Mastering-TypeScript/04-functions/2.3%20Function%20Parameter%20Quiz.html "[Function Parameter Quiz Link")

[Return Type Quiz Link](https://bidursapkota00.github.io/Mastering-TypeScript/04-functions/8.4%20Return%20Type%20Quiz.html "[Return Type Quiz Link")

## Objects in TypeScript

### Basic Object Types

Objects in TypeScript can be typed by declaring what the object should look like in the type annotation. This ensures type safety and prevents accessing undefined properties or performing invalid operations.

**Key Points:**

- Define the exact shape of an object using type annotations
- Accessing undefined properties will throw TypeScript errors
- Operations must respect the defined types

```typescript
// Objects as parameters
function printName(person: { first: string; last: string }): void {
  console.log(`${person.first} ${person.last}`);
}

printName({ first: "Thomas", last: "Jenkins" });

const singer = { first: "Mick", last: "Jagger", age: 473, isAlive: true };
printName(singer); // Works - contains required properties
```

### Type Aliases

Instead of writing object types directly in annotations, you can declare them separately using type aliases. This makes code more readable and allows type reuse.

**Benefits:**

- Improved code readability
- Type reusability across your codebase
- Easier maintenance and updates

```typescript
// Without type alias (commented out for comparison)
// let coordinate: { x: number; y: number } = { x: 34, y: 2 };
// function randomCoordinate(): { x: number; y: number } {
//   return { x: Math.random(), y: Math.random() };
// }

// With type alias - much cleaner!
type Point = {
  x: number;
  y: number;
};

let coordinate: Point = { x: 34, y: 2 };

function randomCoordinate(): Point {
  return { x: Math.random(), y: Math.random() };
}

function doublePoint(point: Point): Point {
  return { x: point.x * 2, y: point.y * 2 };
}
```

### Nested Objects

Objects can contain other objects as properties, creating nested structures that TypeScript can type-check at multiple levels.

```typescript
type Song = {
  title: string;
  artist: string;
  numStreams: number;
  credits: { producer: string; writer: string }; // Nested object
};

function calculatePayout(song: Song): number {
  return song.numStreams * 0.0033;
}

const mySong: Song = {
  title: "Unchained Melody",
  artist: "Righteous Brothers",
  numStreams: 12873321,
  credits: {
    producer: "Phil Spector",
    writer: "Alex North",
  },
};

console.log(calculatePayout(mySong)); // 42522.1593
```

### Optional Properties

Use the `?` symbol to make object properties optional.

```typescript
type Point = {
  x: number;
  y: number;
  z?: number; // Optional property
};

const myPoint: Point = { x: 1, y: 3 }; // Valid without z
```

### Readonly Properties

Use the `readonly` modifier to prevent property modification after object creation.

```typescript
type User = {
  readonly id: number; // Cannot be changed after creation
  username: string;
};

const user: User = {
  id: 12837,
  username: "catgurl",
};

// user.id = 999; // Error! Cannot assign to readonly property
```

### Intersection Types

Combine multiple types using the `&` operator to create intersection types.

```typescript
type Circle = {
  radius: number;
};

type Colorful = {
  color: string;
};

type ColorfulCircle = Circle & Colorful;

const happyFace: ColorfulCircle = {
  radius: 4,
  color: "yellow",
};

// Complex intersection with inline properties
type Cat = { numLives: number };
type Dog = { breed: string };

type CatDog = Cat &
  Dog & {
    age: number;
  };

const christy: CatDog = {
  numLives: 7,
  breed: "Husky",
  age: 9,
};
```

### Type Manipulation Utilities

```ts
// Original User type
type User = {
  id: number;
  name: string;
  email: string;
  age: number;
  isActive: boolean;
};

// 1. Partial<T> - Makes all properties optional
type PartialUser = Partial<User>;
// Result: { id?: number; name?: string; email?: string; age?: number; isActive?: boolean; }

const updateUser: PartialUser = {
  name: "John", // Only updating name
  age: 25, // Only updating age
  // Other properties are optional, so we don't need them
};

// 2. Required<T> - Makes all properties required (opposite of Partial)
type RequiredUser = Required<PartialUser>;
// Result: { id: number; name: string; email: string; age: number; isActive: boolean; }

const completeUser: RequiredUser = {
  id: 1,
  name: "John",
  email: "john@example.com",
  age: 25,
  isActive: true,
  // All properties are required now
};

// 3. Omit<T, K> - Excludes specific properties
type UserWithoutAge = Omit<User, "age">;
// Result: { id: number; name: string; email: string; isActive: boolean; }

const userNoAge: UserWithoutAge = {
  id: 1,
  name: "Jane",
  email: "jane@example.com",
  isActive: true,
  // 'age' property is not allowed here
};

// You can omit multiple properties
type UserBasic = Omit<User, "age" | "isActive">;
// Result: { id: number; name: string; email: string; }

const basicUser: UserBasic = {
  id: 1,
  name: "Bob",
  email: "bob@example.com",
  // Neither 'age' nor 'isActive' are allowed
};

// 4. Pick<T, K> - Selects only specific properties
type UserNameOnly = Pick<User, "name">;
// Result: { name: string; }

const nameOnly: UserNameOnly = {
  name: "Alice",
  // Only 'name' property is allowed
};

// Pick multiple properties
type UserIdAndName = Pick<User, "id" | "name">;
// Result: { id: number; name: string; }

const idAndName: UserIdAndName = {
  id: 1,
  name: "Charlie",
  // Only 'id' and 'name' are allowed
};

// Real-world usage examples:

// API response that might have missing fields
function updateUserProfile(userId: number, updates: PartialUser): void {
  // Can update any combination of user fields
  console.log(`Updating user ${userId} with:`, updates);
}

updateUserProfile(1, { name: "New Name" }); // Valid
updateUserProfile(1, { email: "new@email.com", age: 30 }); // Valid

// Form data that doesn't need sensitive info
function displayUserCard(user: Omit<User, "id" | "email">): string {
  return `${user.name} (${user.age} years old) - ${
    user.isActive ? "Active" : "Inactive"
  }`;
}

// Search function that only needs name
function searchUserByName(criteria: Pick<User, "name">): User[] {
  // Implementation would search by name only
  return [];
}

searchUserByName({ name: "John" }); // Valid
// searchUserByName({ name: "John", age: 25 }); // Error: 'age' not allowed
```

### Exercise 1

- Write the Movie type alias to make the following two variables properly typed
- Make sure that "originalTitle" is optional and "title" is readonly

```ts
const dune: Movie = {
  title: "Dune",
  originalTitle: "Dune Part One",
  director: "Denis Villeneuve",
  releaseYear: 2021,
  boxOffice: {
    budget: 165000000,
    grossUS: 108327830,
    grossWorldwide: 400671789,
  },
};

const cats: Movie = {
  title: "Cats",
  director: "Tom Hooper",
  releaseYear: 2019,
  boxOffice: {
    budget: 95000000,
    grossUS: 27166770,
    grossWorldwide: 73833348,
  },
};
```

### Solution 1

```typescript
type Movie = {
  readonly title: string; // Readonly property
  originalTitle?: string; // Optional property
  director: string;
  releaseYear: number;
  boxOffice: {
    // Nested object
    budget: number;
    grossUS: number;
    grossWorldwide: number;
  };
};
```

### Exercise 2

- Write a function called getProfit that accepts a single Movie object
- It should return the movie's worldwide gross minus its budget

- For example...
- getProfit(cats) => -21166652
- You can apply concept of destructuring too

### Solution 2

```ts
function getProfit({ boxOffice: { grossWorldwide, budget } }: Movie): number {
  return grossWorldwide - budget;
}
```

### Quiz

[Object Type Quiz Link](https://bidursapkota00.github.io/Mastering-TypeScript/05-object-types/4.5%20Object%20Types%20Quiz.html "[Object Type Quiz Link")

## Arrays in TypeScript

### Basic Array Types

Arrays can be typed using a type annotation followed by empty array brackets. Arrays are homogeneous - they only allow data of one specified type.

**Syntax Options:**

- `type[]` - Most common syntax
- `Array<type>` - Generic syntax (alternative)

```typescript
// String array
const activeUsers: string[] = [];
activeUsers.push("Tony");

// Array of numbers
const ageList: number[] = [45, 56, 13];
ageList[0] = 99;

// Alternative generic syntax
const bools: Array<boolean> = []; // Same as boolean[]
```

### Arrays of Custom Types

You can create arrays of your custom types using type aliases.

```typescript
type Point = {
  x: number;
  y: number;
};

const coords: Point[] = [];
coords.push({ x: 23, y: 8 });
```

### Multi-dimensional Arrays

Create arrays of arrays by adding additional bracket pairs.

```typescript
// 2D string array
const board: string[][] = [
  ["X", "O", "X"],
  ["X", "O", "X"],
  ["X", "O", "X"],
];

// Empty 2D array
const gameBoard: string[][] = [];
```

### Working with Array Functions

Arrays work seamlessly with functions, allowing you to process collections of typed data.

```typescript
type Product = {
  name: string;
  price: number;
};

function getTotal(products: Product[]): number {
  let total = 0;
  for (let product of products) {
    total += product.price;
  }
  return total;
}

// Usage
const products: Product[] = [
  { name: "coffee mug", price: 11.5 },
  { name: "notebook", price: 5.25 },
  { name: "pen", price: 2.0 },
];

console.log(getTotal(products)); // 18.75
```

---

## Union Types

Union types allow us to give a value multiple possible types. If the eventual value's type is included in the union, TypeScript will be satisfied. We create union types using the pipe character `|` to separate the types we want to include.

Think of it as saying: **"This thing is allowed to be this, this, or this"**. TypeScript will enforce these constraints throughout your code.

### Basic Example

```typescript
// Basic Union Type
let age: number | string = 21;
age = 23; // Valid - number
age = "24"; // Valid - string
// age = true;   // Error - boolean not in union

type User = {
  firstName: string;
  middleName: string | undefined; // Union with undefined
  lastName: string;
};
```

### Union Types with Type Aliases

```typescript
type Point = {
  x: number;
  y: number;
};

type Location = {
  lat: number;
  long: number;
};

// Union type with custom types
let coordinates: Point | Location = { x: 1, y: 34 };
coordinates = { lat: 321.213, long: 23.334 };
```

### Function Parameters with Union Types

```typescript
function printAge(age: number | string): void {
  console.log(`You are ${age} years old`);
}

printAge(25); // Works with number
printAge("25"); // Works with string
```

### Union Types with Arrays

```typescript
// Array that can contain Point OR Location objects
const coords: (Point | Location)[] = [];
coords.push({ lat: 321.213, long: 23.334 }); // Location
coords.push({ x: 213, y: 43 }); // Point

// Array that is EITHER all numbers OR all strings (not mixed)
const data: number[] | string[] = [1, 2, 3]; // All numbers
// const mixed: number[] | string[] = [1, "2"]; // Mixed not allowed
```

---

## Narrowing the Type with typeof

Narrowing is the process of checking what type a value actually is before working with it. Since union types allow multiple possibilities, it's good practice to verify the type before performing type-specific operations.

### Example with Type Guards

```typescript
function calculateTax(price: number | string, tax: number): number {
  // Type narrowing using typeof
  if (typeof price === "string") {
    // TypeScript now knows price is a string
    price = parseFloat(price.replace("$", ""));
  }
  // TypeScript now knows price is definitely a number
  return price * tax;
}

calculateTax(100, 0.1); // Works with number
calculateTax("$100", 0.1); // Works with string
```

---

## Type Assertions

Sometimes you have more specific information about a value's type than TypeScript can infer. Type assertions allow you to tell TypeScript: **"Trust me, I know this value is of this specific type"**.

Use the `as` keyword followed by the type you want to assert.

**Warning**: Type assertions don't perform runtime checks - they're purely for TypeScript's type system.

### Basic Example

```ts
let instruction: unknown = "saycheze";
console.log(instruction);

let leng: number = (instruction as string).length;
console.log(leng);
```

### Real Example

```ts
function getStudentDataFromServer() {
  let name: string = "yash";
  return {
    id: 89,
    name: name,
  };
}

// Frontend
type Student = {
  id: number;
  name: string;
};

let student = getStudentDataFromServer() as Student;
```

### DOM Example

```typescript
// TypeScript knows this could be any HTMLElement or null
const myPic = document.getElementById("profile-image");

// We assert it's specifically an HTMLImageElement
const myPic = document.getElementById("profile-image") as HTMLImageElement;
```

---

## Literal Types

Literal types **represent exact values, not just types**. Instead of saying "this is a string", you can say **"this is exactly the string 'hello'"**.

On their own, literal types might seem limiting, but when combined with unions, they create powerful, fine-tuned type constraints.

### Basic Literal Types

```typescript
// Literal number type
let zero: 0 = 0;
// zero = 1; // Error - can only be 0

// Literal string type
let mood: "Happy" | "Sad" = "Happy";
mood = "Sad"; // Valid
// mood = "Angry"; // Error - not in union
```

### Practical Literal Type Example

```typescript
type DayOfWeek =
  | "Monday"
  | "Tuesday"
  | "Wednesday"
  | "Thursday"
  | "Friday"
  | "Saturday"
  | "Sunday";

let today: DayOfWeek = "Sunday";
// today = "Funday"; // Error - not a valid day

function isWeekend(day: DayOfWeek): boolean {
  return day === "Saturday" || day === "Sunday";
}
```

### Literal Types with Functions

```typescript
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";

function makeRequest(url: string, method: HttpMethod): void {
  console.log(`Making ${method} request to ${url}`);
}

makeRequest("/api/users", "GET"); // Valid
// makeRequest("/api/users", "PATCH"); // Error
```

---

## Exercises

### Exercise 1: Basic Union Variable

#### Create a variable that can be a number OR a boolean

```typescript
let highScore: number | boolean;
highScore = 1;
highScore = false;
```

### Exercise 2: Array Union Types

#### Create Array that is EITHER all numbers OR all strings (not mixed)

```typescript
const stuff: number[] | string[] = [];
```

### Exercise 3: Literal Type Definition

#### Create a literal type for skill levels. There are 4 allowed values: "Beginner", "Intermediate", "Advanced", and "Expert"

```typescript
type SkillLevel = "Beginner" | "Intermediate" | "Advanced" | "Expert";
```

### Exercise 4: Complex Type with Unions

#### Create a type called SkiSchoolStudent. name must be a string. age must be a number. sport must be "ski" or "snowboard". level must be a value from the SkillLevel type (from above)

```typescript
type SkiSchoolStudent = {
  name: string;
  age: number;
  sport: "ski" | "snowboard"; // Literal union
  level: SkillLevel; // Using our literal type
};

// Example usage
const student: SkiSchoolStudent = {
  name: "Alice",
  age: 25,
  sport: "snowboard",
  level: "Intermediate",
};
```

### Exercise 5: Color System

#### Define a type to represent an RGB color. r should be a number. g should be a number. b should be a number

```typescript
type RGB = {
  r: number;
  g: number;
  b: number;
};
```

#### Define a type to represent an HSL color. h should be a number. s should be a number. l should be a number

```typescript
type HSL = {
  h: number;
  s: number;
  l: number;
};
```

#### Create an array called colors that can hold a mixture of RGB and HSL color types

```typescript
const colors: (RGB | HSL)[] = [];
colors.push({ r: 255, g: 0, b: 0 }); // RGB red
colors.push({ h: 240, s: 100, l: 50 }); // HSL blue
```

### Exercise 6: Function with Union Parameters and Type Narrowing

#### Write a function called greet that accepts a single string OR an array of strings. It should print "Hello, <name>" for that single person OR greet each person in the array with the same format

```typescript
const greet = (person: string | string[]): void => {
  if (typeof person === "string") {
    // Narrowing: we know it's a single string
    console.log(`Hello, ${person}`);
  } else {
    // Narrowing: we know it's an array of strings
    for (let p of person) {
      console.log(`Hello, ${p}`);
    }
  }
};

greet("John"); // Hello, John
greet(["Alice", "Bob", "Carol"]); // Hello, Alice / Hello, Bob / Hello, Carol
```

---

## Tuples

Tuples are a special type exclusive to TypeScript (they don't exist in JavaScript). They are **arrays of fixed lengths** and ordered with specific types - like super rigid arrays.

- Fixed length arrays with predetermined types for each position
- **Order matters - each position has a specific type**
- More restrictive than regular arrays but provide better type safety

### Examples

```typescript
// These are NOT tuples:
// const stuff: (string | number)[] = [1,'asd', 'asdasd', 'asdasd', 2]
// const color: number[] = [23,45,234,234]

// This is a tuple!
const color: [number, number, number] = [255, 0, 45];

type HTTPResponse = [number, string];

const goodRes: HTTPResponse = [200, "OK"];

// An array of tuples:
const responses: HTTPResponse[] = [
  [404, "Not Found"],
  [200, "OK"],
];
```

### Quiz - Tuples

[Tuples Quiz Link](https://bidursapkota00.github.io/Mastering-TypeScript/08-tuples-enums/3.6%20Tuples%20Quiz.html "Tuples Quiz Link")

## Enums

Enums allow us to define a set of **named constants**. We can give these constants numeric or string values. There's quite a few options when it comes to enums!

- Define a set of named constants
- Can have numeric or string values
- **Provide better readability and maintainability**
- Help prevent magic numbers/strings in code

### Examples

#### Numeric Enum

```typescript
enum OrderStatus {
  PENDING,
  SHIPPED,
  DELIVERED,
  RETURNED,
}
const myStatus = OrderStatus.DELIVERED;

function isDelivered(status: OrderStatus) {
  return status === OrderStatus.DELIVERED;
}

isDelivered(OrderStatus.RETURNED);
```

#### String Enum

```typescript
enum ArrowKeys {
  UP = "up",
  DOWN = "down",
  LEFT = "left",
  RIGHT = "right",
}

// Simple use case:
function move(direction: ArrowKeys) {
  console.log(`Moving ${direction}`);
}

move(ArrowKeys.UP); // "Moving up"
move(ArrowKeys.LEFT); // "Moving left"
```

## Interfaces

Interfaces serve almost the exact same purpose as type aliases (with a slightly different syntax). We can use them to create reusable, modular types that describe the shapes of **objects**.

### Basic Interface vs Type Alias

```typescript
// Point as a TYPE ALIAS
type Point = {
  x: number;
  y: number;
};

// Point using an INTERFACE:
interface Point {
  x: number;
  y: number;
}

const pt: Point = { x: 123, y: 1234 };
```

### Interface Properties

Interfaces can include optional properties, readonly properties, and methods.

```typescript
interface Person {
  readonly id: number; // Cannot be modified after creation
  first: string;
  last: string;
  nickname?: string; // Optional property
  sayHi(): string; // Method definition
  // sayHi: () => string;
}

const thomas: Person = {
  first: "Thomas",
  last: "Hardy",
  nickname: "Tom",
  id: 21837,
  sayHi: () => {
    return "Hello!";
  },
};

thomas.first = "Updated"; // Allowed
// thomas.id = 238974;    // Error: readonly property

// Example 2
interface Product {
  name: string;
  price: number;
  applyDiscount(discount: number): number;
}

const shoes: Product = {
  name: "Blue Suede Shoes",
  price: 100,
  applyDiscount(amount: number) {
    const newPrice = this.price * (1 - amount);
    this.price = newPrice;
    return this.price;
  },
};

console.log(shoes.applyDiscount(0.4)); // 60
```

### Re-opening Interfaces

Unlike type aliases, interfaces can be "re-opened" to add new properties.

```typescript
interface Dog {
  name: string;
  age: number;
}

interface Dog {
  breed: string;
  bark(): string;
}

const elton: Dog = {
  name: "Elton",
  age: 0.5,
  breed: "Australian Shepherd",
  bark() {
    return "WOOF WOOF!";
  },
};
```

### Extending Interfaces

Interfaces can extend other interfaces to inherit their properties.

```typescript
// Single inheritance
interface ServiceDog extends Dog {
  job: "drug sniffer" | "bomb" | "guide dog";
}

const chewy: ServiceDog = {
  name: "Chewy",
  age: 4.5,
  breed: "Lab",
  bark() {
    return "Bark!";
  },
  job: "guide dog",
};
```

### Multiple Interface Inheritance

An interface can extend multiple interfaces simultaneously.

```typescript
interface Human {
  name: string;
}

interface Employee {
  readonly id: number;
  email: string;
}

interface Engineer extends Human, Employee {
  level: string;
  languages: string[];
}

const pierre: Engineer = {
  name: "Pierre",
  id: 123897,
  email: "pierre@gmail.com",
  level: "senior",
  languages: ["JS", "Python"],
};
```

### Interface vs Type

- Interface - Only for object shapes
- Type - Can alias any type
- Interfaces are "open" and can be extended/merged, while type aliases are "closed" which means:
  - Interface - Supports declaration merging
  - Type - Does NOT support declaration merging

| Feature             | Interface         | Type Alias          |
| ------------------- | ----------------- | ------------------- |
| Re-opening          | Can be re-opened  | Cannot be re-opened |
| Extending           | `extends` keyword | `&` intersection    |
| Primitives          | Objects only      | Any type            |
| Computed Properties | Limited           | Full support        |

### Quiz - Interface

[Interface Quiz Link](https://bidursapkota00.github.io/Mastering-TypeScript/09-interfaces/5.7%20Interface%20Methods%20Quiz.html "Interface Quiz Link")

## Generics

Generics allow us to **define** reusable **functions and classes that work with multiple types** rather than a single type. They are used extensively throughout TypeScript.

### The Problem Without Generics

Without generics, you'd need separate functions for each type.

```typescript
function numberIdentity(item: number): number {
  return item;
}
function stringIdentity(item: string): string {
  return item;
}
function booleanIdentity(item: boolean): boolean {
  return item;
}

// Hack
function identity(item: any): any {
  return item;
}
```

### Generic Functions

A single generic function can work with multiple types.

```typescript
function identity<T>(item: T): T {
  return item;
}

identity<number>(7);
identity<string>("hello");
identity("auto-inferred"); // Type can be inferred
```

### Generic Functions with Arrays

Generics work great with arrays and collections.

```typescript
function getRandomElement<T>(list: T[]): T {
  const randIdx = Math.floor(Math.random() * list.length);
  return list[randIdx];
}

console.log(getRandomElement<string>(["a", "b", "c"]));
getRandomElement<number>([5, 6, 21, 354, 567, 234, 654]);
getRandomElement([1, 2, 3, 4]); // Type inferred as number[]
```

### Generic Constraints

You can constrain generic types to ensure they have certain properties.

```typescript
// Constraint: T must be an object
function merge<T extends object, U extends object>(object1: T, object2: U) {
  return {
    ...object1,
    ...object2,
  };
}

const comboObj = merge({ name: "colt" }, { pets: ["blue", "elton"] });
```

### Interface-based Constraints

Create interfaces to define what properties your generic types must have.

```typescript
interface Lengthy {
  length: number;
}

function printDoubleLength<T extends Lengthy>(thing: T): number {
  return thing.length * 2;
}

printDoubleLength("string"); // strings have length
printDoubleLength([1, 2, 3]); // arrays have length
// printDoubleLength(234); // numbers don't have length
```

### Default Generic Types

You can provide default types for your generics.

```typescript
function makeEmptyArray<T = number>(): T[] {
  return [];
}

const nums = makeEmptyArray(); // T defaults to number
const bools = makeEmptyArray<boolean>(); // T explicitly set to boolean
```

### Generic Classes

Classes can also use generics to work with multiple types.

```typescript
interface Song {
  title: string;
  artist: string;
}

interface Video {
  title: string;
  creator: string;
  resolution: string;
}

class Playlist<T> {
  public queue: T[] = [];

  add(el: T) {
    this.queue.push(el);
  }
}

const songs = new Playlist<Song>();
const videos = new Playlist<Video>();
```

### Using Built-in Generics

TypeScript provides generics for DOM methods and other built-in functions.

```typescript
// Providing a type to querySelector:
const inputEl = document.querySelector<HTMLInputElement>("#username")!;
inputEl.value = "Hacked!";

const btn = document.querySelector<HTMLButtonElement>(".btn")!;
```

## Type Narrowing

Type narrowing is the process of **refining types within conditional blocks. When working with union types**, TypeScript allows you to narrow down the type to more specific types based on runtime checks.

### Typeof Guards

**Typeof guards** involve checking the type of a value using JavaScript's `typeof` operator before working with it. This is useful when dealing with primitive union types.

```typescript
function triple(value: number | string) {
  if (typeof value === "string") {
    // TypeScript knows value is string here
    return value.repeat(3);
  }
  // TypeScript knows value is number here
  return value * 3;
}

console.log(triple("hello")); // "hellohellohello"
console.log(triple(5)); // 15
```

### Truthiness Guards

**Truthiness guards** check whether a value is truthy or falsy. This is particularly helpful for handling potentially `null` or `undefined` values.

```typescript
const printLetters = (word?: string) => {
  if (word) {
    // TypeScript knows word is string (not undefined) here
    for (let char of word) {
      console.log(char);
    }
  } else {
    console.log("YOU DID NOT PASS IN A WORD!");
  }
};

// DOM element example
const el = document.getElementById("myButton");
if (el) {
  // TypeScript knows el is HTMLElement (not null) here
  el.addEventListener("click", () => console.log("Clicked!"));
} else {
  console.log("Element not found");
}
```

### Equality Narrowing

**Equality narrowing** compares values to determine their types. When two values are equal, TypeScript can infer they must be of the same type.

```typescript
function someDemo(x: string | number, y: string | boolean) {
  if (x === y) {
    // TypeScript knows both x and y are strings here
    x.toUpperCase(); // Works because x is definitely a string
  }
}
```

### In Operator Narrowing

The **`in` operator** checks if a property exists in an object. This allows TypeScript to narrow types based on the presence of specific properties.

```typescript
interface Movie {
  title: string;
  duration: number;
}

interface TVShow {
  title: string;
  numEpisodes: number;
  episodeDuration: number;
}

function getRuntime(media: Movie | TVShow) {
  if ("numEpisodes" in media) {
    // TypeScript knows media is TVShow here
    return media.numEpisodes * media.episodeDuration;
  }
  // TypeScript knows media is Movie here
  return media.duration;
}

console.log(getRuntime({ title: "Amadeus", duration: 140 })); // 140
console.log(
  getRuntime({ title: "Spongebob", numEpisodes: 80, episodeDuration: 30 })
); // 2400
```

### Instanceof Narrowing

**`instanceof` narrowing** checks if an object is an instance of a particular class or constructor function.

```typescript
// With built-in types
function printFullDate(date: string | Date) {
  if (date instanceof Date) {
    // TypeScript knows date is Date here
    console.log(date.toUTCString());
  } else {
    // TypeScript knows date is string here
    console.log(new Date(date).toUTCString());
  }
}

// With custom classes
class User {
  constructor(public username: string) {}
}

class Company {
  constructor(public name: string) {}
}

function printName(entity: User | Company) {
  if (entity instanceof User) {
    // TypeScript knows entity is User here
    console.log(`User: ${entity.username}`);
  } else {
    // TypeScript knows entity is Company here
    console.log(`Company: ${entity.name}`);
  }
}
```

### Type Predicates

**Type predicates** are custom functions that return a boolean and tell TypeScript about the type of a value. They use the special `parameterName is Type` return type syntax.

```typescript
interface Cat {
  name: string;
  numLives: number;
}

interface Dog {
  name: string;
  breed: string;
}

// Type predicate function
function isCat(animal: Cat | Dog): animal is Cat {
  return (animal as Cat).numLives !== undefined;
}

function makeNoise(animal: Cat | Dog): string {
  if (isCat(animal)) {
    // TypeScript knows animal is Cat here
    console.log(`${animal.name} has ${animal.numLives} lives`);
    return "Meow";
  } else {
    // TypeScript knows animal is Dog here
    console.log(`${animal.name} is a ${animal.breed}`);
    return "Woof!";
  }
}
```

### Discriminated Unions

**Discriminated unions** use a common literal property (discriminant) to distinguish between different types in a union. This pattern is extremely powerful for type-safe switch statements.

```typescript
interface Rooster {
  name: string;
  weight: number;
  age: number;
  kind: "rooster"; // Literal type discriminant
}

interface Cow {
  name: string;
  weight: number;
  age: number;
  kind: "cow"; // Literal type discriminant
}

interface Pig {
  name: string;
  weight: number;
  age: number;
  kind: "pig"; // Literal type discriminant
}

interface Sheep {
  name: string;
  weight: number;
  age: number;
  kind: "sheep"; // Literal type discriminant
}

type FarmAnimal = Pig | Rooster | Cow | Sheep;

function getFarmAnimalSound(animal: FarmAnimal) {
  switch (animal.kind) {
    case "pig":
      // TypeScript knows animal is Pig here
      return "Oink!";
    case "cow":
      // TypeScript knows animal is Cow here
      return "Moooo!";
    case "rooster":
      // TypeScript knows animal is Rooster here
      return "Cockadoodledoo!";
    case "sheep":
      // TypeScript knows animal is Sheep here
      return "Baaa!";
    default:
      // Exhaustiveness check - ensures all cases are handled
      const _exhaustiveCheck: never = animal;
      return _exhaustiveCheck;
  }
}

const stevie: Rooster = {
  name: "Stevie Chicks",
  weight: 2,
  age: 1.5,
  kind: "rooster",
};

console.log(getFarmAnimalSound(stevie)); // "Cockadoodledoo!"
```

## Index Signatures

Index signatures **allow you to define the types of object properties when you don't know all the property names ahead of time, but you know the shape of the values and what the keys should look like.**

### Basic Syntax

The basic syntax for an index signature is:

```typescript
[key: KeyType]: ValueType;
```

Where `KeyType` can be `string`, `number`, or `symbol`, and `ValueType` can be any type.

### String Index Signatures

**String index signatures** allow any string as a key and specify the type of all values.

```typescript
// Basic string index signature
interface StringDictionary {
  [key: string]: string;
}

const colors: StringDictionary = {
  red: "#FF0000",
  green: "#00FF00",
  blue: "#0000FF",
  yellow: "#FFFF00",
};

// You can add any string key
colors.purple = "#800080"; // Valid
colors.orange = "#FFA500"; // Valid

// But values must be strings
// colors.count = 42; // Error: Type 'number' is not assignable to type 'string'
```

### Number Index Signatures

**Number index signatures** use numbers as keys, useful for array-like objects.

```typescript
interface NumberDictionary {
  [index: number]: string;
}

const fruits: NumberDictionary = {
  0: "apple",
  1: "banana",
  2: "orange",
};

console.log(fruits[0]); // "apple"
console.log(fruits[1]); // "banana"

// Can be used for sparse arrays
const sparseArray: NumberDictionary = {
  10: "ten",
  100: "hundred",
  1000: "thousand",
};
```

### Mixed Index Signatures

You can combine string and number index signatures.

```typescript
interface MixedDictionary {
  [key: string]: string | number;
  [index: number]: string;
}

const mixed: MixedDictionary = {
  name: "John", // string key, string value
  age: 30, // string key, number value
  0: "first", // number key, string value
  1: "second", // number key, string value
};

console.log(mixed.name); // "John"
console.log(mixed[0]); // "first"
```

### Index Signatures with Known Properties

You can combine index signatures with known properties. Known properties must be compatible with the index signature type.

```typescript
interface UserPreferences {
  theme: "light" | "dark"; // Known property
  language: string; // Known property
  [setting: string]: string; // Index signature
}

const userPrefs: UserPreferences = {
  theme: "dark",
  language: "en",
  fontSize: "16px", // Additional property via index signature
  fontFamily: "Arial", // Additional property via index signature
};

// All known properties are accessible with full type safety
console.log(userPrefs.theme); // Type: "light" | "dark"
console.log(userPrefs.language); // Type: string

// Index signature properties are also accessible
console.log(userPrefs.fontSize); // Type: string
```

### Generic Index Signatures

Use generics to create flexible, reusable index signature types.

```typescript
interface Dictionary<T> {
  [key: string]: T;
}

// Dictionary of numbers
const scores: Dictionary<number> = {
  alice: 95,
  bob: 87,
  charlie: 92,
};

// Dictionary of boolean flags
const flags: Dictionary<boolean> = {
  isEnabled: true,
  isVisible: false,
  isActive: true,
};

// Dictionary of objects
interface User {
  name: string;
  email: string;
}

const users: Dictionary<User> = {
  user1: { name: "Alice", email: "alice@example.com" },
  user2: { name: "Bob", email: "bob@example.com" },
};
```

### Example with function

```ts
//These 2 employees have different comp packages
const employee1 = {
  base: 100000,
  yearlyBonus: 20000,
};

const employee2 = {
  contract: 110000,
};

//But we can still compare their payment using an index sig.!
const totalComp = (salaryObject: { [key: string]: number }) => {
  let income = 0;
  for (const key in salaryObject) {
    income += salaryObject[key];
  }
  return income;
};

totalComp(employee1); // => 120,000
totalComp(employee2); // => 110,000
```

### Index Signatures vs Record Utility Type

TypeScript provides a `Record<K, T>` utility type that's often more concise than index signatures.

```typescript
// Using index signature
interface IndexSignatureDict {
  [key: string]: number;
}

// Using Record utility type (equivalent)
type RecordDict = Record<string, number>;

// Both work the same way
const scores1: IndexSignatureDict = { alice: 95, bob: 87 };
const scores2: RecordDict = { alice: 95, bob: 87 };
```

## TypeScript Classes

### Basic Class Structure

Classes in TypeScript are blueprints for creating objects with specific properties and methods.

```typescript
class Player {
  constructor(
    public first: string,
    public last: string,
    protected _score: number
  ) {}
}
```

The `constructor` defines parameters that become class properties. The access modifiers (`public`, `protected`) automatically create and assign these properties.

### Access Modifiers

TypeScript provides three access modifiers to control visibility:

```typescript
class Player {
  constructor(
    public first: string, // Accessible everywhere
    public last: string,
    protected _score: number // Accessible in this class and subclasses
  ) {}

  private secretMethod(): void {
    // Only accessible within this class
    console.log("SECRET METHOD!!");
  }
}
```

- **public**: accessible from anywhere (default if not specified)
- **protected**: accessible within the class and its subclasses
- **private**: only accessible within the class itself

### Getters and Setters

Getters and setters allow you to control how properties are accessed and modified:

```typescript
get fullName(): string {
  return `${this.first} ${this.last}`;
}

get score(): number {
  return this._score;
}

set score(newScore: number) {
  if (newScore < 0) {
    throw new Error("Score cannot be negative!");
  }
  this._score = newScore;
}

const elton = new Player("Elton", "Steele", 100);
elton.fullName;  // Access like a property, not a method
```

Getters compute values on-the-fly, while setters can validate data before assignment.

### Class Inheritance

Classes can extend other classes to inherit their properties and methods:

```typescript
class SuperPlayer extends Player {
  public isAdmin: boolean = true;

  maxScore() {
    this._score = 99999999; // Can access protected properties from parent
  }
}
```

`SuperPlayer` inherits all properties and methods from `Player` and adds its own functionality.

### Implementing Interfaces

Classes can implement interfaces to ensure they have specific properties and methods:

```typescript
interface Colorful {
  color: string;
}

interface Printable {
  print(): void;
}

class Bike implements Colorful {
  constructor(public color: string) {}
}

class Jacket implements Colorful, Printable {
  constructor(public brand: string, public color: string) {}

  print() {
    console.log(`${this.color} ${this.brand} jacket`);
  }
}

const bike1 = new Bike("red");
const jacket1 = new Jacket("Prada", "black");
```

A class can implement multiple interfaces, ensuring it has all required properties and methods defined by those interfaces.

### Abstract Classes

Abstract classes serve as base classes that cannot be instantiated directly. They can contain abstract methods that must be implemented by subclasses:

```typescript
abstract class Employee {
  constructor(public first: string, public last: string) {}

  abstract getPay(): number; // Must be implemented by subclasses

  greet() {
    // Concrete method available to all subclasses
    console.log("HELLO!");
  }
}
```

You cannot create an instance of `Employee` directly; you must extend it.

### Concrete Implementations of Abstract Classes

Subclasses must implement all abstract methods:

```typescript
class FullTimeEmployee extends Employee {
  constructor(first: string, last: string, private salary: number) {
    super(first, last); // Call parent constructor
  }

  getPay(): number {
    return this.salary;
  }
}

class PartTimeEmployee extends Employee {
  constructor(
    first: string,
    last: string,
    private hourlyRate: number,
    private hoursWorked: number
  ) {
    super(first, last);
  }

  getPay(): number {
    return this.hourlyRate * this.hoursWorked;
  }
}

const betty = new FullTimeEmployee("Betty", "White", 95000);
console.log(betty.getPay()); // 95000

const bill = new PartTimeEmployee("Bill", "Billerson", 24, 1100);
console.log(bill.getPay()); // 26400
```

Each subclass provides its own implementation of the abstract `getPay()` method, while inheriting the `greet()` method from the parent class.
