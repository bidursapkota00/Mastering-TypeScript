# TypeScript Course

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

```bash
{
  "compilerOptions": {
    "target": "ES2020",
    # "lib": [], list of types for DOM, ES2020; better remain commented and just control by target
    "module": "commonjs",    # ES5 or something from browser, commonjs is for node
    "outDir": "./dist",
    "rootDir": "./src",
    "noEmitOnError": true,
    "strict": true,
    # "strictNullChecks": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true
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
