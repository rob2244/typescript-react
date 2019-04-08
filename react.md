# <center>React, a Whirlwind Tour</center>

### What is it?

React is a javascript library for building web applications using reusable building blocks called components. It's simplicity makes it very easy to learn, and alos gives it a lot of power and flkexibility.

### What is the fundamental problem React addresses?

No technology is created in a vacuum, React is no different. Mother is the necessity of invention, and it often helps to know what problem the invention solves in order to understand the technology.

- Reusability: One of the big problems web developers face is reusability. If I create a ui with a user info display, and I want to use that display in 5 other places I'm out of luck. Vanilla css/html/javascript presents no easy way to create pieces of functionality across a website
- Code Duplication: This one kind of goes along with reusability. Naturally if your pieces of functionality aren't reusable, the only choice you have is to duplicate code
- Data changes propagating to the dom: Prior to React/Angular/Vue and a lot of these ither web frameworks/libraries, when your data chanhed you would have to reach out to the dom and manually update an elements data via document.getElementById or jquery. React solves this problem by automagically wiring the dom to re-render on state changes. This means that you should almost never have to interact with the dom directly, instead declaring state bindings which React updates transparently on state changes.
- A host of other things including ease of unit testing, namespacing, uni-directional data flow etc...

# <center> Enough Talk Let's Dance </center>

I know what you're thinking: "All of this theory is great and all Robin, but can we get to how this thing works?". Well here it is dear reader, the secret to React: **everything is... javascript**. Yup, you read that correctly, all those funky html tags you see? javascript. React takes all the javascript your react code creates and creates a virtual dom out of it. Then whenever something in your code changes it checks the new tree that is generated against the existing tree and rerenders only the parts that changed. It also takes that tree and then paints the parts that changed to the browser in the form of html and css.

### JSX

The language that React uses is called JSX, and it looks basically exactly like HTML.

```jsx
// This is not html, it's JSX!
const myJSX = <div className="greeting">Hello I'm JSX<div>
```

under the hood when the JSX is parsed it's actually transformed into this:

```javascript
// See it's just a javascript object!

const myJSX = React.createElement(
  "div",
  { className: "greeting" },
  `Hello I'm JSX`
);
```

You can add any valid javascript expressions to jsx by using curly braces

```javascript
// Value of div will be Hello Robin user
const isLoggedIn = true;
const name = "Robin";

const myJSX = (
  <div>
    Hello {name} {isLoggedIn ? "user" : "guest"}
  </div>
);

// The div will have a css class of greeting
const myClass = "greeting";
const myJSX2 = <div className={myClass}>Hello World!</div>;
```

So far we've only seen JSX elements. A JSX element is just like a regular old dom element, except remember it gets transformed to javascript by React under the hood. All the dom elements you know and love are represented as jsx elements: div, input, main, aside, footer, header etc.

# <center>Enter: React Components</center>

If React only had elements it would already be really cool. However the power in react comes from components. Components are a blend of jsx, js and css which combine to make reusable content. If you're confused right now don't worry, so am I.

```jsx
// In their simplest form components are just javascript functions that return JSX
const MyHelloComponent = () => {
  return <div>Hello World!</div>;
};
```

Above we have a simple component. Two things may be readily apparent:

1. The component is just a javascript function!
2. The name of the component is not camel cased

No, number 2 is not a mistake on my part friends; **The names of react components must start with an uppercase letter.** Remember those jsx elements we talked about a second ago? Component names have to start with an uppercase letter so react doesn't get confusd between components an elements.

```jsx
// Here is our hello component, and an example of how it might be used
// Notice the call to ReactDOM.render this tells react where in the actual HTML DOM
// it should render the virtual dom it constructs.
const MyHelloComponent = () => {
  return <div>Hello World!</div>;
};

const element = <div>
                    <h1>My Hello Website<h1>
                    <MyHelloComponent/>
                </div>

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
