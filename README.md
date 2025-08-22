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
- And many others!

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
