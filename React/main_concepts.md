[Source](https://reactjs.org/docs/hello-world.html)

# Hello World

The smallest react example looks like this 

```jsx
ReactDOM
  .createRoot(document.getElementById('root'))
  .render(<h1>Hello, world!</h1>);
```
# Introducing JSX

JSX produces React “elements”
```jsx
const element = <h1>Hello, world!</h1>;
```
### Embedding Expressions in JSX

You can put any valid JavaScript expression inside the curly braces in JSX.
```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

### JSX is an Expression Too

After compilation, JSX expressions become regular JavaScript function calls and evaluate to JavaScript objects.

This means that you can use JSX inside of `if` statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions:
```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;  
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### Specifying Attributes with JSX

You may use quotes to specify string literals as attributes:
```jsx
const element = <a href="https://www.reactjs.org"> link </a>;
```
You may also use curly braces to embed a JavaScript expression in an attribute:
```jsx
const element = <img src={user.avatarUrl}></img>;
```
>**Warning:**  Since JSX is closer to JavaScript than to HTML, React DOM uses camelCase property naming convention instead of HTML attribute names. For example, `class` becomes `className` in JSX, and `tabindex` becomes `tabIndex`.


### Specifying Children with JSX

If a tag is empty, you may close it immediately with `/>`:
```jsx
const element = <img src={user.avatarUrl} />;
```
JSX tags may contain children:
```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```
### JSX Represents Objects

Babel compiles JSX down to `React.createElement()` calls.

These two examples are identical:
```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
```jsx
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
`React.createElement()` performs a few checks to help you write bug-free code but essentially it creates an object like this:
```jsx
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```
These objects are called “React elements”. You can think of them as descriptions of what you want to see on the screen. React reads these objects and uses them to construct the DOM and keep it up to date.

# Rendering Elements

```jsx
const element = <h1>Hello, world</h1>;
```
Unlike browser DOM elements, React elements are plain objects, and are cheap to create.

## Rendering an Element into the DOM
Let’s say there is a `<div>` somewhere in your HTML file:
```jsx
<div id="root"></div>
```
To render a React element, first pass the DOM element to `ReactDOM.createRoot()`, then pass the React element to `root.render()`:
```jsx
const element = <h1>Hello, world</h1>;
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
root.render(element);
```

## Updating the Rendered Element
**React elements are immutable.** Once you create an element, you can’t change its children or attributes.

Naturally, the next question would then be - How to render a changing UI? Here is a crude ticking clock example:
```jsx
const root = ReactDOM.createRoot(
  document.getElementById('root')
);

function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  root.render(element);}

setInterval(tick, 1000);
```

# Components and Props

## Function and Class Components

The simplest way to define a component is to write a JavaScript function:
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
You can also use an ES6 class to define a component:
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
The above two components are equivalent from React’s point of view.

## Rendering a Component

Previously, we only encountered React elements that represent DOM tags:

```jsx
const element = <div />;
```

However, elements can also represent user-defined components:

```jsx
const element = <Welcome name="Sara" />;
```

When React sees an element representing a user-defined component, it passes JSX attributes and children to this component as a single object. We call this object “props”.

For example, this code renders “Hello, Sara” on the page:

```jsx
function Welcome(props) {  
	return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(element);
```
>**Note: Always start component names with a capital letter.** React treats components starting with lowercase letters as DOM tags.

## Composing Components

Components can refer to other components in their output.
```jsx
function App() {
  return (
    <div>
      <Welcome name="Sara" />      
      <Welcome name="Cahal" />      
      <Welcome name="Edite" />    
    </div>
  );
}
```

## Props are Read-Only

React is pretty flexible but it has a single strict rule:

**All React components must act like pure functions with respect to their props,** ie. never try to modify the props


# State and Lifecycle

Would be better if you read the original [article](https://reactjs.org/docs/state-and-lifecycle.html). I'll just add some important points here.

## Adding Local State to a Class

A class components state variables are stored inside `this.state`. We set the initial state inside the components constructor
```jsx
constructor(props) {
  super(props);    
  this.state = {date: new Date()};
}
```
> **Note:** The constructor is the only the place where we directly assign the state. Otherwise we will use `this.setState`.

## Adding Lifecycle Methods to a Class

The `componentDidMount()` method runs after the component output has been rendered to the DOM.
The `componentWillUnmount()` method runs just before the component gets removed from DOM.

## Using State Correctly

### Do Not Modify State Directly
```jsx
// Wrong
this.state.comment = 'Hello';
```
```jsx
// Correct
this.setState({comment: 'Hello'});
```
The only place where you can assign  `this.state`  is the constructor.

### State Updates May Be Asynchronous

React may batch multiple `setState()` calls into a single update for performance. Because `this.props` and `this.state` may be updated asynchronously. Code like the one shown below is a bad idea.
```jsx
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```
To fix it, use a second form of `setState()` that accepts a function with the previous state as the first argument, and the props at the time the update is applied as the second argument:
```jsx
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

# Handling Events

-   React events are named using camelCase, rather than lowercase.
-   With JSX you pass a function as the event handler, rather than a string.

For example, the HTML:
```jsx
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

is slightly different in React:
```jsx
<button onClick={activateLasers}>  
  Activate Lasers
</button>
```

# Conditional Rendering

You can render components conditionally like so.
```jsx
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
      return <UserGreeting />;
  }  
  return <GuestGreeting />;
}
```

### Inline If with Logical && Operator

```jsx
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>      
       }    
    </div>
  );
}
```

It works because in JavaScript,  `true && expression`  always evaluates to  `expression`, and  `false && expression`  always evaluates to  `false`.

Therefore, if the condition is  `true`, the element right after  `&&`  will appear in the output. If it is  `false`, React will ignore and skip it.

### Preventing Component from Rendering

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return `null` instead of its render output.

# Lists and Keys

### Rendering Multiple Components

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

Then, we can include the entire `listItems` array inside a `<ul>` element:
```jsx
<ul>{listItems}</ul>
```

It is recommended to add unique keys whenever using lists like these. Read about this more in the coming section.

## Keys

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
      {number}
  </li>
);
```

The best way to pick a key is to use a string that uniquely identifies a list item among its siblings. Most often you would use IDs from your data as keys:

```jsx
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
      {todo.text}
  </li>
);
```

When you don’t have stable IDs for rendered items, you may use the item index as a key as a last resort:

```jsx
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
      {todo.text}
  </li>
);
```

If you choose not to assign an explicit key to list items then React will default to using indexes as keys.
