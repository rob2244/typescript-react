# <center>What the Heck is Typescript?</center>

Typescript is a strongly typed language written and maintained by Microsoft which transpiles down to javascript.
What the heck does that mean you ask? Well dear reader it means that **ANY VALID JAVASCRIPT IS ALSO VALID TYPESCRIPT.** Yes ladies and gentlemen that is correct, since typescript becomes javascript eventually anyway; any valid javascript you write is also valid typescript. You can think of typescript as javascript++ or is it ++javascript?

### Whats wrong with javascript! Why do we need to use this typescript thing?

Javascript is dynamically typed, which means things don't go wrong until you actually run the program. In a statically typed language like C# theres a compiler that performs syntax analysis every time you compile. The compiler freaks out if you do things like say: assign a string to a variable you declared as an interger.

```C#
    // DANGER WILL ROBINSON, THIS WON'T COMPILE
    int number = 5
    number = "Hello"
```

However, since javascript is dynamically typed and doesn't have a compiler, it has no problem running the following code

```javascript
let number = 5;
number = "Hello";
```

Indeed it will happily set number equal to whatever you tell it to. More importantly it will try to run any function on the number variable you tell it to, and it will try it's best to make the function work.

```javascript
// THIS IS VALID JS CODE, JAVASCRIPT WILL CAST 5 TO A STRING AND WUT WILL EQUAL Hello5
let number = 5;
number = "Hello";
let wut = number + 5;
```

This can cause many subtle bugs especially in large scale applications.

# <center>Typescript to the rescue</center>

Typescript solves the problems above by introducing type safety. Basically it provides a compiler that works just like the C# compiler does, and will error out if you accidently assign a number to a string or vice versa. It also gives you nice things like method auto completion. **Typescript is a double edged sword, while it becomes much easier to catch mistakes and avoid subtle bugs, sometimes the compiler makes your life harder and you'll miss the dynamic typing of javascript**

Typescript supports all the scalar types javascript does, but unlike javascript it actually enforces them.
It also adds some additional types, but here are the ones we actually care about

- string
- number
- object
- function
- boolean
- any
- undefined and null
- enum
- tuple
- array

Most of these types align pretty closely with their javascript counterparts, so I won't spend too much time going over them. You can find more information on Typescript types [here](https://www.typescriptlang.org/docs/handbook/basic-types.html). The one type I will point out is any. Any is interesting in that it basically tells the compiler: "Hey I have no idea what this is going to be". If you specify a variable is of type any, it can literally be anything at any time, just like vanilla js variables.

**Note: Typescript also supports interfaces, generics and custom types**

## Typing in typescript, no not that kind of typing

Types in typescript are specified by a colon and then the type name. Types come after the variable name instead of before like most languages

```typescript
/*
Note that I tend to use let and const to declare variables. You can use var, but in newer versions of javascript and typescript it's considered best practice to use const for constants and let for mutable variable
*/

let myNum: number = 5;

// The compiler will throw an error here
myNum = "Hello";

// Here are a few more examples of typing
const myString: string = "Hello";
const myObj: object = {};
const myBool: boolean = false;

// Note: the following will not throw a compile error because it is using the type any
let myAny: any = "Hello";
myAny = 5;
myAny = {};

/*
Function parameters and return types can also be typed.
Here I'm writing a function which takes one paramater str of type string
and returns a string. Note the return type is specified after a colon after the
method header closing parenthesis ):string
*/
const myFunc = (str: string): string => hello.toUpperCase();

/*
Arrays in typescript are generic, which means you need to specify the elements they will contain when you create them.
Here I am declaring two string arrays, the second line type definition is just shorthand for the first.
*/

const myArr: Array<number> = [1, 2, 3];
const myArr: number[] = [1, 2, 3];
```

## Custom Typing: Or how I stopped worrying and learned to love the Typescript

Typescript also supports creating classes and interfaces, which can then be used as custom types for functions and variables. Classes support all the things you're used to in an statically typed object oriented language. Fields, properties, methods, a constructor etc. Additionally classes can inherit from other classes which is done with the extends keyword. **Note: whenever accessing class attributes inside of a typescript class you must use the this keyword**

```typescript
// Here I create a class called ThingPrinter

class ThingPrinter {
  private _thing1: string;
  private _thing2: string;
  _thing3: string = "I am thing three";

  constructor(thing1: string) {
    this._thing1 = thing1;
    this._thing2 = "I am thing2";
  }

  printThing1() {
    console.log(this._thing1);
  }

  printThing2() {
    console.log(this._thing2);
  }
}

/* 
Here is a class that inherits from ThingPrinter, the super keyword is used to call the base class constructor.
Notice: overrides don't require any kind of special keyword, any method of the same name in the child class will override 
the parent method.
*/

class InterestingThingPrinter extends ThingPrinter {
  constructor(thing1: string) {
    super(thing1);
  }

  printThing2() {
    console.log("I am the interesting thing 2");
  }
}

// Finally, I can create a function that takes a thing printer as a parameter

const printAllTheThings = (printer: ThingPrinter) => printer.printThing2();
const printer = new InterestingThingPrinter("Hello");

// Will print: I am the interesting thing 2
printAlltheThings(printer);
```

Interfaces work pretty much the same way they do in most OOP languages. An interface is a contract that the implementing class must fulfill.

```typescript
// Here I am declaring an interface called ThingPrinter, the implementing class must have a method called printThing
// that takes in a string and doesn't return anything
interface IThingPrinter {
  printThing(thing: string): void;
}

class ThingPrinter implements IThingPrinter {
  printThing(thing: string): void {
    console.log(thing);
  }
}
```

Unfortunately there's not enough time to cover async/await and promises as they're a whole topic of their own. I also won't write anything about generics because they work pretty much the same way they do in C#
