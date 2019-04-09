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

1. The component is just a javascript function! (Colloquially referred to as a functional component in React, there are also class components which are javascript classes, but we'll get to those later)
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
                    <MyHelloComponent />
                </div>

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

In the example above you can see one of the things that makes React so flexible; the ability to reuse custom components inside other components. This makes it very easy to create pieces of functionality which then can be reused throughput your app, and even in other apps!

I mentioned in an earlier aside that React components can also be javascript classes. React class components must extend (inherit from) the React Component class. They look a little different than function components, instead of returning JSX (Classes can't return anything silly!) React class components have a render method which returns JSX and which will be called every time the component is rendered.

```jsx
// Here is the same hello component as above but as a class component instead
class MyHelloComponent extends React.Component {

  render () {
     return <div>Hello World!</div>;
  }
}

const element = <div>
                    <h1>My Hello Website<h1>
                    <MyHelloComponent />
                </div>

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

There are a few other ways that class components differ from function components, but we'll cover those a little later.

# <center>Communication is Key</center>

Ok, so we learned about JSX and react elements and components. Thats great and all, but what if we want to pass data between components? Well it turns out that the bright people over at facebook have thought about it, and the solution they came up with is to create a special property on a component called props. Props (short for properties, aren't engineers creative?) is just a simple javascript object which contains all the data passed from the parent component. **Props is read only, you should NEVER modify anything on props, only read values passed from the parent.** So we know that the props object is a special property on a component that contains the values passed down from the parent, but how do we pass those values down exactly?

```jsx
// function components recieve props as the first argument to the function
const MyHelloFunctionComponent = props => {
  // Will render <div>Props fresh from the oven</div>
  return <div>{props.message}</div>;
};

// Class components have a property called this.props which will contain props. The props object is also passed in the class constructor. It is considered good practice to call the base class constructor with the props passed in to the constructor in a class component
class MyHelloClassComponent extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
  // Will render <div>My props are the best</div>
    return <div>My props are {props.payload.message}</div>;
  }
}

const payload = { message: 'My props are the best' }

const element = <div>
                    <h1>My Hello Website<h1>
                    <MyHelloFunctionComponent message={'Props fresh from the oven'}/>
                    <MyHelloClassComponent payload={payload}/>
                </div>

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

As you can see in the example above, props are passed to the child components via attributes. These attributes then get passed to the child component as key value pairs on the props object. The values passed to props can be any valid javascript expression, this includes objects, arrays, functions, and even other React components!

## <center>I'm All Out of Clever Titles, I think it's Bed Time</center>

The last thing I want to cover is how function components are different from class components. The main difference is state. Functional components cannot have persistent state, oh sure you can use closures to capture variables just like with any other javascript functions, but react won't pick up those changes and re-render (If you have no idea what I'm talking about no worries, I'm rambling a little). So what is state in the context of react? Well at a high level, it's basically a special property on class components (**Only on class components, except for react hooks but thats another beast**) which stores your mutable data (data that changes). **State is private to a component, however you can pass it down to child components through props**.

A few more important things to know about state

1. Never change state directly, react class components expose a setState function to interact with state
2. You see rule number one? this is the exception to it. You can set state directly **only in the constructor of a class component**
3. The setState function is asynchronous, which means it might not run when you expect it to. If you need to do something based on the previous state or if you need to run something that depends on the new state, use one of thew overloads of setState
4. When the state or props of a component changes, that component will re-render (There are some caveats to this but I won't go into detail here)

```jsx
class MyClickComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      numClicked: 0
    };
  }

  handleClick = () => {
    // This overload to set state passes in the previous state as a parameter
    this.setState(prevState => {
      return { numClicked: prevState.numClicked++ };
    });
  };

  // render will be called every time numClicked changes, essentially whenever the div is clicked
  render() {
    return (
      <div onClick={handleClick}>
        I Have been clicked {this.state.numClicked} times
      </div>
    );
  }
}

const element = (
  <div>
    <MyClickComponent />
  </div>
);

ReactDOM.render(element, document.getElementById("root"));
```
